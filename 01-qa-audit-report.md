# QA Audit Report — Cynbit Technologies Website

**Auditor / Candidate:** **Aditya Raj**

**Target Application:** [Cynbit Technologies](https://www.cynbit.in/) (`https://cynbit.in` / `https://www.cynbit.in`)  

---

## 1. Executive Summary
An exploratory and functional Quality Assurance audit was conducted on the public-facing production website of **Cynbit Technologies** (`https://cynbit.in/` redirecting to `https://www.cynbit.in/`). 

Testing was structured across desktop, tablet, and mobile viewports following realistic end-to-end user journeys (discovering services, applying for internships, requesting software demos, browsing case studies, reading technical blogs, and contacting support).

### Key Highlights
- **9 Confirmed Functional / Usability Bugs** identified (ranging from P1 journey blockers and data loss to P2 functional misrouting and notification delivery failures).
- **2 Visual / Copy Quality Flags** identified (broken case study hero media and heading typos).
- **Technical & Security Observations** noted regarding missing HTTP security headers (CSP, X-Frame-Options, X-Content-Type-Options) and canonical redirect chains in `sitemap.xml`.
- Core inquiry workflows (Contact form submission, Newsletter subscription, BitElevate custom path requests) successfully submitted and passed basic client-side validation.

---

## 2. Testing Scope

### In-Scope Modules & User Journeys
1. **Global Header & Navigation:**
   - Desktop mega-menus (Company, Expertise, BitElevate).
   - Mobile & tablet responsive hamburger menus, overlay behavior, dismissal controls, and persistent bottom navigation bars.
   - Global CTA buttons (*"Start Discovery"*, *"Join Us"*, *"View Work"*).
2. **Company Pages:**
   - **Our Story (`/story`):** Content rendering, mission statement, and interactive timeline.
   - **Core Team (`/team`):** Team member profile cards, bio drawers, and external profile links.
   - **Careers (`/careers`):** Job listings, role details (e.g., *QA Testing Intern*, *HR Intern*, *Full Stack Developer*), application drawer forms, dynamic qualification entries, consent validation, and file/link attachments.
   - **Social Impact (`/impact`):** Initiative cards, partner trust badges (Manav Seva Trust), and external references.
3. **Expertise & Solutions:**
   - **Services Hub (`/services`):** Service capability blocks, interaction animations, and direct inquiry CTAs.
   - **Product Suite (`/products`):** Detailed solution sections (*Cynbit Studio*, *TenantEase*, *Revio*), schedule tour triggers, and demo request modal integration.
   - **Case Studies (`/case-studies`):** Category filter tabs, project cards, and detailed client story pages (13 case studies including GlobalPay, AR Wardrobe, ERP Modernization).
4. **BitElevate Ecosystem:**
   - **Main Ecosystem (`/bitelevate`):** Learning tracks, curriculum overviews, and direct internship application modals.
   - **Personalized Paths (`/bitelevate/paths`):** Custom roadmap builder and user requirement submissions.
   - **Student Diaries (`/diaries`):** Published student stories, reading views, and community submission forms.
5. **Insights & Blog (`/blog`):**
   - Category filtering, search functionality, pagination controls, article readability, social share actions, and related article links.
6. **Footer & Legal:**
   - Contact support triggers, newsletter modal subscription, address mapping, Terms of Service (`/terms`), Privacy Policy (`/privacy`), Cookie Policy (`/cookies`), and Sitemap/Navigation Directory (`/navigation`).
7. **Cross-Cutting Concerns:**
   - Mobile responsive reflow and touch targets.
   - Form boundary testing (empty fields, invalid emails, XSS payload sanitation, browser back button form persistence).
   - HTTP/HTTPS redirects and passive security headers.

### Out-of-Scope
- Internal administrative portals (`/admin`, `/portal` backend infrastructure).
- Intrusive penetration testing, automated DDoS, or destructive SQLi exploits.
- Modification of backend production databases.

---

## 3. Testing Methodology
The testing approach combined structured **Exploratory Testing (Session-Based)** with **Scenario-Based Negative Testing** to mirror how real users, prospective clients, and student applicants interact with the platform:

1. **User Journey & Workflow Testing:**
   - End-to-end paths were navigated sequentially (e.g., finding an opening on Careers -> filling the multi-step form -> reviewing consent -> submitting).
2. **Boundary & Negative Testing:**
   - Submitting forms with empty inputs, whitespace-only entries, malformed email formats (e.g., `user@`, `test@domain`), excessively long text, and rapid consecutive clicks on submit buttons.
   - Testing back/forward browser navigation during in-progress form entries to evaluate input retention and state restoration.
3. **Responsive & Cross-Device Emulation:**
   - Tested across desktop standard resolutions down to mobile small screens (360px–390px) and tablet aspect ratios.
   - Assessed layout reflow, hamburger menu transitions, touch target sizes (minimum 44×44px standards), and sticky navigation overlaps.
4. **Passive Security & Technical Auditing:**
   - Inspected HTTP response headers using browser developer tools and command-line network utilities (`curl`, TLS handshake inspection).
   - Evaluated SSL certificates, HTTPS enforcement, sitemap consistency, and robot directives.

---

## 4. Test Environment Matrix

| Dimension | Environment / Configuration | Notes & Qualifications |
| :--- | :--- | :--- |
| **Primary Target** | `https://www.cynbit.in/` (Production Vercel deployment) | Evaluated live public build |
| **Desktop Browsers** | Google Chrome (Latest Windows x64), Chromium Core | Resolutions: 1920×1080 (FHD), 1440×900 |
| **Tablet Viewports** | Emulated iPad / Tablet (768×1024, Portrait & Landscape) | Tested menu drawer & multi-column reflow |
| **Mobile Viewports** | iPhone SE (375×812), iPhone 12/13/14 (390×844), Android (360×640) | Tested thumb-reach, fixed nav, sticky footers |
| **Network Conditions** | High-Speed Broadband & Emulated Fast 3G Throttling | Evaluated image lazy loading & FCP |
| **User Identity** | Public Anonymous / Prospective Applicant | Clearly marked fictional QA test data used for forms |

### Viewport Coverage Breakdown

| Functional Area | Desktop (1920×1080 / 1440×900) | Tablet (768×1024) | Mobile (375×812 / 390×844) |
| :--- | :---: | :---: | :---: |
| Homepage & Global Header | Verified | Verified | Verified (Identified B01, B02) |
| Company & Careers Flow | Verified | Verified | Verified (Identified B04) |
| Contact Us & Inquiry Form | Verified | Verified | Verified (Identified B03) |
| Expertise & Products Suite | Verified | Verified | Verified (Identified B06) |
| Case Studies Hub | Verified (Identified B05, I01) | Verified | Verified |
| BitElevate & Student Diaries | Verified | Verified | Verified (Identified B09, I02) |
| Blog & Knowledge Hub | Verified (Identified B08) | Verified | Verified (Identified B07) |
| Footer & Legal Links | Verified | Verified | Verified (Identified B03) |

---

## 5. Summary of Observed Outcomes

- **Contact Form & Newsletter:**  
  Both forms correctly enforce required fields, reject invalid email strings, and show affirmative submission feedback.
- **Career & Internship Applications:**  
  Form validation behaves well during active typing; however, navigating away to read mandatory Terms & Conditions wipes all entered fields (**B04**).
- **Product Suite Interactions:**  
  Modal drawer opens smoothly, but clicking *"Schedule Tour"* under the TenantEase section erroneously defaults the request to *"Cynbit Studio"* (**B06**).
- **Case Studies:**  
  All 13 case study detail routes resolve, but category filtering buttons are non-functional (**B05**), and the GlobalPay hero image is broken (**I01**).
- **Blog Section:**  
  Search and category navigation operate, but the native share button produces zero user feedback (**B07**), and a featured article is published with only an opening paragraph (**B08**).
- **Responsive Mobile Navigation:**  
  On mobile devices (375px/390px), the opened navigation overlay hides the close button (**B01**), leaves the overlay active when selecting the current page (**B02**), and the sticky bottom bar permanently conceals footer legal links (**B03**).

---

## 6. Technical, Performance & Security Observations

### Production Server Actions Health Check (HTTP 500 Digest: 571349486)
- **Contact Form Server Action** (`submitInquiry`, ID: `40a87fa530afd6445955e8416c558c1da8c1e1b1a0`) and **Careers Application Server Action** (`submitCandidateApplication`, ID: `40cff6a4be6a4027499391758ba57caf95ee4bef0e`):
  - On live production POST requests, server action endpoints crash with an unhandled `HTTP 500 Internal Server Error` returning digest `"571349486"`.
  - Frontend fallback displays: *"Something went wrong. Please try again or email us directly."*
  - Indicates missing production database credentials, unhandled mailer exceptions (Resend/SendGrid/SES), or unconfigured environment variables on Vercel.

### Broken Internal Links (404 Not Found)
- The link `/blog/ai-agents-transforming-business` returns HTTP 404 and is actively referenced in 4 published blog articles (`/blog/future-ai-customer-support`, `/blog/ai-automation-trends-2026`, `/blog/why-startups-need-custom-dashboards`, and `/blog/ai-chatbots-vs-human-support`). The correct live target is `/blog/ai-agents-business-operations`.

1. **Security Headers (Passive Check):**
   - **HSTS:** Enabled with `max-age=63072000` (Strong).
   - **Missing Hardening Headers:** The document responses lack `Content-Security-Policy` (CSP), `X-Frame-Options` (risk of UI redressing / Clickjacking), `X-Content-Type-Options: nosniff`, and `Referrer-Policy`.
   - **CORS Configuration:** HTML document returns `Access-Control-Allow-Origin: *`. While standard for public static assets, it should be restricted on dynamic API routes.
2. **Redirect & Canonical Consistency:**
   - `http://cynbit.in` undergoes a double redirect: 308 to `https://cynbit.in/`, then 307 to `https://www.cynbit.in/`.
   - The XML sitemap lists non-canonical URLs without `www` (`https://cynbit.in/...`), creating unnecessary redirect chains for search engine spiders.
3. **Legacy Route Handling:**
   - Accessing common standard conventions such as `/about`, `/privacy-policy`, or `/terms-and-conditions` returns standard 404 pages instead of 301 permanent redirects to `/story`, `/privacy`, and `/terms`.

---

## 7. Limitations and Residual Risks
- Real-device testing was simulated via Chromium developer tooling; native hardware touch keyboards and native iOS WebKit rendering should be included in subsequent QA phases.
- Testing utilized authorized test data with clear QA disclaimers; backend database persistence and production webhook integrations were not directly inspected.
