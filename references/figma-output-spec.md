# Figma Output Spec

Structure and naming for Phase 4 Figma push-back.

**Skip this entire spec when `reportOnly: true`** — visual evidence is Cursor-report-only; no Figma sections, matrix, or `upload_assets`.

## Show, don't tell

Every matrix cell must contain **rendered localized UI** — not a label describing what the locale should show.

Before marking a run complete:

- [ ] Every locale × screen has a frame with translated text visible
- [ ] `get_screenshot` confirms target language in user-facing strings
- [ ] Full cell visible — no presentation crop (see [figma-presentation-fitment.md](figma-presentation-fitment.md))
- [ ] **Issues Checklist** frame present (P0→P2) with Issue + Recommendation + Evidence
- [ ] FAIL cells have baseline | locale side-by-side compare
- [ ] No summary-only section without full grid

See [visual-evidence-spec.md](visual-evidence-spec.md), [translation-workflow.md](translation-workflow.md), and [figma-presentation-fitment.md](figma-presentation-fitment.md).

## Section hierarchy

Create one top-level section per run:

```
📁 Global Stress Test — {ProjectName} — {YYYY-MM-DD}
├── 📄 Issues Checklist — prioritized
├── 📄 Summary
├── 📁 i18n Matrix — {localePack} ({localeCount} × {variantCount})
├── 📁 Font Scaling — risk locales
├── 📁 A11y Annotations
└── 📁 Edge cases
```

Place section on the same page as baseline or on a dedicated "Stress Test" page — ask user if unclear.

## Issues Checklist frame (lead deliverable)

Create **first** in the section (above Summary). Name: `Issues Checklist — prioritized`.

Sort entries **P0 → P1 → P2**, then by category impact. Every FAIL/PARTIAL from the run must appear.

**Item format (text in frame):**

```
☐ P0 · {locale} · {screen} · {category}
Issue: {what broke — one sentence}
Recommendation: {concrete fix — copy / layout / a11y / font scaling}
Evidence: {frame name or node link — e.g. L1.1 · FS — DE · Large · widget}
Owner: {Design | Eng | Content | Design+Eng}
```

Example:

```
☐ P0 · de-DE · widget · Font scaling
Issue: Primary CTA German label clips at Large font
Recommendation: Allow 2-line wrap; set min-height 48pt
Evidence: FS — DE · Large · widget · FAIL
Owner: Design+Eng

☐ P1 · de-DE · widget · i18n
Issue: Primary button text truncates at default scale
Recommendation: Shorten DE copy or widen button
Evidence: L1.1 — German · widget · FAIL
Owner: Content
```

If overall PASS with zero FAIL/PARTIAL: frame states `No prioritized issues — all cells PASS` plus link to Summary.

Severity rules: [report-agent-guide.md](report-agent-guide.md).

## Summary frame

Single frame at top of section:

| Element | Content |
|---------|---------|
| Title | Global Stress Test — {Project} |
| Date | {YYYY-MM-DD} |
| Baseline link | Text link to baseline node |
| Pass/fail counts | i18n: {n}/{total} · A11y: {n}/{total} · Font: {n}/{total} · **P0: {n}** · P1: {n} |
| Legend | PASS = green label · FAIL = red label · PARTIAL = amber |
| Overall | PASS / FAIL / PARTIAL badge |

Use design system text styles where available; fallback Inter 14/12.

## i18n Matrix grid

**Layout:** rows = locales, columns = screen variants.

| Property | Value |
|----------|-------|
| Row label | `L{n} — {Locale label} ({code})` |
| Column header | `{Screen variant}` |
| Cell frame name | `L{n}.{m} — {Locale} · {Variant} · {PASS\|FAIL\|PARTIAL}` |
| Cell size | Start from baseline width; **expand height** to fit content after localize/scale (see fitment) |
| Spacing | 40px between cells, 80px between rows |
| Fitment | Full cell visible after `get_screenshot` — never crop documentation cell |

**RTL locales:** mirror cell content; add `RTL` tag in frame name for ar-*.

**MT strings:** append `[MT]` to frame name when machine-translated — **text inside frame must still show the translation**.

**Failure compare:** for FAIL cells, place baseline frame immediately left of failing frame:

```
[ en-US · widget · BASELINE ]  [ de-DE · widget · FAIL ]
```

Add `_Annotation / {locale} · {screen} — {defect}` callout with English → translated string and layer name.

## Font Scaling section

Sub-section under main section or sibling folder:

```
📁 Font Scaling — {tier}
├── Row: FS — Small · {locale} · {screen} · {status}
├── Row: FS — Large · {locale} · {screen} · {status}
└── Highlight: FS — DE · Large · {screen} · WORST CASE
```

Align Small/Large rows horizontally per locale for comparison.

## A11y Annotations

Sidecar frames (lavender `#F3E8FF` background pattern from reconstruct-component-figma):

| Frame name | Contents |
|------------|----------|
| `_Annotation / Contrast failures` | Token pair, Lc value, failing element, fix |
| `_Annotation / RTL focus order` | Numbered focus stops for ar-SA screens |
| `_Annotation / Font scaling — {screen}` | Fixed-height elements, truncate flags |
| `{Component} Screen reader — {locale}` | From create-voice template |

Place annotations to the **right** of the frame they document, 40px gap.

## Edge cases folder

Include when applicable:

| Frame | Purpose |
|-------|---------|
| DE · long-string overflow | Longest label stress |
| ar-SA · mixed script | Latin code in Arabic UI |
| DE · Large · {screen} | Worst-case combo |
| {empty/error variant} | Localized empty/error states |

## Naming conventions

### i18n cells

```
L{row}.{col} — {Locale label} · {Screen variant} · {PASS|FAIL|PARTIAL}
```

Example: `L1.1 — German · Book widget · PASS`

### Font scaling

```
FS — {Small|Large|XL} · {Locale label} · {Screen variant} · {PASS|FAIL}
FS — DE · Large · Book widget · WORST CASE
```

### Annotations

```
_Annotation / {Topic} — {optional scope}
```

## MCP operation sequence

**Phase 4 only** (`reportOnly: false`). Phases 1–2.6: figma-only runs use **evaluation clones** (see visual-evidence-spec Figma write policy); never create `{outputSectionName}` or call `upload_assets` before Phase 4.

1. **Read baseline**
   - `get_design_context` with `fileKey`, `nodeId`
   - `get_screenshot` for visual reference

2. **Create section**
   - `use_figma` with `fileKey` — create section frame, set name

3. **Issues Checklist frame**
   - Create `Issues Checklist — prioritized` as first child
   - Populate P0→P2 items from Phase 1–2.6 findings (Issue + Recommendation + Evidence + Owner)

4. **Clone and localize** (or promote eval clones)
   - Extract string inventory from baseline (translation-workflow)
   - `use_figma` — clone per locale; **set text node `characters`** to translated strings
   - **Fitment:** measure bounds → resize documentation cell to fit → badge
   - `get_screenshot` full cell — verify language **and** no presentation crop
   - Batch by row — one `use_figma` call per locale row

5. **Font scaling clones**
   - Clone from localized frame; scale text per font-scaling-checklist
   - Expand documentation cell after scale; `get_screenshot` full cell

6. **Annotations**
   - `use_figma` — create `_Annotation` frames with findings text

7. **Prototype uploads** (if `executionPath` is `prototype` or `both`)
   - Use PNGs **buffered from Phase 1/2.5** — do not re-capture
   - `upload_assets` **after** section + matrix cell frames exist
   - Place images with **Fit** (contain); resize cell; `get_screenshot` full cell

8. **Fitment pass**
   - Re-check every matrix and FS cell per [figma-presentation-fitment.md](figma-presentation-fitment.md)
   - Fix any cropped cells before FINAL

9. **Return links**
   - Construct `https://www.figma.com/design/{fileKey}/?node-id={sectionId}` (hyphens in URL)
   - Include Issues Checklist node link in report header

## use_figma page context

When accessing nodes from previous steps:

```javascript
let _p = node;
while (_p.parent && _p.parent.type !== 'DOCUMENT') _p = _p.parent;
if (_p.type === 'PAGE') await figma.setCurrentPageAsync(_p);
```

Insert after `getNodeByIdAsync` in every script.

## Status labels in Figma

Add a small status badge component or text tag in corner of each cell:

| Status | Suggested color token |
|--------|----------------------|
| PASS | Green / success |
| FAIL | Red / error |
| PARTIAL | Amber / warning |
| SKIP | Gray / disabled |

New runs create a **new dated section** — do not overwrite prior matrices unless the user requests.
