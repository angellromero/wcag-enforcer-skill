# WCAG 2.2 Level AA Criteria — Actionable Checks

25 criteria (20 from WCAG 2.1 + 6 new in 2.2, minus deprecated 4.1.1). These are checked IN ADDITION to all Level A criteria.

## Table of Contents
- Perceivable (1.2.4–1.4.13)
- Operable (2.4.5–2.5.8)
- Understandable (3.1.2–3.3.8)
- Robust (4.1.3)

---

## PERCEIVABLE

### 1.2.4 Captions (Live)
**Check:** Live audio/video content has real-time captions. Live streams, webinars have CART or auto-captioning.

### 1.2.5 Audio Description (Prerecorded)
**Check:** Prerecorded video has audio description track narrating important visual content not conveyed in dialogue.

### 1.3.4 Orientation
**Check:** Content not locked to portrait or landscape. No `orientation: portrait` CSS lock. Content functions in both orientations.

### 1.3.5 Identify Input Purpose
**Check:** Personal data fields have `autocomplete` attribute with standard tokens: `given-name`, `family-name`, `email`, `tel`, `street-address`, `postal-code`, `country`, `cc-number`, `cc-exp`, `cc-csc`, etc. Enables browser autofill and assistive tech icon/label association.
**Common violation:** Checkout address fields without `autocomplete` attributes.

### 1.4.3 Contrast (Minimum)
**Check:** Normal text: ≥ 4.5:1 contrast ratio against background. Large text (≥24px or ≥18.66px bold): ≥ 3:1. Check ALL text including: prices, placeholder text, disabled text (exempt but good practice), text over images/gradients.
**Common violation:** Light gray text on white (#999 on #fff = 2.85:1). This is the #1 most common WCAG failure on the web (79.1% of sites).

### 1.4.4 Resize Text
**Check:** Content readable and functional at 200% browser zoom. No horizontal scrolling at 200%. No clipped or overlapping text. Use relative units (rem, em, %) for font sizes.

### 1.4.5 Images of Text
**Check:** Real text used instead of images containing text. Logos exempt. If image of text unavoidable, text alternative provided.

### 1.4.10 Reflow
**Check:** Content works at 320px viewport width without horizontal scrolling (equivalent to 400% zoom on 1280px). Responsive design with wrapping. No fixed-width containers.
**Fix pattern:** `max-width` instead of `width`. CSS grid with `auto-fill, minmax()`. Flexbox with `flex-wrap`.

### 1.4.11 Non-text Contrast
**Check:** UI component boundaries ≥ 3:1 contrast (form input borders, buttons). Focus indicators ≥ 3:1. Graphical objects needed to understand content ≥ 3:1. State changes maintain contrast (hover, active, selected).
**Common violation:** Faint gray input borders that disappear against white background.

### 1.4.12 Text Spacing
**Check:** No loss of content when user overrides: line-height 1.5×, paragraph spacing 2×, letter-spacing 0.12×, word-spacing 0.16×. No fixed-height containers clipping text. No `overflow: hidden` on text containers without accommodation.
**Fix pattern:** Use `min-height` not `height`. Avoid `text-overflow: ellipsis` on critical content.

### 1.4.13 Content on Hover or Focus
**Check:** Additional content triggered by hover/focus is: dismissible (Escape key), hoverable (pointer can move to new content without it disappearing), persistent (stays until dismissed or focus/hover removed). Tooltips, popovers, dropdown menus must follow all three rules.
**Common violation:** Tooltip disappears when moving cursor toward it.

---

## OPERABLE

### 2.4.5 Multiple Ways
**Check:** More than one way to locate a page (nav menu + search, or nav + sitemap, etc.). Process steps exempt.

### 2.4.6 Headings and Labels
**Check:** Headings describe topic/purpose. Form labels describe expected input. Heading hierarchy is logical (no gaps).

### 2.4.7 Focus Visible
**Check:** Keyboard focus indicator always visible. `:focus-visible` CSS provides visible outline. Never `outline: none` without replacement. Focus indicator ≥ 3:1 contrast.
**Common violation:** `*:focus { outline: none; }` in CSS reset.
**Fix:** `*:focus-visible { outline: 2px solid #2563eb; outline-offset: 2px; }`

### 2.4.11 Focus Not Obscured (Minimum) — *New in 2.2*
**Check:** When element receives focus, it's not entirely hidden by author-created content (sticky headers, footers, chat widgets, cookie banners). Use `scroll-padding-top` for sticky headers.

### 2.5.7 Dragging Movements — *New in 2.2*
**Check:** Drag operations have single-pointer alternative. Range sliders have text input or +/- buttons. Drag-to-reorder has move up/down buttons.

### 2.5.8 Target Size (Minimum) — *New in 2.2*
**Check:** Interactive targets ≥ 24×24 CSS pixels (or sufficient spacing). Primary CTAs recommended 44×44px for mobile.
**Fix:** `min-height: 24px; min-width: 24px;` on all interactive elements.

---

## UNDERSTANDABLE

### 3.1.2 Language of Parts
**Check:** Non-default language content has `lang` attribute: `<span lang="fr">Bonjour</span>`. Proper names and technical terms exempt.

### 3.2.3 Consistent Navigation
**Check:** Navigation menus in same relative order on all pages. Items can be added but existing order preserved.

### 3.2.4 Consistent Identification
**Check:** Same function = same label across pages. "Search" everywhere, not "Search" on one page and "Find" on another.

### 3.2.6 Consistent Help — *New in 2.2*
**Check:** Help mechanisms (chat, phone, FAQ link) appear in same relative position on every page.

### 3.3.3 Error Suggestion
**Check:** Error messages suggest how to fix the problem. "Enter email as name@example.com" not just "Invalid input". Password requirements shown when validation fails.

### 3.3.4 Error Prevention (Legal, Financial, Data)
**Check:** For checkout/payment/account changes: submission is reversible, OR data checked with correction opportunity, OR review/confirm step before submit. Order review page before "Place Order".
**Common violation:** One-click purchase with no confirmation step.

### 3.3.7 Redundant Entry — *New in 2.2*
**Check:** Previously entered info auto-populated or available for selection. "Billing same as shipping" checkbox. Don't ask for the same data twice in a process.

### 3.3.8 Accessible Authentication (Minimum) — *New in 2.2*
**Check:** Login doesn't require cognitive tests without alternatives. Password fields support paste and password managers. Alternative auth available (magic link, passkey, OAuth). Don't block clipboard paste in password fields.

---

## ROBUST

### 4.1.3 Status Messages
**Check:** Dynamic status updates announced to screen readers WITHOUT receiving focus. Use `role="status"` + `aria-live="polite"` for: search result counts, cart updates, form success messages. Use `role="alert"` for urgent/error messages.
**Common violation:** "Item added to cart" message appears visually but not announced to screen readers.
**Fix pattern:**
```html
<div role="status" aria-live="polite" aria-atomic="true">
  [dynamic content here]
</div>
```
The live region element MUST exist in the DOM before content changes.
