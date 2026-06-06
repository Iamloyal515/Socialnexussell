# MongoDB Models (26 Schemas)

---

## Core Models

### `key` — License Keys
**File:** `backend/src/models/key.js`

| Field | Type | Purpose |
|-------|------|---------|
| `key` | String | License key string |
| `subscriptionTier` | String | tier1/tier2/tier3/lifetime/admin |
| `duration` | Number | Days of access |
| `activatedAt` | Date | First login timestamp |
| `expiresAt` | Date | Expiration timestamp |
| `suspended` | Boolean | Admin suspension flag |
| `frozen` | Boolean | Admin freeze flag |
| `limitOverrides` | Object | Per-user custom limits |
| `statistics` | Object | Usage counters |
| `trial` | Subdocument | Trial info (isTrial, trialEndsAt, telegramUsername) |
| `adminNotes` | String | Internal operator notes |
| `lastActiveAt` | Date | Last API activity |

### `session` — Telegram Sessions
**File:** `backend/src/models/session.js`

| Field | Type | Purpose |
|-------|------|---------|
| `phoneNumber` | String | Account phone |
| `keyId` | ObjectId | Owner license key |
| `sessionString` | String | GramJS session data |
| `isActive` | Boolean | Currently connected |
| `lastUsed` | Date | Last activity |
| `health` | Object | Flood-wait, connection status |

### `task` — Automation Tasks
**File:** `backend/src/models/task.js`

| Field | Type | Purpose |
|-------|------|---------|
| `taskType` | String | e.g. TELEGRAM_BULK_JOIN |
| `status` | String | pending/running/paused/completed/failed |
| `config` | Object | Task-specific settings |
| `progress` | Object | Real-time progress data |
| `keyId` | ObjectId | Owner license key |
| `logs` | Array | Task execution logs |
| `result` | Object | Final outcome data |

---

## Feature Models

### `autoReplySetting` — Auto-Reply Config
| Field | Purpose |
|-------|---------|
| `phoneNumber` | Account |
| `triggers` | Keyword → response mappings |
| `isActive` | Running state |

### `channelReactionSetting` — Reaction Automation
| Field | Purpose |
|-------|---------|
| `channelId` | Target channel |
| `emoji` | Reaction emoji |
| `isActive` | Running state |

### `bulkChannelJoinerUsage` — Daily Join Tracking
| Field | Purpose |
|-------|---------|
| `keyId` | User |
| `date` | Day |
| `joinCount` | Channels joined today |
| `runCount` | Join runs today |

### `telegramContact` — Contact Manager
| Field | Purpose |
|-------|---------|
| `phoneNumber` | Source account |
| `contactId` | Telegram contact ID |
| `tags` | Custom tags |
| `notes` | User notes |
| `isFavorite` | Favorite flag |

### `campaignTemplate` — Message Templates
| Field | Purpose |
|-------|---------|
| `name` | Template name |
| `content` | Message text |
| `keyId` | Owner |

### `campaignReport` — Campaign Analytics
| Field | Purpose |
|-------|---------|
| `taskId` | Related task |
| `sent` | Messages sent |
| `delivered` | Successfully delivered |
| `failed` | Failed deliveries |

### `massReportTask` — Mass Report Jobs
| Field | Purpose |
|-------|---------|
| `targets` | Users/channels to report |
| `reportType` | Spam, violence, etc. |
| `status` | Job status |

### `topicChooserRun` — Topic Search Jobs
| Field | Purpose |
|-------|---------|
| `supergroupId` | Target supergroup |
| `results` | Discovered topics |
| `status` | Job status |

### `groupQuoteReplySetting` — Group Quote Reply
| Field | Purpose |
|-------|---------|
| `groupId` | Target group |
| `triggers` | Reply rules |

---

## Platform Models

### `discordAccount` — Discord Accounts
| Field | Purpose |
|-------|---------|
| `token` | Discord token |
| `userId` | Discord user ID |
| `keyId` | Owner license key |

### `market` — Marketplace Listings
| Field | Purpose |
|-------|---------|
| `name` | Channel name |
| `link` | Telegram/Discord link |
| `category` | Niche/category |
| `memberCount` | Subscribers |
| `approved` | Admin approval status |
| `platform` | telegram/discord |

### `webhook` — Webhook Configs
| Field | Purpose |
|-------|---------|
| `url` | Webhook URL |
| `events` | Trigger events |
| `keyId` | Owner |

### `linkTracker` — Link Click Tracking
| Field | Purpose |
|-------|---------|
| `originalUrl` | Target URL |
| `shortCode` | Tracking code |
| `clicks` | Click count |

---

## System Models

### `trialClaimLog` — Trial Anti-Abuse
| Field | Purpose |
|-------|---------|
| `ip` | Client IP |
| `ipSubnet` | /24 subnet |
| `fingerprintHash` | Device fingerprint |
| `telegramUsername` | Claimed @username |
| `offerToken` | Signed offer token |
| `result` | success/failure |

### `contactInquiry` — Contact Form
| Field | Purpose |
|-------|---------|
| `name` | Sender name |
| `email` | Sender email |
| `message` | Inquiry content |
| `responded` | Admin response status |

### `userDashboardStats` — Per-User Metrics
| Field | Purpose |
|-------|---------|
| `keyId` | User |
| `messagesSent` | Total messages |
| `tasksCompleted` | Completed tasks |
| `lastUpdated` | Cache timestamp |

### `messageLog` — Message Audit
| Field | Purpose |
|-------|---------|
| `taskId` | Related task |
| `recipient` | Target user/channel |
| `content` | Message text |
| `status` | sent/failed |

### `messageMetric` — Volume Metrics
| Field | Purpose |
|-------|---------|
| `keyId` | User |
| `date` | Day |
| `platform` | telegram/discord |
| `count` | Message count |

### `removedChannel` — Left Channel History
| Field | Purpose |
|-------|---------|
| `phoneNumber` | Account |
| `channelId` | Left channel |
| `leftAt` | Timestamp |

### `marketplaceCopyUsage` — Copy Tracking
| Field | Purpose |
|-------|---------|
| `keyId` | User |
| `listingId` | Copied listing |

### `groupChannelCache` — Group Channel Cache
| Field | Purpose |
|-------|---------|
| `groupId` | Group ID |
| `channels` | Cached channel list |

### `usernameCache` — Username Resolution
| Field | Purpose |
|-------|---------|
| `username` | @username |
| `resolvedId` | Telegram user ID |

### `reportHistory` — Report Audit
| Field | Purpose |
|-------|---------|
| `target` | Reported entity |
| `type` | Report type |
| `result` | Outcome |
