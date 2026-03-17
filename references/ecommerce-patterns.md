# Ecommerce Accessibility Patterns

Ecommerce accounts for ~70% of ADA lawsuits. Only 11% of cart/checkout pages pass WCAG. This reference maps accessibility requirements to every ecommerce touchpoint.

## Violation Frequency (Lawsuit Priority Order)

1. Missing/vague product image alt text (1.1.1)
2. Checkout form fields without labels (3.3.2, 1.3.1)
3. Keyboard traps in modals/popups (2.1.2)
4. Insufficient contrast on prices/CTAs (1.4.3)
5. Cart updates not announced (4.1.3)
6. Color-only variant selectors (1.4.1, 4.1.2)
7. Inaccessible carousel/gallery (2.1.1, 2.5.1)
8. Empty links/buttons — icon-only without labels (2.4.4, 4.1.2)
9. Missing document language (3.1.1)
10. Third-party widgets injecting inaccessible content

## Product Discovery

**Search:** Label the search field. Autocomplete dropdown: keyboard navigable (Arrow keys, Enter, Escape), `role="listbox"` + `role="option"`, `aria-activedescendant`. Results count in `aria-live="polite"` region.

**Filters/Facets:** All filters keyboard-operable. Color swatches MUST have text labels (not color-only). Active filters removable with labeled button. Results count announced on filter change. Price slider: provide text input alternative.

**Product Grid:** Descriptive alt text on images. Product links include product name (not "Shop now" × 20). Prices as text (not images). Sufficient contrast on sale prices.

## Product Detail Page

**Gallery/Carousel:** All images have descriptive alt text. Prev/next buttons (not swipe-only). Keyboard navigable. Current slide announced ("Image 3 of 8"). Zoom keyboard-accessible.

**Variant Selectors:** Color swatches: `aria-label="Color: Navy Blue"`. Size: `aria-label="Size: Large"`. Selected state: `aria-pressed="true"`. Out-of-stock: `aria-disabled="true"` + text. Price changes: live region announcement. Image updates: live region.

**Add to Cart:** Button has clear accessible name. Success: `role="status"` announces "[Product] added to cart". Error: `role="alert"` announces "Please select a size". Loading: `aria-busy="true"`. Quantity input labeled.

**Reviews:** Star ratings have text equivalent ("4.5 out of 5 stars").

## Cart

**Mini-Cart Drawer:** `role="dialog"`, `aria-modal="true"`. Focus trapped inside. Escape closes. Focus returns to cart button. Cart count in header updated.

**Full Cart:** Display as `<table>` with `<th>` (Product, Qty, Price, Total). Quantity controls keyboard-accessible. Remove button: live region announces removal. Price updates announced. Don't wrap entire cart in `<form>` — screen readers in forms mode miss product info.

## Checkout (Highest Risk Area)

**Every field:** Visible `<label>` (never placeholder-only). `autocomplete` attributes on all personal/payment fields. `aria-required="true"` on required fields.

**Autocomplete tokens:**
- `given-name`, `family-name`, `email`, `tel`
- `street-address`, `address-level2` (city), `address-level1` (state), `postal-code`, `country`
- `cc-number`, `cc-exp`, `cc-csc`, `cc-name`

**Errors:** Specific message ("Enter valid email"). Associated with field (`aria-describedby`). Focus moves to first error or error summary. `role="alert"` on error container.

**Multi-step:** Progress indicator accessible (`aria-current="step"`). Step changes announced. Data persists between steps. Guest checkout available.

**Order review:** Summary before submission (3.3.4 Error Prevention). "Place Order" button clearly labeled.

**Confirmation:** Descriptive page title. Order number, email notice, summary as table.

## Third-Party Widget Risk

Every third-party widget is a potential WCAG violation. Flag these for manual testing:
- Review widgets (Yotpo, Judge.me, Bazaarvoice)
- Chat widgets (Zendesk, Intercom, Drift)
- Email popups (Klaviyo, Mailchimp)
- Payment forms (Stripe, Adyen, PayPal hosted fields)
- Social proof popups (Fomo, TrustPulse)
- Cookie consent (OneTrust, Cookiebot)
- Search overlays (Algolia, Searchspring)

When encountering third-party widgets in code, always add to the "Manual Testing Required" section of the review.

## Global Patterns

**Header:** Skip nav link first. Nav labeled (`aria-label="Main navigation"`). Search labeled. Mobile hamburger: `aria-expanded` on toggle button.

**Popups/Modals:** Keyboard-dismissible (Escape). Focus trapped. Focus returns on close. No keyboard traps ever.

**Footer:** Links descriptive. Social icons have `aria-label`. Nav labeled (`aria-label="Footer navigation"`).
