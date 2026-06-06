# API Endpoints Catalog

> **461 total REST endpoints** across 75 route files

---

## By Route File

| File | Endpoints | Category |
|------|-----------|----------|
| `admin.js` | 71 | Admin |
| `mass-dm.js` | 24 | Telegram Mass DM |
| `tasks.js` | 21 | Task orchestration |
| `sessions.js` | 17 | Telegram sessions |
| `addlink-creator.js` | 15 | AddLink Creator |
| `account-health.js` | 12 | Account health |
| `bulk-channel-join-v2.js` | 11 | Bulk Channel Joiner |
| `media-copier.js` | 11 | Media Copier |
| `removed-channels.js` | 11 | Removed channels |
| `addlink-creator-async.js` | 11 | AddLink async |
| `discordAccounts.js` | 10 | Discord accounts |
| `admin-task-management.js` | 8 | Admin tasks |
| `user.js` | 8 | User profile |
| `profile-changer.js` (TG) | 8 | TG Profile Changer |
| `profile-changer.js` (DC) | 8 | DC Profile Changer |
| `bulk-user-adder.js` | 8 | Bulk User Adder |
| `telegramConnectionStatus.js` | 8 | TG connection admin |
| `mass-channel-validation.js` | 7 | Mass validation |
| `channel-reactions.js` | 7 | Channel reactions |
| `campaignReports.js` | 7 | Campaign reports |
| `webhooks.js` | 7 | Webhooks |
| `linkTracker.js` | 7 | Link tracking |
| `bulkjoinchannels.js` | 7 | Legacy bulk join |
| `marketPlace.js` | 7 | Marketplace |
| `dmreply.js` (TG) | 6 | TG Auto-reply |
| `bulk-messaging.js` | 6 | Bulk messaging |
| `contact-manager.js` | 6 | Contact manager |
| `session-backup.js` | 6 | Session backup |
| `admin-monetization.js` | 6 | Revenue analytics |
| `dmreply.js` (DC) | 9 | DC Auto-reply |
| `bulk-channel-leave-v2.js` | 5 | Bulk Channel Leaver |
| `validate-channel.js` | 5 | Channel validator |
| `bulk-messaging-with-webhooks.js` | 5 | Bulk + webhooks |
| `account-purchase.js` | 5 | Account purchase |
| `smart-cooldown.js` | 5 | Smart cooldown |
| `reconnect.js` (DC) | 5 | DC reconnect |
| `sitemap.js` | 5 | SEO sitemap |
| `topic-chooser.js` | 4 | Topic chooser |
| `keyword-searcher.js` | 4 | Keyword searcher |
| `group-quote-reply.js` | 4 | Group quote reply |
| `content-orchestrator.js` | 4 | Content pipeline |
| `mass-report.js` | 4 | Mass report |
| `listen-login-code.js` | 4 | Login code listener |
| `bulkleavechannels.js` | 4 | Legacy bulk leave |
| `account-purchase-async.js` | 4 | Account purchase async |
| `fetch-channels.js` | 3 | Channel fetcher |
| `admin-seo-analytics.js` | 3 | SEO analytics |
| `reconnect.js` (TG) | 3 | TG reconnect |
| All others | 1-2 each | Various |

---

## By Category

### Authentication & Keys (5)

```
POST /keys/create
GET  /keys/check
POST /trial/offer
POST /trial/activate
```

### Admin (71+)

See [04-ADMIN-DASHBOARD.md](./04-ADMIN-DASHBOARD.md) for full list.

### Telegram Sessions (17)

```
GET/POST sessions management
POST request-code, verify-code
POST listen-login-code
POST logout, reconnect
GET/POST session-backup
GET account-health, dashboard-data
GET account-details
```

### Telegram Channel Operations (40+)

```
Bulk join v2: preview, start, status, update, resume, stop, list, history, usage
Bulk leave v2: start, status, stop, list, history
Channel validator: validate, batch, mass-validation
Channel fetcher: fetch, status, debug
Channel reactions: start, stop, status, logs, batch, messages, test
AddLink creator: create, export, join, revoke, delete, rename, bulk, async
Removed channels: list, restore, delete, bulk
```

### Telegram Messaging (50+)

```
Auto-reply: start, stop, status, recent-dms, diagnose
Bulk messaging: start, stop, resume, stats
Mass DM: start, stop, pause, resume, templates CRUD, queue, analytics, export
Mass report: start, status, history
Group quote reply: start, stop, status, settings
Content orchestrator: rules, execute, status
Smart cooldown: calculate, config, cache-stats
```

### Telegram User Management (20+)

```
Bulk user adder: start, pause, resume, stop, validate, retry, history
Contact manager: fetch, tag, favorite, notes, delete, search
Profile changer: bio, status, username, mass-edit, bulk-update, logs
Media copier: start, pause, resume, stop, stream, usage, active
Keyword searcher: search, cancel, results, usage
Topic chooser: run, active, cancel, results
```

### Discord (30+)

```
Login, logout, reconnect
Accounts CRUD
Auto-reply: start, stop, status
Bulk messaging: send-to-all, stop
Channel: send, stop, validate
Profile changer: username, avatar, status, bulk
```

### Tasks (21)

```
GET/POST/PUT/DELETE tasks
Start, stop, pause, resume
Status, progress, logs
Bulk operations
```

### Marketplace (9)

```
GET/POST marketplace listings
Approve, reject, search
Access requests
```

### Webhooks & Campaigns (14)

```
Webhooks CRUD + trigger
Campaign reports CRUD
Campaign templates CRUD
```

### User & Others (15)

```
User profile, stats, batch-stats
Contact form
Link tracker CRUD
Logs access
Sitemap generation
```

---

## API Design Patterns

| Pattern | Usage |
|---------|-------|
| License key auth | `X-Access-Key` header on all routes |
| Tier enforcement | Middleware checks limits before execution |
| Task-based async | Long operations return `taskId`, poll status |
| SSE streaming | Media copier, bulk messaging live logs |
| Stale-while-revalidate | Admin stats caching |
| Rate limiting | Per-tier, per-endpoint via Redis/memory |
