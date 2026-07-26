# Global Design Stress Test

A portable **Cursor Agent Skill** (and multi-LLM workflow) for stress-testing UI designs against **internationalization**, **design-time accessibility**, and **font scaling** across global locales.

## What it does

1. **i18n stress** — layout, RTL, long strings, locale formats (13–30+ locales)
2. **Accessibility** — contrast, touch targets, focus order, localized screen reader copy
3. **Font scaling** — small / default / large user settings (Dynamic Type, Android font size, web zoom)
4. **Cursor report** — structured PASS/FAIL matrix with critical findings
5. **Figma push-back** — documented matrix, font scaling rows, and `_Annotation` sidecars

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

| Pack | Locales | Use when |
|------|---------|----------|
| `core` | 13 | Fast run — ~80% of i18n risk |
| `extended` | 30 | Full global market coverage |
| `custom` | Your list | Ad hoc BCP-47 codes |

See [references/locale-registry.md](references/locale-registry.md).

## Workflow phases

| Phase | Focus |
|-------|--------|
| 0 | Load `stress-test-config.json` |
| 1 | i18n matrix (locale × screen) |
| 2 | Design-time a11y |
| 2.5 | Font scaling (risk locales + DE+Large worst case) |
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
