# How to Read Results

Guide for **designers**, **engineers**, **PMs**, and **QA** — no need to read the full skill spec.

## Report structure (top to bottom)

Read in this order:

1. **Report status** — `FINAL` (complete) or `DRAFT` (Figma matrix still pending)
2. **Start here — action items** — P0/P1 fixes with owner and recommended action
3. **Executive summary** — pass rates and top risks
4. **Failures — visual evidence** — screenshots for every FAIL/PARTIAL cell
5. **Full matrix** — appendix with all locales (including PASS)
6. **Figma deliverables** — link to visual matrix (omitted in report-only mode)

## Priority levels

| Priority | Meaning | Who acts |
|----------|---------|----------|
| **P0** | Ship blocker — broken layout, RTL break, unreadable contrast at scale, touch target failure | Design + eng — fix before release |
| **P1** | Launch risk — clipping at large font, partial voice copy, format inconsistency | Design — fix in next sprint |
| **P2** | Polish — minor overflow, `[MT]` strings to replace with real copy | Content / design backlog |

## Status labels

| Label | Meaning |
|-------|---------|
| **PASS** | Cell meets all checked criteria |
| **FAIL** | One or more criteria failed — see action items |
| **PARTIAL** | Incomplete evidence (e.g. no localized screenshot) or mixed pass/fail dimensions |
| **SKIP** | Dimension not evaluated for this cell |
| **—** | Not applicable (e.g. RTL column for non-Arabic locale) |

## Matrix columns (quick reference)

| Column | What it checks |
|--------|----------------|
| i18n Layout | Translated strings fit; no clip/overlap |
| RTL | Arabic (and RTL locales) mirror correctly |
| Formats | Dates, times, numbers match locale |
| Font Small / Large | UI survives smaller/larger user text settings |
| Contrast@Scale | Text readable on background **after** font scaling |
| Touch@Scale | Tap targets ≥ 44pt **at large font** |
| Focus | Keyboard/focus order logical (RTL for Arabic) |
| Voice | Screen reader labels localized |

**Note:** Contrast and Touch are evaluated **after** font scaling (Phase 2.6), not at default size only.

## What to do with findings

Each action item follows:

```
[P0|P1|P2] {locale} · {screen} — {category}: {what broke} → {recommended fix} (Owner: {role})
```

**Designer:** fix layout, auto-layout, min heights, RTL mirroring in Figma.  
**Engineer:** fix truncation, dynamic type, `dir=rtl`, copy keys, font scaling hooks.  
**Content:** replace `[MT]` machine translations with approved copy.

## Figma matrix (when included)

Open the linked section. Each cell is named:

```
L{n}.{m} — {Locale} · {Screen} · {PASS|FAIL}
```

- **Green/red/amber** badges on frames
- **FAIL** cells have baseline vs failing locale side-by-side
- **Font Scaling** rows show Small/Large tiers
- **`FS — DE · Large · {screen} · WORST CASE`** — always review this frame

## Report-only runs (`reportOnly: true`)

- No Figma link — all evidence is **embedded screenshots** in the Cursor chat
- Same action items and matrix apply
- Use when you have a runnable prototype but no Figma write access

## DRAFT vs FINAL

| Status | Meaning |
|--------|---------|
| **DRAFT** | Analysis complete; Figma matrix not yet pushed (figma/both path) |
| **FINAL** | Report + Figma link (if applicable) — safe to share with stakeholders |

Do not share externally until status is **FINAL** (unless explicitly report-only).
