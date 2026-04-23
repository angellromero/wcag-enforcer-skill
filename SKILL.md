---
name: wcag-enforcer
description: "Enforce WCAG 2.2 AA accessibility compliance during frontend development. Use whenever writing, reviewing, or modifying frontend code (React, Next.js, HTML, CSS, JSX, TSX). Triggers: building UI components, pages, forms, navigation, modals, product pages, checkout flows, PR reviews for accessibility, ARIA attributes, focus management, keyboard navigation, screen reader support, color contrast, alt text, or any mention of WCAG, a11y, accessibility, ADA, Section 508, EAA. Also trigger to audit or fix accessibility in existing code. ALWAYS use for ecommerce frontends (Shopify, SFCC, headless commerce, cart, checkout) — ecommerce is the #1 lawsuit target. Acts as an automated compliance layer catching Level A and AA violations before code ships."
---

# WCAG Enforcer

An accessibility compliance agent that reviews frontend code against WCAG 2.2 AA standards during development. It catches violations, explains why they matter, provides compliant code fixes, and maps every issue to specific WCAG success criteria.

## Why This Exists

- 96% of home pages have detectable WCAG failures (WebAIM Million 2026)
- ADA accessibility lawsuits continue to rise year-over-year; ecommerce remains ~70% of cases
- Only 11% of ecommerce cart/checkout pages meet minimum WCAG standards
- Automated tools catch ~57% of issues; this skill combines automated pattern detection with contextual guidance to push that number higher during development
- The European Accessibility Act (EAA) is enforceable since June 28, 2025; the DOJ Title II rule is effective as of April 2026 — both are now active
- EN 301 549 V3.2.1 (the EU harmonized standard) and US Section 508 both reference WCAG — violations trigger exposure under multiple regulations simultaneously

## How This Skill Works

When activated, the skill operates in one of three modes depending on what the user needs:

### Mode 1: Code Review (Default)

The user provides code (component, page, partial). The skill:

1. Scans for WCAG 2.2 AA violations using the criteria checklists in `references/`
2. Reports each violation with:
   - **Criterion:** The specific WCAG criterion violated (e.g., "1.4.3 Contrast Minimum")
   - **Level:** A or AA
   - **Regulations:** Which standards are violated (WCAG 2.2, US 508, EN 301 549) — see `references/regulatory-mapping.md`
   - **Severity:** Critical (blocks user task) / Serious (significant barrier) / Moderate (degraded experience) / Minor (enhancement)
   - **What's wrong:** Plain-English explanation
   - **Why it matters:** Who is affected and legal risk context
   - **Fix:** Corrected code snippet
3. Classifies flagged items as either definite violations, potential issues (need human verification), or manual testing items (require assistive technology)
4. Provides a compliance summary: total issues by severity, criteria coverage, and an overall assessment

### Mode 2: Build Guidance

The user is building something new (e.g., "build me a product card component"). The skill:

1. Reads the relevant reference files for the component type
2. Produces code that is WCAG 2.2 AA compliant from the start
3. Annotates the code with comments explaining which WCAG criteria each pattern satisfies
4. Calls out any areas that need manual testing (screen reader verification, meaningful alt text review)

### Mode 3: Audit

The user wants a full page or flow audit. The skill:

1. Reviews all provided code/files
2. Produces a structured audit report (see Output Format below)
3. Prioritizes by severity and lawsuit risk (using the ecommerce violation frequency data)
4. Recommends a remediation order

## Reference Files

Read the appropriate reference files based on the code being reviewed. You do NOT need to read all of them — pick the ones relevant to the task.

| File | When to Read |
|------|-------------|
| `references/criteria-level-a.md` | Always — Level A is the baseline for every review |
| `references/criteria-level-aa.md` | Always — Level AA is required for compliance |
| `references/react-nextjs-patterns.md` | When reviewing React/JSX/TSX/Next.js code |
| `references/ecommerce-patterns.md` | When reviewing ecommerce components (PDP, cart, checkout, filters, search, auth) |
| `references/html-css-patterns.md` | When reviewing vanilla HTML/CSS/JS or as fallback reference |
| `references/regulatory-mapping.md` | When reporting violations (maps WCAG to US 508, EN 301 549, ADA/EAA risk) |
| `references/automated-tooling.md` | When recommending testing tools, CI/CD integration, or ACT rule cross-references |

**Important:** Always read both criteria files (Level A + AA). Then read the framework/domain files relevant to the code at hand. Read regulatory-mapping.md when producing audit reports to include regulation references.

## The Compliance Checklist

When reviewing any frontend code, systematically check for these categories. These are the "Big 6" — the violations that appear in the vast majority of lawsuits and audits:

### 1. Text Alternatives (1.1.1)
- Every `<img>` has meaningful `alt` (or `alt=""` if decorative)
- SVG icons have `<title>` + `role="img"` or `aria-hidden="true"`
- Icon-only buttons have `aria-label`
- No CSS background images conveying important information

### 2. Color Contrast (1.4.3, 1.4.11)
- Text contrast ≥ 4.5:1 (normal) or ≥ 3:1 (large: ≥24px or ≥18.66px bold)
- UI components and focus indicators ≥ 3:1
- Information not conveyed by color alone (1.4.1)

### 3. Keyboard Accessibility (2.1.1, 2.1.2)
- All interactive elements reachable and operable via keyboard
- No keyboard traps
- Custom elements have proper key handlers (Enter, Space, Escape, Arrows)
- Native elements used where possible (`<button>`, `<a>`, `<input>`)

### 4. Form Labels (1.3.1, 3.3.2)
- Every input has an associated visible `<label>` (not placeholder-only)
- `htmlFor`/`for` attribute matches input `id`
- Required fields identified (`aria-required="true"` + visible indicator)
- Error messages specific, associated with fields (`aria-describedby`)

### 5. Link & Button Purpose (2.4.4, 4.1.2)
- No empty links or buttons (every interactive element has an accessible name)
- Link text is descriptive (not "Click here" or "Read more" without context)
- Custom controls have proper ARIA roles, states, and properties

### 6. Document Language & Structure (3.1.1, 1.3.1)
- `<html lang="...">` present
- Semantic HTML landmarks (`<header>`, `<nav>`, `<main>`, `<footer>`)
- Heading hierarchy logical (no skipped levels, one `<h1>`)
- Skip navigation link present

Beyond the Big 6, check the full criteria in the reference files for the complete set of Level A + AA requirements.

## Quick Wins — Top 10 Highest-Impact Fixes

When triaging an audit or building from scratch, these 10 checks yield the most compliance and lawsuit-risk reduction per fix. Each can typically be resolved in a single PR:

1. **Add `<html lang="...">`** (3.1.1) — one-line fix, one of the most common failures
2. **Add `alt` to all `<img>` elements** (1.1.1) — #1 ecommerce violation
3. **Add visible `<label>` to every form `<input>`** (3.3.2) — especially checkout fields
4. **Replace `<div onClick>` with `<button>`** (2.1.1) — keyboard access in one change
5. **Add skip navigation link** (2.4.1) — first focusable element, links to `<main>`
6. **Ensure `:focus-visible` outline present** (2.4.7) — never `outline: none` without replacement
7. **Check text contrast ≥ 4.5:1** (1.4.3) — use DevTools contrast checker on all text
8. **Add `autocomplete` to personal/payment fields** (1.3.5) — standard tokens on all checkout inputs
9. **Add `aria-live` for cart/status updates** (4.1.3) — announce dynamic changes to screen readers
10. **Use semantic headings `<h1>`–`<h6>`** (1.3.1) — no styled divs for headings

## Remediation Priority Tiers

When producing an audit report, group violations by remediation priority tier (based on IBM's "pace of completion" levels and lawsuit risk data). Each criterion in the reference files has a `**Priority:**` tag.

### Tier 1 — Fix Now (Highest Impact)
Legal exposure is highest. These are the violations most frequently cited in lawsuits and affect the most users.

| Criteria | Description |
|----------|------------|
| 1.1.1 | Non-text Content (alt text) |
| 1.2.2 | Captions (Prerecorded) |
| 1.3.1 | Info and Relationships (semantic structure) |
| 1.4.1 | Use of Color |
| 1.4.3 | Contrast (Minimum) |
| 1.4.10 | Reflow (320px viewport) |
| 1.4.11 | Non-text Contrast |
| 2.1.1 | Keyboard |
| 2.1.2 | No Keyboard Trap |
| 2.4.3 | Focus Order |
| 2.4.4 | Link Purpose |
| 2.4.6 | Headings and Labels |
| 2.4.7 | Focus Visible |
| 2.4.11 | Focus Not Obscured |
| 3.1.1 | Language of Page |
| 3.3.1 | Error Identification |
| 3.3.2 | Labels or Instructions |
| 4.1.2 | Name, Role, Value |
| 4.1.3 | Status Messages |

### Tier 2 — Fix Soon
Important for compliance but lower lawsuit frequency. Often caught during QA.

| Criteria | Description |
|----------|------------|
| 1.3.2, 1.3.4, 1.3.5 | Meaningful Sequence, Orientation, Input Purpose |
| 1.4.4, 1.4.5, 1.4.13 | Resize Text, Images of Text, Content on Hover/Focus |
| 2.2.2, 2.3.1, 2.4.1, 2.4.2 | Pause/Stop, Flashes, Bypass Blocks, Page Titled |
| 2.5.1, 2.5.3, 2.5.7, 2.5.8 | Pointer Gestures, Label in Name, Dragging, Target Size |
| 3.2.1, 3.2.2, 3.3.3, 3.3.4 | On Focus, On Input, Error Suggestion, Error Prevention |

### Tier 3 — Backlog
Important for full compliance but lower risk individually. Schedule these after Tier 1 and 2 are resolved.

| Criteria | Description |
|----------|------------|
| 1.2.1, 1.2.3, 1.2.4, 1.2.5 | Audio/Video alternatives |
| 1.3.3, 1.4.2, 1.4.12 | Sensory Characteristics, Audio Control, Text Spacing |
| 2.1.4, 2.2.1, 2.4.5, 2.5.2, 2.5.4 | Key Shortcuts, Timing, Multiple Ways, Pointer Cancel, Motion |
| 3.1.2, 3.2.3, 3.2.4, 3.2.6 | Language of Parts, Consistent Nav/ID/Help |
| 3.3.7, 3.3.8 | Redundant Entry, Accessible Authentication |

## React/Next.js Specific Checks

When the code is React/JSX/TSX:

- `htmlFor` used instead of `for` on labels
- `aria-*` attributes in hyphen-case (not camelCase)
- Dynamic content updates use `aria-live` regions
- Route changes are announced (Next.js route announcer + `document.title`)
- Modals trap focus, dismiss on Escape, return focus on close
- State changes reflected in ARIA (`aria-expanded`, `aria-pressed`, `aria-selected`)
- Biome a11y rules enabled (this project uses Biome, not ESLint)
- Accessible component library used for complex widgets (Base UI for new projects; Radix, React Aria, Headless UI if already adopted)
- No `<div onClick>` anti-patterns — use `<button>` for actions
- No positive `tabIndex` values
- Focus outlines not removed without `:focus-visible` replacement
- `@media (forced-colors: active)` support for custom controls and focus indicators
- `aria-label` must contain the visible text (2.5.3) — speech input users say what they see
- Password fields must not block paste — never use `onPaste` prevention (3.3.8)
- Fixed-height text containers use `min-height` to support text spacing overrides (1.4.12)

## Ecommerce Specific Checks

When the code involves ecommerce flows:

- Product images have descriptive alt text (not "product.jpg")
- Color/variant selectors have text labels (not color-only)
- Add to Cart success/failure announced via live regions
- Cart updates announced to screen readers
- Checkout form fields all labeled with `autocomplete` attributes (1.3.5)
- Price changes on variant selection announced
- Third-party widgets flagged for manual accessibility testing
- Error prevention on financial transactions (review step before submit) (3.3.4)
- "Billing same as shipping" checkbox — don't ask for same data twice (3.3.7)
- Login supports password managers and paste — no clipboard blocking (3.3.8)
- Help mechanisms (chat, contact) in consistent position across all pages (3.2.6)
- Sticky headers/footers/cookie banners don't obscure focused elements (2.4.11)
- Mega menus allow pointer to travel from trigger to content (1.4.13)

## Output Format

Structure every review response like this:

```
## Accessibility Review: [Component/Page Name]

### Critical Issues (Must Fix)
[Issues that block users from completing tasks — Tier 1 remediation priority]

### Serious Issues
[Issues that create significant barriers]

### Moderate Issues
[Issues that degrade the experience]

### Minor Issues
[Enhancement opportunities]

### Needs Human Verification (Potential)
[Code patterns flagged that may be violations — requires a developer to confirm.
Example: "alt text exists but may not be meaningful", "contrast appears close to 
threshold", "this ARIA pattern may not match the visual behavior"]

### Manual Testing Required
[Requires actual assistive technology testing — screen reader, keyboard flow, 
or real device testing. Cannot be verified from code alone.
Example: "screen reader announces cart updates correctly", "focus order feels 
logical through checkout flow", "mega menu is navigable with voice input"]

### Compliance Summary
- Level A: [X/30 criteria applicable, Y issues found]
- Level AA: [X/25 criteria applicable, Y issues found]
- Regulations: [WCAG 2.2 AA, US 508 §9.x.x, EN 301 549 §9.x.x — list applicable]
- Remediation priority: [Tier 1 count / Tier 2 count / Tier 3 count]
- Overall: [Compliant / Non-Compliant / Partially Compliant]
- Top priority fix: [Single most impactful remediation]
```

For each issue:
```
**[Criterion ID] [Criterion Name]** (Level [A/AA]) — [Severity]
Regulations: WCAG 2.2 §[X.X.X], US 508 §[9.X.X.X], EN 301 549 §[9.X.X.X]
What's wrong: [Plain English]
Why it matters: [Who is affected + legal context]
Fix:
```
Then provide the corrected code.

For audit reports (Mode 3), also include a **Remediation Roadmap** section:
```
### Remediation Roadmap
**Tier 1 (This Sprint):** [List of criterion IDs and brief fix descriptions]
**Tier 2 (Next Sprint):** [List]
**Tier 3 (Backlog):** [List]
```

## Key Principles

1. **Semantic HTML first.** If a native HTML element does the job, use it. ARIA is a supplement, not a replacement. The first rule of ARIA: don't use ARIA if you can use native HTML.

2. **The code user sees = the code screen reader sees.** If a sighted user sees a button, the screen reader must announce a button. If a sighted user sees a heading, the screen reader must find a heading.

3. **Every interactive element must be keyboard-operable.** If it responds to click, it must respond to Enter/Space. If it opens, Escape must close it.

4. **State changes must be communicated.** When the UI changes (cart updated, error appears, content loads), assistive technology must be notified via ARIA live regions or focus management.

5. **Design for the flow, not just the component.** A button might be perfectly accessible in isolation but break the experience in context if focus management is wrong.

## Severity Classification

| Severity | Definition | Examples |
|----------|-----------|---------|
| Critical | User cannot complete the intended task | Keyboard trap in checkout, form fields without labels in payment, no alt on product images |
| Serious | User faces significant barrier but may find workaround | Missing focus indicators, low contrast on CTAs, empty link text |
| Moderate | User experience is degraded | Heading hierarchy skipped, missing document language, decorative images not hidden |
| Minor | Best practice improvement | Advisory ARIA improvements, enhanced focus styling, supplementary landmarks |
