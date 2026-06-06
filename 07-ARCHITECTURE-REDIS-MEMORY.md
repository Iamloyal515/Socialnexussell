# Architecture: Redis & In-Memory Support

---

## Design Philosophy

**Redis-first with graceful in-memory fallback.**

The system tries Redis on startup. If Redis is unavailable, it automatically falls back to in-memory `Map()` storage with zero downtime. When Redis comes back, it reconnects via a 30-second heartbeat.

```
┌─────────────────────────────────────────────────────────┐
│                    Application Layer                     │
├─────────────────────────────────────────────────────────┤
│  cacheService.js  ←→  redisRateLimiter.js               │
│       ↓                    ↓                             │
│  ┌─────────┐         ┌──────────┐                       │
│  │  Redis  │         │ In-Memory│                       │
│  │ ioredis │         │   Map()  │                       │
│  └─────────┘         └──────────┘                       │
│       ↑                    ↑                             │
│  30s heartbeat      Auto-enabled on Redis failure       │
└─────────────────────────────────────────────────────────┘
```

---

## Redis Configuration

**File:** `backend/src/services/cacheService.js`  
**Config resolver:** `backend/src/utils/portDetector.js`

| Env Variable | Default | Purpose |
|--------------|---------|---------|
| `REDIS_HOST` | `127.0.0.1` | Redis server host |
| `REDIS_PORT` | `6379` | Redis server port |
| `REDIS_PASSWORD` | (none) | Redis auth password |
| `REDIS_DB` | `0` | Redis database number |

### Connection Behavior

| Setting | Value |
|---------|-------|
| Connect timeout | 2 seconds |
| Max retries per request | 0 (no retry storms) |
| Retry strategy | `null` (heartbeat handles reconnect) |
| Heartbeat interval | 30 seconds |
| Error log throttle | Once per minute when down |
| Lazy connect | Yes (non-blocking startup) |

### Global Flag

```javascript
let isRedisAvailable = false; // Set true on 'ready', false on first error
```

All Redis operations check this flag before hitting the socket. Prevents `ECONNREFUSED` spam.

---

## What Uses Redis

| Consumer | File | Purpose | TTL |
|----------|------|---------|-----|
| **Key validation cache** | `authenticateKey.js` | Cache license key lookups | 5 min |
| **Distributed rate limiting** | `redisRateLimiter.js` | Per-tier endpoint limits | Per-endpoint |
| **General cache** | `cacheService.js` | get/set/delete with TTL | Configurable |

### Rate Limit Tiers (via Redis)

`redisRateLimiter.js` enforces limits per endpoint category:

| Category | Applies To |
|----------|-----------|
| Auth | Login, key check |
| API | General API calls |
| Marketplace | Marketplace CRUD |
| Upload | File uploads |
| Admin | Admin endpoints |
| Telegram ops | Telegram feature calls |

---

## What Uses In-Memory (Always or as Fallback)

### Must Be In-Memory (By Design)

| Consumer | File | Why |
|----------|------|-----|
| **GramJS Telegram sessions** | `backend/index.js` `authenticatedUsers` | Live MTProto connections must be in-process |
| **Active task processors** | `bulk-channel-join-v2.js` | Real-time task state |
| **Flood wait tracking** | `enhancedFloodWaitService.js` | Sub-millisecond lookups |

### In-Memory with Redis Fallback

| Consumer | File | Purpose | Fallback Trigger |
|----------|------|---------|-----------------|
| **Key validation** | `authenticateKey.js` | 5-min key cache | Redis down |
| **Rate limiting** | `redisRateLimiter.js` | Per-IP/endpoint limits | Redis down |
| **General cache** | `cacheService.js` | Any cached data | Redis down |

### Always In-Memory

| Consumer | File | Purpose | Size |
|----------|------|---------|------|
| Admin API cache | `admin.js` `_adminCache` | Stale-while-revalidate stats | Unbounded |
| Admin token cache | `admin.js` `_adminTokenCache` | 5-min admin auth | Small |
| Tier usage cache | `tierEnforcement.js` `usageCache` | 1-min usage stats | Per-key |
| Auth rate limit fallback | `authenticateKey.js` `authRateLimitStore` | IP auth throttling | Per-IP |
| Mongo session query cache | `mongoSessionPersistence.js` `queryCache` | Session DB queries | 800 entries, 60s TTL |
| Channel entity cache | `channelEntityCache.js` | GramJS entity objects | RAM-scaled |
| Content dedupe | `content-orchestrator-dedupe-sanitize.js` | Media/text fingerprints | RAM-scaled |
| Membership cache | `bulk-channel-join-v2.js` | Already-joined channels | Per-task |
| Dynamic config cache | `dynamic-config.js` | RAM-scaled cache limits | Hardware-aware |

---

## Memory Management

### Memory Guard (`memoryGuard.js`)

| Threshold | Action |
|-----------|--------|
| Soft (70% heap) | Trim GramJS entity caches |
| Hard (85% heap) | Shed in-memory caches, reject new tasks |
| Critical (95% heap) | Force GC, disconnect idle sessions |

### In-Memory Cache Cleanup

`cacheService.js` runs periodic cleanup:
- Evict expired TTL entries
- Shed oldest entries under memory pressure
- Log cache stats (hits, misses, errors)

### Dynamic Config Scaling

`dynamic-config.js` reads hardware on startup and sets:

| Setting | Low RAM (<4GB) | High RAM (>16GB) |
|---------|----------------|------------------|
| MongoDB pool | 5 | 50 |
| UV threadpool | 4 | 16 |
| Worker pool | 2 | 8 |
| In-memory cache max | 500 entries | 10,000 entries |

---

## PM2 / Scaling Notes

**File:** `backend/ecosystem.config.js`

| Setting | Value | Reason |
|---------|-------|--------|
| Mode | `fork` (single process) | Telegram sessions in memory |
| Heap | 6GB (`--max-old-space-size=6144`) | Large GramJS entity caches |
| Instances | 1 | Cannot cluster without session registry |

### Future Cluster Scaling Requires

Before enabling PM2 cluster mode:

1. **Redis-backed session registry** — share session state across workers
2. **Task state in Redis/Mongo** — not just in-process Maps
3. **Sticky sessions** — route requests to correct worker
4. **Session migration** — move sessions between workers on restart

> The codebase already has MongoDB task persistence and session backup/restore as foundations for this.

---

## Health Reporting

| Endpoint | Redis Info |
|----------|-----------|
| `GET /health` | Redis connected/fallback status |
| Admin Performance tab | Redis hit rate, connection state |
| `livenessWatchdog.js` | Periodic Redis ping |

### Cache Statistics

`cacheService.js` tracks:

```javascript
stats: { hits, misses, sets, deletes, errors }
```

Visible in admin performance panel.

---

## Deployment Scenarios

### Scenario 1: No Redis (Works Out of Box)

```
REDIS_HOST=     (not set)
→ isRedisAvailable = false from start
→ All caching uses in-memory Maps
→ Rate limiting uses in-memory fallback
→ Full functionality, single-server only
```

### Scenario 2: Redis Available

```
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
→ Redis connects on startup
→ Distributed rate limiting active
→ Key validation cached in Redis
→ Survives process restarts (cache persists)
```

### Scenario 3: Redis Goes Down Mid-Operation

```
Redis running → Redis crashes
→ isRedisAvailable = false (immediate)
→ Fallback to in-memory (zero downtime)
→ Heartbeat retries every 30s
→ Redis recovers → isRedisAvailable = true
→ Seamless switch back to Redis
```
