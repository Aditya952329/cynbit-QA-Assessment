# Cynbit Website Quality Audit — QA Testing Assessment

**Role Applied For:** QA Testing Intern  
**Company:** [Cynbit Technologies](https://www.cynbit.in/)  
**Candidate Name:** Aditya Raj  
**Assessment Task:** Phase 2 Technical Role Assessment — Cynbit Website Quality Audit  
**Submission Date:** October 2026  

---

## 📌 Project Overview
This repository contains the complete Quality Assurance audit for the Cynbit website (`https://cynbit.in` / `https://www.cynbit.in`), submitted as part of the technical assessment for the **QA Testing Intern** role.

The audit was conducted to analyze the platform from the perspective of an end-user, prospective corporate client, and student applicant. It identifies usability friction points, functional breakdowns, responsive layout defects, and passive security considerations.

---

## 📂 Assessment Deliverables

All required deliverables have been structured into dedicated reports inside the [`reports/`](reports/) directory:

| Deliverable | Document Link | Description & Key Contents |
| :--- | :--- | :--- |
| **Deliverable 1** | [**QA Audit Report**](reports/01-qa-audit-report.md) | Comprehensive report detailing the testing scope, exploratory and scenario-based methodology, test environment matrix (desktop, tablet, mobile viewports), observed outcomes, and risk limitations. |
| **Deliverable 2** | [**Bug & Issue Report**](reports/02-bug-and-issue-report.md) | Structured bug tracking document detailing **9 confirmed functional & usability defects** and **2 visual/copy flags**, complete with reproduction steps, expected vs. actual behavior, severity/priority, and acceptance criteria. |
| **Deliverable 3** | [**Improvement Recommendations**](reports/03-improvement-recommendations.md) | Prioritized engineering action plan (P1 immediate blockers to P3 polish), security/infrastructure recommendations (CSP, X-Frame-Options, sitemap cleanup), and an automated end-to-end regression testing blueprint. |

---

## 🔍 Key Findings At A Glance

### Summary of Logged Bugs
* **B01 (P1 - High):** Mobile navigation drawer completely conceals its own close control, trapping touch users inside the menu.
* **B02 (P2 - Medium):** Re-selecting the currently active page from the mobile menu leaves the overlay open.
* **B03 (P2 - Medium):** Fixed mobile bottom navigation bar permanently overlaps footer copyright and legal links.
* **B04 (P1 - High):** Navigating to view Terms & Conditions during a career application reloads the page and clears all candidate inputs (Data Loss).
* **B05 (P2 - Medium):** Category filter tabs on the Case Studies hub do not filter project cards.
* **B06 (P2 - Medium):** TenantEase *"Schedule Tour"* CTA incorrectly opens the demo request with *"Cynbit Studio"* pre-selected.
* **B07 (P2 - Medium):** Blog article share icon changes color but fails to copy the link or display feedback.
* **B08 (P2 - Medium):** A featured 12-minute article contains only a single unfinished paragraph before the author card.
* **B09 (P2 - Medium):** BitElevate internship submission displays a success banner stating a confirmation email was sent, but no email is received.
* **B10 (P1 - High):** Contact Inquiry and Career application forms trigger an unhandled Next.js Server Action crash (`HTTP 500`, digest `571349486`) on production submission.
* **B11 (P2 - Medium):** Internal 404 broken link to `/blog/ai-agents-transforming-business` referenced across 4 published blog articles (valid slug is `/blog/ai-agents-business-operations`).
* **I01 (P2 - Medium):** GlobalPay case study hero image fails to load, leaving an empty box with alt text.
* **I02 (P3 - Low):** Student Diaries hero heading contains a spelling error: *"Unheared Stories"*.

### Technical & Infrastructure Observations
* **Missing Security Hardening Headers:** Missing `Content-Security-Policy`, `X-Frame-Options` (Clickjacking risk), and `X-Content-Type-Options: nosniff`.
* **Redirect Chains:** `http://cynbit.in` triggers a 308 redirect to `https://cynbit.in/`, which then issues a 307 redirect to `https://www.cynbit.in/`.
* **Sitemap Host Discrepancy:** The `sitemap.xml` specifies apex URLs (`https://cynbit.in/...`) rather than canonical `www` URLs.

---

## 💻 Test Environment Matrix

Testing was performed across multiple viewports and form factors to simulate real-world usage:

- **Desktop Form Factor:** 1920×1080 (FHD) & 1440×900 (Chrome / Chromium Core on Windows x64).
- **Tablet Form Factor:** 768×1024 (iPad Portrait & Landscape viewports).
- **Mobile Form Factor:** 375×812 (iPhone SE), 390×844 (iPhone 12/13/14), and 360×640 (Android small screen).
- **Network Profiles:** Unthrottled High-Speed Fiber & Emulated Fast 4G.

---

## 🛠 Directory Structure

```
cynbit-qa-assessment/
│
├── README.md                                  # Repository overview and summary
│
└── reports/
    ├── 01-qa-audit-report.md                  # Deliverable 1: Methodology, scope & matrix
    ├── 02-bug-and-issue-report.md             # Deliverable 2: Structured bug logs (B01-B09, I01-I02)
    └── 03-improvement-recommendations.md      # Deliverable 3: Prioritized dev fixes & test suite
```

---

## ✍️ Author & Submission Information
- **Applicant:** Aditya Raj
- **Target Role:** QA Testing Intern
