# Admin Dashboard

> **Route:** `/admin/*`  
> **Frontend:** `AdminDashboard.jsx` (~6,700 lines)  
> **Backend:** `admin.js` (~3,700 lines, 71 API endpoints)  
> **Access:** Requires `admin` tier license key

---

## Admin Tabs (6 Active)

| Tab | Component | Key Capabilities |
|-----|-----------|-----------------|
| **Overview** | `AdminOverview.jsx` | Key counts, messages, tasks, sessions, platform stats |
| **User Management** | Inline in `AdminDashboard.jsx` | Full license key CRUD, suspend, freeze, extend, expire |
| **Analytics** | Inline charts | Time-range charts, message stats, platform breakdown |
| **Performance** | `AdminPerformance.jsx` | Hardware, heap, MongoDB pool, Redis status, event loop |
| **Marketplace** | Inline | Approve/reject listings, bulk upload/delete, settings |
| **Contact** | Inline | View/respond to contact form submissions |

---

## User Management — Full License Lifecycle

### Key Operations

| Action | Description |
|--------|-------------|
| Create key | Generate new license key with tier + duration |
| Bulk create | Generate multiple keys at once |
| Edit key | Change tier, duration, notes |
| Suspend | Temporarily disable key |
| Unsuspend | Re-enable suspended key |
| Freeze | Lock key (stricter than suspend) |
| Unfreeze | Unlock frozen key |
| Extend | Add days to subscription |
| Expire | Force immediate expiration |
| Regenerate | Create new key, invalidate old |
| Reset | Randomize key, delete tasks, reset username |
| Delete | Permanently remove key |

### Per-User Limit Overrides

Admin can override any tier limit per key without code changes:

| Override Field | Example |
|----------------|---------|
| `maxPhoneNumberSessions` | Give user 50 sessions on tier1 |
| `maxAutomationTasks` | Allow 20 tasks |
| `maxCustomDmCommands` | 50 DM commands |
| `maxContacts` | 10,000 contacts |
| `maxChannels` | 1,000 channels |
| `mediaCopierDaily` | 5,000 copies/day |
| `bulkChannelJoinerDaily` | Unlimited joins |
| `bulkChannelJoinerMaxChannels` | 10,000 channels |

### User Dashboard Modal

Click any user to open detailed modal:

| Section | Data Shown |
|---------|------------|
| Telegram sessions | Active + all sessions per phone |
| Discord accounts | Connected Discord accounts |
| Limit overrides | Current custom limits |
| Admin notes | Internal notes per user |
| Statistics | Message counts, task history |
| Actions | Suspend, freeze, extend, reset, delete |

---

## Overview Tab

| Metric | Source |
|--------|--------|
| Total license keys | MongoDB `key` collection |
| Active users (logged in today) | Key `lastActiveAt` |
| Total messages sent | `messageMetric` aggregation |
| Active tasks | `task` collection |
| Telegram sessions | In-memory `authenticatedUsers` |
| Discord accounts | `discordAccount` collection |
| Platform breakdown | Telegram vs Discord stats |

---

## Analytics Tab

| Chart | Data |
|-------|------|
| Messages over time | Daily/weekly/monthly message volume |
| Platform split | Telegram vs Discord message ratio |
| User activity | Active users per time period |
| Task completion | Success/failure rates |

---

## Performance Tab (`AdminPerformance.jsx`)

| Monitor | Details |
|---------|---------|
| **Hardware** | CPU cores, RAM, OS |
| **Node.js heap** | Used/total heap, GC stats |
| **MongoDB pool** | Connection count, query latency |
| **Redis status** | Connected/fallback, hit rate |
| **Event loop lag** | ms delay indicator |
| **Memory guard** | Soft/hard threshold status |
| **Dynamic config** | Auto-scaled settings |
| **HTTP connections** | Active request count |
| **Worker pool** | Thread utilization |

### Dynamic Config (Auto-Scaling)

`backend/src/services/dynamic-config.js` auto-adjusts based on hardware:

| Setting | Scales With |
|---------|-------------|
| MongoDB pool size | Available RAM |
| UV threadpool size | CPU cores |
| Worker pool size | CPU cores |
| In-memory cache limits | Available RAM |
| Memory guard thresholds | Total heap |

---

## Marketplace Tab

| Action | Description |
|--------|-------------|
| View all listings | Paginated marketplace entries |
| Approve listing | Make channel visible to users |
| Reject listing | Deny with reason |
| Bulk upload | CSV/JSON mass import |
| Bulk delete | Remove multiple listings |
| Settings | Configure marketplace rules |
| Access requests | Handle tier-gated access requests |
| Delete all Telegram marketplaces | Nuclear option |
| Delete all Discord marketplaces | Nuclear option |

---

## Contact Tab

| Action | Description |
|--------|-------------|
| View inquiries | All contact form submissions |
| Mark responded | Track response status |
| View details | Full inquiry content |

---

## Admin API Endpoints (71 total)

### Statistics

```
GET /admin/statistics
GET /admin/statistics/analytics
GET /admin/statistics/analytics-data
GET /admin/statistics/:keyId
```

### Key Management

```
GET    /admin/keys
POST   /admin/keys
POST   /admin/keys/bulk-create
PUT    /admin/keys/:id
DELETE /admin/keys/:id
POST   /admin/keys/:id/suspend
POST   /admin/keys/:id/unsuspend
POST   /admin/keys/:id/freeze
POST   /admin/keys/:id/unfreeze
POST   /admin/keys/:id/extend
POST   /admin/keys/:id/expire
POST   /admin/keys/:id/regenerate
POST   /admin/keys/:id/reset
```

### Marketplace

```
GET    /admin/marketplace
PUT    /admin/marketplace/:id
DELETE /admin/marketplace/:id
POST   /admin/marketplace/bulk-upload
POST   /admin/marketplace/bulk-delete
GET    /admin/marketplace/settings
PUT    /admin/marketplace/settings
GET    /admin/marketplace/access-requests
```

### Sessions & System

```
GET  /admin/sessions/status
POST /admin/sessions/restore
POST /admin/sessions/health-check
GET  /admin/sessions/metrics
POST /admin/sessions/reconnect
GET  /admin/sessions/monitor-status
POST /admin/sessions/toggle-quiet-mode
GET  /admin/performance
GET  /admin/routes
POST /admin/maintenance/force-clear
POST /admin/maintenance/restore-failed-tasks
```

### Telegram Connection Status

```
GET  /admin/telegram/connection-status
GET  /admin/telegram/health
POST /admin/telegram/reconnect
```

---

## Built But Not Wired to UI

These components exist but are not rendered in the current admin dashboard:

| File | Intended Purpose |
|------|-----------------|
| `AdminKeyManagement.jsx` | Standalone key table (logic duplicated inline) |
| `AdminSEO.jsx` | SEO management |
| `AdminExpiredKeys.jsx` | Expired key management |
| `AdminLiveMap.jsx` | Live user geographic map |
| `admin-monetization.js` | Revenue, churn, high-value user analytics |
| `admin-seo-analytics.js` | SEO performance data |
| `admin-task-management.js` | Cross-user task management |

> **Upside for buyer:** These are partially built features ready to wire up.

---

## Admin Auth

- Requires license key with `subscriptionTier: 'admin'`
- Admin token cached 5 minutes (`_adminTokenCache` in `admin.js`)
- Admin stats cached with stale-while-revalidate (`_adminCache` Map)
- Rate limited via Redis (or in-memory fallback)
