# Figma Presentation Fitment

Rules for **documentation frames** in the stress-test section — so clones and screenshots are never cut off by the matrix cell.

**Required reading before Phase 4.** Wire from [figma-output-spec.md](figma-output-spec.md).

## Two kinds of clipping (do not conflate)

| Kind | Meaning | Agent action |
|------|---------|--------------|
| **Product UI clipping** | Text/controls clip *inside* the design (fixed CTA, truncate) | Mark **FAIL** (i18n / font scaling). Still show the **full** screen in the documentation cell so the defect is visible. |
| **Presentation clipping** | Matrix cell, PNG crop, or parent frame crops the evidence | **Invalid deliverable.** Expand cell or Fit image; re-`get_screenshot` until full UI is visible. |

Never “fix” product clipping by cropping the documentation frame tighter.

## Presentation rules

| Asset type | Behavior |
|------------|----------|
| Localized **evaluation / matrix clone** | After text swap + font scale, measure content bounds. If content exceeds baseline height, **expand the cell frame height** (or wrap clone in an auto-layout viewport that grows). Never crop product UI to fit a fixed cell. |
| **Prototype PNG** via `upload_assets` | Scale mode **Fit** (contain), not Crop. Expand cell to match image aspect × target width, or letterbox on a clear canvas. Prefer Fit over Cut. |
| Side-by-side FAIL compare | Same **width** for both frames; heights independent (grow to content). Align tops. |
| Status badge | Outside content or pinned top-right with padding — never covers clipped chrome or crops the cell. |
| Font Scaling rows | Same fitment as i18n cells — expand documentation cell after scaling text; do not crop Large-tier reflow. |

## Per-cell fitment loop (required)

```
1. Place clone or PNG in matrix / FS cell
2. Measure content bounds (or image aspect)
3. Resize documentation cell to fit — expand height/width as needed
4. Apply status badge without reducing content area
5. get_screenshot of the FULL cell
6. Inspect: is any chrome, CTA, or bottom content cut off by the cell?
   Yes → FAIL presentation; fix resize/Fit; repeat from 2
   No  → accept cell
```

Do not mark Phase 4 complete until every matrix and Font Scaling cell passes step 6.

## Product evaluation (unchanged)

During Phases 1–2.6 **product** evaluation:

- Do **not** auto-expand **product** fixed-height parents (CTAs, card rows) — that would hide real layout failures.
- Do expand **documentation** cell frames after evaluation so the full (possibly clipped) product UI remains visible in evidence.

See [font-scaling-checklist.md](font-scaling-checklist.md).

## Anti-patterns

| Anti-pattern | Why invalid |
|--------------|-------------|
| Cell height locked to baseline while Large-font clone is taller | Presentation crop |
| PNG `Crop` / Fill that cuts edges | Evidence incomplete |
| Badge overlapping and covering FAIL region | Hides defect |
| Shrinking text/font to force fit in documentation cell | Hides product failure |
| Shipping matrix without full-cell `get_screenshot` check | Unverified deliverable |
