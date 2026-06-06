# Project Overview

## Product Name: SocialNexus

**Tagline:** Professional Telegram + Discord social media automation SaaS  
**Live domain:** [socialnexus.us](https://socialnexus.us)  
**Version:** 2.0.0 (both frontend and backend)

---

## What It Does

SocialNexus is a **web-based control plane** for automating Telegram and Discord at industrial scale. Users log in with a license key and get access to a dashboard with 18 Telegram tools and 5 Discord tools for:

- Multi-account session management
- Bulk channel joining/leaving
- Mass messaging and DM campaigns
- Auto-reply automation
- Media copying across channels
- Keyword-based channel discovery
- Profile management
- Account health monitoring
- And much more

Platform operators use the **admin dashboard** to manage users, keys, marketplace content, and system performance.

---

## Folder Structure

```
social-nexus/
├── backend/                    # Node.js API server (v2.0.0)
│   ├── index.js                # Main entry point
│   ├── ecosystem.config.js     # PM2 deployment
│   ├── package.json
│   ├── docs/                   # Technical documentation
│   └── src/
│       ├── routes/
│       │   ├── telegram/       # 58 files — all Telegram features
│       │   ├── discord/        # 14 files — Discord features
│       │   ├── admin.js        # Admin API (71 endpoints)
│       │   ├── keys/           # License key + trial
│       │   ├── tasks.js        # Task orchestration
│       │   ├── user.js         # User profile
│       │   └── others/         # Marketplace, contact, logs
│       ├── services/           # 51 service modules
│       ├── models/             # 26 Mongoose schemas
│       ├── middleware/         # Tier enforcement, logging
│       ├── config/             # Telegram client config
│       └── utils/              # Session persistence, limiters
│
├── FRONTEND/                   # React 19 SPA (v2.0.0)
│   └── src/
│       ├── App.js              # Router
│       ├── components/         # 143 JSX components
│       │   ├── admin/          # 6 admin sub-components
│       │   ├── Telegram*.jsx   # ~25 Telegram components
│       │   └── Discord*.jsx    # ~8 Discord components
│       ├── config/             # API endpoints, tiers, nav
│       ├── contexts/           # Toast, theme, layout
│       ├── hooks/              # SSE, keyboard shortcuts
│       └── features/           # Feature modules
│
└── sell/                       # This sales package
```

---

## User-Facing Routes

| Route | Page | Purpose |
|-------|------|---------|
| `/` | LandingPage | Marketing, pricing, trial CTA, FAQ |
| `/login` | Login | License key authentication |
| `/dashboard` | Dashboard | Main hub with stats |
| `/dashboard/telegram/:viewId` | TelegramPage | 18 Telegram tools |
| `/dashboard/discord/*` | DiscordPage | 5 Discord tools |
| `/dashboard/marketplace` | MarketplacePage | Channel marketplace |
| `/dashboard/personal` | PersonalPage | Link manager |
| `/dashboard/tools` | ToolsPage | Utility tools |
| `/dashboard/profile` | UserProfilePage | User profile & stats |
| `/admin/*` | AdminDashboard | Full admin control plane |
| `/poc` | POCPage | Investor/sales proof-of-concept |
| `/contact` | ContactPage | Contact form |

---

## Authentication Model

- **License key** based (no email/password)
- Key format: alphanumeric string
- Stored in `localStorage` as `accessKey`
- Validated on every API request via `authenticateKey.js`
- Tier limits enforced by `tierEnforcement.js` middleware
- Admin keys have `subscriptionTier: 'admin'`

---

## Task System

All automation runs as **tasks** stored in MongoDB:

| Field | Purpose |
|-------|---------|
| `taskType` | e.g. `TELEGRAM_BULK_JOIN`, `TELEGRAM_MASS_DM` |
| `status` | pending, running, paused, completed, failed |
| `config` | Task-specific settings (channels, messages, etc.) |
| `progress` | Real-time progress tracking |
| `keyId` | Owner license key |

Tasks survive server restarts via `taskRestoration.js`.

---

## Real-Time Features

| Feature | Technology |
|---------|-----------|
| Live task logs | SSE (Server-Sent Events) |
| Toast notifications | Global toast context |
| Task progress | Polling + SSE |
| Admin stats | Stale-while-revalidate cache |

---

## Security Measures

| Measure | Implementation |
|---------|---------------|
| Helmet | HTTP security headers |
| CORS | Configured origins |
| Rate limiting | Redis + in-memory per tier |
| Tier enforcement | Middleware on every feature route |
| Trial anti-fraud | IP, subnet, fingerprint, username limits |
| CAPTCHA | Altcha on contact form |
| Compression | gzip responses |
| Auth rate limit | Per-IP login throttling |
| Device fingerprinting | Trial offer token binding |
