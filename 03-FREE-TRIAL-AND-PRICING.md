# Free Trial & Pricing

---

## Free Trial — 30 Days, Full Tier 3 Access

### What Users Get

- **30 days** of **Tier 3 (Scale)** access — the highest self-serve tier
- **No payment required**
- All Tier 3 limits apply during trial (16 sessions, 16 tasks, Mass DM, Bulk Joiner, Keyword Searcher, etc.)

### How It Works (Current Flow)

1. User visits landing page (`/`) or clicks "Claim free trial"
2. `TrialOfferPopup.jsx` opens with activation steps
3. User must **join Telegram channel**: `https://t.me/SocialNexusX`
4. User enters their **Telegram @username**
5. System auto-generates a **new license key** with 30-day trial
6. User logs in at `/login` with the key

### Legacy Flow (Still Supported)

- Attach 30-day trial to an **existing unused license key**
- Key must not be activated yet

### Anti-Fraud System

| Protection | Limit |
|------------|-------|
| IP per day | 1 trial |
| IP per month | 3 trials |
| Subnet (/24) per day | 2 trials |
| Subnet per month | 6 trials |
| Device fingerprint per hour | 8 attempts |
| Offer token window | 10 minutes |
| Telegram username | Must be unique (one trial per @username) |
| Audit log | `trialClaimLog` MongoDB collection |

### API Endpoints

| Method | Route | Purpose |
|--------|-------|---------|
| POST | `/trial/offer` | Generate signed offer token |
| POST | `/trial/activate` | Activate trial (new or legacy flow) |

### Source Files

- `backend/src/routes/keys/trial-activate.js`
- `backend/src/services/trialTier.js` — grants Tier 3 while active
- `backend/src/models/trialClaimLog.js` — anti-abuse audit
- `FRONTEND/src/components/TrialOfferPopup.jsx`

---

## Subscription Tiers

Canonical config: `FRONTEND/src/config/tiers.js`  
Enforcement: `backend/src/services/limits.js`, `backend/src/middleware/tierEnforcement.js`

### Tier 1 — Starter ($5)

| Limit | Value |
|-------|-------|
| Telegram sessions | 4 |
| Automation tasks | 4 |
| Custom DM commands | 1 |
| Contacts | 50 |
| Channels | 10 |
| Channel fetch | 25 per fetch |
| Media copier | ❌ |
| Bulk channel joiner | ❌ |
| Keyword searcher | ❌ |
| Mass DM | ❌ |
| Bulk user adder | ❌ |
| Auto-reply | ✅ |
| Discord accounts | 2 |
| Discord tasks | 2 |
| Trackable links | 1 |
| Marketplace approvals needed | 3 |

### Tier 2 — Pro ($10)

| Limit | Value |
|-------|-------|
| Telegram sessions | 8 |
| Automation tasks | 8 |
| Custom DM commands | 3 |
| Recent DMs | 100 |
| Contacts | 200 |
| Channels | 50 |
| Media copier | 10/day, 1 concurrent |
| Bulk channel joiner | ✅ 1 run/day, 100 channels |
| Keyword searcher | ❌ |
| Mass DM | ❌ |
| Auto-reply | ✅ |
| Discord accounts | 4 |
| Discord tasks | 4 |
| Trackable links | 2 |
| Marketplace approvals | 1 |

### Tier 3 — Scale ($15)

| Limit | Value |
|-------|-------|
| Telegram sessions | 16 |
| Automation tasks | 16 |
| Custom DM commands | 10 |
| Recent DMs | 500 |
| Contacts | 500 |
| Channels | 300 |
| Media copier | 100/day, 2 concurrent |
| Bulk channel joiner | ✅ 3 runs/day, 300 channels |
| Keyword searcher | ✅ 2/day, 50 keywords, 10 accounts |
| Mass DM | ✅ |
| Bulk user adder | ✅ |
| Auto-reply | ✅ |
| Discord accounts | 6 |
| Discord tasks | 6 |
| Trackable links | 10 |
| Marketplace | Instant access |

### Lifetime ($350 one-time)

| Limit | Value |
|-------|-------|
| Telegram sessions | 24 |
| Automation tasks | 24 |
| Recent DMs | 1,000 |
| Contacts | 2,000 |
| Channels | 500 |
| Media copier | 1,000/day |
| Bulk channel joiner | **Unlimited** |
| Keyword searcher | 10/day |
| Mass DM | ✅ |
| Discord accounts | 20 |
| Trackable links | 20 |
| Priority | VIP |

### Admin (Custom)

- **Unlimited everything**
- Full admin dashboard access
- Used for platform operators

---

## Self-Service Checkout (SellAuth)

Landing page (`LandingPage.jsx`) has a **parallel pricing model** via SellAuth embed:

| Plan | Price | SellAuth Product ID | Variant ID |
|------|-------|---------------------|------------|
| 1 Year self-service | $10/year | 665857 | 1052298 |
| Lifetime self-service | $50 one-time | 440295 | 641785 |

- Checkout via `useSellAuthEmbed` hook
- Grants Tier 3 equivalent access
- No Stripe — SellAuth handles payment

---

## License Key System

### Key Model (`backend/src/models/key.js`)

| Field | Purpose |
|-------|---------|
| `subscriptionTier` | tier1 / tier2 / tier3 / lifetime / admin |
| `duration` | Subscription length in days |
| `activatedAt` | When user first logged in |
| `expiresAt` | Expiration timestamp |
| `limitOverrides` | Per-user custom limits (set by admin) |
| `statistics` | Usage tracking per key |
| `trial` | Trial subdocument (isTrial, trialEndsAt, telegramUsername) |
| `suspended` / `frozen` | Admin moderation flags |

### Key API Routes

| Route | Purpose |
|-------|---------|
| `POST /keys/create` | Admin creates keys |
| `GET /keys/check` | Validate key on login |
| `POST /trial/activate` | Activate free trial |

---

## Monetization Analytics (Backend Ready)

These exist but admin UI is not fully wired:

| File | Capabilities |
|------|--------------|
| `backend/src/routes/admin-monetization.js` | Revenue metrics, churn risk, high-value users |
| `backend/src/services/usageTracker.js` | Conversion probability, engagement scores |

---

## Managed Service Plans (Landing Page)

The landing page also advertises **managed retainers** where SocialNexus runs campaigns for clients — separate from self-service software tiers. Copy is in `LandingPage.jsx` FAQ and pricing sections.
