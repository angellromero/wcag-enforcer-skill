---
name: wcag-enforcer
description: "Enforce WCAG 2.2 AA accessibility compliance during frontend development. Use whenever writing, reviewing, or modifying frontend code (React, Next.js, HTML, CSS, JSX, TSX). Triggers: building UI components, pages, forms, navigation, modals, product pages, checkout flows, PR reviews for accessibility, ARIA attributes, focus management, keyboard navigation, screen reader support, color contrast, alt text, or any mention of WCAG, a11y, accessibility, ADA, Section 508, EAA. Also trigger to audit or fix accessibility in existing code. ALWAYS use for ecommerce frontends (Shopify, SFCC, headless commerce, cart, checkout) — ecommerce is the #1 lawsuit target. Acts as an automated compliance layer catching Level A and AA violations before code ships."
---

# WCAG Enforcer

An accessibility compliance agent that reviews frontend code against WCAG 2.2 AA standards during development. It catches violations, explains why they matter, provides compliant code fixes, and maps every issue to specific WCAG success criteria.

## Why This Exists

- 94.8% of websites have detectable WCAG failures
- 5,114 ADA accessibility lawsuits filed in 2025; ecommerce = 70% of cases
- Only 11% of ecommerce cart/checkout pages meet minimum WCAG standards
- Automated tools catch ~57% of issues; this skill combines automated pattern detection with contextual guidance to push that number higher during development
- The European Accessibility Act (EAA) is now enforceable; the DOJ Title II deadline hits April 2026

## How This Skill Works

When activated, the skill operates in one of three modes depending on what the user needs:

### Mode 1: Code Review (Default)

The user provides code (component, page, partial). The skill:

1. Scans for WCAG 2.2 AA violations using the criteria checklists in `references/`
2. Reports each violation with:
   - **Criterion:** The specific WCAG criterion violated (e.g., "1.4.3 Contrast Minimum")
   - **Level:** A or AA
   - **Severity:** Critical (blocks user task) / Serious (significant barrier) / Moderate (degraded experience) / Minor (enhancement)
   - **What's wrong:** Plain-English explanation
   - **Why it matters:** Who is affected and legal risk context
   - **Fix:** Corrected code snippet
3. Provides a compliance summary: total issues by severity, criteria coverage, and an overall assessment

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
| `references/ecommerce-patterns.md` | When reviewing ecommerce components (PDP, cart, checkout, filters, search) |
| `references/html-css-patterns.md` | When reviewing vanilla HTML/CSS/JS or as fallback reference |

**Important:** Always read both criteria files (Level A + AA). Then read the framework/domain files relevant to the code at hand.

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

## React/Next.js Specific Checks

When the code is React/JSX/TSX:

- `htmlFor` used instead of `for` on labels
- `aria-*` attributes in hyphen-case (not camelCase)
- Dynamic content updates use `aria-live` regions
- Route changes are announced (Next.js route announcer + `document.title`)
- Modals trap focus, dismiss on Escape, return focus on close
- State changes reflected in ARIA (`aria-expanded`, `aria-pressed`, `aria-selected`)
- `eslint-plugin-jsx-a11y` rules would pass
- Accessible component library used for complex widgets (Base UI for new projects; Radix, React Aria, Headless UI if already adopted)
- No `<div onClick>` anti-patterns — use `<button>` for actions
- No positive `tabIndex` values
- Focus outlines not removed without `:focus-visible` replacement

## Ecommerce Specific Checks

When the code involves ecommerce flows:

- Product images have descriptive alt text (not "product.jpg")
- Color/variant selectors have text labels (not color-only)
- Add to Cart success/failure announced via live regions
- Cart updates announced to screen readers
- Checkout form fields all labeled with `autocomplete` attributes
- Price changes on variant selection announced
- Third-party widgets flagged for manual accessibility testing
- Error prevention on financial transactions (review step before submit)

## Output Format

Structure every review response like this:

```
## Accessibility Review: [Component/Page Name]

### Critical Issues (Must Fix)
[Issues that block users from completing tasks]

### Serious Issues
[Issues that create significant barriers]

### Moderate Issues
[Issues that degrade the experience]

### Minor Issues
[Enhancement opportunities]

### Manual Testing Required
[Things automation can't verify — needs screen reader or keyboard testing]

### Compliance Summary
- Level A: [X/30 criteria applicable, Y issues found]
- Level AA: [X/20 criteria applicable, Y issues found]
- Overall: [Compliant / Non-Compliant / Partially Compliant]
- Top priority fix: [Single most impactful remediation]
```

For each issue:
```
**[Criterion ID] [Criterion Name]** (Level [A/AA]) — [Severity]
What's wrong: [Plain English]
Why it matters: [Who is affected + legal context]
Fix:
```
Then provide the corrected code.

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
