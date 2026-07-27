# Mobile Booking Widget Example

Reference workflow for the **global-design-stress-test** skill — a multi-screen mobile booking flow with localized copy, RTL, and sheet overlays.

## Project profile

| Field | Example value |
|-------|----------------|
| Project name | Mobile Booking Widget |
| Platform | iOS / Android |
| Screen variants | `widget`, `after-hours`, `calendar`, `discounts` |
| Baseline locale | `en-US` |

## Sample config

```json
{
  "projectName": "Mobile Booking Widget",
  "figmaFileKey": "YOUR_FIGMA_FILE_KEY",
  "baselineNodeId": "YOUR_BASELINE_NODE_ID",
  "outputSectionName": "Global Stress Test — Mobile Booking Widget",
  "platform": "ios",
  "baselineLocale": "en-US",
  "localePack": "core",
  "screenVariants": ["widget", "after-hours", "calendar", "discounts"],
  "executionPath": "both",
  "fontScaleSteps": ["small", "default", "large"],
  "fontScaleSweepLocales": "risk-set",
  "fontScaleProfile": "platform-default",
  "prototype": {
    "devServerUrl": "http://localhost:5177",
    "scenarioParam": "scenario",
    "captureSelector": ".phone-screen",
    "fontScaleParam": "fontScale"
  }
}
```

Save as `stress-test-config.json` in your project root.

## Phase 1 — i18n (13 × 4 = 52 cells)

| Result | Detail |
|--------|--------|
| Locales | 13 (Core tier — see locale-registry) |
| Variants | widget, after-hours, calendar, discounts |
| Target | All cells PASS |

**Patterns that work well:**

- Centralized copy bundles (`widgetCopy`, `sheetCopy`) keyed by BCP-47 locale
- Scenario IDs: `i18n-{short}-{variant}` (e.g. `i18n-de-widget`, `i18n-be-nl-widget` for Belgian Dutch — see locale-registry Short codes)
- RTL via `dir=rtl` on root container for `ar-*` locales
- Batch capture script (Playwright or similar) driven by scenario URL params

## Phase 2 — A11y (scale-independent sample)

Run **after Phase 1**, **before Phase 2.5**. Do not record Contrast or Touch here — those belong in Phase 2.6.

| Locale | Screen | Focus | Voice | Status |
|--------|--------|-------|-------|--------|
| de-DE | widget | PASS | PARTIAL | PARTIAL |
| ar-SA | discounts | FAIL | FAIL | FAIL |
| ja-JP | calendar | PASS | PARTIAL | PARTIAL |

**Typical findings:**

- RTL discounts sheet: document focus order for dismiss control
- Localize screen reader announcement strings per locale

## Phase 2.5 — Font scaling (risk set)

| Locale | Screen | Small | Large | Combo |
|--------|--------|-------|-------|-------|
| de-DE | widget | PASS | FAIL | **DE+Large FAIL** — fixed CTA height |
| ar-SA | widget | PASS | PASS | — |
| ja-JP | calendar | PASS | PARTIAL | Long month header wraps |
| en-US | widget | PASS | PASS | Baseline reference |

## Phase 2.6 — A11y (post-scale sample)

Run **after Phase 2.5** on scaled screenshots (Large tier minimum). Re-check Contrast and Touch only.

| Locale | Screen | Scale | Contrast | Touch | Status |
|--------|--------|-------|----------|-------|--------|
| de-DE | widget | Large | PASS | FAIL | FAIL |
| ar-SA | discounts | Large | PASS | FAIL | FAIL |
| ja-JP | calendar | Large | PASS | PASS | PASS |
| en-US | widget | Large | PASS | PASS | PASS |

**Typical findings:**

- de-DE widget at Large: CTA height fixed — touch target below 44pt after scale
- ar-SA discounts at Large: dismiss control overlaps secondary action — touch FAIL

## Phase 3 — Report

Use [report-template.md](../references/report-template.md). Stakeholders read **Action items** first, then failure screenshots, then matrix appendix.

Example action item: `P0 · de-DE · widget · Font scaling · CTA clips at Large → min-height 48pt (Design+Eng)`

See [how-to-read-results.md](../references/how-to-read-results.md).

## Phase 4 — Figma

Create a dated section per [figma-output-spec.md](../references/figma-output-spec.md):

- Summary frame (pass/fail counts)
- i18n matrix grid (locale rows × screen columns)
- Font Scaling rows for risk locales
- `_Annotation / Font scaling` and `_Annotation / RTL focus order` sidecars

## Invoke the skill

```
Run global-design-stress-test on [Figma URL] using stress-test-config.json
```

## Lessons for other projects

1. Define `screenVariants` early — they drive matrix columns
2. Start with `localePack: core` before `extended`
3. Use `executionPath: figma` when no runnable prototype exists
4. Always evaluate **DE + Large** worst-case combo
5. Run contrast and touch checks in **Phase 2.6** on scaled UI — not in Phase 2
6. Delegate contrast and voice specs to companion skills when available
