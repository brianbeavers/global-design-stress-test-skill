# Accessibility Checklist (Design-Time)

Pass/fail criteria for Phase 2. Run per **locale × screen variant**; prioritize Phase 1 failures.

## Contrast

| Check | Pass | Fail |
|-------|------|------|
| Text on background | APCA Lc ≥ 60 for UI labels; Lc ≥ 75 for body; WCAG AA 4.5:1 text | Longest localized string drops below threshold after wrap |
| Icons | Same contrast as adjacent text | Icon-only control below 3:1 |
| Disabled state | Lc ≥ 30, still perceivable | Disabled text invisible |
| Error state | Error text meets contrast on error bg | Red-on-red below threshold |

**Delegation:** contrast failures → read `apca-compliance-figma` skill; annotate failing token pairs on Figma canvas.

Re-evaluate contrast **after** font scaling (Phase 2.5) — small scale can push secondary text below floor.

## Touch targets

| Check | Pass | Fail |
|-------|------|------|
| Minimum size | Interactive elements ≥ 44×44 pt (iOS) / 48×48 dp (Android) / 44×44 CSS px (web) | Tap target shrinks when label wraps |
| Spacing | Targets separated; no overlapping hit areas | Adjacent tappable elements overlap |
| CTA | Primary action remains full-width tappable at large font scale | CTA height collapses |

## Focus order

| Check | Pass | Fail |
|-------|------|------|
| LTR flow | Top-to-bottom, left-to-right matches visual hierarchy | Focus jumps skip visible controls |
| RTL flow | Focus follows visual RTL order | Arabic sheet: focus stays LTR |
| Modal/sheet | Focus trapped in overlay; dismiss reachable | Focus escapes behind sheet |
| Skip links | Web: skip to main content available | No bypass for keyboard users |

Document focus order as numbered list in `_Annotation / RTL focus order` frame for RTL locales.

## Non-color cues

| Check | Pass | Fail |
|-------|------|------|
| State communication | Error/success/disabled use icon, text, or pattern — not color alone | Error = red text only |
| Required fields | Asterisk, label, or aria-required — not color border only | Required state color-only |
| Links | Underline or icon in addition to color | Link distinguished by color only |

## Screen reader copy

| Check | Pass | Fail |
|-------|------|------|
| Localized labels | `accessibilityLabel` / `aria-label` intent matches locale | English announcement in non-English UI |
| Decorative elements | Hidden from AT (`aria-hidden`, `importantForAccessibility`) | Redundant noise in rotor |
| Dynamic content | Live regions for async updates | Status changes not announced |
| Calendar | Month/day announced in locale | English month names in DE UI |
| Mixed script | Document expected announcement for Latin in Arabic (e.g. "M I A" vs "Miami") | Ambiguous or wrong code pronunciation |

**Delegation:** top 3 risk locales (DE, ar-SA, ja-JP) → read `create-voice` skill; produce `{Component} Screen reader — {locale}` frames with **localized** announcement strings.

## RTL-specific a11y

| Check | Pass | Fail |
|-------|------|------|
| Reading direction | `dir=rtl` on root; logical properties used | Hardcoded left/right margins break RTL |
| Focus vs visual | Focus order matches mirrored layout | First focus stop on wrong edge |
| Directional gestures | Swipe-to-dismiss mirrors | Gesture direction LTR-only |
| Icons | Back chevron, forward arrow mirror | Directional icon not mirrored |

## Status values

Same as i18n checklist: **PASS** | **FAIL** | **PARTIAL** | **SKIP**

## Matrix columns (report)

| Column | Source check |
|--------|--------------|
| Contrast | Contrast section |
| Touch | Touch targets |
| Focus | Focus order (+ RTL) |
| Voice | Screen reader copy |

## Findings format

```
[{locale}] [{screen}] A11y/{dimension}: {observation} → {recommendation}
```

Example:

```
[de-DE] [widget] A11y/Contrast: Label Lc 52 after wrap → increase contrast token or allow 2-line label
[ar-SA] [sheet] A11y/Focus: dismiss button first in LTR order → reorder for RTL flow
[ja-JP] [calendar] A11y/Voice: month announced in English → localize accessibilityLabel
```
