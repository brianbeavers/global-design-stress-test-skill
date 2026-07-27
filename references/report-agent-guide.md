# Report Agent Guide

**Agents only** — do not paste this file into the stakeholder report.

Read before filling [report-template.md](report-template.md).

## Delivery rules

1. **Stakeholder report** = fill `report-template.md` only — no HTML comments, no branching tables, no `{placeholder}` URLs
2. **Lead with action items** — P0 first, then P1, then P2
3. **Failures-first visuals** — full PASS locale screenshots go in appendix unless user asked for full inline embeds
4. **Assign severity** to every FAIL/PARTIAL — use [how-to-read-results.md](how-to-read-results.md) priority definitions
5. **Assign owner** — `Design`, `Eng`, `Content`, or `Design+Eng` on every action item

## Report status banner

| When | Status | Banner text |
|------|--------|-------------|
| After Phase 3, before Phase 4 (`reportOnly: false`) | DRAFT | `Report status: DRAFT — Figma matrix pending (Phase 4)` |
| After Phase 4 completes | FINAL | `Report status: FINAL` |
| `reportOnly: true` (no Phase 4) | FINAL | `Report status: FINAL — Cursor report only (no Figma)` |

**Prefer one FINAL message** after Phase 4 when possible. If you posted DRAFT early, send a short FINAL update with Figma link — do not duplicate the full report.

## Branch on `reportOnly`

| Section | `reportOnly: false` | `reportOnly: true` |
|---------|---------------------|---------------------|
| Figma baseline link in header | Include | Omit — use **Design reference** instead |
| Per-cell "Figma frame" bullet | Include after Phase 4 | Omit |
| **Figma deliverables** section | Include with real URLs | **Omit entire section** |
| Sign-off "Figma section link" | Include | Omit |

Never insert `{url}` placeholders — real links or omit.

## Severity assignment (required for every FAIL)

| Category | Default severity |
|----------|------------------|
| Layout clip / overlap at default scale | P0 |
| RTL break / wrong reading order | P0 |
| Contrast fail at Large font | P0 |
| Touch target < 44pt at Large font | P0 |
| Layout fail at Large font only | P1 |
| English fallback / incomplete translation | P1 |
| Voice copy missing / English only | P1 |
| `[MT]` string acceptable but flagged | P2 |
| Format inconsistency (date/time) | P1 |

Escalate DE · Large worst-case combo failures to **P0** always.

## Collapsing findings + recommendations

Do **not** duplicate prose in both "Critical findings" and "Recommendations". Use the single **Action items** table as source of truth; optional narrative only for complex RTL/focus issues.

## Embed policy

Default: embed screenshots for **FAIL + PARTIAL** cells inline; PASS cells summarized in matrix appendix only.

If user or config requires full matrix embeds (`visualEvidence: full-matrix`), embed all locales but still lead with action items table.
