# Telegram Features — Complete Catalog

> **18 dashboard tools** across 5 categories  
> **58 backend route files** · **64,895 lines** of Telegram-specific code

---

## Navigation (User Dashboard)

Defined in `FRONTEND/src/config/telegramNav.js`:

| # | Tool | Category | Tier Gate |
|---|------|----------|-----------|
| 1 | Sessions & Overview | Core | All |
| 2 | Account Purchase | Core | All |
| 3 | Channel Validator | Channels | All |
| 4 | Bulk Channel Joiner | Channels | Tier 2+ |
| 5 | Bulk Channel Leaver | Channels | All |
| 6 | Channel Reactions | Channels | All |
| 7 | Channel Fetcher | Channels | All |
| 8 | AddLink Creator | Channels | All |
| 9 | Auto-Reply | Messaging | All |
| 10 | Bulk Messaging | Messaging | All |
| 11 | Mass DM | Messaging | Tier 3+ |
| 12 | Bulk User Adder | Management | Tier 3+ |
| 13 | Media Copier | Tools | Tier 2+ |
| 14 | Profile Changer | Management | All |
| 15 | Mass Report | Tools | All |
| 16 | Keyword Searcher | Tools | Tier 3+ |
| 17 | Topic Chooser | Tools | All |
| 18 | Contact Manager | Management | All |

---

## 1. Sessions & Overview

**Backend:** `sessions.js`, `request-code.js`, `verify-code.js`, `listen-login-code.js`, `logout.js`, `reconnect.js`, `session-backup.js`, `account-health.js`, `dashboard-data.js`, `account-details.js`  
**Frontend:** `TelegramPage.jsx`, `ImprovedSessionList.jsx`, `TelegramVerification.jsx`, `LoginCodeListenerModal.jsx`, `AccountHealthDashboard.jsx`, `AccountHealthAlert.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Phone login (OTP) | Request code → verify code flow |
| Login code listener | Auto-capture codes from Telegram app |
| Session list | Active, idle, disconnected states |
| Session load/recover | Reload sessions from MongoDB persistence |
| Session delete | Remove individual or bulk sessions |
| Test send | Send test message to verify session health |
| Health check (per phone) | Flood-wait status, connection state |
| Validate all sessions | Bulk connection test |
| Bulk cleanup | Remove dead/idle sessions |
| Session backup/restore | Export/import session strings |
| Account health dashboard | Per-account rest/wake, flood status |
| Best account selector | Auto-pick healthiest account for tasks |
| Group health monitoring | Track group-level account health |
| Dashboard stats | Aggregated Telegram metrics |
| MongoDB session persistence | Sessions survive server restart |
| Idle disconnect | Auto-disconnect inactive sessions |
| Limit checks | Per-tier session cap enforcement |

---

## 2. Account Purchase

**Backend:** `account-purchase.js`, `account-purchase-async.js`  
**Frontend:** `AccountPurchase.jsx` (6,900+ lines), `AccountPurchaseRefactored.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Bot command integration | `/bot-command` API for purchase bots |
| Job status polling | `/bot-command/status/:jobId` |
| User center | View purchased accounts |
| Import account | Import existing Telegram accounts |
| Click-button automation | Auto-click purchase UI buttons |
| Wallet recharge | Payment timer + confirmation polling |
| Async job processing | Large purchases run in background |
| Payment flow | Bot-mediated wallet top-up |

---

## 3. Channel Validator

**Backend:** `validate-channel.js`, `mass-channel-validation.js`  
**Frontend:** `ChannelValidator.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Single channel validation | Check if channel exists, is public/private |
| Batch validation | Validate multiple channels at once |
| Mass channel validation | Large-scale validation jobs |
| Flood protection reset | Clear flood-wait after validation burst |
| Health cache clear | Reset cached validation results |
| Status polling | Track validation job progress |
| Member count fetch | Get subscriber counts |
| Channel type detection | Group vs channel vs supergroup |

---

## 4. Bulk Channel Joiner (v2) ⭐ Flagship Feature

**Backend:** `bulk-channel-join-v2.js` (**5,497 lines**)  
**Frontend:** `EnhancedBulkChannelJoiner.jsx`  
**Legacy:** `bulkjoinchannels.js` (v1, still present)

### API Endpoints

| Method | Route | Purpose |
|--------|-------|---------|
| POST | `/preview` | Smart channel distribution preview |
| POST | `/start` | Start multi-account parallel join |
| GET | `/:taskId/status` | Real-time task progress |
| PATCH | `/:taskId/update-setting` | Change settings mid-run |
| POST | `/:taskId/resume-account` | Resume paused account |
| POST | `/:taskId/process-remaining` | Process remaining channels |
| POST | `/:taskId/stop` | Stop task |
| GET | `/list` | List active tasks |
| GET | `/history` | Past join history |
| GET | `/usage` | Daily usage vs tier limits |

### Mini-Features

| Feature | Description |
|---------|-------------|
| Multi-account parallel joining | Distribute channels across N accounts |
| Smart distribution preview | Preview how channels split before starting |
| Per-account progress tracking | Individual account join counts |
| Global flood-wait pause | Pause all accounts when flood hit |
| Per-account resume | Resume individual paused accounts |
| Mid-run setting updates | Change delays/limits without restart |
| Worker pool integration | CPU-heavy work off main thread |
| Membership cache | Skip already-joined channels |
| MongoDB task persistence | Tasks survive server restart |
| Task restoration | Auto-resume after server reboot |
| Usage tracking | Daily join count vs tier cap |
| History log | Full audit of past join campaigns |
| Process remaining | Continue from where stopped |
| Channel deduplication | No duplicate joins across accounts |

---

## 5. Bulk Channel Leaver

**Backend:** `bulk-channel-leave-v2.js`, `bulkleavechannels.js`, `bulk-auto-leave.js`  
**Frontend:** `BulkChannelLeaver.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Multi-account leave | Leave channels across accounts |
| Selective leave | Choose specific channels to leave |
| Auto-leave | Leave channels matching criteria |
| Batch processing | Process in chunks with delays |
| Status tracking | Real-time leave progress |
| Removed channels log | `removed-channels.js` tracks what was left |

---

## 6. Channel Reactions

**Backend:** `channel-reactions.js`  
**Frontend:** `TelegramChannelAutoReaction.jsx`  
**Model:** `channelReactionSetting.js`

### API Endpoints

| Method | Route |
|--------|-------|
| POST | `/start` |
| POST | `/stop` |
| GET | `/status` |
| GET | `/logs` |
| GET | `/status/batch` |
| GET | `/messages` |
| POST | `/test-latest` |

### Mini-Features

| Feature | Description |
|---------|-------------|
| Auto-react to new posts | Monitor channel, react automatically |
| Custom emoji selection | Choose reaction emoji |
| Batch status | Monitor multiple channels |
| Message history | View reacted messages |
| Test latest | React to most recent post |
| Start/stop per channel | Individual channel control |
| Reaction logs | Full audit trail |

---

## 7. Channel Fetcher

**Backend:** `fetch-channels.js`  
**Frontend:** `ChannelFetcher.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Fetch all dialogs | Up to 1,000 channels/groups per account |
| Per-phone endpoint | `GET /:phoneNumber` |
| Status/debug endpoints | Job progress and diagnostics |
| Global cache option | Cache results across requests |
| Health-check pruning | Remove stale cache entries |
| Export channel list | Download fetched channels |
| Filter by type | Channels vs groups vs supergroups |

---

## 8. AddLink Creator (Telegram Folders/Chatlists)

**Backend:** `addlink-creator.js`, `addlink-creator-batch.js`, `addlink-creator-async.js`, `components/addlink-creator-engine.js`  
**Frontend:** `AddLinkCreator.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Create addlist link | Generate Telegram folder invite links |
| Export chatlist invite | Share folder with others |
| Join addlist | Join someone else's folder |
| Revoke link | Invalidate existing links |
| Delete folder | Remove Telegram folders |
| Rename folder | Change folder names |
| Bulk operations | Create/manage multiple folders |
| Async job status | Long operations run in background |
| Cleanup | Remove orphaned folders/links |
| Batch creator | Process multiple folders at once |

---

## 9. Auto-Reply (DM)

**Backend:** `dmreply.js`  
**Frontend:** `TelegramAutoReply.jsx`  
**Model:** `autoReplySetting.js`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Start/stop per account | Toggle auto-reply |
| Custom DM commands | Up to N commands per tier |
| Keyword triggers | Reply based on message content |
| Status endpoint | Check if auto-reply is active |
| Recent DMs | View recent direct messages |
| Performance metrics | Response rate, message counts |
| Diagnose endpoint | Debug auto-reply issues |
| Multi-account support | Different rules per phone |

---

## 10. Bulk Messaging

**Backend:** `bulk-messaging.js`, `bulk-messaging-with-webhooks.js`, `components/bulk-messaging-engine.js`  
**Frontend:** `TelegramBulkMessaging.jsx`, `EnhancedBulkMessagingUI.jsx`, `EnhancedTaskManager.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Send to all contacts | Broadcast to entire contact list |
| Send to all groups | Message all joined groups |
| Channel-specific sending | Target specific channels |
| Time-based sending | Schedule messages at intervals |
| Activity-based sending | Send when users are active |
| Start/stop/resume | Full task lifecycle control |
| Webhook integration | Trigger via external webhooks |
| Campaign reports | Track delivery stats |
| Content orchestrator | Rules-based content pipeline |
| Dedupe/sanitize | Prevent duplicate content |
| SSE live logs | Real-time message delivery logs |
| Template support | Reusable message templates |

---

## 11. Mass DM ⭐ (Tier 3+)

**Backend:** `mass-dm.js`, `components/mass-dm-engine.js`  
**Frontend:** `TelegramMassDM.jsx`, `MassDMWizard.jsx`, `MassDMTutorial.jsx`

### API Endpoints (24 total)

Includes: start, stop, pause, resume, status, templates CRUD, queue management, analytics, export, cancel-active, group stats, deferred send

### Mini-Features

| Feature | Description |
|---------|-------------|
| Mass DM wizard | Step-by-step campaign setup |
| Template CRUD | Create/edit/delete message templates |
| Queue management | View and manage send queue |
| Pause/resume | Pause mid-campaign, resume later |
| Deferred send | Schedule sends for later |
| Analytics | Open rate, delivery stats |
| Export results | Download campaign data |
| Cancel active | Stop all active mass DM tasks |
| Group stats | Per-group delivery metrics |
| Tutorial mode | Built-in onboarding wizard |
| Tier 3+ gate | Enforced at middleware level |

---

## 12. Bulk User Adder (Tier 3+)

**Backend:** `bulk-user-adder.js`, `components/bulk-user-adder-engine.js`  
**Frontend:** `TelegramBulkUserAdder.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Add users to groups | Bulk invite users to Telegram groups |
| Validate users | Check if usernames exist before adding |
| Pause/resume/stop | Full task control |
| Retry failed | Re-attempt failed additions |
| Task history | View past add campaigns (tier3+) |
| Per-group targeting | Add to specific groups |
| Rate limiting | Smart delays between adds |

---

## 13. Media Copier (Tier 2+)

**Backend:** `media-copier.js`, `components/media-copier/media-copier-worker.js`  
**Frontend:** `TelegramMediaCopier.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Cross-channel copy | Copy media from source to destination |
| Start/pause/resume/stop | Full lifecycle |
| SSE stream | Real-time copy progress |
| Usage limits | Daily copy count per tier |
| Active tasks view | Monitor running copies |
| Worker thread | CPU-heavy copy off main thread |
| Concurrent copies | Multiple simultaneous (tier-dependent) |

---

## 14. Profile Changer

**Backend:** `profile-changer.js`  
**Frontend:** `TelegramProfileChanger.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Change bio | Update account bio text |
| Change status | Set online status message |
| Change username | Update @username |
| Mass edit | Apply changes across accounts |
| Bulk update | Batch profile changes |
| Rotation stop | Stop username rotation |
| Per-phone logs | View change history per account |

---

## 15. Mass Report

**Backend:** `mass-report.js`  
**Frontend:** `TelegramMassReport.jsx`  
**Model:** `massReportTask.js`, `reportHistory.js`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Report users/channels | Bulk report to Telegram |
| Report types | Spam, violence, etc. |
| Task tracking | Monitor report job progress |
| History log | Past reports audit trail |

---

## 16. Keyword Searcher (Tier 3+)

**Backend:** `keyword-searcher.js`  
**Frontend:** `TelegramKeywordSearcher.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Search Telegram | Find channels/groups by keyword |
| Cancel search | Stop running search |
| Results export | Download search results |
| Usage tracking | Daily search count vs tier limit |
| Multi-account search | Search across N accounts |
| Competitor discovery | Find niche channels |

---

## 17. Topic Chooser

**Backend:** `topic-chooser.js`  
**Frontend:** `TelegramTopicChooser.jsx`  
**Model:** `topicChooserRun.js`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Forum topic search | Find topics in supergroup forums |
| Run search | Execute topic discovery |
| Active jobs | Monitor running searches |
| Cancel job | Stop search mid-run |
| Results view | Browse discovered topics |

---

## 18. Contact Manager

**Backend:** `contact-manager.js`, `contacts.js`  
**Frontend:** `TelegramContactManager.jsx`  
**Model:** `telegramContact.js`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Fetch contacts | Load all Telegram contacts |
| Tag contacts | Organize with custom tags |
| Favorite contacts | Mark important contacts |
| Notes | Add notes to contacts |
| Delete contacts | Remove from list |
| Search contacts | Filter by name/username/tag |

---

## Supporting Infrastructure (Not User-Facing Tools)

| Service/File | Purpose |
|--------------|---------|
| `telegramClient.js` | GramJS client factory |
| `enhancedFloodWaitService.js` | Per-session/chat flood tracking |
| `floodProtectionService.js` | Global flood gate |
| `globalTelegramOpLimiter.js` | Cross-feature operation limiting |
| `telegramLimiter.js` | Per-operation rate limits |
| `telegramConcurrencyLimiter.js` | Concurrent operation cap |
| `multiAccountManager.js` | Multi-account coordination |
| `session-idle-disconnect.js` | Idle session cleanup |
| `taskRestoration.js` | Restore tasks after restart |
| `channelEntityCache.js` | GramJS entity caching |
| `smart-cooldown.js` | AI-calculated optimal delays |
| `group-quote-reply.js` | Auto-quote-reply in groups |
| `content-orchestrator.js` | Rules-based content pipeline |
| `resolve-peer.js` | Resolve Telegram usernames to entities |
| `recent-dms.js` | Recent DM history API |
