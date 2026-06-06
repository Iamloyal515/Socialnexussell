# Screenshot Checklist for Sales Deck

> No static screenshots exist in the repo. The UI is fully functional and live at **socialnexus.us**.  
> Capture these screenshots from the running application for your sales materials.

---

## Priority 1 — Must Have (Core Value)

| # | Page | URL | What to Capture |
|---|------|-----|-----------------|
| 1 | Landing page hero | `/` | Hero section with headline + CTA |
| 2 | Landing pricing | `/` | Pricing section with all tiers |
| 3 | Free trial popup | `/` (click "Claim free trial") | Trial activation modal |
| 4 | Login page | `/login` | License key login form |
| 5 | Main dashboard | `/dashboard` | Stats overview with active sessions |
| 6 | Telegram hub | `/dashboard/telegram/sessions` | Sidebar with all 18 tools |
| 7 | Bulk Channel Joiner | `/dashboard/telegram/bulk-channel-joiner` | Multi-account join interface |
| 8 | Mass DM | `/dashboard/telegram/mass-dm` | Campaign wizard |
| 9 | Account Health | `/dashboard/telegram/sessions` | Health dashboard section |
| 10 | Admin overview | `/admin` | Admin stats overview tab |

---

## Priority 2 — Feature Showcase

| # | Page | URL | What to Capture |
|---|------|-----|-----------------|
| 11 | Channel Validator | `/dashboard/telegram/channel-validator` | Validation results |
| 12 | Channel Fetcher | `/dashboard/telegram/channel-fetcher` | Fetched channel list |
| 13 | AddLink Creator | `/dashboard/telegram/addlink-creator` | Folder management |
| 14 | Auto-Reply | `/dashboard/telegram/autoreply` | DM auto-reply config |
| 15 | Bulk Messaging | `/dashboard/telegram/automation` | Task manager with live logs |
| 16 | Media Copier | `/dashboard/telegram/media-copier` | Copy progress |
| 17 | Keyword Searcher | `/dashboard/telegram/keyword-searcher` | Search results |
| 18 | Profile Changer | `/dashboard/telegram/profile` | Mass profile edit |
| 19 | Contact Manager | `/dashboard/telegram/contact-manager` | Tagged contacts |
| 20 | Channel Reactions | `/dashboard/telegram/channel-reactions` | Auto-reaction config |

---

## Priority 3 — Platform & Business

| # | Page | URL | What to Capture |
|---|------|-----|-----------------|
| 21 | Marketplace | `/dashboard/marketplace` | Channel listings |
| 22 | Discord overview | `/dashboard/discord` | Discord dashboard |
| 23 | Discord auto-reply | `/dashboard/discord` (autoreply tab) | DC auto-reply |
| 24 | User profile | `/dashboard/profile` | Usage stats + tier info |
| 25 | POC page | `/poc` | Investor documentation page |
| 26 | Admin users | `/admin` (users tab) | Key management table |
| 27 | Admin performance | `/admin` (performance tab) | System monitoring |
| 28 | Admin marketplace | `/admin` (marketplace tab) | Listing moderation |
| 29 | Contact page | `/contact` | Contact form |
| 30 | SellAuth checkout | `/` (click buy button) | Payment modal |

---

## Priority 4 — Admin Deep Dive

| # | Page | What to Capture |
|---|------|-----------------|
| 31 | User modal | Click any user → session list + limit overrides |
| 32 | Key creation | Create new license key form |
| 33 | Bulk key create | Bulk key generation |
| 34 | Analytics charts | Admin analytics tab charts |
| 35 | Contact inquiries | Admin contact tab |

---

## How to Capture

### Option A: Live Site
1. Visit `https://socialnexus.us`
2. Use browser dev tools → responsive mode for consistent sizing
3. Recommended resolution: **1920x1080** or **1440x900**
4. Use Windows Snipping Tool or ShareX

### Option B: Local Dev
```bash
# Terminal 1: Backend
cd backend && npm start

# Terminal 2: Frontend
cd FRONTEND && npm start
# Visit http://localhost:3000
```

### Option C: Full Page Screenshots
Use browser extension like "GoFullPage" for long pages (landing, POC).

---

## Suggested File Naming

Save screenshots in `sell/images/` with this naming:

```
sell/images/
├── 01-landing-hero.png
├── 02-landing-pricing.png
├── 03-trial-popup.png
├── 04-login.png
├── 05-dashboard.png
├── 06-telegram-hub.png
├── 07-bulk-channel-joiner.png
├── 08-mass-dm.png
├── 09-account-health.png
├── 10-admin-overview.png
├── 11-channel-validator.png
├── ...
└── 35-contact-inquiries.png
```

---

## Existing Brand Assets

| Asset | Location |
|-------|----------|
| Logo (512px) | `https://socialnexus.us/logo512.png` |
| Favicon | Referenced in SEO config |
| Brand colors | Dark theme: slate/emerald/purple gradients |
| Font | System fonts via Tailwind |

---

## Video Demo (Recommended)

The landing page has a hidden demo video modal (`#demo-video-modal`). Consider recording:

1. **2-min overview**: Login → dashboard → start bulk join → show progress
2. **1-min trial flow**: Landing → claim trial → enter username → login
3. **1-min admin**: Admin login → create key → view performance
4. **30-sec marketplace**: Browse → copy channel link

Place videos in `sell/videos/` for the sales package.
