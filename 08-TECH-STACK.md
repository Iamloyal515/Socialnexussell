# Tech Stack

---

## Backend

| Layer | Technology | Version |
|-------|-----------|---------|
| **Runtime** | Node.js | 6GB heap (`--max-old-space-size=6144`) |
| **Framework** | Express | 4.21 |
| **Telegram API** | GramJS (`telegram` package) | 2.26 |
| **Discord API** | discord.js-selfbot-v13 | 3.6 |
| **Database** | MongoDB via Mongoose | 8.10 |
| **Cache** | Redis via ioredis | 5.8 |
| **Auth** | License key + jsonwebtoken | — |
| **Security** | Helmet, CORS, compression | — |
| **Scheduling** | node-cron | — |
| **HTTP client** | Axios | — |
| **Testing** | Mocha | — |
| **Deployment** | PM2 (fork mode) | ecosystem.config.js |
| **Legacy DB** | quick.db, better-sqlite3 | Largely replaced by MongoDB |

---

## Frontend

| Layer | Technology | Version |
|-------|-----------|---------|
| **Framework** | React | 19 |
| **Routing** | React Router DOM | 7 |
| **Styling** | Tailwind CSS | 3.4 |
| **UI Components** | MUI (Material UI) | 7 |
| **Icons** | Lucide React, Tabler Icons | — |
| **Charts** | Chart.js, Recharts | — |
| **Animation** | Framer Motion | 12 |
| **HTTP** | Axios | — |
| **Real-time** | SSE (event-source-polyfill) | — |
| **Build** | Create React App (react-scripts) | 5 |
| **Production serve** | `serve` package | Port 3841 |
| **Analytics** | Custom GA integration | — |
| **CAPTCHA** | Altcha | — |
| **Checkout** | SellAuth embed | useSellAuthEmbed hook |

---

## Infrastructure Services (Backend)

| Service | File | Purpose |
|---------|------|---------|
| Cache service | `cacheService.js` | Redis + in-memory hybrid |
| Rate limiter | `redisRateLimiter.js` | Distributed per-tier limits |
| Memory guard | `memoryGuard.js` | Heap monitoring + cache shedding |
| Dynamic config | `dynamic-config.js` | Hardware-aware auto-scaling |
| Worker pool | `workerPool.js` | CPU-heavy task offloading |
| Flood wait | `enhancedFloodWaitService.js` | Telegram rate limit tracking |
| Flood protection | `floodProtectionService.js` | Global flood gate |
| Task restoration | `taskRestoration.js` | Resume tasks after restart |
| Session idle disconnect | `session-idle-disconnect.js` | Cleanup inactive sessions |
| Multi-account manager | `multiAccountManager.js` | Coordinate N accounts |
| Content orchestrator | `content-orchestrator.js` | Rules-based content pipeline |
| Usage tracker | `usageTracker.js` | Monetization analytics |
| Liveness watchdog | `livenessWatchdog.js` | Health monitoring |
| Event loop monitor | Event loop lag detection | Performance |
| Degraded mode | `degradedMode.js` | Serve under load pressure |
| SSE manager | Live log streaming | Real-time updates |

---

## MongoDB Models (26)

| Model | Purpose |
|-------|---------|
| `key` | License keys + trial data |
| `session` | Telegram session storage |
| `task` | All automation tasks |
| `market` | Marketplace listings |
| `discordAccount` | Discord account storage |
| `webhook` | Webhook configurations |
| `campaignReport` | Campaign analytics |
| `campaignTemplate` | Message templates |
| `autoReplySetting` | Auto-reply configs |
| `channelReactionSetting` | Reaction automation |
| `bulkChannelJoinerUsage` | Daily join usage tracking |
| `trialClaimLog` | Trial anti-abuse audit |
| `telegramContact` | Contact manager data |
| `linkTracker` | Link click tracking |
| `contactInquiry` | Contact form submissions |
| `topicChooserRun` | Topic search jobs |
| `removedChannel` | Left channel history |
| `userDashboardStats` | Per-user dashboard metrics |
| `messageLog` | Message audit log |
| `messageMetric` | Message volume metrics |
| `massReportTask` | Mass report jobs |
| `marketplaceCopyUsage` | Marketplace copy tracking |
| `groupQuoteReplySetting` | Group quote reply config |
| `groupChannelCache` | Group channel cache |
| `usernameCache` | Username resolution cache |
| `reportHistory` | Report audit trail |

---

## Environment Variables

| Variable | Required | Purpose |
|----------|----------|---------|
| `MONGODB_URI` | Yes | MongoDB connection string |
| `JWT_SECRET` | Yes | Token signing |
| `REDIS_HOST` | No | Redis host (fallback to in-memory) |
| `REDIS_PORT` | No | Redis port |
| `REDIS_PASSWORD` | No | Redis auth |
| `REDIS_DB` | No | Redis database |
| `TRIAL_OFFER_SECRET` | No | Trial token signing |
| `PORT` | No | Server port (auto-detected) |
| `NODE_ENV` | No | production/development |

---

## Deployment Architecture

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Browser    │────▶│  FRONTEND    │     │   MongoDB    │
│  (React SPA) │     │  serve:3841  │     │   Database   │
└──────┬───────┘     └──────────────┘     └──────┬───────┘
       │                                          │
       │              ┌──────────────┐            │
       └─────────────▶│   BACKEND    │────────────┘
                      │  PM2 fork     │
                      │  Express API  │     ┌──────────────┐
                      │  GramJS       │────▶│    Redis     │
                      │  discord.js   │     │  (optional)  │
                      └──────────────┘     └──────────────┘
```

---

## Key Dependencies (Backend package.json highlights)

```
express, mongoose, telegram (GramJS), discord.js-selfbot-v13,
ioredis, jsonwebtoken, helmet, cors, compression, axios,
node-cron, mocha, pm2, cross-env
```

## Key Dependencies (Frontend package.json highlights)

```
react, react-router-dom, tailwindcss, @mui/material,
framer-motion, chart.js, recharts, lucide-react,
axios, event-source-polyfill, react-scripts
```
