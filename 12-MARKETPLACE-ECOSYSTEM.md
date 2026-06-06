# Marketplace & Ecosystem Features

---

## Channel Marketplace

### Overview

2,000+ pre-vetted Telegram channels available to users through the marketplace. Admin-curated with approval workflow.

**Backend:** `backend/src/routes/others/marketPlace.js` (9 endpoints)  
**Frontend:** `EnhancedMarketplace.jsx`, `MarketplacePage.jsx`  
**Model:** `backend/src/models/market.js`

### User Flow

```
Browse marketplace → Find channel → Copy link / Request access
                                          ↓
                              Tier 3+: Instant access
                              Tier 1-2: Admin approval required
                                          ↓
                              Admin approves → User gets access
```

### Tier-Gated Access

| Tier | Marketplace Approvals Needed |
|------|---------------------------|
| Tier 1 | 3 approvals required |
| Tier 2 | 1 approval required |
| Tier 3 | Instant access |
| Lifetime | Instant access |
| Admin | Instant access |

### Marketplace Features

| Feature | Description |
|---------|-------------|
| Browse listings | Paginated channel list with search |
| Filter by category | Niche, member count, platform |
| Copy channel link | One-click copy |
| Request access | Submit access request (lower tiers) |
| Submit listing | Users can add channels |
| Member count display | Subscriber numbers |
| Platform filter | Telegram vs Discord |

### Admin Marketplace Management

| Action | Endpoint |
|--------|----------|
| View all listings | `GET /admin/marketplace` |
| Approve listing | `PUT /admin/marketplace/:id` |
| Reject listing | `DELETE /admin/marketplace/:id` |
| Bulk upload | `POST /admin/marketplace/bulk-upload` |
| Bulk delete | `POST /admin/marketplace/bulk-delete` |
| Settings | `GET/PUT /admin/marketplace/settings` |
| Access requests | `GET /admin/marketplace/access-requests` |
| Delete all TG marketplaces | `DELETE /admin/marketplace/telegram/all` |
| Delete all DC marketplaces | `DELETE /admin/marketplace/discord/all` |

---

## Personal Link Manager

**Route:** `/dashboard/personal`  
**Frontend:** `PersonalPage.jsx`  
**Backend:** `backend/src/routes/others/linkTracker.js` (7 endpoints)

### Features

| Feature | Description |
|---------|-------------|
| Create trackable links | Short links with click tracking |
| View click stats | Per-link analytics |
| Edit/delete links | Manage link collection |
| Tier limits | 1-20 trackable links per tier |

---

## Contact System

**Route:** `/contact`  
**Frontend:** `ContactPage.jsx`  
**Backend:** `backend/src/routes/others/contact.js`  
**Model:** `contactInquiry.js`

| Feature | Description |
|---------|-------------|
| Contact form | Name, email, message |
| CAPTCHA | Altcha bot protection |
| Admin view | Contact tab in admin dashboard |
| Response tracking | Mark as responded |

---

## Tools Page

**Route:** `/dashboard/tools`  
**Frontend:** `ToolsPage.jsx`

Utility tools accessible from the dashboard hub.

---

## User Profile

**Route:** `/dashboard/profile`  
**Frontend:** `UserProfilePage.jsx`  
**Backend:** `backend/src/routes/user.js`

| Feature | Description |
|---------|-------------|
| Usage statistics | Messages, tasks, sessions |
| Tier information | Current plan + limits |
| Discord account list | Connected accounts |
| Upgrade prompts | Suggest next tier |
| Activity history | Recent actions |

---

## Webhooks

**Backend:** `backend/src/routes/webhooks.js` (7 endpoints)  
**Model:** `webhook.js`

| Feature | Description |
|---------|-------------|
| Create webhook | Register URL + events |
| Trigger webhook | Fire on task events |
| Bulk messaging integration | `bulk-messaging-with-webhooks.js` |
| Campaign integration | Auto-report on completion |

---

## Campaign Reports

**Backend:** `backend/src/routes/campaignReports.js` (7 endpoints)  
**Models:** `campaignReport.js`, `campaignTemplate.js`

| Feature | Description |
|---------|-------------|
| Campaign templates | Reusable message templates |
| Campaign reports | Delivery analytics |
| Export results | Download campaign data |

---

## SEO & Sitemap

**Backend:** `backend/src/routes/others/sitemap.js` (5 endpoints)  
**Frontend:** `SEOHead.jsx`, `seoOptimization.js`

| Feature | Description |
|---------|-------------|
| Auto sitemap | `/sitemap.xml` generation |
| Robots.txt | Search engine directives |
| Structured data | 5 schema types (SoftwareApplication, Organization, WebSite, FAQPage, Breadcrumb) |
| Meta tags | Per-page SEO optimization |
| Canonical URLs | HTTPS-first routing |

---

## Analytics

**Frontend:** `utils/analytics.js`  
**Backend:** `usageTracker.js`

| Feature | Description |
|---------|-------------|
| Google Analytics | Custom GA integration |
| Button click tracking | CTA conversion tracking |
| Checkout start tracking | SellAuth funnel |
| Usage tracking | Per-feature usage for monetization |
| Engagement scores | User activity scoring |
| Churn risk | Predict churn probability |
