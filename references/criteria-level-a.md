# WCAG 2.2 Level A Criteria — Actionable Checks

30 criteria. Every item below is a check the skill must verify when reviewing code.

## Table of Contents
- Perceivable (1.1.1–1.3.3)
- Operable (2.1.1–2.5.4)
- Understandable (3.1.1–3.3.2)
- Robust (4.1.2)

---

## PERCEIVABLE

### 1.1.1 Non-text Content
**Check:** Every `<img>` has `alt`. Meaningful images describe content. Decorative images use `alt=""` or `role="presentation"`. SVG icons: `aria-hidden="true"` (decorative) or `role="img"` + `<title>` (meaningful). Icon-only buttons: `aria-label`. No CSS background images conveying essential info.
**Common violation:** `<img src="product.jpg">` with no alt attribute.

### 1.2.1 Audio-only / Video-only (Prerecorded)
**Check:** Audio-only has text transcript. Video-only (no audio) has text description or audio track alternative.

### 1.2.2 Captions (Prerecorded)
**Check:** `<video>` with audio has `<track kind="captions">`. Captions include dialogue AND significant sounds.

### 1.2.3 Audio Description or Media Alternative
**Check:** Video with meaningful visual content has audio description track or full text transcript alternative.

### 1.3.1 Info and Relationships
**Check:** Headings use `<h1>`–`<h6>` (not styled `<div>`). Lists use `<ul>`/`<ol>`/`<li>`. Tables use `<th>` with `scope`. Form inputs associated with `<label>`. Grouped fields use `<fieldset>`/`<legend>`. Landmarks present (`<header>`, `<nav>`, `<main>`, `<footer>`). No "div soup" for structural content.
**Common violation:** `<div class="heading">Title</div>` instead of `<h2>Title</h2>`.

### 1.3.2 Meaningful Sequence
**Check:** DOM order matches visual reading order. CSS `order` property doesn't contradict logical sequence. Flexbox/grid layout doesn't visually reorder content in a way that breaks meaning.

### 1.3.3 Sensory Characteristics
**Check:** Instructions don't rely solely on shape, size, visual location, or sound. No "click the green button" or "see the sidebar on the right" without additional context.

### 1.4.1 Use of Color
**Check:** Color is not the only means of conveying information. Links distinguished from text by more than color alone (underline, bold, or 3:1 contrast + hover/focus change). Error states use text/icons in addition to red. Chart data uses patterns/labels not just color.
**Common violation:** Required fields indicated only by a red asterisk with no text explanation.

### 1.4.2 Audio Control
**Check:** No audio auto-plays for more than 3 seconds without pause/stop/mute control.

---

## OPERABLE

### 2.1.1 Keyboard
**Check:** All interactive elements operable via keyboard. `<button>` for actions (not `<div onClick>`). Custom elements have `tabindex="0"` and key handlers (Enter, Space). No mouse-only interactions. Dropdown menus, carousels, tabs all keyboard-navigable.
**Common violation:** `<div onClick={handler}>` with no `role`, `tabindex`, or `onKeyDown`.

### 2.1.2 No Keyboard Trap
**Check:** Focus can always be moved away from any element. Modals trap focus within (Tab cycles inside) but release on close. Escape closes overlays. Iframes don't trap focus.
**Common violation:** Modal dialog with no escape mechanism.

### 2.1.4 Character Key Shortcuts
**Check:** If single-character keyboard shortcuts exist, they can be turned off, remapped, or are only active when component is focused.

### 2.2.1 Timing Adjustable
**Check:** Time limits can be turned off, adjusted, or extended (20+ second warning). Session timeouts warn before expiring. Auto-advancing content can be paused.

### 2.2.2 Pause, Stop, Hide
**Check:** Moving/blinking content lasting >5 seconds has pause/stop/hide control. Auto-updating content has pause mechanism. Carousels have pause button. `prefers-reduced-motion` respected.
**Common violation:** Auto-rotating hero banner with no pause button.

### 2.3.1 Three Flashes or Below Threshold
**Check:** No content flashes more than 3 times per second.

### 2.4.1 Bypass Blocks
**Check:** Skip navigation link exists as first focusable element, linking to `<main>`. HTML5 landmarks present (`<header>`, `<nav>`, `<main>`, `<footer>`).
**Common violation:** No skip link on pages with large navigation.

### 2.4.2 Page Titled
**Check:** `<title>` is unique and descriptive per page. Pattern: "[Page Name] | [Site Name]". SPAs update `document.title` on route change.
**Common violation:** Same `<title>` on every page, or generic "Home".

### 2.4.3 Focus Order
**Check:** Tab order is logical and follows visual layout. No positive `tabindex` values (>0). Dynamically inserted content placed in logical DOM position. Modal focus management: focus to modal on open, restore on close.
**Common violation:** `tabindex="5"` creating unpredictable tab order.

### 2.4.4 Link Purpose (In Context)
**Check:** Link text describes destination. No "Click here", "Read more", "Learn more" without surrounding context. Image links have descriptive `alt`. `aria-label` or `aria-labelledby` when visual context needed.
**Common violation:** Multiple "Read more" links with no distinguishing context.

### 2.5.1 Pointer Gestures
**Check:** Multipoint/path-based gestures have single-pointer alternative. Swipe carousels have prev/next buttons. Pinch-to-zoom has button alternative.

### 2.5.2 Pointer Cancellation
**Check:** Actions fire on `click`/`mouseup`/`touchend`, not `mousedown`/`touchstart`. User can move pointer off target before release to cancel.

### 2.5.3 Label in Name
**Check:** Accessible name contains (or matches) visible label text. If button shows "Search", `aria-label` must start with "Search". Voice input users say what they see.
**Common violation:** Button text "Submit" but `aria-label="Send form data to server"`.

### 2.5.4 Motion Actuation
**Check:** Device motion-triggered functions have UI alternative. Motion response can be disabled. Exception: motion is essential (pedometer).

---

## UNDERSTANDABLE

### 3.1.1 Language of Page
**Check:** `<html lang="en">` (or appropriate ISO 639-1 code) is present.
**Common violation:** Missing `lang` attribute — one of the easiest fixes, one of the most common failures.

### 3.2.1 On Focus
**Check:** Focusing an element does not trigger context change (navigation, form submit, new window).

### 3.2.2 On Input
**Check:** Changing a form control doesn't auto-submit or navigate without prior warning. Select dropdowns don't navigate on selection without a go button.

### 3.3.1 Error Identification
**Check:** Errors identified in text (not color-only). Error message associated with field via `aria-describedby` or `aria-errormessage`. Error message is specific ("Email must include @").
**Common violation:** Red border on invalid field with no text message.

### 3.3.2 Labels or Instructions
**Check:** Every input has visible associated `<label>`. Placeholder is NOT used as the only label. Required fields identified (text + `aria-required`). Format hints provided ("MM/DD/YYYY").
**Common violation:** `<input placeholder="Email">` with no `<label>`.

---

## ROBUST

### 4.1.2 Name, Role, Value
**Check:** Custom controls have appropriate ARIA roles. States communicated (`aria-expanded`, `aria-pressed`, `aria-checked`, `aria-selected`). State changes update ARIA attributes dynamically. Tabs use `role="tablist"`, `role="tab"`, `role="tabpanel"`. Notifications of changes available to assistive tech.
**Common violation:** Custom dropdown with no `role="listbox"`, `aria-expanded`, or keyboard interaction.

---

**Note:** 4.1.1 Parsing was removed in WCAG 2.2 as modern browsers handle parsing errors. Not checked.
