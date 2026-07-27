# Global Design Stress Test — {Project}

**Date:** {YYYY-MM-DD}  
**Locale pack:** {core | extended | custom}  
**Platform:** {ios | android | web}  
**Execution path:** {figma | prototype | both}  
**Report mode:** {reportOnly ? `Cursor-report-only (Figma-free)` : `Cursor + Figma`}

<!-- Agent: branch this template on config.reportOnly — see "Template branching" below -->

**Figma baseline:** {include only when `reportOnly` is `false`}  
[{baselineNodeId}](https://www.figma.com/design/{fileKey}/?node-id={nodeIdHyphen})

**Design reference:** {include when `reportOnly` is `true` — prototype URL, screenshot bundle, or "captured via browser MCP"}

---

## Template branching (agents — do not paste into stakeholder report)

| `reportOnly` | Include in report | Omit / mark N/A |
|--------------|-------------------|-----------------|
| `false` (default) | Figma baseline link, per-cell Figma frame refs, **Figma deliverables** section, Figma sign-off item | — |
| `true` | Embedded screenshots for every locale, matrix, findings, font scaling summary | Figma baseline, Figma frame refs, **Figma deliverables** section, "Figma section link" sign-off |

When `reportOnly: true`, the Cursor report **is** the deliverable. Do not insert placeholder Figma URLs (`{url}`).

---

## Executive summary

| Metric | Value |
|--------|-------|
| Locales tested | {n} |
| Screen variants | {m} |
| Total cells | {n × m} |
| Overall status | **PASS** / **FAIL** / **PARTIAL** |
| i18n pass rate | {x}% |
| A11y pass rate | {y}% |
| Font scale pass rate | {z}% |

### Top 3 risks

1. {risk 1 — with link to visual evidence section below}
2. {risk 2}
3. {risk 3}

---

## Visual evidence — all locales

Embed screenshots for **every** locale × screen. Do not list FAIL-only.

### {locale} · {screen} · {PASS|FAIL|PARTIAL}

| Baseline ({failureComparisonBaseline}) | {locale} |
|----------------------------------------|----------|
| ![baseline-{locale}-{screen}](path-or-embed) | ![{locale}-{screen}](path-or-embed) |

- **Strings swapped:** {list key strings: EN → target, tag [MT] if machine-translated}
- **Defect (if FAIL):** {visible issue — clip, overlap, RTL break, English fallback}
- **Figma frame:** {link or node id — **omit this bullet when `reportOnly: true`**}

Repeat for each locale × screen in the matrix.

---

## Matrix

| Locale | Screen | i18n Layout | RTL | Formats | Font Small | Font Large | Contrast@Scale | Touch@Scale | Focus | Voice | Status |
|--------|--------|-------------|-----|---------|------------|------------|----------|-------|-------|-------|--------|
| de-DE | widget | PASS | — | PASS | PASS | FAIL | PASS | PASS | PASS | SKIP | **FAIL** |
| ar-SA | discounts | PASS | PASS | PASS | PASS | PASS | PASS | FAIL | FAIL | PARTIAL | **FAIL** |
| … | … | … | … | … | … | … | … | … | … | … | … |

**Legend:** PASS | FAIL | PARTIAL | SKIP | — (not applicable)

### Worst-case combo

| Combo | Screen | Status | Notes |
|-------|--------|--------|-------|
| DE · Large | widget | FAIL | CTA clips at 3 lines |

---

## Critical findings (must fix)

1. **[{locale}] [{screen}]** {category}: {observation} → {recommendation}
2. …

---

## Recommendations

1. {Prioritized fix with design + eng owner if known}
2. …

---

## Font scaling summary

| Tier | Locales tested | Pass | Fail |
|------|----------------|------|------|
| Small | {risk set} | {n} | {n} |
| Large | {risk set + failures} | {n} | {n} |
| DE+Large combo | widget | {PASS/FAIL} | {notes} |

---

## A11y delegation notes

<!-- When reportOnly: true — describe findings in prose; reference embedded screenshots, not Figma frame names -->

**When `reportOnly: false`:**

| Skill invoked | Scope | Output |
|---------------|-------|--------|
| apca-compliance-figma | {token pairs flagged} | `_Annotation / Contrast failures` |
| create-voice | de-DE, ar-SA, ja-JP | `{Component} Screen reader — {locale}` frames |

**When `reportOnly: true`:**

| Skill invoked | Scope | Output |
|---------------|-------|--------|
| apca-compliance-figma | {token pairs flagged} | Contrast notes in report + screenshot refs |
| create-voice | de-DE, ar-SA, ja-JP | Localized voice copy in report (no Figma frames) |

---

## Figma deliverables

<!-- **Omit this entire section when `reportOnly: true`** — do not leave placeholder URLs -->

| Section | Link | Frames |
|---------|------|--------|
| Summary | {url} | 1 |
| i18n Matrix | {url} | {locales × variants} |
| Font Scaling | {url} | {small + large rows} |
| A11y Annotations | {url} | {annotation count} |
| Edge cases | {url} | {DE+Large, mixed-script, …} |

---

## Sign-off checklist

**Always:**

- [ ] All FAIL cells have findings with recommendations
- [ ] DE+Large worst-case evaluated
- [ ] RTL locales have focus order documented
- [ ] Embedded screenshots for **every** locale × screen in this report
- [ ] Optional: persisted to `docs/global-stress-test-{date}.md`

**When `reportOnly: false` only:**

- [ ] Figma section link returned to stakeholder

**When `reportOnly: true` only:**

- [ ] Confirmed: no Figma deliverables section in this report (Cursor-report-only run)
- [ ] Prototype or capture source documented in header

---

## Out of scope (this run)

- {List anything skipped: runtime axe, XL tier, custom locales, Figma push when reportOnly, …}
