# Changelog

All notable changes to the global-design-stress-test skill.

## [1.2.4] — 2026-07-27

### Fixed

- **both + reportOnly false:** Phase 1 no longer describes read-only Figma — uses prototype captures + eval clones; Phase 4 matrix + PNG overlay

## [1.2.3] — 2026-07-27

### Fixed

- **Figma-only path:** Phases 1–2.6 use evaluation clones (`use_figma` scratch frames) for localized screenshots; Phase 4 creates official `{outputSectionName}` section — resolves contradiction with PASS/FAIL from screenshot evidence

## [1.2.2] — 2026-07-27

### Fixed

- **SKILL intro:** Figma matrix optional when `reportOnly: true`; valid mode table replaces "both outputs" wording
- **Invalid combo:** Phase 0 blocks `executionPath: "figma"` + `reportOnly: true` (cannot produce localized UI without writes or prototype)

## [1.2.1] — 2026-07-27

### Fixed

- **nl-BE short code:** `be` → `be-nl` (matches `be-fr` pattern; fixes wrong prototype scenario IDs)
- **Adapter Phase 0:** Claude/ChatGPT prompts reject `Your Project Name` on every path; Figma placeholders when `reportOnly: false` or figma/both (not only “when Figma MCP runs”)

## [1.2.0] — 2026-07-27

### Added — usability

- **Action-first report** — P0/P1/P2 action items table with owner and recommended fix
- **Failures-first visual evidence** — full PASS grid moved to matrix appendix
- **report-agent-guide.md** — agent branching separated from stakeholder report
- **how-to-read-results.md** — guide for designers, PMs, engineers, QA
- **quick-start.md** — minimal 3-step run instructions
- **DRAFT / FINAL** report status for figma runs (Phase 4 pending vs complete)

### Changed

- **report-template.md** — rewritten for clarity; no agent comments in deliverable
- README, SKILL, adapters updated for easier invoke and output reading

## [1.1.8] — 2026-07-27

### Fixed

- **Figma write policy:** Phase 4 is sole Figma write phase; `reportOnly: true` forbids all `use_figma` in Phases 1–2.6
- **Prototype uploads:** removed `upload_assets` from Phase 1 — buffer PNGs; Phase 4 uploads after section exists

## [1.1.7] — 2026-07-27

### Fixed

- **Decision tree:** `executionPath: "both"` now branches on `reportOnly` — report-only mode skips Figma matrix push and `upload_assets` while still allowing dual capture in the Cursor report

## [1.1.6] — 2026-07-27

### Fixed

- **report-template.md:** branches on `reportOnly` — omits Figma baseline, deliverables section, frame refs, and Figma sign-off in Cursor-report-only mode
- **Phase 3 (SKILL.md):** explicit template branching instructions; adapters updated

## [1.1.5] — 2026-07-27

### Fixed

- **`reportOnly` + prototype consistency:** Figma-free mode (`executionPath: "prototype"`, `reportOnly: true`) no longer requires `upload_assets` or Figma matrix in Phase 1; visual-evidence spec and execution path matrix document Cursor-report-only deliverable

## [1.1.4] — 2026-07-27

### Fixed

- **Phase 0 Figma placeholders:** blocks unchanged template values (`YOUR_FIGMA_FILE_KEY`, `0000:0000`, default project name) before Phases 1–4 when Figma MCP will run
- **Config template:** inline notes on fields that must be replaced before Phase 1

## [1.1.3] — 2026-07-27

### Fixed

- **Custom locale pack:** Phase 0 now requires non-empty `customLocales` when `localePack` is `custom`; blocks Phases 1–4 with zero matrix cells (`CONFIG_INVALID`)
- **Config template:** documents that `customLocales` is ignored unless pack is `custom`

## [1.1.2] — 2026-07-27

### Fixed

- **Booking widget example:** Phase 2 table is scale-independent only; added Phase 2.6 post-scale contrast/touch sample
- **Decision tree:** Figma and prototype paths now include Phase 2.6 (not 1–2.5 only)
- **Cursor adapter:** install verification lists Phase 2.6

## [1.1.1] — 2026-07-27

### Fixed

- **Phase order:** split a11y into Phase 2 (scale-independent) and Phase 2.6 (post-scale contrast + touch after font scaling)
- **ChatGPT adapter:** require `visual-evidence-spec.md` and `translation-workflow.md` in knowledge upload and instructions

## [1.1.0] — 2026-07-27

### Added

- **Show, don't tell** — mandatory visual evidence for all locales
- [visual-evidence-spec.md](references/visual-evidence-spec.md) — full matrix, screenshots in Cursor + Figma
- [translation-workflow.md](references/translation-workflow.md) — string inventory, real text swap (no placeholders)
- Config: `visualEvidence`, `embedScreenshotsInReport`, `failureComparisonBaseline`, `reportOnly`
- Failure side-by-side: baseline vs failing locale in report and Figma
- Verification loop: screenshot must show target language before PASS

### Changed

- Phase 4 Figma push is required unless `reportOnly: true`
- i18n checklist requires screenshot evidence per cell

## [1.0.1] — 2026-07-26

### Changed

- Removed proprietary / organization-specific references for open-source publication
- Renamed extended locale pack identifier to `extended` for neutral OSS naming
- Generalized example and install documentation

## [1.0.0] — 2026-07-24

### Added

- Initial skill release with 6-phase workflow (0–4 + 2.5 font scaling)
- Locale registry: Core (13) + Extended (30) tiers
- Checklists: i18n, design-time a11y, font scaling (small/default/large)
- Report template and Figma output spec
- Hybrid execution: Figma-first + optional prototype capture
- Multi-LLM adapters: Cursor, Claude, ChatGPT
- Mobile booking widget example
- Font scaling worst-case combo: DE + Large
- Risk locale set for font sweep: de-DE, ar-SA, ja-JP, baseline

### Out of scope (v1)

- Runtime axe/browser audits
- Live translation API
- CI / GitHub Action gates
