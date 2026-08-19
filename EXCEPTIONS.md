# A11y Exceptions Log

This document logs known deviations from accessibility standards (WCAG 2.2 AA / EN 301 549) that have been temporarily accepted.

> **Objective:** Provide technical and legal transparency by documenting *where*, *why*, and *how* we temporarily mitigate guidelines that could not be met due to technical, platform, or scope limitations.

> **Rules:**
> 1. An exception is **temporary** and does not change the requirement.
> 2. Every exception MUST have a **risk owner**, an **approver**, a **tracking issue**, and an **expiry date** — "dependent on third-party" still gets a review date.
> 3. Scope is the **narrowest practical**: one component/selector, never a whole rule.
> 4. At expiry, the exception is reviewed: fixed and removed, or consciously renewed with a new date. **Never silently suppressed.**
> 5. **AI duty:** in review mode, the AI MUST flag any exception past its expiry date as 🟠 HIGH technical debt.
> 6. This log is a **versioned project record** — never add it to `.gitignore`. Exceptions must be visible in pull requests and auditable later; a risk record hidden from version control protects no one.

**Compliance profile in force:** ⚖️ Standard (AA) — confirmed by the repository owner, 2026-08-19.

---

## 🛑 Exception Log

### 1. Basic Details
- **Exception ID:** EXT-2026-001
- **Component / Page:** Decorative looping animations on both pages —
  - `/` (Accidental Intelligence): `.stripe-run` hazard-stripe conveyor (×2), `.cue-arrow` scroll-cue bounce
  - `/desker/`: `.p-folder` / `.p-clock` / `.p-notes` float loops (`fl`), `cursor-breathe`, `ripple-idle`, and the `#dwave` hero canvas animation
- **WCAG Guideline Affected:** 2.2.2 Pause, Stop, Hide (Level A)
- **Severity (User Impact):** 🟠 HIGH — perception/comfort failure. Continuous unstoppable motion is a vestibular trigger (nausea, dizziness) and a sustained distraction for users with attention- or cognition-related disabilities. Not classified 🔴 CRITICAL because no task on either page depends on the animated elements: every link, control and block of copy remains fully operable and readable while the motion runs.
- **Risk Owner:** Sam — repository owner and sole developer (`SamSDK`). *A formal legal name will be required if this log is ever submitted for external audit or a VPAT.*
- **Approved by:** Sam — repository owner, decision taken 2026-08-19 ("leave them be") after the deviation and its remedies were presented.
- **Tracking Issue:** ⚠️ **Not filed.** No issue tracker is configured on `SamSDK/accidentalintelligenceinc-site`. This is itself a gap against Rule 2 of this log — the exception is recorded, but the debt is not yet chased anywhere. **Action: open a GitHub issue on this repo and link it here.**

### 2. Technical Blockade Description
- **What is broken?** Both pages run decorative animations that start automatically, loop indefinitely (well beyond the 5-second threshold) and play alongside the page's content. SC 2.2.2 requires that such motion offer a mechanism to **pause, stop or hide** it. No such in-page mechanism exists on either page, so a user who needs the motion to stop — but who has not configured an operating-system preference — has no way to stop it.
- **Why did it happen?** This is **not** a technical blockade. Three conforming remedies were offered and priced (a persistent motion toggle in the header; making each loop finite so it never crosses 5 seconds; removing the loops entirely). The owner elected to keep the animations as-is because the perpetual motion — particularly the crawling hazard stripe — carries the brand's identity. This exception therefore records an **accepted product risk**, not an engineering limitation. It is the weakest category of exception and should be treated as the first candidate for repayment.

### 3. Workaround (Fallback / Remediation)
- **How can the user still complete the task?** No task is blocked: the motion is decorative and sits behind or beside content that stays fully readable and operable throughout. The real mitigation is the operating-system preference:
  - **`/desker/`** honours `prefers-reduced-motion: reduce` globally (`*{animation:none!important;transition:none!important}`), so **every** animation on that page — floats, cursor loops and the `#dwave` canvas — stops completely. The canvas additionally pauses when the hero scrolls out of view or the tab is hidden.
  - **`/`** honours the same preference for the named animations (`.stripe-run`, `.cue-arrow`, hero entrance) and renders all scroll-revealed content immediately.
- **Why this is a mitigation and not conformance:** SC 2.2.2 asks for a mechanism *in the content*. An OS-level preference is neither discoverable from the page nor available to a user on a shared, locked-down or kiosk machine, and many users do not know the setting exists. Users who have set the preference are fully served; users who have not are not served at all.

### 4. Resolution Plan and Expiry
- **Expiry (review-by date):** **2026-11-19** (three months). To be reviewed earlier if it arrives first: **the Desker public launch**, since that is when the site starts carrying real acquisition traffic and the audience stops being the developer.
- **Resolution Criterion:** This exception is closed when either (a) a persistent, keyboard-operable motion toggle exists in the header of both pages, stopping all looping motion and persisting the choice across pages and reloads, or (b) every loop listed above is finite and completes within 5 seconds of starting. Whichever ships, `REPORT.md` must be regenerated and this entry deleted rather than renewed.

---
*Blank copy (paste below as you create new exceptions):*

### 1. Basic Details
- **Exception ID:** 
- **Component / Page:** 
- **WCAG Guideline Affected:** 
- **Severity:** 
- **Risk Owner:** 
- **Approved by:** 
- **Tracking Issue:** 

### 2. Technical Blockade Description
- **What is broken?** 
- **Why did it happen?** 

### 3. Workaround (Fallback / Remediation)
- **How can the user still complete the task?** 

### 4. Resolution Plan and Expiry
- **Expiry (review-by date):** 
- **Resolution Criterion:** 
