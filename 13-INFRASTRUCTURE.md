# Infrastructure & DevOps

---

## PM2 Deployment

**File:** `backend/ecosystem.config.js`

| Setting | Value |
|---------|-------|
| App name | `socialnexus-backend` |
| Mode | `fork` (single process) |
| Instances | 1 |
| Max memory restart | 5GB |
| Node args | `--max-old-space-size=6144` |
| Auto restart | Yes |
| Watch | No |

### Why Fork Mode (Not Cluster)

Telegram GramJS sessions live in process memory. Cluster mode would require:
- Redis-backed session registry
- Task state externalization
- Sticky session routing

> Foundation already exists: MongoDB task persistence + session backup/restore.

---

## Memory Management

### Memory Guard (`memoryGuard.js`)

```
Heap Usage:
  0%──────────70%──────────85%──────────95%───100%
              SOFT         HARD        CRITICAL
              │            │           │
              Trim caches   Shed caches  Force GC
              │            Reject tasks Disconnect idle
```

| Threshold | Action |
|-----------|--------|
| 70% (soft) | Trim GramJS entity caches |
| 85% (hard) | Shed in-memory caches, reject new heavy tasks |
| 95% (critical) | Force garbage collection, disconnect idle sessions |

### Dynamic Config (`dynamic-config.js`)

Auto-scales on startup based on detected hardware:

| Resource | Detection | Scaling |
|----------|-----------|---------|
| CPU cores | `os.cpus().length` | Worker pool, UV threadpool |
| Total RAM | `os.totalmem()` | MongoDB pool, cache limits |
| Free RAM | `os.freemem()` | Memory guard thresholds |

---

## Worker Thread Pool

**File:** `backend/src/services/workerPool.js`

| Use Case | Worker File |
|----------|------------|
| Media copying | `media-copier-worker.js` |
| CPU-heavy validation | Worker threads |
| Bulk operations | Offloaded processing |

Pool size auto-scales with CPU cores via dynamic config.

---

## Health Monitoring

| Monitor | File | Checks |
|---------|------|--------|
| Liveness watchdog | `livenessWatchdog.js` | Process alive, Redis ping |
| Event loop monitor | Built into performance | Event loop lag (ms) |
| HTTP connection tracker | Built into index.js | Active connections |
| Degraded mode | `degradedMode.js` | Load pressure detection |

### Health Endpoint

```
GET /health
```

Returns:
- Server uptime
- MongoDB connection status
- Redis status (connected/fallback)
- Memory usage
- Active sessions count
- Active tasks count

---

## Task Restoration

**File:** `backend/src/services/taskRestoration.js`

On server restart:
1. Query MongoDB for `status: 'running'` tasks
2. Restore task processors
3. Resume from last known progress
4. Reconnect required Telegram sessions

Ensures zero data loss on deployment/restart.

---

## Session Management

### Idle Disconnect (`session-idle-disconnect.js`)

- Monitors session activity
- Disconnects sessions idle for N minutes
- Frees memory from unused GramJS clients
- Sessions can be reloaded from MongoDB on next use

### Session Persistence (`mongoSessionPersistence.js`)

- All session strings stored in MongoDB
- Query cache: 800 entries, 60s TTL
- Survives server restarts
- Per-key session isolation

---

## SSE (Server-Sent Events)

Real-time streaming used across the platform:

| Feature | SSE Endpoint |
|---------|-------------|
| Bulk messaging logs | Task-specific SSE stream |
| Media copier progress | Copy progress stream |
| Global notifications | Toast notification stream |
| Task manager | Live task status updates |

**Frontend:** `SSELiveLogs.jsx`, `event-source-polyfill`

---

## Rate Limiting Architecture

```
Request → authenticateKey → tierEnforcement → redisRateLimiter → route handler
                ↓                  ↓                    ↓
           Key cache          Usage cache         Per-tier limits
           (5min TTL)        (1min TTL)          (Redis or memory)
```

### Per-Tier Rate Limits

| Category | Tier 1 | Tier 2 | Tier 3 | Lifetime |
|----------|--------|--------|--------|----------|
| API calls/min | Lower | Medium | Higher | Highest |
| Telegram ops/min | Limited | Medium | High | Unlimited |
| Marketplace | Standard | Standard | Premium | VIP |
| Upload | Limited | Medium | High | High |

---

## Logging

| Logger | Purpose |
|--------|---------|
| `logger.js` | Structured application logging |
| `channelLogger.js` | Per-channel operation logs |
| `discordLogger.js` | Discord-specific logs |
| Slow request middleware | Log requests > N seconds |
| Admin error handler | Centralized error tracking |

---

## Startup Sequence

```
1. Load environment variables
2. Detect hardware → dynamic-config
3. Connect MongoDB
4. Initialize Redis (or fallback)
5. Start memory guard
6. Start liveness watchdog
7. Mount Express routes
8. Restore running tasks
9. Start idle session monitor
10. Listen on port
```

---

## Frontend Build & Serve

```bash
# Development
cd FRONTEND && npm start        # Port 3000

# Production build
cd FRONTEND && npm run build

# Production serve
cd FRONTEND && npx serve -s build -l 3841
```

Backend runs separately on auto-detected port via PM2.

---

## Required Services for Production

| Service | Required | Notes |
|---------|----------|-------|
| MongoDB | **Yes** | Primary database |
| Node.js 18+ | **Yes** | Backend runtime |
| Redis | No | Recommended for rate limiting |
| PM2 | Recommended | Process management |
| Nginx/Caddy | Recommended | Reverse proxy + SSL |
| Domain + SSL | Recommended | socialnexus.us setup |
