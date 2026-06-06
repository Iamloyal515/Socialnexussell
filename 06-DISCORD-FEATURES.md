# Discord Features

> **5 dashboard tools** · **14 backend route files** · **5,060 lines**

---

## Navigation (User Dashboard)

From `DiscordPage.jsx`:

| # | Tool | Description |
|---|------|-------------|
| 1 | Overview | Account stats, active tasks, success rate |
| 2 | Channel Tools | Channel messaging + validator |
| 3 | Auto-Reply | DM auto-reply automation |
| 4 | Bulk Messaging | Mass messaging to users/channels |
| 5 | Profile Changer | Change Discord profile settings |

---

## 1. Overview

**Frontend:** `DiscordPage.jsx` (overview section)

### Mini-Features

| Feature | Description |
|---------|-------------|
| Account list | All connected Discord accounts |
| Active tasks | Running automation tasks |
| Message stats | Total messages, success rate |
| Add account | Connect new Discord token |
| Remove account | Disconnect account |
| Account details | Avatar, username, status |
| Interactive tutorial | Built-in onboarding |
| Contextual help | Per-feature help panels |

---

## 2. Channel Tools

**Backend:** `discord/validate-channel.js`, `discord/components/channel/send-message-to-channel.js`, `discord/components/channel/stop-message-channel.js`  
**Frontend:** `DiscordChannelValidator.jsx`, `EnhancedTaskManager.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Channel validator | Verify channel exists and is accessible |
| Send to channel | Post messages to specific channels |
| Stop channel messaging | Halt active channel campaigns |
| Channel-specific tasks | `DISCORD_CHANNEL_SPECIFIC` task type |
| Bulk channel messaging | Message multiple channels |

---

## 3. Auto-Reply

**Backend:** `discord/dmreply.js` (9 endpoints)  
**Frontend:** `DiscordAutoReply.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Start/stop auto-reply | Toggle per account |
| Custom triggers | Keyword-based responses |
| Status check | Is auto-reply active |
| Multi-account | Different rules per account |
| DM monitoring | Watch incoming messages |

---

## 4. Bulk Messaging

**Backend:** `discord/components/general/send-message-to-all.js`, `discord/components/general/stop-message.js`  
**Frontend:** `EnhancedTaskManager.jsx` (Discord section)

### Task Types

| Type | Description |
|------|-------------|
| `DISCORD_BULK` | Mass message to users or channels |
| `DISCORD_CHANNEL_SPECIFIC` | Target specific channels |

### Mini-Features

| Feature | Description |
|---------|-------------|
| Send to all users | DM all server members |
| Send to all channels | Post in multiple channels |
| Bulk target selection | Choose users vs channels |
| Start/stop/resume | Full task lifecycle |
| SSE live logs | Real-time delivery logs |
| Task history | Past campaign records |

---

## 5. Profile Changer

**Backend:** `discord/profile-changer.js` (8 endpoints)  
**Frontend:** `DiscordProfileChanger.jsx`

### Mini-Features

| Feature | Description |
|---------|-------------|
| Change username | Update display name |
| Change avatar | Update profile picture |
| Change status | Set custom status |
| Bulk update | Apply across accounts |
| Per-account logs | Change history |

---

## Discord Account Management

**Backend:** `discordAccounts.js` (10 endpoints), `discord/login.js`, `discord/logout.js`, `discord/reconnect.js`

| Feature | Description |
|---------|-------------|
| Login | Connect Discord token |
| Logout | Disconnect account |
| Reconnect | Re-establish dropped connection |
| List accounts | All connected accounts |
| Account details | Token, user ID, avatar |
| Delete account | Remove from system |

---

## Tier Limits (Discord)

| Tier | Max Accounts | Max Tasks | Auto-Reply |
|------|-------------|-----------|------------|
| Tier 1 | 2 | 2 | ✅ |
| Tier 2 | 4 | 4 | ✅ |
| Tier 3 | 6 | 6 | ✅ |
| Lifetime | 20 | 20 | ✅ |
| Admin | Unlimited | Unlimited | ✅ |

---

## Discord vs Telegram Comparison

| Aspect | Telegram | Discord |
|--------|----------|---------|
| Tools | 18 | 5 |
| Backend files | 58 | 14 |
| Lines of code | 64,895 | 5,060 |
| API library | GramJS (MTProto) | discord.js-selfbot-v13 |
| Session type | Phone number + OTP | Token-based |
| Marketplace | ✅ 2,000+ channels | ✅ Separate marketplace |
| Mass DM | ✅ Tier 3+ | Via bulk messaging |
| Account health | ✅ Full dashboard | Basic status |
| Flood protection | ✅ Enterprise-grade | Basic rate limiting |
