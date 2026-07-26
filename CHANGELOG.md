# Changelog

All notable changes to the global-design-stress-test skill.

## [1.0.1] — 2026-07-26

### Changed

- Removed proprietary / organization-specific references for open-source publication
- Renamed locale pack `hertz-extended` → `extended`
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
