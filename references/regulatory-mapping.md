# Regulatory Mapping — WCAG to Standards & Laws

Maps WCAG 2.2 criteria to the three major standards (WCAG, US 508, EN 301 549) and enforcement laws (ADA, EAA). Use this when reporting violations to include the regulatory context.

## Standards Overview

| Standard | Scope | Status |
|----------|-------|--------|
| **WCAG 2.2** (W3C) | Global reference standard | Published Oct 2023 |
| **US Section 508** (Access Board) | US federal & federally funded | Enforceable; references WCAG 2.0 A/AA via EN 301 549 alignment |
| **EN 301 549 V3.2.1** (ETSI) | EU harmonized standard | References WCAG 2.1 A/AA; EAA enforcement active |
| **ADA Title III** (DOJ) | US private-sector websites | Active; DOJ Title II rule effective April 2026; ecommerce is primary target |
| **European Accessibility Act** (EU) | Digital products & services sold in EU | Enforcement deadline June 28 2025; applies to ecommerce, banking, transport |

## Criterion → Standard Mapping

When reporting a violation, include the applicable regulations using this mapping.

### Perceivable

| WCAG Criterion | Level | US 508 | EN 301 549 | ADA Risk | EAA Risk |
|----------------|-------|--------|------------|----------|----------|
| 1.1.1 Non-text Content | A | §11.1.1.1 / §9.1.1.1 | §9.1.1.1 | High | High |
| 1.2.1 Audio-only/Video-only | A | §9.1.2.1 | §9.1.2.1 | Low | Medium |
| 1.2.2 Captions (Prerecorded) | A | §9.1.2.2 | §9.1.2.2 | Medium | Medium |
| 1.2.3 Audio Description/Alternative | A | §9.1.2.3 | §9.1.2.3 | Low | Medium |
| 1.2.4 Captions (Live) | AA | §9.1.2.4 | §9.1.2.4 | Low | Low |
| 1.2.5 Audio Description | AA | §9.1.2.5 | §9.1.2.5 | Low | Low |
| 1.3.1 Info and Relationships | A | §9.1.3.1 | §9.1.3.1 | High | High |
| 1.3.2 Meaningful Sequence | A | §9.1.3.2 | §9.1.3.2 | Medium | Medium |
| 1.3.3 Sensory Characteristics | A | §9.1.3.3 | §9.1.3.3 | Low | Low |
| 1.3.4 Orientation | AA | §9.1.3.4 | §9.1.3.4 | Low | Medium |
| 1.3.5 Identify Input Purpose | AA | §9.1.3.5 | §9.1.3.5 | Medium | High |
| 1.4.1 Use of Color | A | §9.1.4.1 | §9.1.4.1 | High | High |
| 1.4.2 Audio Control | A | §9.1.4.2 | §9.1.4.2 | Low | Low |
| 1.4.3 Contrast (Minimum) | AA | §9.1.4.3 | §9.1.4.3 | High | High |
| 1.4.4 Resize Text | AA | §9.1.4.4 | §9.1.4.4 | Medium | Medium |
| 1.4.5 Images of Text | AA | §9.1.4.5 | §9.1.4.5 | Medium | Medium |
| 1.4.10 Reflow | AA | §9.1.4.10 | §9.1.4.10 | Medium | High |
| 1.4.11 Non-text Contrast | AA | §9.1.4.11 | §9.1.4.11 | High | High |
| 1.4.12 Text Spacing | AA | §9.1.4.12 | §9.1.4.12 | Low | Medium |
| 1.4.13 Content on Hover/Focus | AA | §9.1.4.13 | §9.1.4.13 | Medium | Medium |

### Operable

| WCAG Criterion | Level | US 508 | EN 301 549 | ADA Risk | EAA Risk |
|----------------|-------|--------|------------|----------|----------|
| 2.1.1 Keyboard | A | §9.2.1.1 | §9.2.1.1 | High | High |
| 2.1.2 No Keyboard Trap | A | §9.2.1.2 | §9.2.1.2 | High | High |
| 2.1.4 Character Key Shortcuts | A | §9.2.1.4 | §9.2.1.4 | Low | Low |
| 2.2.1 Timing Adjustable | A | §9.2.2.1 | §9.2.2.1 | Medium | Medium |
| 2.2.2 Pause, Stop, Hide | A | §9.2.2.2 | §9.2.2.2 | Medium | Medium |
| 2.3.1 Three Flashes | A | §9.2.3.1 | §9.2.3.1 | Low | Low |
| 2.4.1 Bypass Blocks | A | §9.2.4.1 | §9.2.4.1 | Medium | Medium |
| 2.4.2 Page Titled | A | §9.2.4.2 | §9.2.4.2 | Medium | Medium |
| 2.4.3 Focus Order | A | §9.2.4.3 | §9.2.4.3 | High | High |
| 2.4.4 Link Purpose (In Context) | A | §9.2.4.4 | §9.2.4.4 | High | High |
| 2.4.5 Multiple Ways | AA | §9.2.4.5 | §9.2.4.5 | Low | Medium |
| 2.4.6 Headings and Labels | AA | §9.2.4.6 | §9.2.4.6 | Medium | Medium |
| 2.4.7 Focus Visible | AA | §9.2.4.7 | §9.2.4.7 | High | High |
| 2.4.11 Focus Not Obscured | AA | §9.2.4.11 | §9.2.4.11 | Medium | Medium |
| 2.5.1 Pointer Gestures | A | §9.2.5.1 | §9.2.5.1 | Low | Medium |
| 2.5.2 Pointer Cancellation | A | §9.2.5.2 | §9.2.5.2 | Low | Low |
| 2.5.3 Label in Name | A | §9.2.5.3 | §9.2.5.3 | Medium | Medium |
| 2.5.4 Motion Actuation | A | §9.2.5.4 | §9.2.5.4 | Low | Low |
| 2.5.7 Dragging Movements | AA | §9.2.5.7 | §9.2.5.7 | Low | Medium |
| 2.5.8 Target Size (Minimum) | AA | §9.2.5.8 | §9.2.5.8 | Medium | Medium |

### Understandable

| WCAG Criterion | Level | US 508 | EN 301 549 | ADA Risk | EAA Risk |
|----------------|-------|--------|------------|----------|----------|
| 3.1.1 Language of Page | A | §9.3.1.1 | §9.3.1.1 | High | High |
| 3.1.2 Language of Parts | AA | §9.3.1.2 | §9.3.1.2 | Low | Medium |
| 3.2.1 On Focus | A | §9.3.2.1 | §9.3.2.1 | Medium | Medium |
| 3.2.2 On Input | A | §9.3.2.2 | §9.3.2.2 | Medium | Medium |
| 3.2.3 Consistent Navigation | AA | §9.3.2.3 | §9.3.2.3 | Low | Medium |
| 3.2.4 Consistent Identification | AA | §9.3.2.4 | §9.3.2.4 | Low | Medium |
| 3.2.6 Consistent Help | AA | §9.3.2.6 | §9.3.2.6 | Low | Medium |
| 3.3.1 Error Identification | A | §9.3.3.1 | §9.3.3.1 | High | High |
| 3.3.2 Labels or Instructions | A | §9.3.3.2 | §9.3.3.2 | High | High |
| 3.3.3 Error Suggestion | AA | §9.3.3.3 | §9.3.3.3 | Medium | High |
| 3.3.4 Error Prevention (Legal/Financial) | AA | §9.3.3.4 | §9.3.3.4 | High | High |
| 3.3.7 Redundant Entry | AA | §9.3.3.7 | §9.3.3.7 | Low | Medium |
| 3.3.8 Accessible Authentication | AA | §9.3.3.8 | §9.3.3.8 | Medium | Medium |

### Robust

| WCAG Criterion | Level | US 508 | EN 301 549 | ADA Risk | EAA Risk |
|----------------|-------|--------|------------|----------|----------|
| 4.1.2 Name, Role, Value | A | §9.4.1.2 | §9.4.1.2 | High | High |
| 4.1.3 Status Messages | AA | §9.4.1.3 | §9.4.1.3 | High | High |

## EN 301 549 Requirements Beyond WCAG (EAA Relevant)

These EN 301 549 clauses are not directly mapped from WCAG but apply to web applications under the EAA:

| EN 301 549 Clause | Requirement | Relevance |
|--------------------|-------------|-----------|
| §5.2 | Activation of accessibility features | Medium — ensure a11y features (high contrast, reduced motion) are user-activatable |
| §5.3 | Biometrics | Low — if biometric auth is used, provide non-biometric alternative |
| §6.1 | Audio bandwidth for speech | Low — applies if app has voice features |
| §11.5 | Interoperability with assistive technology | High — ARIA roles, states, properties must be programmatically exposed |
| §12.1.1 | Accessibility and compatibility features documentation | Medium — document accessibility features for users |
| §12.2.4 | Accessible support services | Medium — customer support must be accessible |

## How to Use in Reports

When reporting a violation, include the `Regulations` field:

```
**[1.1.1] Non-text Content** (Level A) — Critical
Regulations: WCAG 2.2 §1.1.1, US 508 §9.1.1.1, EN 301 549 §9.1.1.1
ADA lawsuit risk: High (missing alt text is the #1 ecommerce violation)
EAA enforcement: Applicable
What's wrong: ...
```

For ecommerce platforms operating in both US and EU markets, violations marked "High" in both ADA Risk and EAA Risk columns are the top remediation priority.
