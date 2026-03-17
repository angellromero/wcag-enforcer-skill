# React / Next.js Accessibility Patterns

Actionable patterns for WCAG 2.2 AA compliance in React and Next.js projects.

## JSX Accessibility Rules

- Use `htmlFor` (not `for`) on `<label>` elements
- All `aria-*` attributes in hyphen-case: `aria-label`, `aria-expanded`, `aria-describedby`
- Use native elements: `<button>` for actions, `<a href>` for navigation, `<input>` for form fields
- Never: `<div onClick>` without `role`, `tabindex`, and `onKeyDown`
- Never: positive `tabIndex` values (only `0` or `-1`)
- Always: `alt` on `<Image>` components (Next.js Image enforces this in TypeScript)

## Next.js Built-in Features

**Route Announcer:** Next.js announces client-side route changes to screen readers by reading `document.title`, then `<h1>`, then pathname. Ensure every page has a unique, descriptive `<title>` and `<h1>`.

**ESLint:** `eslint-plugin-jsx-a11y` included in `next lint`. Extend with `plugin:jsx-a11y/recommended` for stricter checks.

**next/link:** Keyboard-navigable by default (Enter/Space). Works with `tabIndex`.

**next/image:** Requires `alt` prop.

## Component Patterns

### Skip Navigation
```tsx
<a href="#main-content" className="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:bg-white focus:px-4 focus:py-2">
  Skip to main content
</a>
// ... header ...
<main id="main-content" tabIndex={-1}>{children}</main>
```

### Labeled Form Field with Error
```tsx
<label htmlFor={id}>{label}</label>
<input id={id} aria-invalid={!!error} aria-describedby={error ? `${id}-error` : undefined} aria-required={required} autoComplete={autocompleteToken} />
{error && <p id={`${id}-error`} role="alert">{error}</p>}
```

### Modal / Dialog (Use Radix, React Aria, or Headless UI)
Requirements: focus trap inside, Escape closes, focus returns to trigger on close, `role="dialog"`, `aria-modal="true"`, `aria-labelledby` for title, background content inert.

### Live Region for Dynamic Updates
```tsx
<div role="status" aria-live="polite" aria-atomic="true" className="sr-only">
  {announcement}
</div>
```
Use `role="status"` for cart updates, search counts. Use `role="alert"` for errors.

### Tabs
Use Radix Tabs, React Aria Tabs, or Headless UI Tab. Keyboard: Arrow keys between tabs, Tab to panel, Enter/Space activates. Required ARIA: `role="tablist"`, `role="tab"`, `role="tabpanel"`, `aria-selected`.

### Visually Hidden (Screen Reader Only)
```css
.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border: 0; }
```

## Focus Management

**Route changes:** Next.js announces page title but doesn't move focus. For complex layouts, move focus to `<h1>` on navigation:
```tsx
useEffect(() => { document.querySelector('h1')?.focus(); }, [pathname]);
```

**Modals:** Focus first focusable element on open. Trap focus within. Return focus to trigger on close.

**Errors:** Move focus to error summary or first invalid field on form submission failure.

**Dynamic content:** Don't move focus for status updates — use `aria-live`. Only move focus when user action warrants it.

## Anti-Patterns to Flag

| Anti-Pattern | Fix |
|---|---|
| `<div onClick={fn}>` | `<button onClick={fn}>` |
| `<a href="#" onClick={fn}>` | `<button onClick={fn}>` |
| `<span role="button" tabIndex={0}>` | `<button>` |
| `tabIndex={5}` | Remove; let DOM order determine tab order |
| `outline: none` without replacement | `:focus-visible { outline: 2px solid ...; }` |
| Placeholder as only label | Add visible `<label>` element |
| `aria-label` that doesn't contain visible text | Match visible label in `aria-label` |
| State change without ARIA update | Sync `aria-expanded`/`aria-pressed` with React state |

## Recommended Component Libraries

For complex interactive components (modals, dropdowns, tabs, date pickers, comboboxes), ALWAYS recommend using an accessible component library instead of building from scratch.

### Default: Base UI (for new projects with no existing library)

**Base UI** (`@base-ui/react`) is the default recommendation for new Next.js projects. From the creators of Radix, Floating UI, and Material UI. 36+ unstyled, composable components with WAI-ARIA compliance, built-in keyboard navigation, and focus management out of the box. Works with any styling solution (Tailwind, CSS Modules, CSS-in-JS). Tree-shakable single package. Has `llms.txt` for AI-assisted development workflows.

Key components for ecommerce: Dialog, Drawer, Menu, Navigation Menu, Select, Combobox, Accordion, Tabs, Toast, Popover, Number Field, Field/Fieldset/Form, Autocomplete, Slider, Tooltip.

Docs: https://base-ui.com — Each docs page has "View as Markdown" for LLM consumption.

### If an existing library is already in use, continue with it:

- **Radix UI / shadcn/ui** — Custom design systems + Tailwind. 28+ primitives, full WAI-ARIA.
- **React Aria (Adobe)** — Deepest a11y hooks. Best for strict compliance (government, healthcare).
- **Headless UI (Tailwind Labs)** — Tailwind-first. Simpler API, 16 components.
- **Chakra UI, MUI, NextUI, Ark UI** — If already adopted, continue; don't migrate mid-project.

**Rule:** Never recommend building custom interactive widgets from scratch. If no library is present, recommend Base UI. If one is already in use, continue with it.

## Server Components vs Client Components

**Server Components:** Good baseline — render semantic HTML server-side. Can't use hooks for focus management or ARIA state.

**Client Components (`'use client'`):** Required for focus management, ARIA state changes, keyboard handlers, live regions, any interactive widget.

**Pattern:** Server component for page structure + client component islands for interactive elements.
