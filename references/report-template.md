# Global Design Stress Test — {Project}

**Date:** {YYYY-MM-DD}  
**Figma baseline:** [{baselineNodeId}](https://www.figma.com/design/{fileKey}/?node-id={nodeIdHyphen})  
**Locale pack:** {core | extended | custom}  
**Platform:** {ios | android | web}  
**Execution path:** {figma | prototype | both}

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
- **Figma frame:** {link or node id}

Repeat for each locale × screen in the matrix.

---

## Matrix

| Locale | Screen | i18n Layout | RTL | Formats | Font Small | Font Large | Contrast | Touch | Focus | Voice | Status |
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

| Skill invoked | Scope | Output |
|---------------|-------|--------|
| apca-compliance-figma | {token pairs flagged} | `_Annotation / Contrast failures` |
| create-voice | de-DE, ar-SA, ja-JP | `{Component} Screen reader — {locale}` frames |

---

## Figma deliverables

| Section | Link | Frames |
|---------|------|--------|
| Summary | {url} | 1 |
| i18n Matrix | {url} | {locales × variants} |
| Font Scaling | {url} | {small + large rows} |
| A11y Annotations | {url} | {annotation count} |
| Edge cases | {url} | {DE+Large, mixed-script, …} |

---

## Sign-off checklist

- [ ] All FAIL cells have findings with recommendations
- [ ] DE+Large worst-case evaluated
- [ ] RTL locales have focus order documented
- [ ] Figma section link returned to stakeholder
- [ ] Optional: persisted to `docs/global-stress-test-{date}.md`

---

## Out of scope (this run)

- {List anything skipped: runtime axe, XL tier, custom locales, …}
