# Global Design Stress Test

A portable **Cursor Agent Skill** (and multi-LLM workflow) for stress-testing UI designs against **internationalization**, **design-time accessibility**, and **font scaling** across **13 core languages** (30 locales in the extended pack).

## Languages stress-tested

Every run evaluates your design against localized copy, layout, RTL, date/time formats, and font scaling. The **core pack** (default) covers **13 languages**:

| # | Language | Locale | Script | RTL | Notes |
|---|----------|--------|--------|-----|-------|
| 1 | **German** | `de-DE` | Latin | — | Long-string stress priority |
| 2 | **French** | `fr-FR` | Latin | — | Long labels |
| 3 | **Italian** | `it-IT` | Latin | — | |
| 4 | **Dutch** | `nl-NL` | Latin | — | |
| 5 | **Spanish** | `es-ES` | Latin | — | |
| 6 | **Portuguese** | `pt-PT` | Latin | — | |
| 7 | **Arabic** | `ar-SA` | Arabic | ✓ | RTL mirroring, mixed-script |
| 8 | **Chinese (Simplified)** | `zh-CN` | Han | — | CJK line breaking |
| 9 | **Japanese** | `ja-JP` | CJK | — | CJK line breaking |
| 10 | **Korean** | `ko-KR` | Hangul | — | |
| 11 | **Dutch (Belgium)** | `nl-BE` | Latin | — | Regional variant |
| 12 | **Polish** | `pl-PL` | Latin | — | Long-string stress priority |
| 13 | **Norwegian** | `nb-NO` | Latin | — | |

Set `"localePack": "core"` in config (default) to run all 13.

### Extended pack — 17 additional locales (30 total)

Add `"localePack": "extended"` for global market coverage. **Additional languages and regional variants:**

| Language | Locales |
|----------|---------|
| **English** | UK (`en-GB`), Ireland (`en-IE`), US (`en-US`), South Africa (`en-ZA`) |
| **Czech** | `cs-CZ` |
| **German** | Austria (`de-AT`), Switzerland (`de-CH`), Luxembourg (`de-LU`) |
| **French** | Switzerland (`fr-CH`), Luxembourg (`fr-LU`), Belgium (`fr-BE`) |
| **Italian** | Switzerland (`it-CH`) |
| **Danish** | `da-DK` |
| **Finnish** | `fi-FI` |
| **Greek** | `el-GR` |
| **Russian** | `ru-RU` |
| **Swedish** | `sv-SE` |

**Gulf / MENA markets** (Bahrain, Kuwait, Lebanon, Oman, Qatar, Saudi Arabia, UAE) are stress-tested via **Arabic** (`ar-SA`) for RTL layout and script — regional copy may differ.

**Custom locales:** pass any BCP-47 code with `"localePack": "custom"` and a **non-empty** `customLocales` array. Phase 0 blocks the run if `customLocales` is empty.

Full metadata (24h clock, long-string risk, scripts): [references/locale-registry.md](references/locale-registry.md).

## Show, don't tell

The skill requires **visual proof** for every locale:

- **All 13+ languages** get a screenshot / Figma frame — not failures only
- **Real translated text** in the UI (machine translation OK, tagged `[MT]`)
- **Cursor chat** embeds images; **Figma** gets the full matrix
- **Figma-only or prototype-only** projects both supported

See [references/visual-evidence-spec.md](references/visual-evidence-spec.md).

## What it does

1. **i18n stress** — layout, RTL, long strings, locale formats (**13 core languages**, up to **30 locales** extended)
2. **Accessibility** — contrast, touch targets, focus order, localized screen reader copy
3. **Font scaling** — small / default / large user settings (Dynamic Type, Android font size, web zoom)
4. **Cursor report** — structured PASS/FAIL matrix with **embedded screenshots for every locale**
5. **Figma push-back** — full visual matrix with **real translated UI** in every cell (not placeholder labels)

## Install (Cursor)

```bash
git clone https://github.com/brianbeavers/global-design-stress-test-skill.git ~/.cursor/skills/global-design-stress-test
```

Or copy this folder:

```bash
cp -R global-design-stress-test-skill ~/.cursor/skills/global-design-stress-test
```

See [PUBLISH_PERSONAL_GITHUB.md](PUBLISH_PERSONAL_GITHUB.md) if you are publishing your own fork.

## Configure

Copy `stress-test-config.template.json` to your project root as `stress-test-config.json`:

```json
{
  "projectName": "My App",
  "figmaFileKey": "YOUR_FILE_KEY",
  "baselineNodeId": "0000:0000",
  "platform": "ios",
  "localePack": "core",
  "screenVariants": ["default"],
  "executionPath": "figma",
  "fontScaleSteps": ["small", "default", "large"],
  "fontScaleSweepLocales": "risk-set"
}
```

## Invoke

In Cursor:

```
Run global-design-stress-test on [Figma URL] using stress-test-config.json
```

Or mention: *i18n stress test*, *font scaling QA*, *RTL layout check*, *multilingual matrix*.

## Locale packs

| Pack | Languages / locales | Use when |
|------|---------------------|----------|
| `core` | **13 languages** (see table above) | Default — covers ~80% of i18n risk |
| `extended` | **30 locales** across 20+ language variants | Full EU, Gulf, and APAC coverage |
| `custom` | Your BCP-47 list | Ad hoc markets |

See [references/locale-registry.md](references/locale-registry.md) for the complete registry.

## Workflow phases

| Phase | Focus |
|-------|--------|
| 0 | Load `stress-test-config.json` |
| 1 | i18n matrix (locale × screen) |
| 2 | Accessibility — scale-independent (focus, voice, non-color) |
| 2.5 | Font scaling (risk locales + DE+Large worst case) |
| 2.6 | Accessibility — post-scale (contrast, touch on scaled UI) |
| 3 | Cursor report |
| 4 | Figma section + annotations |

## Execution paths

| Path | When |
|------|------|
| `figma` (default) | Figma MCP — clone frames, swap text, annotate |
| `prototype` | Runnable app + screenshot capture |
| `both` | Figma matrix + prototype PNG verification |

## Optional companion skills

If available in your environment, delegate when flagged:

- **apca-compliance-figma** — contrast (APCA Lc / WCAG)
- **create-voice** — VoiceOver / TalkBack / ARIA specs per locale
- **figma-use** — Figma canvas operations

## Multi-LLM usage

| Tool | Guide |
|------|-------|
| Cursor | [adapters/cursor.md](adapters/cursor.md) |
| Claude | [adapters/claude.md](adapters/claude.md) |
| ChatGPT | [adapters/chatgpt.md](adapters/chatgpt.md) |

## Example

[Mobile booking widget example](examples/mobile-booking-widget-example.md) — 13×4 i18n matrix, font scaling, and Figma documentation patterns.

## Repository structure

```
global-design-stress-test-skill/
├── SKILL.md
├── stress-test-config.template.json
├── references/
├── examples/
├── adapters/
└── README.md
```

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Open an issue or PR to extend locale packs, platform font-scale mappings, or checklist criteria.
