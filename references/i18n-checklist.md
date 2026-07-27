# i18n Checklist

Pass/fail criteria for Phase 1. Evaluate each **locale × screen variant** cell.

**Visual evidence required:** Every cell needs a screenshot or Figma frame showing **actual translated strings** in the UI. See [visual-evidence-spec.md](visual-evidence-spec.md). English-only frames → automatic **FAIL** (incomplete translation).

## Layout and copy

| Check | Pass | Fail |
|-------|------|------|
| No English fallback | All user-facing strings localized | Calendar, sheets, or CTAs show English when locale is non-English |
| Long-string stress | Labels wrap or truncate intentionally | Unexpected clip, overlap, or broken layout (DE/FR/FI/CS/RU) |
| Truncation pattern | Ellipsis or multi-line per design spec | Single-line truncation hides primary meaning |
| Dual-line labels | Secondary text visible | Badge or subtitle pushed off-screen |
| CTA width | Button accommodates longest label | CTA text clips or breaks out of tap target |
| Discount / promo rows | Single row on widget; stacking in sheet only | Dual cards on widget when sheet-only stacking is spec |

Tag machine-translated copy with `[MT]` in frame names — still valid for layout stress.

## Direction and script

| Check | Pass | Fail |
|-------|------|------|
| RTL mirroring | `ar-*` layouts mirror (auto-layout, alignment) | LTR layout in RTL locale |
| Directional icons | Chevrons, back, dismiss mirror in RTL | Icons point wrong direction |
| Mixed script | Latin codes (e.g. MIA) render correctly in Arabic UI | Bidi breaks, wrong order, orphaned glyphs |
| CJK rhythm | Japanese/Chinese line breaks natural | Orphaned characters, cramped vertical rhythm |
| Sheet mirroring | Bottom sheets, swipe dismiss mirror in RTL | Sheet chrome stays LTR |

## Locale formats

| Check | Pass | Fail |
|-------|------|------|
| Time format | 24h where locale requires; 12h AM/PM for en-US / ar-SA | Wrong clock format for locale |
| Date order | Matches locale convention | US order in EU locale |
| Number separators | Decimal/grouping match locale | Wrong separator pattern |
| Calendar headers | Localized weekday/month names | English weekday names |
| First day of week | Matches locale (if calendar shown) | Wrong week start — flag as gap if design-only |

## Per-variant guidance

Adapt screen variant names to project. Common patterns:

| Variant | Focus |
|---------|--------|
| `default` / `widget` | Primary surface — trip, location, date, primary CTA |
| `sheet` / `calendar` / `discounts` | Overlay copy, leg labels, sheet title |
| `after-hours` | Time-specific copy, moon indicator, alert banner |
| `error` | Error strings localized, not English-only |
| `empty` | Empty-state copy and CTA |

## Status values

- **PASS** — all checks pass
- **FAIL** — one or more fail checks
- **PARTIAL** — layout pass with known format gaps (document in findings)
- **SKIP** — locale not in scope for this run

## Findings format

For each FAIL, record:

```
[{locale}] [{screen}] {category}: {observation} → {recommendation}
```

Example:

```
[de-DE] [widget] Layout: CTA label clips at 2 lines → allow height growth or shorten DE copy
[ar-SA] [discounts] RTL: sheet dismiss on wrong edge → mirror sheet chrome
```
