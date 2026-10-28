# Bug & Issue Report — Cynbit Technologies Website
**Auditor / Candidate:** Aditya Raj  
**Target Application:** Cynbit Technologies ("https://www.cynbit.in/" (https://www.cynbit.in/))  
**Scope:** Functional, Responsiveness, UI/UX, Content, and Workflow Testing  
**Total Issues Logged:** 11 (9 Confirmed Defects + 2 Quality / Content Flags)

---

## 1. Master Bug & Issue Summary Matrix

| Bug ID | Title / Summary | Area / Module | Category | Severity / Priority | Status |
| :---: | :--- | :--- | :--- | :---: | :---: |
| **B01** | Mobile navigation overlay covers its own close control | Global Header | Usability / Responsiveness | **P1 (High)** | Confirmed |
| **B02** | Selecting current route from mobile menu fails to close overlay | Global Navigation | Usability / Navigation | **P2 (Medium)** | Confirmed |
| **B03** | Fixed mobile bottom navigation permanently obscures footer content | Global Footer | Responsiveness / CSS | **P2 (Medium)** | Confirmed |
| **B04** | Reading Terms & Conditions during career application clears entered inputs | Careers / Job Application | Data Loss / Form Workflow | **P1 (High)** | Confirmed |
| **B05** | Case study category filter buttons do not update or filter cards | Case Studies (`/case-studies`) | Functional Bug | **P2 (Medium)** | Confirmed |
| **B06** | TenantEase *"Schedule Tour"* CTA defaults demo request to Cynbit Studio | Products (`/products`) | Functional / Logic Bug | **P2 (Medium)** | Confirmed |
| **B07** | Blog article share button provides no visual feedback or clipboard trigger | Blog (`/blog`) | Usability / Feedback | **P2 (Medium)** | Confirmed |
| **B08** | Advertised 12-minute read article is incomplete (single paragraph) | Blog Content | Content Integrity / UX | **P2 (Medium)** | Confirmed |
| **B09** | BitElevate internship confirmation email not delivered after successful submit | BitElevate Internship | Notification / Workflow | **P2 (Medium)** | Confirmed |
| **B10** | Contact & Career forms encounter HTTP 500 on live submission | Backend Server Actions | Functional / Backend Crash | **P1 (High)** | Confirmed |
| **B11** | Internal broken link `/blog/ai-agents-transforming-business` (404) | Blog Internal Links | Navigation / Broken Link | **P2 (Medium)** | Confirmed |
| **I01** | GlobalPay case study detail hero image is broken / missing | Case Study Detail | Media / Visual Asset | **P2 (Medium)** | Confirmed Flag |
| **I02** | Hero heading in Student Diaries misspells "Unheard" as "Unheared" | BitElevate Diaries | Copy / Editorial Defect | **P3 (Low)** | Confirmed Flag |

> **Priority Classification Guide:**  
> - **P1 (High):** Critical journey blocked or applicant data destroyed; fix immediately before launch.  
> - **P2 (Medium):** Functional defect or misdirection affecting core features; fix in the upcoming sprint.  
> - **P3 (Low):** Cosmetic or typo issue with low user friction; polish in regular maintenance.

---

## 2. Detailed Bug Reports

---

### Bug ID: B01
**Title:** Mobile navigation overlay covers its own close control button  
**Category:** Usability & Responsive Layout  
**Severity:** High | **Priority:** P1  
**Affected Viewports:** Mobile (375×812, 390×844) & Tablet (768×1024)  
**Affected Route:** Global (all pages on mobile view)  

* **Preconditions:**  
  Browser viewport set to mobile portrait mode (e.g., iPhone SE 375×812 or iPhone 14 390×844). User is signed out.
* **Steps to Reproduce:**
  1. Navigate to `https://www.cynbit.in/`.
  2. Tap the hamburger menu icon in the top header.
  3. Notice the navigation overlay opens to full screen.
  4. Attempt to dismiss the menu by tapping the top-right header area where the close/menu trigger was located (`x=339, y=36`).
* **Expected Result:**  
  A clearly visible, reachable close icon (`×`) should remain on top of the overlay (with high `z-index`) allowing users to easily dismiss the menu and return to the underlying page.
* **Actual Result:**  
  The slide-out/modal navigation drawer covers the header and close trigger. Tapping the top-right corner does nothing, trapping the user inside the menu overlay. (Only selecting an external link or using keyboard controls closes it).
* **User Impact:**  
  Touch users who open the menu by curiosity or mistake are unable to return to their reading context without being forced to click an unwanted link or refresh the page.
* **Acceptance Criteria:**  
  - Ensure a persistent close (`×`) button with `z-index > 50` is visible and clickable on all mobile and tablet resolutions.
  - Pressing the `Escape` key should also dismiss the overlay.

---

### Bug ID: B02
**Title:** Selecting the currently active page from mobile menu fails to close overlay  
**Category:** Navigation & State Management  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** Mobile (375×812, 390×844)  
**Affected Route:** `/products` (and other subpages)  

* **Preconditions:**  
  User is currently on the Product Suite page (`https://www.cynbit.in/products`) on mobile.
* **Steps to Reproduce:**
  1. Open the mobile menu.
  2. Tap **Expertise** to expand the dropdown accordion.
  3. Tap **Product Suite** (the page currently being viewed).
* **Expected Result:**  
  Since the user selected the page they are on, the menu overlay should close immediately and reveal the page content (optionally scrolling to the top).
* **Actual Result:**  
  The menu overlay remains stuck open over the page. Next.js router does not trigger a route transition event because the pathname did not change, so the menu's `isOpen` state is never reset to `false`.
* **User Impact:**  
  Users perceive the button as frozen/broken and are stuck in the overlay.
* **Acceptance Criteria:**  
  - The menu click handler should call `closeMenu()` regardless of whether the target route matches the active route.

---

### Bug ID: B03
**Title:** Fixed mobile bottom navigation permanently overlaps and obscures footer content  
**Category:** Responsiveness & CSS Layout  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** Mobile (375×812, 360×640)  
**Affected Route:** Global (`/contact`, `/story`, `/team`, etc.)  

* **Preconditions:**  
  View page on mobile screen (375px width).
* **Steps to Reproduce:**
  1. Navigate to `https://www.cynbit.in/contact`.
  2. Scroll down to the very bottom of the page to access footer links.
  3. Attempt to read the legal copyright notice and tap legal links (Terms, Cookies, Navigation).
* **Expected Result:**  
  The page body should include adequate bottom padding (`padding-bottom: 5rem` or `env(safe-area-inset-bottom)`) so all footer text and links sit cleanly above the fixed bottom navigation bar.
* **Actual Result:**  
  The fixed bottom app bar sits directly on top of the footer text. The copyright line, location text, and legal links are partially or completely cut off and unclickable.
* **User Impact:**  
  Mobile users cannot read legal disclaimers or access Terms & Conditions and Cookie policies in the footer.
* **Acceptance Criteria:**  
  - Add appropriate bottom padding to `<footer>` or main container to ensure full scroll clearance above fixed navigation elements.

---

### Bug ID: B04
**Title:** Navigating to Terms & Conditions during career application wipes all entered applicant data  
**Category:** Data Loss & Form Usability  
**Severity:** High | **Priority:** P1  
**Affected Viewports:** All (Desktop & Mobile)  
**Affected Route:** `/careers/[role]` (e.g., `/careers/hr-talent-acquisition-intern`, `/careers/qa-testing-intern`)  

* **Preconditions:**  
  User is applying for an internship role on Cynbit Careers.
* **Steps to Reproduce:**
  1. Open any job opening (e.g., `/careers/qa-testing-intern`) and click **"Apply Now"**.
  2. Fill in all required fields: Full Name, Email, Phone number, College, Degree, Experience, and write a statement of purpose.
  3. Add an entry under the dynamic **"Add Qualification"** section.
  4. Scroll to the consent checkbox: *"I agree to the Terms & Conditions and Privacy Policy"*.
  5. Click the hyperlinked **"Terms & Conditions"** text to read what is being agreed to.
  6. After reviewing terms, click the browser's **Back** button to return to the application form.
* **Expected Result:**  
  The application form state should be preserved (either via React state, `sessionStorage`, or opening legal terms in a new tab / modal), allowing the candidate to continue without losing their progress.
* **Actual Result:**  
  The page completely reloads in its initial empty state. All text fields, dynamic qualification rows, and uploaded attachments are wiped clean.
* **User Impact:**  
  Severe frustration and high applicant drop-off rate. Candidates who spend 10-15 minutes writing thoughtful responses lose their work simply by trying to read the required consent terms.
* **Acceptance Criteria:**  
  - Open legal links (Terms and Privacy) in a new browser tab (`target="_blank" rel="noopener noreferrer"`) or within an in-page modal dialog.
  - Alternatively, persist form drafts in local/session storage during the session.

---

### Bug ID: B05
**Title:** Case study category filter buttons do not filter project cards  
**Category:** Functional Defect  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** All (Desktop & Mobile)  
**Affected Route:** `https://www.cynbit.in/case-studies`  

* **Preconditions:**  
  Navigate to the Case Studies hub.
* **Steps to Reproduce:**
  1. Scroll to the category filter bar (e.g., *"All Success Stories"*, *"Cloud Strategy"*, *"AI & ML"*, *"Healthcare"*).
  2. Click on **"Cloud Strategy"** or **"Healthcare"**.
  3. Observe the highlighted active category pill and the displayed case study cards below.
* **Expected Result:**  
  The selected category tab should display an active visual state, and the grid below should update to display only case studies matching that category (or show a friendly empty state if none match).
* **Actual Result:**  
  *"All Success Stories"* remains permanently selected. No filtering occurs; all 13 case study cards remain visible on screen regardless of which tab is clicked.
* **User Impact:**  
  Prospective clients cannot filter Cynbit's portfolio by industry or technology stack, reducing conversion and usability.
* **Acceptance Criteria:**  
  - Wire the category buttons to a state filter (e.g., `selectedCategory`) and filter the rendered list accordingly.

---

### Bug ID: B06
**Title:** TenantEase *"Schedule Tour"* CTA incorrectly selects Cynbit Studio in the demo modal  
**Category:** Functional & Business Logic Defect  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** All viewports  
**Affected Route:** `https://www.cynbit.in/products`  

* **Preconditions:**  
  User is evaluating proprietary software products on the Product Suite page.
* **Steps to Reproduce:**
  1. Navigate to `https://www.cynbit.in/products`.
  2. Scroll down to the **TenantEase** product card.
  3. Click the primary button: **"Schedule Tour"**.
  4. Inspect the opened *"Schedule a Demo"* drawer/modal.
* **Expected Result:**  
  The product dropdown/radio selector should automatically pre-select **"TenantEase"**, and the submit button should indicate *"Request TenantEase Demo"*.
* **Actual Result:**  
  The drawer defaults to **"Cynbit Studio"**, and the submit CTA displays *"Request Studio Demo"*. (A similar misselection was noted for Revio).
* **User Impact:**  
  Users interested in TenantEase inadvertently submit demo requests for Cynbit Studio, causing sales lead misrouting and client confusion.
* **Acceptance Criteria:**  
  - Pass the corresponding product ID (e.g., `tenantease`, `revio`) through the modal trigger props so the correct product is pre-selected upon opening.

---

### Bug ID: B07
**Title:** Blog article share button produces no visual feedback or clipboard copy action  
**Category:** Functional & UX Feedback  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** Mobile & Desktop  
**Affected Route:** `/blog/[slug]` (e.g., `/blog/ai-lead-generation`, `/blog/best-ai-tools-startups`)  

* **Preconditions:**  
  Viewing any published article in the Blog section.
* **Steps to Reproduce:**
  1. Open a blog article.
  2. Locate the share icon button adjacent to the author metadata and publication date.
  3. Click or tap the share button.
* **Expected Result:**  
  On supported devices/browsers, the native Web Share API dialog should trigger; on browsers without native share, it should copy the URL to the clipboard and display a clear toast confirmation (e.g., *"Link copied to clipboard!"*).
* **Actual Result:**  
  The button changes color to teal upon being clicked, but no share dialog opens, no toast notification appears, and no clipboard feedback is given.
* **User Impact:**  
  Users wanting to share Cynbit articles on LinkedIn/X cannot tell if the link was copied, hindering organic social sharing.
* **Acceptance Criteria:**  
  - Implement a reliable fallback using `navigator.clipboard.writeText(window.location.href)` with an immediate, accessible toast notification.

---

### Bug ID: B08
**Title:** Article advertised as a "12-minute read" consists of only a single unfinished paragraph  
**Category:** Content Integrity & Usability  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** All viewports  
**Affected Route:** `https://www.cynbit.in/blog/efficiency-killing-digital-transformation`  

* **Preconditions:**  
  User browsing the Blog archive.
* **Steps to Reproduce:**
  1. Navigate to `/blog` and go to page 5 of the blog listing.
  2. Locate the article card titled *"Why 'Efficiency' is Killing Your Digital Transformation"*, advertised with a badge of **"12 min read"**.
  3. Click into the article.
  4. Scroll past the hero banner into the body content.
* **Expected Result:**  
  A complete, comprehensive in-depth article corresponding to a 12-minute read (~2,000–2,500 words).
* **Actual Result:**  
  The article consists solely of one incomplete opening paragraph ending in an ellipsis before immediately transitioning to the author bio card. The remainder of the article body is missing.
* **User Impact:**  
  Damages company credibility and thought-leadership positioning when visitors click expecting an in-depth whitepaper and encounter placeholder content.
* **Acceptance Criteria:**  
  - Publish the complete draft content for this article or archive/unpublish the post until the full copy is ready.

---

### Bug ID: B09
**Title:** BitElevate internship confirmation email is not delivered despite success message  
**Category:** Notification & Integration Workflow  
**Severity:** Medium | **Priority:** P2  
**Affected Viewports:** All  
**Affected Route:** `https://www.cynbit.in/bitelevate`  

* **Preconditions:**  
  Fresh, verified test email inbox ready for monitoring.
* **Steps to Reproduce:**
  1. Go to **BitElevate > The Ecosystem > Apply for Internship**.
  2. Complete the candidate application form with valid details and test email address.
  3. Select *Full Stack Developer Internship (4 Weeks Foundation Sprint)* and click **Submit**.
  4. Notice the confirmation screen: *"Application Submitted! We have sent a confirmation email with next steps."*
  5. Monitor the test inbox (including spam/promotions) for 10 minutes.
* **Expected Result:**  
  An automated confirmation email is delivered to the applicant with verification details and application timeline.
* **Actual Result:**  
  While the frontend displays a success message (HTTP 200 response), no email arrives in the applicant's inbox.
* **User Impact:**  
  Applicants are left uncertain whether their submission was actually received, leading to duplicate submissions or inquiries to support.
* **Acceptance Criteria:**  
  - Verify SMTP/SendGrid/Resend API keys and transactional email triggers on the serverless backend.
  - Add backend error handling so the frontend does not promise an email if the mailing queue fails.

---

### Visual Flag: I01
**Title:** GlobalPay Case Study hero image is broken and renders alt text placeholder  
**Category:** Broken Media / Visual Quality  
**Severity:** Medium | **Priority:** P2  
**Affected Route:** `https://www.cynbit.in/case-studies/fintech-payments-evolution`  

* **Observation:**  
  On the case study detail page for GlobalPay (*"Revolutionizing cross-border payments for the next billion users"*), the primary hero photograph fails to load, leaving a large blank empty container displaying raw alternative text.
* **Fix Recommendation:**  
  Update the image source path in the case study dataset and verify asset availability on the CDN.

---

### Visual Flag: I02
**Title:** Hero heading in Student Diaries misspells "Unheard" as "Unheared"  
**Category:** Copy & Editorial Quality  
**Severity:** Low | **Priority:** P3  
**Affected Route:** `https://www.cynbit.in/diaries`  

* **Observation:**  
  The main hero banner of the Student Diaries landing page reads: **"Unheared Stories."**  
* **Fix Recommendation:**  
  Correct the typographical error in the template to **"Unheard Stories."**

---

### Bug ID: B10
**Title:** Contact & Career form Next.js Server Actions return HTTP 500 error  
**Category:** Backend & Form Submission Failure  
**Severity:** High | **Priority:** P1  
**Affected Route:** `/contact` & `/careers/*`  

* **Preconditions:**  
  User navigates to `/contact` or any job opening on `/careers/*` on the live site.
* **Steps to Reproduce:**
  1. Complete the Contact Inquiry form with valid name, email, and message.
  2. Click **"Send Inquiry"**.
  3. Inspect the Network tab in developer tools.
* **Expected Result:**  
  Server Action responds with HTTP 200/201 and user receives a confirmation toast/screen.
* **Actual Result:**  
  Server Action responds with `HTTP 500 Internal Server Error` with payload `1:E{"digest":"571349486"}`. User sees error message: *"Something went wrong. Please try again or email us directly."*
* **Root Cause:**  
  Missing environment variable (e.g. database connection string or transactional mailer key) in production Vercel configuration causing unhandled server-side crash.
* **Acceptance Criteria:**  
  - Ensure backend database and email services handle connection errors gracefully and verify production environment variables.

---

### Bug ID: B11
**Title:** Internal 404 broken link to AI Agents article across 4 published blog posts  
**Category:** Broken Internal Link  
**Severity:** Medium | **Priority:** P2  
**Affected Route:** `/blog/future-ai-customer-support`, `/blog/ai-automation-trends-2026`, etc.  

* **Observation:**  
  Four separate blog posts link to `https://www.cynbit.in/blog/ai-agents-transforming-business`, which throws a 404 page. The valid article URL is `https://www.cynbit.in/blog/ai-agents-business-operations`.
* **Fix Recommendation:**  
  Update internal markdown links or add a 301 redirect from `/blog/ai-agents-transforming-business` to `/blog/ai-agents-business-operations`.
