# Font Scaling Checklist

Pass/fail criteria for Phase 2.5. Run **after** i18n string swap so worst case = **longest locale + largest scale**.

Every font-scale tier requires a **screenshot** showing scaled text in the UI. See [visual-evidence-spec.md](visual-evidence-spec.md).

## Why separate from i18n

Font scaling changes line height, reflow, icon-to-label ratios, and fixed-height clipping — not just string length. WCAG 1.4.4 (web) requires 200% zoom without loss of content/function. iOS Dynamic Type and Android font/display size are native equivalents.

## Stress tiers

| Tier | User setting | When to run |
|------|--------------|-------------|
| **Small** | Minimum comfortable / system Small | Risk locales + baseline |
| **Default** | Design baseline (100%) | All locales — Phase 1 matrix |
| **Large** | High accessibility setting | Risk locales + Phase 1 failures |
| **XL** (optional) | Web 200% zoom | Baseline + longest locale only |

Config: `fontScaleSteps` — default `["small", "default", "large"]`.

## Platform mapping

Use `fontScaleProfile: "platform-default"` unless project defines custom tokens.

### iOS (Dynamic Type)

| Tier | Setting | Approx. scale |
|------|---------|---------------|
| Small | Extra Small | ~85% of body |
| Default | Body / Large (design spec) | 100% |
| Large | Accessibility Large | ~135–160% |
| XL | Accessibility XXXL | ~190%+ |

Apply via typography variable modes if design system defines Dynamic Type tokens; otherwise scale `fontSize` on text nodes in Figma.

### Android

| Tier | Setting | Approx. scale |
|------|---------|---------------|
| Small | System font size −1 / smallest | ~85% |
| Default | Default | 100% |
| Large | Font size +2 / Display size Large | ~130–150% |
| XL | Largest combined font + display | ~180%+ |

### Web

| Tier | Setting | Approx. scale |
|------|---------|---------------|
| Small | `font-size: 87.5%` on `:root` or 90% zoom | ~87–90% |
| Default | 100% | 100% |
| Large | `font-size: 125–150%` on `:root` | 125–150% |
| XL | 200% browser zoom | 200% (WCAG 1.4.4) |

Prototype hook: `?fontScale=small|default|large|xl` via config `prototype.fontScaleParam`.

## Sampling strategy

Do **not** run all locales × all scales × all screens by default.

### Default (`fontScaleSweepLocales: "risk-set"`)

1. **Default scale** — all locales in Phase 1 matrix
2. **Small + Large** on risk set: `de-DE`, `ar-SA`, `ja-JP`, + config `baselineLocale`
3. **Large** on every locale that **failed** Phase 1 layout
4. **Worst-case combo** frame: `DE · Large · {screen}` (longest locale + largest scale)

### All locales (`fontScaleSweepLocales: "all"`)

Run Small + Large for every locale — use only for release sign-off; warn user about matrix size.

### Custom array

```json
"fontScaleSweepLocales": ["de-DE", "ar-SA", "fr-FR"]
```

## Pass criteria

| Check | Pass | Fail |
|-------|------|------|
| Primary content | No clip or truncate without intentional ellipsis | Fixed-height row clips translated text at Large |
| CTAs | Remain ≥ 44×44 tappable at Large | CTA text overflows or shrinks hit area |
| Sheets/modals | Scroll or grow; content reachable | Content permanently hidden below fold |
| Icon + text | No overlap after reflow | Badge overlaps body text |
| RTL at Large | Mirroring preserved after reflow | RTL breaks only at Large scale |
| Small legibility | Secondary labels readable; APCA floor met | Microcopy illegible at Small |
| Horizontal scroll | None on primary screen (unless designed) | Unexpected horizontal scroll at Large |
| Single-line inputs | Labels visible or wrap | Truncated user-visible labels |

## Fail criteria (explicit)

- Layout passes at Default but fails at Large — **FAIL** (ship criteria must not assume default type only)
- Fixed-height containers with `truncate` or `HUG` disabled — flag in `_Annotation / Font scaling` sidecar
- DE + Large combo fails — **Critical finding** (always escalate)

## Figma execution

1. Clone locale frame from Phase 1
2. Scale text nodes per platform table — **do not** auto-expand fixed parent frames
3. Name: `FS — {Small|Large} · {Locale} · {Screen} · {PASS|FAIL}`
4. Worst case: `FS — DE · Large · {Screen} · WORST CASE`
5. Sidecar: `_Annotation / Font scaling — {screen}` listing:
   - Elements with fixed height
   - `truncate` / single-line constraints
   - Non-wrapping auto-layout

## Prototype execution

1. Append `?fontScale=large` (or config param) to capture URL
2. Capture screenshot per tier
3. Upload to Font Scaling section via `upload_assets` — **skip when `reportOnly: true`**; embed tiers in Cursor report only

## Report columns

| Column | Meaning |
|--------|---------|
| Font Small | PASS/FAIL at Small tier (risk locales) |
| Font Large | PASS/FAIL at Large tier |
| Scale+Locale combo | Worst-case result (e.g. DE+Large FAIL) |

## Findings format

```
[{locale}] [{screen}] FontScale/{tier}: {observation} → {recommendation}
```

Example:

```
[de-DE] [widget] FontScale/Large: CTA label wraps to 3 lines, clips → increase min-height or shorten copy
[ar-SA] [sheet] FontScale/Large: RTL header overlaps body → fix auto-layout hug height
[de-DE] [widget] FontScale/Combo: DE+Large WORST CASE FAIL → critical: fix before ship
```
