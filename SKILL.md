---
name: global-design-stress-test
description: Stress-tests UI designs for internationalization and design-time accessibility across 13 core languages (30 locales extended), including font scaling at small, default, and large user settings. Use when the user mentions i18n stress test, multilingual design QA, RTL layout, locale matrix, global markets, German French Arabic Japanese, dynamic type, font scaling, or accessibility + translation review. Produces a Cursor report and Figma documentation matrix.
---

# Global Design Stress Test

Orchestrates i18n layout stress, design-time accessibility, and font scaling across locales. Delivers a structured Cursor report **with embedded screenshots** and pushes a **full visual matrix** back to Figma.

## Show, don't tell (mandatory)

Every run must **show** translated and accessibility-stressed UI — not only PASS/FAIL prose.

| Requirement | Detail |
|-------------|--------|
| **All locales** | Full matrix — every locale × screen gets a visual cell |
| **Real translations** | Actual target-language strings in UI — not placeholder frame labels |
| **Both outputs** | Figma matrix + Cursor chat with embedded screenshots |
| **Both paths** | Figma clone and/or prototype capture — use what the project has |
| **Failure compare** | Side-by-side baseline vs failing locale with callouts |

Read [visual-evidence-spec.md](references/visual-evidence-spec.md) and [translation-workflow.md](references/translation-workflow.md) before Phase 1.

**Banned:** Figma sections with placeholder labels and English-only frames inside.

Phase 4 (Figma) is **not optional** unless `reportOnly: true` in config. When `reportOnly: true`, visual evidence is **Cursor-report-only** — no Figma matrix, no `upload_assets`, no Phase 4. Pair with `executionPath: "prototype"` when Figma is unavailable.

## Languages covered

**Core (default — 13 languages):** German, French, Italian, Dutch, Spanish, Portuguese, Arabic (RTL), Chinese, Japanese, Korean, Dutch (Belgium), Polish, Norwegian.

**Extended (+17 locales, 30 total):** adds English (UK, IE, US, ZA), Czech, German (AT, CH, LU), French (CH, LU, BE), Italian (CH), Danish, Finnish, Greek, Russian, Swedish. Gulf markets via Arabic.

See [locale-registry.md](references/locale-registry.md) or [README.md](README.md#languages-stress-tested).

## Prerequisites

Read these reference files when executing (do not duplicate their content here):

| File | When |
|------|------|
| [visual-evidence-spec.md](references/visual-evidence-spec.md) | **Required** — show-don't-tell rules, screenshots, full matrix |
| [translation-workflow.md](references/translation-workflow.md) | **Required** — extract strings, swap real translations |
| [locale-registry.md](references/locale-registry.md) | Resolving locale packs and BCP-47 metadata |
| [i18n-checklist.md](references/i18n-checklist.md) | Phase 1 pass/fail |
| [a11y-checklist.md](references/a11y-checklist.md) | Phase 2 + Phase 2.6 pass/fail |
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
- [ ] Phase 2: Accessibility — scale-independent (focus, voice, non-color, RTL)
- [ ] Phase 2.5: Font scaling (risk locales + failures)
- [ ] Phase 2.6: Accessibility — post-scale (contrast, touch targets on scaled UI)
- [ ] Phase 3: Emit Cursor report
- [ ] Phase 4: Push Figma section + annotations (skip if `reportOnly: true`)
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
4. **Validate config** before Phase 1 — see [locale-registry.md](references/locale-registry.md) Phase 0 validation:
   - If `localePack` is `custom`, `customLocales` must contain at least one BCP-47 code (empty `[]` is **CONFIG_INVALID**)
   - Resolved locale count and `screenVariants` must each be ≥ 1
   - Log expected matrix size: `locales × screenVariants` cells
   - When Figma MCP will run: reject template placeholders for `figmaFileKey` (`YOUR_FIGMA_FILE_KEY`, etc.) and `baselineNodeId` (`0000:0000`, etc.) — parse Figma URL first if user provided one
   - Reject default `projectName` (`Your Project Name`)
   - **STOP** and ask the user to fix config — do not run Phases 1–4 with invalid or placeholder values

### Config fields

| Field | Purpose |
|-------|---------|
| `projectName` | Report and Figma section title — must not remain template default |
| `figmaFileKey` | Figma file key — required when Figma MCP runs; no template placeholders |
| `baselineNodeId` | Baseline frame node ID (`1234:5678`) — required when Figma MCP runs; no template placeholders |
| `localePack` | Which locales to include — see locale-registry tiers |
| `customLocales` | BCP-47 codes — **required non-empty** when `localePack` is `custom`; ignored otherwise |
| `screenVariants` | Project screens to stress (e.g. widget, sheet, error) — must be non-empty |
| `executionPath` | `figma` \| `prototype` \| `both` |
| `fontScaleSteps` | `["small", "default", "large"]` — default all three |
| `fontScaleSweepLocales` | `"risk-set"` (default) \| `"all"` \| array of BCP-47 codes |
| `fontScaleProfile` | `"platform-default"` — maps to platform table in font-scaling-checklist |
| `visualEvidence` | `"full-matrix"` (default) — visual cell for every locale × screen |
| `embedScreenshotsInReport` | `true` (default) — inline images in Cursor chat |
| `failureComparisonBaseline` | `"en-US"` — side-by-side compare for FAIL cells |
| `reportOnly` | `false` — set `true` for Cursor-report-only (skip Figma matrix, `upload_assets`, Phase 4) |

## Hybrid execution decision tree

```
1. stress-test-config.json exists?
   No → create from template, gather inputs
2. executionPath?
   figma     → Phases 1–2.6 via Figma MCP (default)
   prototype → Phases 1–2.6 via dev server + capture scripts
   both      → Figma matrix + prototype PNG overlay; Phases 1–2.6 on both
3. localePack?
   core            → 13 locales (fast)
   extended        → core + extended markets
   custom          → user-supplied list (customLocales must be non-empty)
   → Validate: len(locales) >= 1 and len(screenVariants) >= 1
      Fail → STOP (CONFIG_INVALID), do not proceed
   → Validate Figma targets when executionPath is figma/both or reportOnly is false
      Reject YOUR_FIGMA_FILE_KEY, 0000:0000, and other template placeholders
      Fail → STOP (CONFIG_INVALID), ask for Figma URL
4. All paths — Phase 2.5 font scaling on risk locales (or all if configured)
5. All paths — Phase 2.6 post-scale a11y on scaled screenshots (contrast + touch — mandatory)
6. Delegate flagged a11y to apca-compliance-figma + create-voice
7. reportOnly?
   true  → Cursor report with embedded screenshots only — skip Phase 4 and all upload_assets
   false → push Figma full visual matrix after report
```

## Phase 1 — i18n stress

For each **locale × screen variant** in config, evaluate against [i18n-checklist.md](references/i18n-checklist.md).

Follow [translation-workflow.md](references/translation-workflow.md) for string inventory and swap. Follow [visual-evidence-spec.md](references/visual-evidence-spec.md) for screenshots.

### Figma path

1. `get_design_context` + `get_screenshot` on baseline node
2. Build **string inventory** from baseline text nodes
3. Translate inventory per locale (project copy or `[MT]`)
4. `use_figma` — create output section; clone baseline per locale (skip matrix push when `reportOnly: true` — capture screenshots for report only)
5. **Set `characters` on text nodes** to translated strings — verify with `get_screenshot` that UI is not English
6. RTL mirroring for `ar-*`; locale formats per locale-registry
7. Push each cell to matrix; buffer screenshot for Cursor report — **skip matrix push when `reportOnly: true`**
8. FAIL cells: add side-by-side baseline compare frame — in report when `reportOnly: true`; in Figma when `reportOnly: false`

### Prototype path

When `executionPath` is `prototype` or `both`:

1. Apply translations via scenario presets, copy bundles, or temporary `[MT]` locale files
2. Capture PNG per locale × variant (Playwright or browser MCP)
3. **Verify** PNG shows target language — reject English-only captures
4. **If `reportOnly` is `false`:** `upload_assets` → place in Figma matrix (requires valid `figmaFileKey`)
5. Embed PNGs in Cursor report (always — sole visual channel when `reportOnly: true`)

If prototype unavailable, Figma path alone satisfies visual evidence (unless `reportOnly: true` with no Figma — ask user for Figma URL or runnable prototype).

Record PASS/FAIL per cell **from screenshot evidence**, not inference.

## Phase 2 — Accessibility (scale-independent)

Run **after Phase 1**, **before Phase 2.5**. Per locale × variant, run the **scale-independent** sections of [a11y-checklist.md](references/a11y-checklist.md):

| Dimension | When evaluated |
|-----------|----------------|
| Focus order | Default-scale localized UI |
| Non-color cues | Default scale |
| Screen reader copy | Default scale |
| RTL-specific a11y | Default scale (ar-*) |

**Do not mark Contrast or Touch targets PASS/FAIL here** — those require Phase 2.6 on scaled UI.

**Delegation (scale-independent):**

Top 3 risk locales (DE, ar-SA, ja-JP) → read `create-voice`, produce localized voice frames.

## Phase 2.5 — Font scaling

Run **after Phase 2** (scale-independent a11y). Worst case = **longest locale + largest scale**.

See [font-scaling-checklist.md](references/font-scaling-checklist.md) for platform mapping and sampling.

### Default sampling

1. All locales at **default** scale — covered in Phase 1
2. **Small + Large** on risk set: `de-DE`, `ar-SA`, `ja-JP`, plus baseline (`en-US` or `en-GB`)
3. **Large** on every locale that failed Phase 1 layout
4. Explicit combo frame: `DE · Large · {screen}` (worst case)

### Figma path

Clone locale frames; scale text nodes per platform table. Do **not** auto-expand fixed frames — surface clipping failures. `get_screenshot` each tier.

### Prototype path

Apply `?fontScale=small|large` or CSS root scaling if project supports it; capture per tier.

## Phase 2.6 — Accessibility (post-scale)

Run **after Phase 2.5** — mandatory. Re-open [a11y-checklist.md](references/a11y-checklist.md) **Post-scale** sections only:

| Dimension | Evaluate on |
|-----------|-------------|
| **Contrast** | Small + Large tier screenshots — longest string after wrap/reflow |
| **Touch targets** | Large tier (and Small if CTA shrinks) — ≥ 44×44 pt/dp |

Rules:

- A cell **cannot** be marked PASS for Contrast or Touch in the report until Phase 2.6 completes on scaled UI.
- If default-scale contrast passed but Large tier fails → overall **FAIL** for that dimension.
- Embed scaled screenshots as evidence per [visual-evidence-spec.md](references/visual-evidence-spec.md).

**Delegation (post-scale contrast):** failures → read `apca-compliance-figma`, annotate on scaled frames.

## Phase 3 — Cursor report

Fill [report-template.md](references/report-template.md) and post in chat.

**Required:** embed screenshots for **every locale × screen** when `embedScreenshotsInReport` is true (default). FAIL cells include baseline side-by-side tables per [visual-evidence-spec.md](references/visual-evidence-spec.md).

Optionally write `docs/global-stress-test-{YYYY-MM-DD}.md` if user requests persistence (include image paths or links).

## Phase 4 — Figma push-back

Follow [figma-output-spec.md](references/figma-output-spec.md). **Skip entire phase** when `reportOnly: true` — do not call `upload_assets` or create Figma sections.

### MCP sequence

1. `get_design_context` + `get_screenshot` on baseline
2. `use_figma` — create section `{outputSectionName} — {date}`
3. Build **full** i18n matrix — every locale × variant with **translated UI inside frames**
4. Add failure comparison pairs (baseline | failing locale)
5. Add a11y annotation sidecars with screenshot refs
6. Add Font Scaling rows with Small / Large screenshots
7. `upload_assets` — when prototype captures exist (`both` or `prototype`) **and `reportOnly` is `false`**
8. Return Figma section link; confirm no placeholder-only cells

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
