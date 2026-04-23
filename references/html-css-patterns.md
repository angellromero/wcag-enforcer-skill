# HTML / CSS / JS Accessibility Patterns

Framework-agnostic patterns. These are the foundation that React/Next.js patterns build on.

## Semantic HTML Quick Reference

| Instead of | Use | Why |
|---|---|---|
| `<div onclick>` | `<button>` | Built-in keyboard, focus, role |
| `<span class="link">` | `<a href>` | Navigation semantics |
| `<div class="header">` | `<header>` | Landmark |
| `<div class="nav">` | `<nav>` | Navigation landmark |
| `<div class="main">` | `<main>` | Main content landmark |
| `<b>` / `<i>` | `<strong>` / `<em>` | Semantic emphasis |
| CSS columns for data | `<table>` + `<th>` + `<td>` | Table navigation |
| `<div class="group">` | `<fieldset>` + `<legend>` | Form grouping |

## Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Page Name] | [Site Name]</title>
</head>
<body>
  <a href="#main-content" class="skip-link">Skip to main content</a>
  <header>
    <nav aria-label="Main navigation">...</nav>
  </header>
  <main id="main-content">
    <h1>Page Title</h1>
  </main>
  <aside aria-label="Related content">...</aside>
  <footer>
    <nav aria-label="Footer navigation">...</nav>
  </footer>
</body>
</html>
```

## CSS Patterns

### Focus Indicators
```css
/* Keyboard-only focus ring */
*:focus-visible { outline: 2px solid #2563eb; outline-offset: 2px; }
*:focus:not(:focus-visible) { outline: none; }
/* High contrast mode */
@media (forced-colors: active) { *:focus-visible { outline: 2px solid ButtonText; } }
```

### Reduced Motion
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Text Spacing Defense
```css
/* Don't constrain text containers */
.text-container { min-height: auto; overflow: visible; }
/* Avoid on critical content: */
/* height: fixed; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; */
```

### Sticky Header Compensation (2.4.11)
```css
/* Prevent sticky headers from obscuring focused elements */
html { scroll-padding-block-start: var(--header-height, 80px); }
/* Also compensate for sticky footers/cookie banners */
html { scroll-padding-block-end: var(--footer-height, 60px); }
```

### Target Sizes
```css
button, a, input, select, [role="button"] { min-height: 24px; min-width: 24px; }
.btn-primary { min-height: 44px; min-width: 44px; }
```

### Visually Hidden (Screen Reader Only)
```css
.sr-only {
  position: absolute; width: 1px; height: 1px; padding: 0;
  margin: -1px; overflow: hidden; clip: rect(0,0,0,0);
  white-space: nowrap; border: 0;
}
.sr-only:focus, .sr-only:focus-within {
  position: static; width: auto; height: auto; padding: inherit;
  margin: inherit; overflow: visible; clip: auto; white-space: normal;
}
```

## Form Patterns

```html
<!-- Labeled input (explicit association) -->
<label for="email">Email address</label>
<input id="email" type="email" autocomplete="email" required aria-required="true">

<!-- Error state -->
<label for="email">Email address (required)</label>
<input id="email" type="email" aria-invalid="true" aria-describedby="email-error">
<p id="email-error" role="alert">Please enter a valid email (e.g., name@example.com)</p>

<!-- Grouped fields -->
<fieldset><legend>Shipping Address</legend><!-- fields --></fieldset>

<!-- Hidden label (when visual label not possible) -->
<input type="search" aria-label="Search products" placeholder="Search...">
```

## Image Patterns

```html
<!-- Meaningful -->
<img src="product.jpg" alt="Women's leather handbag, tan brown, front view">
<!-- Decorative -->
<img src="divider.svg" alt="" role="presentation">
<!-- Linked -->
<a href="/product/123"><img src="shoe.jpg" alt="Running shoes — view details"></a>
<!-- SVG meaningful -->
<svg role="img" aria-label="Shopping cart"><title>Shopping cart</title>...</svg>
<!-- SVG decorative -->
<svg aria-hidden="true" focusable="false">...</svg>
```

## ARIA Live Regions

```html
<!-- Status (polite) — search results, cart updates -->
<div role="status" aria-live="polite" aria-atomic="true">[dynamic text]</div>
<!-- Alert (assertive) — errors, urgent messages -->
<div role="alert">[error message]</div>
```
The live region element MUST exist in DOM before content changes.

## Data Tables

```html
<table>
  <caption>Order Summary</caption>
  <thead><tr><th scope="col">Product</th><th scope="col">Qty</th><th scope="col">Price</th></tr></thead>
  <tbody><tr><td>Shoes</td><td>1</td><td>$120</td></tr></tbody>
  <tfoot><tr><th scope="row" colspan="2">Total</th><td>$120</td></tr></tfoot>
</table>
```

## Keyboard Behavior Expectations

| Key | Expected Behavior |
|---|---|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous |
| Enter / Space | Activate buttons and links |
| Escape | Close modal/dropdown/popup |
| Arrow keys | Navigate within composite widgets (tabs, menus, radio groups) |
| Home / End | Jump to first/last item in list |
