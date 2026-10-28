# Prioritized Improvement Recommendations — Cynbit Technologies
**Auditor / Candidate:** Aditya Raj  
**Target Application:** Cynbit Technologies (`https://www.cynbit.in/`)  
**Purpose:** Actionable Engineering Fixes, Quality Gates, and Long-Term QA Strategy  

---

## 1. Prioritized Development Fix Roadmap

The identified issues have been categorized into a pragmatic 3-tier roadmap based on user journey friction, data integrity risk, and implementation effort.

```
+-----------------------------------------------------------------------------------+
| PRIORITY 1: IMMEDIATE JOURNEY BLOCKERS & DATA LOSS                                 |
| - B04: Retain application input on legal consent navigation                        |
| - B01: Ensure mobile menu close control is always visible and operable             |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| PRIORITY 2: FUNCTIONAL CORRECTNESS & CORE WORKFLOWS                                |
| - B06: Match product CTA selection to correct demo modal choice (TenantEase/Revio) |
| - B05: Wire case-study category filter tabs to actual datasets                     |
| - B03: Add safe-area padding above mobile fixed navigation bar                     |
| - B09: Verify and log transactional email dispatch on BitElevate applications      |
| - B07: Provide reliable clipboard copy feedback for blog sharing                   |
| - B08 & I01: Replace truncated article copy and restore broken GlobalPay image     |
| - B02: Auto-dismiss mobile drawer on active route re-selection                     |
+-----------------------------------------------------------------------------------+
                                      |
                                      v
+-----------------------------------------------------------------------------------+
| PRIORITY 3: POLISH, COPY & EDITORIAL INTEGRITY                                     |
| - I02: Correct "Unheared Stories" to "Unheard Stories" in Student Diaries          |
+-----------------------------------------------------------------------------------+
```

---

### Phase 1: Immediate Fixes (P1 — Release Blockers)

#### 1. Form Data Loss Prevention (Bug B04)
* **Problem:** Candidates lose all entered application details when navigating to read Terms & Conditions.
* **Engineering Recommendation:**
  1. **Preferred Solution (Zero State Loss):** Open external legal links (`/terms`, `/privacy`) in a new browser tab with `target="_blank" rel="noopener noreferrer"`, or present the terms in an in-page modal dialog.
  2. **Secondary Guard:** Implement local session caching (e.g., lightweight form persistence hook using `sessionStorage`) so that unsubmitted form progress survives accidental back/refresh actions.

#### 2. Mobile Navigation Trap (Bug B01 & B02)
* **Problem:** At viewport widths under 768px, opening the hamburger menu places the overlay directly over the close control. Re-selecting the current page also leaves the drawer permanently open.
* **Engineering Recommendation:**
  1. Restructure the header component so the close icon (`×`) has an explicit high `z-index` (`z-50`) positioned statically above the sliding backdrop.
  2. Bind an `onClick` listener on all navigation links that unconditionally invokes `setIsMenuOpen(false)`, even when clicking the active route.
  3. Bind an accessible keyboard listener for the `Escape` key to trigger drawer closure.

---

### Phase 2: Next Sprint Fixes (P2 — Functional & Workflow Enhancements)

#### 1. Product Demo Modal Routing (Bug B06)
* **Problem:** Clicking *"Schedule Tour"* on the TenantEase card opens the demo drawer with *"Cynbit Studio"* pre-selected.
* **Engineering Recommendation:**
  - Update the `openDemoModal(productId)` function to accept a dynamic parameter (`'tenantease'`, `'studio'`, `'revio'`) and set the initial form state accordingly.

#### 2. Case Study Category Filter Wiring (Bug B05)
* **Problem:** Filter buttons ("Cloud Strategy", "Healthcare", "AI & ML") are static and do not trigger filtering.
* **Engineering Recommendation:**
  - Create a reactive filter state `const [activeCategory, setActiveCategory] = useState('all')` and render filtered cards: `cards.filter(c => activeCategory === 'all' || c.category === activeCategory)`. Add a friendly empty-state illustration if zero case studies match.

#### 3. Mobile Viewport Layout & Safe-Area Padding (Bug B03)
* **Problem:** Fixed mobile bottom bar covers the footer copyright and legal links.
* **Engineering Recommendation:**
  - Add responsive bottom padding to the main page container or footer: `padding-bottom: calc(4.5rem + env(safe-area-inset-bottom, 0px))`.

#### 4. Transactional Email Queue Observability (Bug B09)
* **Problem:** BitElevate application form displays a *"Confirmation email sent"* banner, but no email is delivered.
* **Engineering Recommendation:**
  - Add end-to-end logging for email delivery services (e.g., Resend, SendGrid, AWS SES).
  - Ensure API responses differentiate between *"application saved to database"* and *"email dispatched successfully"*. If the email service fails, the UI should gracefully state *"Application received. Our team will contact you shortly"* rather than promising an immediate email.

#### 5. Social Share Action & Fallback (Bug B07)
* **Problem:** Blog share button gives no indication of success or link copying.
* **Engineering Recommendation:**
  - Implement a reliable share fallback using `navigator.clipboard.writeText(window.location.href)` with an immediate, accessible toast notification when native sharing is unavailable.

#### 6. Content Integrity & Asset Verification (Bug B08 & I01)
* **Problem:** Truncated 12-minute article; missing GlobalPay hero image.
* **Engineering Recommendation:**
  - Implement a pre-publish CMS check ensuring published articles have a minimum word count matching their estimated read time.
  - Fix the image path in `/case-studies/fintech-payments-evolution` and add a fallback image handler.

---

## 2. Infrastructure, SEO & Security Hardening Recommendations

1. **Implement Missing Security Headers:**
   Configure standard security headers in `next.config.js` or Vercel header rules:
   - `X-Content-Type-Options: nosniff`
   - `X-Frame-Options: SAMEORIGIN` (protect against Clickjacking)
   - `Referrer-Policy: strict-origin-when-cross-origin`
   - `Permissions-Policy: camera=(), microphone=(), geolocation=()`
   - `Content-Security-Policy` configured with appropriate directives for Next.js and Google Fonts/Analytics.
2. **Eliminate Redirect Chains & Fix Sitemap:**
   - Update `sitemap.xml` entries to use the canonical domain with `www` (`https://www.cynbit.in/...`) rather than apex (`https://cynbit.in/...`). This saves crawl budget and eliminates redirect delays.
3. **Configure 301 Redirects for Legacy / Conventional URLs:**
   Add redirects in `next.config.js` to guide users reaching standard paths:
   - `/about` -> `/story` (301 Permanent)
   - `/privacy-policy` -> `/privacy` (301 Permanent)
   - `/terms-and-conditions` -> `/terms` (301 Permanent)
   - `/contact-us` -> `/contact` (301 Permanent)

---

## 3. QA Testing Enhancements & Automated Regression Suite

To ensure regressions do not slip into production, Cynbit should introduce a lightweight automated end-to-end testing suite (using **Playwright** or **Cypress**):

### Recommended Automated Test Cases

1. **Navigation & Overlay Test:**
   - Emulate mobile viewport (375×812).
   - Assert menu opens, close icon is visible with positive bounding box, and tapping close icon hides menu.
   - Assert pressing `Escape` dismisses menu.
2. **Form Resilience & Legal Navigation Test:**
   - Fill career application inputs.
   - Click Terms of Service link and trigger browser back.
   - Assert input values remain intact.
3. **Product Selection Agreement Test:**
   - Loop through all product CTA triggers (*Cynbit Studio*, *TenantEase*, *Revio*).
   - Assert modal title and selected radio/dropdown exactly match the clicked product.
4. **Case Study Filter Test:**
   - Click each category pill.
   - Assert that non-matching card elements are excluded from the DOM.
5. **Transactional Email Health Check (Smoke Test):**
   - Scheduled daily synthetic test submitting an inquiry to a disposable inbox (e.g., Mailosaur or Mailtrap API) verifying delivery within 60 seconds.

---

## 4. Suggested Release Gates (Definition of Done)

Before promoting future builds to production, the QA team recommends the following release criteria:
* [ ] **Zero P1 defects:** No data loss scenarios or broken navigation traps.
* [ ] **Consistent modal logic:** All dynamic triggers pre-select the appropriate options.
* [ ] **100% Asset availability:** No broken image links or placeholder text in published case studies.
* [ ] **Mobile layout sign-off:** Critical CTAs and footer links must be fully reachable without overlay clipping across 360px, 375px, and 390px viewports.
* [ ] **Security validation:** Core security headers must be verified on staging before production deployment.
