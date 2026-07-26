---
name: global-design-stress-test
description: Stress-tests UI designs for internationalization and design-time accessibility across locales and regions, including font scaling at small, default, and large user settings. Use when the user mentions i18n stress test, multilingual design QA, RTL layout, locale matrix, global markets, dynamic type, font scaling, or accessibility + translation review. Produces a Cursor report and Figma documentation matrix.
---

# Global Design Stress Test

Orchestrates i18n layout stress, design-time accessibility, and font scaling across locales. Delivers a structured Cursor report and pushes documentation back to Figma.

## Prerequisites

Read these reference files when executing (do not duplicate their content here):

| File | When |
|------|------|
| [locale-registry.md](references/locale-registry.md) | Resolving locale packs and BCP-47 metadata |
| [i18n-checklist.md](references/i18n-checklist.md) | Phase 1 pass/fail |
| [a11y-checklist.md](references/a11y-checklist.md) | Phase 2 pass/fail |
| [font-scaling-checklist.md](references/font-scaling-checklist.md) | Phase 2.5 pass/fail |
| [report-template.md](references/report-template.md) | Phase 3 output |
| [figma-output-spec.md](references/figma-output-spec.md) | Phase 4 Figma push |

**Specialist skills** (read and delegate when flagged):

- `apca-compliance-figma` — contrast on longest localized strings
- `create-voice` — localized screen reader specs (VoiceOver / TalkBack / ARIA)
- `figma-use` — Figma MCP canvas operations

## Workflow checklist

Copy and track progress:

```
Task Progress:
- [ ] Phase 0: Load or create stress-test-config.json
- [ ] Phase 1: i18n stress (locale × screen variants)
- [ ] Phase 2: Design-time a11y checks
- [ ] Phase 2.5: Font scaling (risk locales + failures)
- [ ] Phase 3: Emit Cursor report
- [ ] Phase 4: Push Figma section + annotations
```

## Phase 0 — Configuration

1. Look for `stress-test-config.json` in the project root (or path user specifies).
2. If missing, copy from [stress-test-config.template.json](stress-test-config.template.json) and ask the user for:
   - Figma URL (extract `fileKey` and `baselineNodeId`)
   - Platform: `ios` | `android` | `web`
   - Locale pack: `core` | `extended` | `custom`
   - Screen variants (project-specific list)
   - Execution path: `figma` (default) | `prototype` | `both`
3. Parse Figma URL:
   - Standard: `figma.com/design/:fileKey/...?node-id=:nodeId` → convert `-` to `:` in nodeId
   - Branch URL: use `branchKey` as fileKey

### Config fields

| Field | Purpose |
|-------|---------|
| `localePack` | Which locales to include — see locale-registry tiers |
| `screenVariants` | Project screens to stress (e.g. widget, sheet, error) |
| `executionPath` | `figma` \| `prototype` \| `both` |
| `fontScaleSteps` | `["small", "default", "large"]` — default all three |
| `fontScaleSweepLocales` | `"risk-set"` (default) \| `"all"` \| array of BCP-47 codes |
| `fontScaleProfile` | `"platform-default"` — maps to platform table in font-scaling-checklist |

## Hybrid execution decision tree

```
1. stress-test-config.json exists?
   No → create from template, gather inputs
2. executionPath?
   figma     → Phases 1–2.5 via Figma MCP (default)
   prototype → require dev server + capture scripts
   both      → Figma matrix + prototype PNG overlay
3. localePack?
   core            → 13 locales (fast)
   extended        → core + extended markets
   custom          → user-supplied list
4. Phase 2.5 font scaling on risk locales (or all if configured)
5. Delegate flagged a11y to apca-compliance-figma + create-voice
6. Emit report → push Figma section
```

## Phase 1 — i18n stress

For each **locale × screen variant** in config, evaluate against [i18n-checklist.md](references/i18n-checklist.md).

### Figma path (default)

1. `get_design_context` + `get_screenshot` on baseline node
2. `use_figma` — create output section (see figma-output-spec)
3. Clone baseline frame per locale; swap text with localized strings
   - Machine translation acceptable — tag with `[MT]` in frame name
   - Prioritize long-string locales: DE, FI, CS, RU
4. For RTL locales (`ar-*`): mirror auto-layout, flip directional icons/chevrons
5. Apply locale format rules (24h clock, date order) per locale-registry

### Prototype path (optional)

When `executionPath` is `prototype` or `both`:

1. Start dev server from config `prototype.devServerUrl`
2. Navigate `?{scenarioParam}={scenarioId}` per locale/variant
3. Capture screenshots via Playwright or browser MCP
4. Upload via Figma MCP `upload_assets` + place in matrix section

Reference pattern: `booking-widget-prototype/scripts/capture-i18n-screens.mjs`

Record PASS/FAIL per cell. Flag English fallback, truncation, RTL mirroring failures.

## Phase 2 — Design-time accessibility

Per locale × variant (prioritize failures from Phase 1), run [a11y-checklist.md](references/a11y-checklist.md):

| Dimension | Quick check |
|-----------|-------------|
| Contrast | APCA Lc / WCAG AA on longest string + bg pair |
| Touch targets | Interactive elements ≥ 44×44 pt/dp after wrap |
| Focus order | Logical; RTL reverses appropriately |
| Non-color cues | State not color-only |
| Screen reader | Localized label intent, not English fallback |

**Delegation:**

1. Contrast failures → read `apca-compliance-figma`, annotate token pairs
2. Top 3 risk locales (DE, ar-SA, ja-JP) → read `create-voice`, produce localized voice frames

**RTL a11y:** focus order follows visual RTL; document mixed-script announcements (Latin codes in Arabic UI); mirror directional icons.

## Phase 2.5 — Font scaling

Run **after** i18n string swap. Worst case = **longest locale + largest scale**.

See [font-scaling-checklist.md](references/font-scaling-checklist.md) for platform mapping and sampling.

### Default sampling

1. All locales at **default** scale — covered in Phase 1
2. **Small + Large** on risk set: `de-DE`, `ar-SA`, `ja-JP`, plus baseline (`en-US` or `en-GB`)
3. **Large** on every locale that failed Phase 1 layout
4. Explicit combo frame: `DE · Large · {screen}` (worst case)

### Figma path

Clone locale frames; scale text nodes per platform table. Do **not** auto-expand fixed frames — surface clipping failures.

### Prototype path

Apply `?fontScale=small|large` or CSS root scaling if project supports it; capture per tier.

## Phase 3 — Cursor report

Fill [report-template.md](references/report-template.md) and post in chat.

Optionally write `docs/global-stress-test-{YYYY-MM-DD}.md` if user requests persistence.

## Phase 4 — Figma push-back

Follow [figma-output-spec.md](references/figma-output-spec.md).

### MCP sequence

1. `get_design_context` + `get_screenshot` on baseline
2. `use_figma` — create section `{outputSectionName} — {date}`
3. Build i18n matrix grid (row = locale, column = screen variant)
4. Add a11y annotation sidecars (`_Annotation / …` lavender frames)
5. Add Font Scaling rows (Small / Large for risk locales)
6. `upload_assets` — if prototype captures exist
7. Return Figma section link in report

### Frame naming

`L{n}.{m} — {Locale label} · {Screen variant} · {PASS|FAIL}`

Font scaling: `FS — {Small|Large} · {Locale} · {Screen} · {PASS|FAIL}`

Worst case: `FS — DE · Large · {Screen} · WORST CASE`

## Risk locale set (default)

When `fontScaleSweepLocales` is `"risk-set"`:

| Code | Why |
|------|-----|
| `de-DE` | Long compound strings |
| `ar-SA` | RTL + mixed script |
| `ja-JP` | CJK line breaking |
| `en-US` or `en-GB` | Baseline locale (from config) |

## Out of scope (v1)

- Runtime axe/browser audits
- Live translation API (use `[MT]` tagged strings)
- CI gates / GitHub Actions
- Project-specific scenario IDs

## Examples

See [examples/mobile-booking-widget-example.md](examples/mobile-booking-widget-example.md) for a full reference run.
