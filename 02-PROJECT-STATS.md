# Project Statistics

> Measured June 6, 2026 — excludes `node_modules`, `.git`, `dist`, `build`, `.next`, `coverage`

---

## Totals

| Metric | Count |
|--------|-------|
| **Total files** | 463 |
| **Total lines of code** | 314,112 |
| **JavaScript (.js)** | 279 files |
| **React JSX (.jsx)** | 152 files |
| **CSS** | 6 files |
| **JSON** | 5 files |
| **Markdown** | 7 files |

---

## By Major Directory

| Directory | Files | Lines |
|-----------|-------|-------|
| `backend/` | 226 | 120,038 |
| `FRONTEND/` | 235 | 193,912 |
| `backend/src/routes/` | 91 | 81,698 |
| `backend/src/routes/telegram/` | 58 | 64,895 |
| `backend/src/routes/discord/` | 14 | 5,060 |
| `backend/src/services/` | 51 | 17,130 |
| `backend/src/models/` | 26 | 2,632 |
| `FRONTEND/src/components/` | 143 | 157,785 |

---

## Largest Single Files (Top 10)

| File | Lines | Role |
|------|-------|------|
| `backend/src/routes/telegram/bulk-channel-join-v2.js` | 5,497 | Multi-account channel joiner engine |
| `FRONTEND/src/components/AccountPurchase.jsx` | ~6,900+ | Telegram account marketplace UI |
| `FRONTEND/src/components/AdminDashboard.jsx` | ~6,700+ | Full admin control plane |
| `FRONTEND/src/components/EnhancedTaskManager.jsx` | ~15,000+ | Unified task orchestration UI |
| `FRONTEND/src/components/POCPage.jsx` | ~3,384 | Investor/sales documentation page |
| `backend/src/routes/admin.js` | ~3,700+ | Admin API |
| `FRONTEND/src/components/EnhancedBulkChannelJoiner.jsx` | large | Bulk joiner frontend |
| `FRONTEND/src/components/LandingPage.jsx` | large | Marketing landing page |
| `FRONTEND/src/components/TelegramPage.jsx` | large | Telegram hub (18 tools) |
| `backend/index.js` | large | Main server entry |

---

## Backend Route Files Breakdown

| Category | Files | Approx Lines |
|----------|-------|--------------|
| Telegram routes | 58 | 64,895 |
| Discord routes | 14 | 5,060 |
| Admin routes | 5+ | ~4,000+ |
| User routes | 3 | ~500 |
| Keys / auth | 3 | ~600 |
| Tasks | 2 | ~1,500 |
| Marketplace / others | 6 | ~3,000 |
| Webhooks / campaigns | 3 | ~1,000 |

---

## Frontend Component Count

| Category | Count |
|----------|-------|
| Total JSX components | 143 |
| Telegram-specific | ~25 |
| Discord-specific | ~8 |
| Admin sub-components | 6 |
| Shared (modals, task manager, etc.) | ~50+ |

---

## API Surface

| Metric | Count |
|--------|-------|
| **REST API endpoints** | 461 |
| **MongoDB models** | 26 |
| **Backend services** | 51 |
| **Middleware modules** | 5+ |

---

## Comparison: POC Page Marketing vs Actual

The built-in `/poc` page shows aspirational/marketing numbers. **Actual measured stats:**

| Metric | POC Page Claims | Actual (Measured) |
|--------|-----------------|-------------------|
| Total lines | 450,000 | **314,112** |
| Total files | 8,500 | **463** |
| API endpoints | 180 | **461** |
| Database models | 15 | **26** |
| Frontend components | 120 | **143** |
| Backend services | 35 | **51** |

> The actual codebase is smaller in file count (excludes node_modules/deps) but has **more API endpoints and models** than the POC page states.
