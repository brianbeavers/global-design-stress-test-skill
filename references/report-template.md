# Global Design Stress Test — {Project}

> **Report status:** {FINAL | DRAFT — Figma matrix pending}  
> **Overall result:** {PASS | FAIL | PARTIAL}  
> **Run date:** {YYYY-MM-DD} · **Platform:** {ios | android | web} · **Locales:** {n} · **Screens:** {m}  
> **Report mode:** {Cursor + Figma | Cursor only (no Figma)}  
> **Figma matrix:** {link or N/A} · **Design reference:** {baseline link or prototype URL}

---

## Start here — action items

Fix these first. Sorted by priority (P0 → P1 → P2).

| Priority | Locale | Screen | Category | Issue | Recommended fix | Owner |
|----------|--------|--------|----------|-------|-----------------|-------|
| P0 | de-DE | widget | Font scaling | Primary CTA clips at Large font | Set min-height 48pt; allow 2-line wrap | Design+Eng |
| P0 | ar-SA | discounts | RTL / Focus | Dismiss control last in focus order | Mirror sheet; dismiss first in RTL tab order | Eng |
| P1 | ja-JP | calendar | i18n | Month header wraps to 3 lines | Shorten label or increase header height | Design |
| … | … | … | … | … | … | … |

**Counts:** {p0} P0 · {p1} P1 · {p2} P2 · {pass} PASS cells

If this table is empty, overall status is **PASS** — see appendix for full matrix.

---

## How to read this report

- **P0** = ship blocker · **P1** = fix before launch · **P2** = polish / content
- **Contrast@Scale** and **Touch@Scale** = checked at **large font**, not default size only
- Full locale grid (including PASS) is in the **Matrix appendix** below
- Detail guide: [how-to-read-results.md](how-to-read-results.md)

---

## Executive summary

| Metric | Value |
|--------|-------|
| Total cells tested | {n × m} |
| Overall status | **{PASS | FAIL | PARTIAL}** |
| i18n pass rate | {x}% |
| Accessibility pass rate | {y}% |
| Font scaling pass rate | {z}% |

### Top risks (narrative)

1. {One sentence — e.g. German + Large breaks primary CTA across widget and sheet}
2. {One sentence — e.g. Arabic discounts sheet RTL focus order blocks keyboard users}
3. {One sentence — e.g. Machine-translated Polish strings need content review}

---

## Failures — visual evidence

Screenshots for **FAIL** and **PARTIAL** cells only. Each item maps to a row in **Action items** above.

### {locale} · {screen} · {FAIL | PARTIAL} · {P0 | P1 | P2}

| Baseline ({failureComparisonBaseline}) | {locale} |
|----------------------------------------|----------|
| ![baseline](embed-or-path) | ![locale](embed-or-path) |

- **Category:** {i18n | RTL | Font scaling | Contrast | Touch | Focus | Voice | Formats}
- **What broke:** {visible defect — clip, overlap, English fallback, contrast pair}
- **Strings:** {EN → target, tag [MT] if machine-translated}
- **Figma frame:** {link — omit when report mode is Cursor only}

---

## Font scaling summary

| Tier | Locales tested | Pass | Fail | Notes |
|------|----------------|------|------|-------|
| Small | {risk set} | {n} | {n} | |
| Large | {risk set + failures} | {n} | {n} | |
| **DE + Large (worst case)** | {screen} | {PASS/FAIL} | — | {one-line note} |

---

## Matrix appendix (all locales)

| Locale | Screen | i18n | RTL | Formats | Font S | Font L | Contrast* | Touch* | Focus | Voice | Status |
|--------|--------|------|-----|---------|--------|--------|-----------|--------|-------|-------|--------|
| de-DE | widget | PASS | — | PASS | PASS | FAIL | PASS | PASS | PASS | PARTIAL | **FAIL** |
| ar-SA | discounts | PASS | PASS | PASS | PASS | PASS | PASS | FAIL | FAIL | FAIL | **FAIL** |
| en-US | widget | PASS | — | PASS | PASS | PASS | PASS | PASS | PASS | PASS | PASS |
| … | … | … | … | … | … | … | … | … | … | … | … |

\* Contrast and Touch evaluated at **scaled** UI (Phase 2.6).

**Legend:** PASS · FAIL · PARTIAL · SKIP · — (n/a)

---

## Figma deliverables

{Include this section only when report mode is Cursor + Figma. Omit entirely for Cursor-only runs.}

| Section | Link | Frames |
|---------|------|--------|
| Summary | {url} | 1 |
| i18n Matrix | {url} | {locales × variants} |
| Font Scaling | {url} | {tiers × risk locales} |
| A11y Annotations | {url} | {count} |
| Edge cases | {url} | DE+Large, mixed-script, … |

---

## Sign-off

- [ ] P0 action items documented with owners
- [ ] DE + Large worst-case reviewed
- [ ] RTL locales have focus order noted (if applicable)
- [ ] FAIL/PARTIAL cells have screenshots
- [ ] Report status is **FINAL** before external share
- [ ] Figma matrix link shared (Cursor + Figma mode only)

---

## Out of scope (this run)

- {runtime axe/browser audits, XL font tier, locales skipped, …}
