# WCAG 2.2 Level A Criteria — Actionable Checks

30 criteria. Every item below is a check the skill must verify when reviewing code.

## Table of Contents
- Perceivable (1.1.1–1.4.2)
- Operable (2.1.1–2.5.4)
- Understandable (3.1.1–3.3.2)
- Robust (4.1.2)

---

## PERCEIVABLE

### 1.1.1 Non-text Content
**Priority:** Tier 1
**Check:** Every `<img>` has `alt`. Meaningful images describe content. Decorative images use `alt=""` or `role="presentation"`. SVG icons: `aria-hidden="true"` (decorative) or `role="img"` + `<title>` (meaningful). Icon-only buttons: `aria-label`. No CSS background images conveying essential info.
**Common violation:** `<img src="product.jpg">` with no alt attribute.
**Exceptions:** Controls/inputs (covered by 4.1.2), tests where text would invalidate the test, specific sensory experiences, CAPTCHA (must provide alternative forms), purely decorative content.
**Test:** Run axe-core for missing alt. Manually verify alt text quality — automation can't assess meaningfulness. Check with screen reader that all images are announced or correctly hidden.

### 1.2.1 Audio-only / Video-only (Prerecorded)
**Priority:** Tier 3
**Check:** Audio-only has text transcript. Video-only (no audio) has text description or audio track alternative.
**Exceptions:** Media that is itself an alternative for text content and labeled as such.
**Test:** Verify transcript exists and covers all meaningful content.

### 1.2.2 Captions (Prerecorded)
**Priority:** Tier 1
**Check:** `<video>` with audio has `<track kind="captions">`. Captions include dialogue AND significant sounds.
**Exceptions:** Media that is itself an alternative for text content and labeled as such.
**Test:** Play video and verify captions are synchronized, accurate, and include non-speech sounds.

### 1.2.3 Audio Description or Media Alternative
**Priority:** Tier 3
**Check:** Video with meaningful visual content has audio description track or full text transcript alternative.
**Exceptions:** Media that is itself an alternative for text content and labeled as such.
**Test:** Watch video with audio description; verify all important visual information is narrated.

### 1.3.1 Info and Relationships
**Priority:** Tier 1
**Check:** Headings use `<h1>`–`<h6>` (not styled `<div>`). Lists use `<ul>`/`<ol>`/`<li>`. Tables use `<th>` with `scope`. Form inputs associated with `<label>`. Grouped fields use `<fieldset>`/`<legend>`. Landmarks present (`<header>`, `<nav>`, `<main>`, `<footer>`). No "div soup" for structural content.
**Common violation:** `<div class="heading">Title</div>` instead of `<h2>Title</h2>`.
**Exceptions:** None — if structure is conveyed visually, it must be conveyed programmatically.
**Test:** Navigate by headings in screen reader — hierarchy should be logical. Navigate by landmarks — all regions should be labeled. Inspect form fields — all must have programmatic labels.

### 1.3.2 Meaningful Sequence
**Priority:** Tier 2
**Check:** DOM order matches visual reading order. CSS `order` property doesn't contradict logical sequence. Flexbox/grid layout doesn't visually reorder content in a way that breaks meaning.
**Exceptions:** Sequence is only required when it affects meaning — side navigation before or after main content is acceptable if meaning is preserved.
**Test:** Linearize the page (disable CSS) and read — does the content make sense? Screen reader reads in DOM order, not visual order.

### 1.3.3 Sensory Characteristics
**Priority:** Tier 3
**Check:** Instructions don't rely solely on shape, size, visual location, or sound. No "click the green button" or "see the sidebar on the right" without additional context.
**Exceptions:** None.
**Test:** Read all instructional text — could a blind user follow the instructions? Could a deaf user follow the instructions?

### 1.4.1 Use of Color
**Priority:** Tier 1
**Check:** Color is not the only means of conveying information. Links distinguished from text by more than color alone (underline, bold, or 3:1 contrast + hover/focus change). Error states use text/icons in addition to red. Chart data uses patterns/labels not just color.
**Common violation:** Required fields indicated only by a red asterisk with no text explanation.
**Exceptions:** Visited links (color change alone is acceptable). Mouseover focus highlighting on navigation.
**Test:** View page in grayscale — is all information still distinguishable? Check with color blindness simulator (Protanopia, Deuteranopia).

### 1.4.2 Audio Control
**Priority:** Tier 3
**Check:** No audio auto-plays for more than 3 seconds without pause/stop/mute control.
**Exceptions:** None — auto-playing audio is strongly discouraged regardless.
**Test:** Load the page — does audio start automatically? If so, verify control is immediately available and keyboard-accessible.

---

## OPERABLE

### 2.1.1 Keyboard
**Priority:** Tier 1
**Check:** All interactive elements operable via keyboard. `<button>` for actions (not `<div onClick>`). Custom elements have `tabindex="0"` and key handlers (Enter, Space). No mouse-only interactions. Dropdown menus, carousels, tabs all keyboard-navigable.
**Common violation:** `<div onClick={handler}>` with no `role`, `tabindex`, or `onKeyDown`.
**Exceptions:** Functions requiring path-dependent input (e.g., freehand drawing) — the underlying function (not the input technique) must be keyboard-accessible.
**Test:** Unplug mouse. Tab through entire page. Can you reach and operate every interactive element? Can you complete a full purchase flow with keyboard only?

### 2.1.2 No Keyboard Trap
**Priority:** Tier 1
**Check:** Focus can always be moved away from any element. Modals trap focus within (Tab cycles inside) but release on close. Escape closes overlays. Iframes don't trap focus.
**Common violation:** Modal dialog with no escape mechanism.
**Exceptions:** None — this is a Conformance Requirement 5 (Non-interference) issue.
**Test:** Tab through all interactive elements. Enter every modal/overlay/dropdown. Verify you can always escape with Tab/Shift+Tab or Escape. Pay special attention to iframes and third-party widgets.

### 2.1.4 Character Key Shortcuts
**Priority:** Tier 3
**Check:** If single-character keyboard shortcuts exist, they can be turned off, remapped, or are only active when component is focused.
**Exceptions:** None when shortcuts use only letter/punctuation/number/symbol keys. Shortcuts with modifier keys (Ctrl, Alt) are not subject to this requirement.
**Test:** Check if any single-character shortcuts exist. If so, verify they can be disabled or remapped. Test with speech recognition software (shortcuts can fire accidentally from speech input).

### 2.2.1 Timing Adjustable
**Priority:** Tier 3
**Check:** Time limits can be turned off, adjusted, or extended (20+ second warning). Session timeouts warn before expiring. Auto-advancing content can be paused.
**Exceptions:** Real-time events (auctions). Essential time limits (security, data integrity). Time limits longer than 20 hours.
**Test:** Trigger session timeout — is there a warning? Can the user extend? Check auto-advancing carousels — can the user pause?

### 2.2.2 Pause, Stop, Hide
**Priority:** Tier 2
**Check:** Moving/blinking content lasting >5 seconds has pause/stop/hide control. Auto-updating content has pause mechanism. Carousels have pause button. `prefers-reduced-motion` respected.
**Common violation:** Auto-rotating hero banner with no pause button.
**Exceptions:** Essential animations (e.g., loading indicators during a preload phase where no user interaction is possible).
**Test:** Identify all moving/auto-updating content. Verify each has a pause/stop control. Check that `prefers-reduced-motion: reduce` is respected in CSS.

### 2.3.1 Three Flashes or Below Threshold
**Priority:** Tier 2
**Check:** No content flashes more than 3 times per second.
**Exceptions:** Flash is below the general flash threshold (small area, low contrast) or below the red flash threshold.
**Test:** Review animations and video content. Use the Photosensitive Epilepsy Analysis Tool (PEAT) for suspect content.

### 2.4.1 Bypass Blocks
**Priority:** Tier 2
**Check:** Skip navigation link exists as first focusable element, linking to `<main>`. HTML5 landmarks present (`<header>`, `<nav>`, `<main>`, `<footer>`).
**Common violation:** No skip link on pages with large navigation.
**Exceptions:** Non-web documents and software (mark N/A).
**Test:** Tab once on page load — skip link should appear. Activate it — focus should move to main content. Verify landmarks with screen reader landmark navigation.

### 2.4.2 Page Titled
**Priority:** Tier 2
**Check:** `<title>` is unique and descriptive per page. Pattern: "[Page Name] | [Site Name]". SPAs update `document.title` on route change.
**Common violation:** Same `<title>` on every page, or generic "Home".
**Exceptions:** None.
**Test:** Check browser tab/title bar on every page. In SPAs, navigate between routes and verify title changes. Screen readers announce the title first when landing on a page.

### 2.4.3 Focus Order
**Priority:** Tier 1
**Check:** Tab order is logical and follows visual layout. No positive `tabindex` values (>0). Dynamically inserted content placed in logical DOM position. Modal focus management: focus to modal on open, restore on close.
**Common violation:** `tabindex="5"` creating unpredictable tab order.
**Exceptions:** When different orders equally preserve meaning and operability (e.g., navigating table cells by row or column).
**Test:** Tab through entire page. Does focus order match visual layout? Open modals — does focus move inside? Close modals — does focus return to trigger?

### 2.4.4 Link Purpose (In Context)
**Priority:** Tier 1
**Check:** Link text describes destination. No "Click here", "Read more", "Learn more" without surrounding context. Image links have descriptive `alt`. `aria-label` or `aria-labelledby` when visual context needed.
**Common violation:** Multiple "Read more" links with no distinguishing context.
**Exceptions:** Links whose purpose is ambiguous to all users (e.g., mystery game links).
**Test:** List all links with screen reader link list feature — each link should make sense out of context. If not, verify surrounding context provides meaning.

### 2.5.1 Pointer Gestures
**Priority:** Tier 2
**Check:** Multipoint/path-based gestures have single-pointer alternative. Swipe carousels have prev/next buttons. Pinch-to-zoom has button alternative.
**Exceptions:** Gestures required by the OS/browser (not author-controlled). Essential gestures (rare — e.g., drawing a signature).
**Test:** Identify all gesture-based interactions. Verify each has a single-click/tap alternative.

### 2.5.2 Pointer Cancellation
**Priority:** Tier 3
**Check:** Actions fire on `click`/`mouseup`/`touchend`, not `mousedown`/`touchstart`. User can move pointer off target before release to cancel.
**Exceptions:** Down-event is essential (e.g., piano keyboard emulator). Up-event reverses the down-event outcome.
**Test:** Press and hold on interactive elements, then drag off — action should not fire. Native HTML buttons/links meet this by default.

### 2.5.3 Label in Name
**Priority:** Tier 2
**Check:** Accessible name contains (or matches) visible label text. If button shows "Search", `aria-label` must start with "Search". Voice input users say what they see.
**Common violation:** Button text "Submit" but `aria-label="Send form data to server"`.
**Exceptions:** None when visible text label exists.
**Test:** Compare visible label to computed accessible name (use browser DevTools Accessibility tab). Run IBM Equal Access Checker rule `e6952f`. Test with voice input: say the visible label — does it activate?

### 2.5.4 Motion Actuation
**Priority:** Tier 3
**Check:** Device motion-triggered functions have UI alternative. Motion response can be disabled. Exception: motion is essential (pedometer).
**Exceptions:** Motion supported by the user interface (the element moves with the device). Motion is essential to the function.
**Test:** Identify motion-based features (shake to undo, tilt to scroll). Verify UI alternative exists. Verify motion can be disabled in settings.

---

## UNDERSTANDABLE

### 3.1.1 Language of Page
**Priority:** Tier 1
**Check:** `<html lang="en">` (or appropriate ISO 639-1 code) is present.
**Common violation:** Missing `lang` attribute — one of the easiest fixes, one of the most common failures.
**Exceptions:** None.
**Test:** Inspect `<html>` element. Verify `lang` attribute present and correct for the page's primary language. For multi-locale sites (19 locales), verify locale routing sets the correct `lang`.

### 3.2.1 On Focus
**Priority:** Tier 2
**Check:** Focusing an element does not trigger context change (navigation, form submit, new window).
**Exceptions:** None — context changes on focus are always a violation.
**Test:** Tab through all interactive elements. Does focusing any element cause navigation, form submission, or new window? Focus should only cause visual focus indicator changes, not context changes.

### 3.2.2 On Input
**Priority:** Tier 2
**Check:** Changing a form control doesn't auto-submit or navigate without prior warning. Select dropdowns don't navigate on selection without a go button.
**Exceptions:** The user is advised of the behavior before using the component.
**Test:** Change every form control value. Does any cause unexpected navigation or submission? Select elements and radio buttons are common offenders.

### 3.3.1 Error Identification
**Priority:** Tier 1
**Check:** Errors identified in text (not color-only). Error message associated with field via `aria-describedby` or `aria-errormessage`. Error message is specific ("Email must include @").
**Common violation:** Red border on invalid field with no text message.
**Exceptions:** None.
**Test:** Submit forms with invalid data. Are errors announced by screen reader? Is each error associated with the correct field? Is the error message specific enough to fix the problem?

### 3.3.2 Labels or Instructions
**Priority:** Tier 1
**Check:** Every input has visible associated `<label>`. Placeholder is NOT used as the only label. Required fields identified (text + `aria-required`). Format hints provided ("MM/DD/YYYY").
**Common violation:** `<input placeholder="Email">` with no `<label>`.
**Exceptions:** None.
**Test:** Tab to each form field. Does screen reader announce a label? Is the label visible? Are required fields identified before submission?

---

## ROBUST

### 4.1.2 Name, Role, Value
**Priority:** Tier 1
**Check:** Custom controls have appropriate ARIA roles. States communicated (`aria-expanded`, `aria-pressed`, `aria-checked`, `aria-selected`). State changes update ARIA attributes dynamically. Tabs use `role="tablist"`, `role="tab"`, `role="tabpanel"`. Notifications of changes available to assistive tech.
**Common violation:** Custom dropdown with no `role="listbox"`, `aria-expanded`, or keyboard interaction.
**Exceptions:** None — if it's an interactive control, it must have an accessible name, role, and value.
**Test:** Inspect custom controls in Accessibility DevTools. Are roles correct? Toggle states (expand/collapse, check/uncheck) — do ARIA attributes update? Screen reader must announce the role, name, and current state.

---

**Note:** 4.1.1 Parsing was removed in WCAG 2.2 as modern browsers handle parsing errors. Not checked.
