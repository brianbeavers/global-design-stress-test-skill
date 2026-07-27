# Figma Output Spec

Structure and naming for Phase 4 Figma push-back.

## Show, don't tell

Every matrix cell must contain **rendered localized UI** — not a label describing what the locale should show.

Before marking a run complete:

- [ ] Every locale × screen has a frame with translated text visible
- [ ] `get_screenshot` confirms target language in user-facing strings
- [ ] FAIL cells have baseline | locale side-by-side compare
- [ ] No summary-only section without full grid

See [visual-evidence-spec.md](visual-evidence-spec.md) and [translation-workflow.md](translation-workflow.md).

## Section hierarchy

Create one top-level section per run:

```
📁 Global Stress Test — {ProjectName} — {YYYY-MM-DD}
├── 📄 Summary
├── 📁 i18n Matrix — {localePack} ({localeCount} × {variantCount})
├── 📁 Font Scaling — risk locales
├── 📁 A11y Annotations
└── 📁 Edge cases
```

Place section on the same page as baseline or on a dedicated "Stress Test" page — ask user if unclear.

## Summary frame

Single frame at top of section:

| Element | Content |
|---------|---------|
| Title | Global Stress Test — {Project} |
| Date | {YYYY-MM-DD} |
| Baseline link | Text link to baseline node |
| Pass/fail counts | i18n: {n}/{total} · A11y: {n}/{total} · Font: {n}/{total} |
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
| Cell size | Match baseline frame dimensions |
| Spacing | 40px between cells, 80px between rows |

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

1. **Read baseline**
   - `get_design_context` with `fileKey`, `nodeId`
   - `get_screenshot` for visual reference

2. **Create section**
   - `use_figma` with `fileKey` — create section frame, set name

3. **Clone and localize**
   - Extract string inventory from baseline (translation-workflow)
   - `use_figma` — clone per locale; **set text node `characters`** to translated strings
   - `get_screenshot` per cell — verify language before marking PASS/FAIL
   - Batch by row — one `use_figma` call per locale row

4. **Font scaling clones**
   - Clone from localized frame; scale text per font-scaling-checklist

5. **Annotations**
   - `use_figma` — create `_Annotation` frames with findings text

6. **Prototype uploads** (if `executionPath` prototype or both)
   - `upload_assets` with PNG captures
   - Place images in matrix cells via `use_figma`

7. **Return links**
   - Construct `https://www.figma.com/design/{fileKey}/?node-id={sectionId}` (hyphens in URL)

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
