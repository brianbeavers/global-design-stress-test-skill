# Visual Evidence Spec — Show, Don't Tell

Every stress-test run must **show** localized and accessibility-stressed UI — not only report PASS/FAIL in prose.

## Core rule

> **No finding is valid without visual proof.**  
> Placeholder labels, empty frames, or English-only screenshots do **not** count as a completed cell.

A completed cell includes:

1. **Translated strings** visible in the UI (machine translation OK — tag `[MT]` in frame name)
2. **Screenshot or Figma frame** of the full screen (not a cropped label)
3. **PASS/FAIL badge** derived from what is visible, not inferred

## Output channels

| Channel | When required |
|---------|----------------|
| **Cursor chat** | Always — embedded screenshots for every locale × screen (default `embedScreenshotsInReport: true`) |
| **Figma matrix** | When `reportOnly` is `false` (default) — full locale grid with translated UI |

When `reportOnly: true`, the **Cursor report is the sole visual deliverable**. Skip Figma matrix creation, `upload_assets`, and Phase 4 entirely. Use the **reportOnly branch** of [report-template.md](report-template.md) — omit Figma deliverables and placeholder URLs. Prototype PNGs or Figma MCP screenshots still feed the report — they are not uploaded to Figma.

When `reportOnly: false` (default), both Cursor embeds **and** Figma matrix are mandatory. Phase 3 report uses the full [report-template.md](report-template.md) including Figma deliverables.

## Full matrix — all locales

When `visualEvidence: "full-matrix"` (default):

- Produce **every** locale × screen variant cell — not failures only
- PASS cells still get a frame and screenshot
- FAIL cells additionally get baseline comparison (see below)

## Translation requirements

### Extract source strings first

Before swapping copy:

1. `get_design_context` on baseline frame(s)
2. Build a **string inventory**: `{ nodeId, layerName, enText, role }` for each user-facing text node
3. Translate inventory per target locale (prefer project copy bundles; else machine-translate)

### Apply translations — Figma path

For each locale row:

1. Clone baseline frame
2. **Set `characters` on each text node** to the translated string — do not only rename the frame
3. Apply RTL mirroring for `ar-*` (layout + directional icons)
4. Apply locale formats (dates, times, numbers)
5. `get_screenshot` on the frame — **verify** text is not still English
6. If screenshot shows English fallback → mark **FAIL** (incomplete translation)

**Banned:** frames named `de-widget · FAIL` with English text inside.

### Apply translations — Prototype path

When `executionPath` is `prototype` or `both`:

1. If project has i18n copy bundles / scenario presets — use them
2. If not — inject translations via documented hooks or temporary locale files the agent creates (tag `[MT]`)
3. Capture PNG per locale × variant via Playwright or browser MCP
4. **If `reportOnly` is `false`:** `upload_assets` → place PNGs in Figma matrix (requires valid `figmaFileKey`)
5. **Always:** embed PNGs in Cursor report

When `reportOnly` is `true`, **skip step 4** — prototype captures go to the Cursor report only. Do not call `upload_assets` or create a Figma matrix.

If no prototype exists, Figma path alone is sufficient — do not skip matrix unless `reportOnly: true`.

## Failure comparison layout

For each **FAIL** cell, show baseline vs failing locale:

### Figma

Place side by side (40px gap):

```
[ L{n}.{m} — en-US · widget · BASELINE ]  [ L{n}.{m} — de-DE · widget · FAIL ]
```

Add callout annotation listing:

- Failing element (layer name)
- English string → translated string
- Visible defect (clip, overlap, overflow, RTL break)

### Cursor report

```markdown
#### de-DE · widget — FAIL: CTA clip

| Baseline (en-US) | German (de-DE) |
|------------------|----------------|
| ![en-us-widget](...) | ![de-de-widget](...) |

- **Element:** Primary CTA button
- **English:** "Book now"
- **German:** "Jetzt buchen und sparen" [MT]
- **Defect:** Label clips at 2 lines; bottom descenders cut off
```

## Font scaling visual evidence

For each risk locale (or all locales if `fontScaleSweepLocales: "all"`):

1. Clone localized frame
2. Scale text per [font-scaling-checklist.md](font-scaling-checklist.md)
3. Screenshot Small / Default / Large tiers
4. Place in Figma Font Scaling section (skip when `reportOnly: true`)
5. Embed failure tiers in Cursor report with side-by-side Default vs Large

## Accessibility visual evidence

Design-time a11y failures must reference a screenshot:

- Contrast failure → crop or full frame showing text/bg pair
- RTL focus order → annotated frame with numbered focus stops overlaid
- Font scaling → Default vs Large comparison (same as above)

## Agent verification loop (required)

After each locale frame or capture:

```
1. Apply translations / scale
2. get_screenshot (Figma MCP) or browser screenshot (prototype)
3. Inspect: are user-facing strings in target language?
   No  → FAIL "incomplete translation" + fix before continuing
   Yes → evaluate layout/a11y checklist against screenshot
4. Record PASS/FAIL + attach screenshot to report buffer
5. Push frame to Figma matrix — **skip when `reportOnly: true`**
```

Do not advance to the next locale until step 3 passes for string visibility.

## Execution path matrix

| executionPath | reportOnly | Figma matrix | Prototype PNGs | Cursor embeds |
|---------------|------------|--------------|----------------|---------------|
| `figma` | `false` | Required — translated clones | — | Screenshots from Figma MCP |
| `figma` | `true` | Skip — MCP capture for report only | — | Required (sole deliverable) |
| `prototype` | `false` | Required — `upload_assets` PNGs | Required | Same PNGs embedded |
| `prototype` | `true` | **Skip** — no `upload_assets` | Required | Required (sole deliverable) |
| `both` | `false` | Required | Required | Both sources embedded |
| `both` | `true` | Skip | Required | Required (sole deliverable) |

**Figma-free mode:** `executionPath: "prototype"` + `reportOnly: true` — no `figmaFileKey`, no Phase 4, no `upload_assets`. Visual evidence lives in the Cursor report only.

**`both` + `reportOnly: true`:** run Figma MCP and prototype captures for cross-check; embed both in the Cursor report — do **not** push Figma matrix or call `upload_assets`.

## Anti-patterns (never ship)

| Anti-pattern | Why invalid |
|--------------|-------------|
| Summary frame only, no locale grid | User cannot see translations |
| Frame labels describe locale but UI is English | Placeholder — your reported issue |
| PASS/FAIL table with no images | Tell without show |
| Failures-only matrix | User asked for all locales |
| Skipping Phase 4 when `reportOnly: false` | Figma is primary async deliverable |
| Prototype + `reportOnly: true` but calling `upload_assets` | Contradicts Figma-free mode — report only |
| "CTA probably clips in DE" without screenshot | Inference without evidence |

## Config

```json
{
  "visualEvidence": "full-matrix",
  "embedScreenshotsInReport": true,
  "failureComparisonBaseline": "en-US",
  "reportOnly": false
}
```

| Field | Default | Purpose |
|-------|---------|---------|
| `visualEvidence` | `"full-matrix"` | All locales get visual cells |
| `embedScreenshotsInReport` | `true` | Inline images in Cursor chat |
| `failureComparisonBaseline` | `"en-US"` | Side-by-side compare locale |
| `reportOnly` | `false` | `true` = Cursor report only; skip Figma matrix, `upload_assets`, and Phase 4 |
