# Accessibility Checklist (Design-Time)

Pass/fail criteria split across **Phase 2** (scale-independent) and **Phase 2.6** (post-scale). **Do not mark Contrast or Touch PASS until Phase 2.6 completes.**

## Phase 2 — Scale-independent (run after Phase 1, before Phase 2.5)

Evaluate on **default-scale** localized UI screenshots.

### Focus order

| Check | Pass | Fail |
|-------|------|------|
| LTR flow | Top-to-bottom, left-to-right matches visual hierarchy | Focus jumps skip visible controls |
| RTL flow | Focus follows visual RTL order | Arabic sheet: focus stays LTR |
| Modal/sheet | Focus trapped in overlay; dismiss reachable | Focus escapes behind sheet |
| Skip links | Web: skip to main content available | No bypass for keyboard users |

Document focus order as numbered list in `_Annotation / RTL focus order` frame for RTL locales.

### Non-color cues

| Check | Pass | Fail |
|-------|------|------|
| State communication | Error/success/disabled use icon, text, or pattern — not color alone | Error = red text only |
| Required fields | Asterisk, label, or aria-required — not color border only | Required state color-only |
| Links | Underline or icon in addition to color | Link distinguished by color only |

### Screen reader copy

| Check | Pass | Fail |
|-------|------|------|
| Localized labels | `accessibilityLabel` / `aria-label` intent matches locale | English announcement in non-English UI |
| Decorative elements | Hidden from AT (`aria-hidden`, `importantForAccessibility`) | Redundant noise in rotor |
| Dynamic content | Live regions for async updates | Status changes not announced |
| Calendar | Month/day announced in locale | English month names in DE UI |
| Mixed script | Document expected announcement for Latin in Arabic (e.g. "M I A" vs "Miami") | Ambiguous or wrong code pronunciation |

**Delegation:** top 3 risk locales (DE, ar-SA, ja-JP) → read `create-voice` skill; produce `{Component} Screen reader — {locale}` frames with **localized** announcement strings.

### RTL-specific a11y

| Check | Pass | Fail |
|-------|------|------|
| Reading direction | `dir=rtl` on root; logical properties used | Hardcoded left/right margins break RTL |
| Focus vs visual | Focus order matches mirrored layout | First focus stop on wrong edge |
| Directional gestures | Swipe-to-dismiss mirrors | Gesture direction LTR-only |
| Icons | Back chevron, forward arrow mirror | Directional icon not mirrored |

---

## Phase 2.6 — Post-scale (run after Phase 2.5 font scaling)

Evaluate on **scaled UI screenshots** (Small and/or Large tiers). Required before final PASS/FAIL.

### Contrast

| Check | Pass | Fail |
|-------|------|------|
| Text on background | APCA Lc ≥ 60 for UI labels; Lc ≥ 75 for body; WCAG AA 4.5:1 text at **scaled** size | Longest localized string drops below threshold after wrap at Large tier |
| Small tier legibility | Secondary text still meets Lc ≥ 30 floor | Microcopy illegible at Small |
| Icons | Same contrast as adjacent text at scaled size | Icon-only control below 3:1 after scale |
| Disabled state | Lc ≥ 30, still perceivable at scaled size | Disabled text invisible |
| Error state | Error text meets contrast on error bg at scaled size | Red-on-red below threshold |

**Delegation:** contrast failures → read `apca-compliance-figma` skill; annotate failing token pairs on **scaled** Figma frames.

Default-scale contrast alone is **insufficient** — always re-check on Large tier (and Small when secondary text is at risk).

### Touch targets

| Check | Pass | Fail |
|-------|------|------|
| Minimum size at Large | Interactive elements ≥ 44×44 pt (iOS) / 48×48 dp (Android) / 44×44 CSS px (web) | Tap target shrinks when label wraps at Large scale |
| CTA at Large | Primary action remains tappable | CTA height collapses under scaled text |
| Spacing | Targets separated; no overlapping hit areas after reflow | Adjacent tappable elements overlap at Large |

---

## Status values

**PASS** | **FAIL** | **PARTIAL** | **SKIP**

Overall a11y status for a cell = worst of Phase 2 and Phase 2.6 dimensions.

## Matrix columns (report)

| Column | Phase |
|--------|-------|
| Focus | 2 |
| Voice | 2 |
| Contrast | **2.6** (post-scale) |
| Touch | **2.6** (post-scale) |

## Findings format

```
[{locale}] [{screen}] A11y/{dimension}: {observation} → {recommendation}
[{locale}] [{screen}] A11y/Contrast@Large: {observation} → {recommendation}
```

Examples:

```
[de-DE] [widget] A11y/Contrast@Large: Label Lc 52 after wrap at Accessibility Large → increase contrast or allow 2-line label
[de-DE] [widget] A11y/Touch@Large: CTA height 36pt after DE label wraps → min-height 44pt
[ar-SA] [sheet] A11y/Focus: dismiss button first in LTR order → reorder for RTL flow
```
