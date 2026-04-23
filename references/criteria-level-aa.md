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
**Priority:** Tier 3
**Check:** Live audio/video content has real-time captions. Live streams, webinars have CART or auto-captioning.
**Exceptions:** None.
**Test:** Join live stream. Verify captions appear in real-time, are reasonably accurate, identify speakers, and note non-speech sounds.

### 1.2.5 Audio Description (Prerecorded)
**Priority:** Tier 3
**Check:** Prerecorded video has audio description track narrating important visual content not conveyed in dialogue.
**Exceptions:** All important visual information is already in the existing audio track (e.g., "talking head" video with static background).
**Test:** Watch video with audio description enabled. Verify visual-only content is narrated during dialogue pauses.

### 1.3.4 Orientation
**Priority:** Tier 2
**Check:** Content not locked to portrait or landscape. No `orientation: portrait` or `orientation: landscape` CSS lock in `@media` queries. Content functions in both orientations. Do not use `screen-orientation-lock` meta tag.
**Exceptions:** A specific display orientation is essential to the function (e.g., piano keyboard emulator, check deposit requiring landscape for check scanning). These are rare.
**Test:** Rotate device between portrait and landscape. Verify content reflows and remains functional. Check CSS for `orientation` media query locks. Test on mobile devices with orientation lock disabled.

### 1.3.5 Identify Input Purpose
**Priority:** Tier 2
**Check:** Personal data fields have `autocomplete` attribute with standard tokens: `given-name`, `family-name`, `email`, `tel`, `street-address`, `postal-code`, `country`, `cc-number`, `cc-exp`, `cc-csc`, etc. Enables browser autofill and assistive tech icon/label association.
**Common violation:** Checkout address fields without `autocomplete` attributes.
**Exceptions:** Only applies to fields collecting information about the user (not about other people/entities). Only applies to fields serving purposes listed in WCAG's [Input Purposes for User Interface Components](https://www.w3.org/TR/WCAG22/#input-purposes). Only applies when technology supports `autocomplete` (HTML does).
**Test:** Inspect all user-input forms. Verify `autocomplete` attribute present on name, email, phone, address, and payment fields. Test browser autofill — does it populate correctly?

### 1.4.3 Contrast (Minimum)
**Priority:** Tier 1
**Check:** Normal text: ≥ 4.5:1 contrast ratio against background. Large text (≥24px or ≥18.66px bold): ≥ 3:1. Check ALL text including: prices, placeholder text, disabled text (exempt but good practice), text over images/gradients.
**Common violation:** Light gray text on white (#999 on #fff = 2.85:1). This is the #1 most common WCAG failure on the web (81% of sites per WebAIM Million 2026).
**Exceptions:** Logotypes/brand text. Inactive/disabled UI components. Purely decorative text. Text in images that contain significant other visual content (e.g., a photograph with incidental text on a sign).
**Test:** Use browser DevTools contrast checker on all text. Use axe-core/IBM Checker for automated scan. Manually check text over images, gradients, and dynamic backgrounds. Check placeholder text (often fails). Check in both light and dark mode if applicable.

### 1.4.4 Resize Text
**Priority:** Tier 2
**Check:** Content readable and functional at 200% browser zoom. No horizontal scrolling at 200%. No clipped or overlapping text. Use relative units (rem, em, %) for font sizes.
**Exceptions:** Captions and images of text are exempt.
**Test:** Set browser zoom to 200%. Navigate the entire site. Look for: text overlap, truncation, scrollbar issues, broken layouts. Verify all interactive elements remain operable.

### 1.4.5 Images of Text
**Priority:** Tier 2
**Check:** Real text used instead of images containing text. Logos exempt. If image of text unavoidable, text alternative provided.
**Exceptions:** Text is customizable by the user. The particular presentation of text is essential (logotypes, brand names).
**Test:** Search for images containing text (hero banners with text baked into images are common). Verify these use real text overlays instead. Check Cloudinary transforms for text-in-image patterns.

### 1.4.10 Reflow
**Priority:** Tier 1
**Check:** Content works at 320px viewport width without horizontal scrolling (equivalent to 400% zoom on 1280px). Responsive design with wrapping. No fixed-width containers.
**Fix pattern:** `max-width` instead of `width`. CSS grid with `auto-fill, minmax()`. Flexbox with `flex-wrap`.
**Exceptions:** Content requiring two-dimensional layout for usage/meaning: data tables, maps, videos, diagrams, toolbars with full functionality. These may scroll horizontally.
**Test:** Set browser window to 320px wide (or zoom to 400% on 1280px). Navigate the entire page. No horizontal scrolling should be needed (vertical scrolling is fine). Check on actual mobile devices.

### 1.4.11 Non-text Contrast
**Priority:** Tier 1
**Check:** UI component boundaries ≥ 3:1 contrast (form input borders, buttons). Focus indicators ≥ 3:1. Graphical objects needed to understand content ≥ 3:1. State changes maintain contrast (hover, active, selected).
**Common violation:** Faint gray input borders that disappear against white background.
**Exceptions:** Unmodified browser-default controls (stock HTML checkboxes, radio buttons). Inactive/disabled UI components. Essential presentation (logos, flags, photographs).
**Test:** Check input field borders against background. Check button borders against background. Check focus indicator contrast. Check icon contrast against background. Verify in all states (default, hover, focus, active, disabled).

### 1.4.12 Text Spacing
**Priority:** Tier 3
**Check:** No loss of content when user overrides: line-height 1.5×, paragraph spacing 2×, letter-spacing 0.12×, word-spacing 0.16×. No fixed-height containers clipping text. No `overflow: hidden` on text containers without accommodation.
**Fix pattern:** Use `min-height` not `height`. Avoid `text-overflow: ellipsis` on critical content.
**Exceptions:** This requirement only applies to content implemented in markup languages that support text styling (HTML, CSS). PDF and non-markup technologies are excluded. Languages/scripts that don't use a specific spacing property (e.g., CJK for word spacing) are exempt for that property.
**Test:** Apply the text spacing bookmarklet (available at https://www.html5accessibility.com/tests/tsbookmarklet.html). Navigate the site — no content should be clipped, overlapped, or lost. Check fixed-height containers especially.

### 1.4.13 Content on Hover or Focus
**Priority:** Tier 2
**Check:** Additional content triggered by hover/focus is: dismissible (Escape key), hoverable (pointer can move to new content without it disappearing), persistent (stays until dismissed or focus/hover removed). Tooltips, popovers, dropdown menus must follow all three rules. Mega menus must allow pointer to travel from trigger to menu content. Product quick-view popovers must be dismissible with Escape. "Free shipping" tooltip badges must be hoverable.
**Common violation:** Tooltip disappears when moving cursor toward it. Mega menu closes when pointer leaves trigger before reaching menu.
**Exceptions:** Content whose visual presentation is entirely controlled by the user agent (e.g., HTML `title` attribute — though `title` is discouraged). Content that communicates an input error (error tooltips may not need to be dismissible).
**Test:** Hover over every tooltip, popover, mega menu, and dropdown. Slowly move pointer from trigger to content — does it stay? Press Escape — does it dismiss? Tab through — does content appear and persist while focused?

---

## OPERABLE

### 2.4.5 Multiple Ways
**Priority:** Tier 3
**Check:** More than one way to locate a page (nav menu + search, or nav + sitemap, etc.). Process steps exempt.
**Exceptions:** Pages that are steps in a process (checkout steps, form wizards).
**Test:** Verify at least two navigation methods exist (e.g., navigation menu + search + breadcrumbs). For ecommerce: header nav + search + category links + breadcrumbs.

### 2.4.6 Headings and Labels
**Priority:** Tier 1
**Check:** Headings describe topic/purpose. Form labels describe expected input. Heading hierarchy is logical (no gaps).
**Exceptions:** None when headings or labels are present — they must be descriptive. This doesn't require headings/labels to exist (that's covered by 1.3.1 and 3.3.2).
**Test:** Navigate by headings in screen reader. Each heading should describe its section. Read form labels — each should clearly describe the expected input. Check heading hierarchy (h1 → h2 → h3, no skipping).

### 2.4.7 Focus Visible
**Priority:** Tier 1
**Check:** Keyboard focus indicator always visible. `:focus-visible` CSS provides visible outline. Never `outline: none` without replacement. Focus indicator ≥ 3:1 contrast.
**Common violation:** `*:focus { outline: none; }` in CSS reset.
**Fix:** `*:focus-visible { outline: 2px solid #2563eb; outline-offset: 2px; }`
**Exceptions:** None.
**Test:** Tab through every interactive element. Is a focus ring always visible? Check in both light and dark mode. Test in Windows High Contrast mode (`@media (forced-colors: active)`). Verify focus indicator meets 3:1 contrast against adjacent colors.

### 2.4.11 Focus Not Obscured (Minimum) — *New in 2.2*
**Priority:** Tier 1
**Check:** When element receives focus, it's not entirely hidden by author-created content (sticky headers, footers, chat widgets, cookie banners). Use `scroll-padding-top` and `scroll-padding-bottom` for sticky headers/footers. Ensure fixed-position chat widgets don't cover focused elements. Cookie consent banners must not obscure focused content.
**Exceptions:** The user can reposition the obscuring content (e.g., a movable dialog). Partial obscuring is acceptable — only "entirely hidden" fails this criterion.
**Test:** Tab through all elements with sticky header/footer present. Tab through elements behind cookie banners. Open chat widget and tab through page — is any focused element completely hidden? Use `scroll-padding-block-start` CSS to compensate for sticky headers.

### 2.5.7 Dragging Movements — *New in 2.2*
**Priority:** Tier 2
**Check:** Drag operations have single-pointer alternative. Range sliders have text input or +/- buttons. Drag-to-reorder has move up/down buttons. Color pickers with drag have text input for hex/RGB values.
**Exceptions:** Dragging is essential to the function (rare). Dragging is at the OS/browser level and not author-controlled.
**Test:** Identify all drag interactions (sliders, reordering, drag-and-drop). Verify each has an alternative that doesn't require dragging (click, tap, type, or button press).

### 2.5.8 Target Size (Minimum) — *New in 2.2*
**Priority:** Tier 2
**Check:** Interactive targets ≥ 24×24 CSS pixels (or sufficient spacing). Primary CTAs recommended 44×44px for mobile.
**Fix:** `min-height: 24px; min-width: 24px;` on all interactive elements.
**Exceptions:** Inline links within body text. Browser-default rendering (e.g., `<select>` dropdown options). Equivalent target exists elsewhere on the page with sufficient size. Essential — the target size is legally or functionally required to be small. Spacing — undersized target has sufficient spacing around it equivalent to a 24×24 area.
**Test:** Use browser DevTools to measure interactive element sizes. Check all buttons, links, checkboxes, radio buttons. Pay attention to close buttons on modals/toasts and icon-only buttons.

---

## UNDERSTANDABLE

### 3.1.2 Language of Parts
**Priority:** Tier 3
**Check:** Non-default language content has `lang` attribute: `<span lang="fr">Bonjour</span>`. Proper names and technical terms exempt.
**Exceptions:** Proper names, technical terms, words of indeterminate language, words that have become part of surrounding language vernacular.
**Test:** Identify content in a different language from the page's primary `lang`. Verify it has the correct `lang` attribute. Screen readers switch pronunciation rules based on this attribute.

### 3.2.3 Consistent Navigation
**Priority:** Tier 3
**Check:** Navigation menus in same relative order on all pages. Items can be added but existing order preserved.
**Exceptions:** Only applies within a set of pages. User-initiated changes to navigation order are acceptable.
**Test:** Compare navigation across multiple pages. Is the order consistent? Are new items added without disrupting existing order?

### 3.2.4 Consistent Identification
**Priority:** Tier 3
**Check:** Same function = same label across pages. "Search" everywhere, not "Search" on one page and "Find" on another.
**Exceptions:** Only applies within a set of pages.
**Test:** Compare labels for repeated functionality across pages (search, login, cart, menu toggle). Verify consistent naming.

### 3.2.6 Consistent Help — *New in 2.2*
**Priority:** Tier 3
**Check:** Help mechanisms (chat, phone, FAQ link) appear in same relative position on every page. For ecommerce: support chat widget must be in the same location on PLP, PDP, cart, checkout, account, and order pages. "Contact Us" or "Help" link must be in a consistent footer/header position.
**Exceptions:** Only applies when help mechanisms are repeated across multiple pages.
**Test:** Navigate to 5+ different page types. Verify help mechanisms (chat bubble, help link, phone number) are in the same relative position. Check that they remain present and functional during checkout flow.

### 3.3.3 Error Suggestion
**Priority:** Tier 2
**Check:** Error messages suggest how to fix the problem. "Enter email as name@example.com" not just "Invalid input". Password requirements shown when validation fails.
**Exceptions:** Providing suggestions would jeopardize security (e.g., "that username already exists" on a login form — though many sites still do this).
**Test:** Submit forms with various invalid inputs. Are error messages specific and actionable? Do they tell the user what format is expected?

### 3.3.4 Error Prevention (Legal, Financial, Data)
**Priority:** Tier 2
**Check:** For checkout/payment/account changes: submission is reversible, OR data checked with correction opportunity, OR review/confirm step before submit. Order review page before "Place Order".
**Common violation:** One-click purchase with no confirmation step.
**Exceptions:** Only applies to submissions that cause legal commitments, financial transactions, or modify/delete user-controlled data.
**Test:** Complete a full checkout flow. Is there a review step before final submission? Can the user go back and correct information? After submission, can the order be modified/cancelled within a reasonable window?

### 3.3.7 Redundant Entry — *New in 2.2*
**Priority:** Tier 3
**Check:** Previously entered info auto-populated or available for selection. "Billing same as shipping" checkbox. Don't ask for the same data twice in a process.
**Exceptions:** Re-entering information is essential for security (e.g., confirming a password or email). The previously entered information is no longer valid (e.g., session expired).
**Test:** Complete multi-step checkout. Does shipping address auto-populate billing? Does the user have to re-enter any information they already provided? Check for "same as shipping" checkbox.

### 3.3.8 Accessible Authentication (Minimum) — *New in 2.2*
**Priority:** Tier 3
**Check:** Login doesn't require cognitive function tests without alternatives. Password fields support paste and password managers (`autocomplete="current-password"` or `autocomplete="new-password"`). Alternative auth available (magic link, passkey/WebAuthn, OAuth). Don't block clipboard paste in password fields. CAPTCHA must provide an alternative (audio, logic-based, or object recognition alternative to visual puzzles).
**Exceptions:** Cognitive test is to identify non-text content the user provided (e.g., "select the photo you uploaded"). Two-factor auth using a pattern the user set is acceptable.
**Test:** Can you paste into password fields? Does the browser password manager autofill work? Is CAPTCHA avoidable? Are there alternative login methods? Test with a password manager — does it work end-to-end?

---

## ROBUST

### 4.1.3 Status Messages
**Priority:** Tier 1
**Check:** Dynamic status updates announced to screen readers WITHOUT receiving focus. Use `role="status"` + `aria-live="polite"` for: search result counts, cart updates, form success messages. Use `role="alert"` for urgent/error messages.
**Common violation:** "Item added to cart" message appears visually but not announced to screen readers.
**Exceptions:** None — if a status message appears visually, it must be announced.
**Fix pattern:**
```html
<div role="status" aria-live="polite" aria-atomic="true">
  [dynamic content here]
</div>
```
The live region element MUST exist in the DOM before content changes.
**Test:** Add item to cart — is it announced? Apply a filter — is result count announced? Submit a form successfully — is success message announced? Do any of these steal focus? (They shouldn't for status messages; they may for errors.)
