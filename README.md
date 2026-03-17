# WCAG Enforcer

An AI agent skill that enforces WCAG 2.2 AA accessibility compliance during frontend development. It reviews code, catches violations, explains why they matter, provides compliant fixes, and maps every issue to specific WCAG success criteria — so your team ships accessible code by default instead of retrofitting after a lawsuit.

---

## The Problem

- **94.8%** of websites have detectable WCAG failures (WebAIM Million 2025)
- **5,114** ADA accessibility lawsuits filed in 2025 — ecommerce accounts for **70%** of cases
- Only **11%** of ecommerce cart and checkout pages meet minimum WCAG standards
- The average lawsuit settlement ranges from **$25,000–$75,000**, not including remediation costs
- The **European Accessibility Act (EAA)** is now enforceable as of June 2025
- The **DOJ Title II deadline** (April 2026) requires WCAG 2.1 AA for government digital services

Retrofitting accessibility is expensive. Catching violations during development costs a fraction of what it costs after a demand letter arrives.

---

## What This Skill Does

The WCAG Enforcer acts as an automated compliance layer that sits inside your AI-assisted development workflow. When activated, it systematically reviews frontend code against all **55 WCAG 2.2 AA success criteria** (30 Level A + 25 Level AA) and produces actionable, developer-friendly output.

### Three Operating Modes

| Mode | When It Activates | What It Does |
|------|-------------------|--------------|
| **Code Review** (default) | User provides existing code for review | Scans for violations, reports each with criterion ID, severity, explanation, and corrected code |
| **Build Guidance** | User asks to build something new | Produces WCAG-compliant code from the start, annotated with criteria comments |
| **Audit** | User requests a full page or flow review | Structured audit report prioritized by severity and lawsuit risk |

### What It Catches

The skill checks for the **Big 6** violations that account for the vast majority of lawsuits and audit failures, plus the full set of Level A and AA criteria:

1. **Text Alternatives** — Missing or vague alt text, unlabeled icon buttons, SVGs without titles
2. **Color Contrast** — Text below 4.5:1, UI components below 3:1, color-only information
3. **Keyboard Accessibility** — `<div onClick>` anti-patterns, keyboard traps, missing key handlers
4. **Form Labels** — Placeholder-as-label, missing `<label>` association, unlabeled required fields
5. **Link & Button Purpose** — Empty links, "Click here" text, custom controls without ARIA
6. **Document Structure** — Missing `lang` attribute, broken heading hierarchy, no skip navigation

Beyond the Big 6, it covers all remaining criteria including the 6 new additions in WCAG 2.2: Focus Not Obscured, Dragging Movements, Target Size Minimum, Consistent Help, Redundant Entry, and Accessible Authentication.

---

## Skill Architecture

```
wcag-enforcer/
├── SKILL.md                              # Core skill (194 lines)
│   ├── Operating modes & workflow
│   ├── Big 6 compliance checklist
│   ├── React/Next.js specific checks
│   ├── Ecommerce specific checks
│   ├── Output format template
│   └── Severity classification system
│
└── references/                           # Loaded on demand based on context
    ├── criteria-level-a.md               # All 30 Level A criteria as actionable checks
    ├── criteria-level-aa.md              # All 25 Level AA criteria (includes WCAG 2.2)
    ├── react-nextjs-patterns.md          # JSX rules, Next.js features, component patterns
    ├── ecommerce-patterns.md             # Shopping journey patterns, lawsuit priority ranking
    └── html-css-patterns.md              # Vanilla HTML/CSS/JS fallback patterns
```

The skill uses **progressive disclosure** — the SKILL.md is always loaded (~194 lines), and reference files are loaded only when relevant to the code being reviewed. A React checkout component pulls in the criteria files + react-nextjs-patterns + ecommerce-patterns. A vanilla HTML form only pulls in the criteria files + html-css-patterns.

---

## Coverage

### Standards

| Standard | Coverage |
|----------|----------|
| WCAG 2.2 Level A | All 30 criteria |
| WCAG 2.2 Level AA | All 25 criteria |
| WCAG 2.2 new additions | All 6 AA criteria (2.4.11, 2.5.7, 2.5.8, 3.2.6, 3.3.7, 3.3.8) |
| WCAG 2.1 backward compatibility | Full — 2.2 is a superset of 2.1 |

### Frameworks

| Framework | Support Level |
|-----------|--------------|
| React / JSX / TSX | Primary — dedicated reference file with component patterns |
| Next.js (App Router) | Primary — covers route announcer, Server/Client components, next/image, next/link |
| Base UI | Default component library recommendation for new projects |
| Radix / React Aria / Headless UI | Supported — continue if already adopted in project |
| Vanilla HTML / CSS / JS | Full — dedicated reference file as universal fallback |

### Domains

| Domain | Support Level |
|--------|--------------|
| Ecommerce (general) | Deep — full shopping journey mapped to WCAG criteria |
| Shopify | Covered — theme/app accessibility notes |
| Salesforce Commerce Cloud | Covered — SFRA and headless architecture notes |
| Headless / composable commerce | Covered — developer responsibility model documented |

---

## Output Format

Every review follows a consistent structure:

```
## Accessibility Review: [Component/Page Name]

### Critical Issues (Must Fix)
Issues that block users from completing tasks

### Serious Issues
Issues that create significant barriers

### Moderate Issues
Issues that degrade the experience

### Minor Issues
Enhancement opportunities

### Manual Testing Required
Things automation can't verify — needs screen reader or keyboard testing

### Compliance Summary
- Level A: X/30 criteria applicable, Y issues found
- Level AA: X/20 criteria applicable, Y issues found
- Overall: Compliant / Non-Compliant / Partially Compliant
- Top priority fix: Single most impactful remediation
```

Each issue includes:
- **Criterion ID and Name** (e.g., "1.4.3 Contrast Minimum")
- **Level** (A or AA)
- **Severity** (Critical / Serious / Moderate / Minor)
- **What's wrong** — plain-English explanation
- **Why it matters** — who is affected and legal risk context
- **Fix** — corrected code snippet

---

## Severity System

| Severity | Definition | Lawsuit Risk | Examples |
|----------|-----------|-------------|---------|
| **Critical** | User cannot complete the intended task | Very High — primary basis for most filings | Keyboard trap in checkout, form fields without labels in payment, no alt on product images |
| **Serious** | User faces significant barrier but may find workaround | High — frequently cited in complaints | Missing focus indicators, low contrast on CTAs, empty link text |
| **Moderate** | User experience is degraded | Medium — contributes to pattern of non-compliance | Heading hierarchy skipped, missing document language, decorative images not hidden |
| **Minor** | Best practice improvement | Low — unlikely to trigger standalone action | Advisory ARIA improvements, enhanced focus styling, supplementary landmarks |

Severity is informed by lawsuit filing data — the skill prioritizes the violations that actually trigger legal action, not just theoretical non-compliance.

---

## Example Usage

### Reviewing a Component

**Prompt:**
> Review this ProductCard component for accessibility:
> ```tsx
> export function ProductCard({ product }) {
>   return (
>     <div onClick={() => navigate(product.url)}>
>       <img src={product.image} />
>       <div style={{ color: '#999' }}>{product.name}</div>
>       <a href="#">Read more</a>
>     </div>
>   );
> }
> ```

**The skill will identify:**
- Missing `alt` on `<img>` (1.1.1 — Critical)
- `<div onClick>` instead of `<a>` or `<button>` (2.1.1 — Critical)
- Likely contrast failure with `#999` on white (1.4.3 — Serious)
- Empty link purpose on "Read more" (2.4.4 — Serious)
- No semantic structure (1.3.1 — Moderate)

...and provide corrected code for each.

### Building Something New

**Prompt:**
> Build me an accessible product variant selector with color swatches and size buttons for a Next.js ecommerce PDP.

**The skill will produce** WCAG-compliant code using Base UI components (or the project's existing component library if one is already adopted), with text labels on color swatches (not color-only), `aria-pressed` state management, keyboard navigation, and live region announcements for selection changes — annotated with the criteria each pattern satisfies.

### Auditing a Flow

**Prompt:**
> Audit our checkout flow for WCAG compliance. Here are the components: [shipping form, payment form, order review, confirmation page]

**The skill will produce** a structured audit report covering every component, prioritized by the ecommerce violation frequency data (checkout is the highest-risk area — 89% failure rate).

---

## Research Foundation

This skill was built from a structured 7-exercise deep research process covering ~100 sources across:

| Exercise | Focus | Key Sources |
|----------|-------|-------------|
| 1 — Full Ruleset | All 50+ WCAG 2.1/2.2 AA criteria | W3C spec, WebAIM, Deque University |
| 2 — Legal Landscape | Lawsuit data, regulations, financial impact | EcomBack, UsableNet, Seyfarth Shaw, DarrowEverett |
| 3 — React/Next.js | Framework-specific implementation patterns | React docs, Next.js docs, React Aria, Radix UI |
| 4 — Ecommerce | Shopping journey accessibility patterns | Baymard, TestParty, DigitalA11y, Practical Ecommerce |
| 5 — HTML/CSS/JS | Vanilla fallback patterns | MDN, W3C WAI, A11y Project |
| 6 — Testing & Tooling | Automation, CI/CD, manual protocols | axe-core, Playwright, Storybook, Lighthouse |
| 7 — Future-Proofing | WCAG 2.2 additions, WCAG 3.0 direction | W3C WCAG 3 Working Draft, AbilityNet, AudioEye |

The research deliverables (7 markdown files, ~3,800 lines, ~144KB) were distilled into the 5 reference files that power the skill.

---

## Limitations

This skill is a development-time compliance layer, not a complete accessibility solution. Important caveats:

- **Automated detection covers ~57% of WCAG issues.** The skill pushes this higher through contextual pattern matching, but some issues require manual testing — which is why every review includes a "Manual Testing Required" section.
- **Cannot verify visual design.** The skill works with code, not rendered pixels. Actual contrast ratios should be verified with tools like WebAIM Contrast Checker against final rendered output.
- **Cannot replace screen reader testing.** The skill flags where live regions, focus management, and ARIA states are needed, but real screen reader testing (NVDA + Firefox, VoiceOver + Safari) is required to validate the user experience.
- **Cannot audit third-party widgets.** The skill flags third-party integrations (review widgets, chat, payment forms) as requiring manual testing, but can't inspect their internal code.
- **Not legal advice.** This skill provides technical accessibility guidance based on WCAG standards. Consult legal counsel for compliance obligations specific to your jurisdiction.

---

## Recommended Companion Tooling

The skill works best as part of a layered accessibility strategy:

| Layer | Tool | What It Catches |
|-------|------|----------------|
| Linting (static) | `eslint-plugin-jsx-a11y` | Basic JSX violations before code runs |
| Unit testing | `jest-axe` + `@testing-library/react` | Component-level ARIA and structure |
| Component dev | `@storybook/addon-a11y` | Per-story accessibility auditing |
| E2E testing | `@axe-core/playwright` | Full-page violations in real browser |
| Browser audit | axe DevTools extension, Lighthouse | Quick manual page audits |
| **Development** | **wcag-enforcer skill** | **Contextual review during code authoring** |
| Manual | Keyboard + screen reader testing | The ~43% that automation misses |

---

## Installation

Download the `wcag-enforcer.skill` file and install it in your Claude environment. The skill will automatically trigger when you're working with frontend code and accessibility is relevant.

For manual installation, copy the `wcag-enforcer/` directory to your skills location with the following structure:

```
wcag-enforcer/
├── SKILL.md
└── references/
    ├── criteria-level-a.md
    ├── criteria-level-aa.md
    ├── react-nextjs-patterns.md
    ├── ecommerce-patterns.md
    └── html-css-patterns.md
```

---

## Versioning

- **Current standard:** WCAG 2.2 AA (W3C Recommendation, October 2023)
- **U.S. legal baseline:** WCAG 2.1 AA (ADA / DOJ Title II)
- **EU standard:** EN 301 549 referencing WCAG 2.1 AA, expected to adopt 2.2
- **WCAG 3.0 status:** Working Draft (March 2026), Candidate Recommendation expected Q4 2027, final not before 2028

The skill is built to WCAG 2.2 AA, which is a superset of 2.1. It covers all current legal requirements and is architecturally forward-compatible with WCAG 3.0's outcome-based direction.

---

Powered by 7 research exercises, ~100 sources, and the belief that accessibility should be built in from the first line of code — not bolted on after the lawsuit.
