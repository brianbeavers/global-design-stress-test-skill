# Changelog

All notable changes to the global-design-stress-test skill.

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
