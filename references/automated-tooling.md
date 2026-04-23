# Automated Tooling Integration

Complement agent-driven code reviews with automated accessibility testing. Automation catches ~57% of WCAG issues; the remaining ~43% requires manual or contextual review (which is where this skill operates).

## Recommended Tools

### Linter-Time (Fastest Feedback)

**Biome** — This project uses Biome, not ESLint. Biome has built-in accessibility lint rules under `a11y/` that cover many jsx-a11y equivalents. Ensure the `a11y` rules are enabled in `biome.json`.

Key Biome a11y rules:
- `useAltText` — img elements must have alt
- `useButtonType` — button elements must have type
- `useHtmlLang` — html element must have lang
- `useValidAriaRole` — ARIA roles must be valid
- `noAriaHiddenOnFocusable` — focusable elements must not have aria-hidden
- `noNoninteractiveTabindex` — no tabindex on non-interactive elements
- `useKeyWithClickEvents` — onClick must have onKeyDown/onKeyUp
- `useSemanticElements` — prefer semantic HTML over ARIA roles
- `noAccessKey` — avoid accessKey attribute
- `useAriaPropsForRole` — required ARIA props present for roles
- `useValidAriaValues` — ARIA attribute values must be valid

### Component Testing (Vitest + Testing Library)

**@testing-library/jest-dom** + custom matchers for accessibility assertions:

```tsx
import { render, screen } from '@testing-library/react';

it('has accessible form labels', () => {
  render(<CheckoutForm />);
  expect(screen.getByRole('textbox', { name: /email/i })).toBeInTheDocument();
  expect(screen.getByRole('textbox', { name: /email/i })).toHaveAttribute('autocomplete', 'email');
});

it('announces cart update', () => {
  render(<CartButton />);
  const liveRegion = screen.getByRole('status');
  expect(liveRegion).toBeInTheDocument();
});
```

**vitest-axe** — Run axe-core in component tests:

```tsx
import { axe, toHaveNoViolations } from 'vitest-axe';
expect.extend(toHaveNoViolations);

it('has no accessibility violations', async () => {
  const { container } = render(<ProductCard product={mockProduct} />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### CI/CD Pipeline

**axe-core/cli** or **pa11y-ci** — headless accessibility scanning on build:

```yaml
# Example CI step
- name: Accessibility audit
  run: npx @axe-core/cli --exit http://localhost:3000
```

**Lighthouse CI** — includes accessibility score (aim for 90+):

```yaml
- name: Lighthouse CI
  run: npx lhci autorun --collect.url=http://localhost:3000
  env:
    LHCI_ASSERT_ACCESSIBILITY_MIN_SCORE: 0.9
```

### Browser Extensions (QA / Manual Testing)

| Tool | Best For |
|------|----------|
| **IBM Equal Access Checker** | Most comprehensive rule set; maps to WCAG 2.2, US 508, EN 301 549. Multi-scan for full page coverage. |
| **axe DevTools** (Deque) | Quick scans during development; good guided testing for manual checks |
| **WAVE** (WebAIM) | Visual overlay showing errors in context on the page |
| **Lighthouse** (Chrome DevTools) | Quick audit; included in Chrome; good for performance + a11y together |

### Screen Reader Testing

Manual testing with actual screen readers catches issues automation misses:

| Screen Reader | OS | Browser | When to Use |
|---------------|-----|---------|-------------|
| **VoiceOver** | macOS/iOS | Safari | Primary for Mac-based QA |
| **NVDA** | Windows | Firefox/Chrome | Primary for Windows QA (free) |
| **JAWS** | Windows | Chrome/Edge | Enterprise / government compliance |
| **TalkBack** | Android | Chrome | Mobile testing |

**Minimum screen reader test checklist:**
1. Navigate by headings — logical hierarchy?
2. Navigate by landmarks — all regions labeled?
3. Tab through all interactive elements — logical order?
4. Activate every form — labels announced? Errors announced?
5. Complete a purchase flow — every step announced and operable?

## What Automation Catches vs What It Misses

| Automation Catches (~57%) | Requires Manual/Contextual Review (~43%) |
|---------------------------|------------------------------------------|
| Missing alt attributes | Alt text quality/meaningfulness |
| Missing form labels | Label clarity and helpfulness |
| Low contrast ratios | Contrast on dynamic states (hover, etc.) |
| Missing lang attribute | Correct lang value for content |
| Missing ARIA roles | Correct ARIA role selection |
| Duplicate IDs | Logical reading order |
| Empty buttons/links | Link/button text clarity |
| Keyboard trap detection (partial) | Full keyboard flow testing |
| Missing autocomplete | Correct autocomplete token selection |
| Invalid ARIA values | Meaningful live region announcements |

## W3C ACT Rules Reference

The [W3C ACT-Rules Community](https://www.w3.org/WAI/standards-guidelines/act/) publishes standardized test rules with clear pass/fail conditions. IBM's Equal Access Checker harmonizes with these rules. Key ACT rule IDs for ecommerce:

| ACT Rule | WCAG Criterion | What It Tests |
|----------|---------------|---------------|
| `23a2a8` | 1.1.1 | Image has accessible name |
| `bc4a75` | 1.1.1 | Image button has accessible name |
| `e086e5` | 1.3.1 | Form field has accessible name |
| `307n5z` | 1.3.1 | Heading is descriptive |
| `afw4f7` | 1.3.4 | Meta viewport does not lock orientation |
| `b33eff` | 1.4.3 | Text has minimum contrast |
| `09o5cg` | 1.4.11 | Non-text element has minimum contrast |
| `0ssw9k` | 2.1.1 | Element with role is focusable |
| `ebe86a` | 2.1.2 | No keyboard trap |
| `c3232f` | 2.4.4 | Link has accessible name |
| `e6952f` | 2.5.3 | Accessible name contains visible text |
| `674b10` | 3.1.1 | HTML page has lang attribute |
| `5b7ae0` | 3.1.1 | HTML page lang value is valid |
| `ucwvc8` | 3.1.2 | HTML element language is valid |
| `4e8ab6` | 4.1.2 | Element with role has required states/properties |

Cross-reference these ACT rule IDs when automated tooling flags a violation — it provides a standardized, verifiable test definition.
