# Quick Start

Run a global design stress test in three steps.

## 1. Install (once)

```bash
git clone https://github.com/brianbeavers/global-design-stress-test-skill.git ~/.cursor/skills/global-design-stress-test
```

## 2. Configure (per project)

Copy `stress-test-config.template.json` → `stress-test-config.json` in your project root. Minimum fields:

| Field | Example | Notes |
|-------|---------|-------|
| `projectName` | `"Booking Widget"` | Not `"Your Project Name"` |
| `figmaFileKey` | from Figma URL | Skip if prototype-only + `reportOnly: true` |
| `baselineNodeId` | `1234:5678` | Frame to stress-test |
| `screenVariants` | `["widget", "sheet"]` | Your screens |
| `localePack` | `"core"` | 13 languages (default) |

**No Figma access?** Set `"executionPath": "prototype"` and `"reportOnly": true`.

## 3. Invoke

In Cursor:

```
Run global-design-stress-test on https://www.figma.com/design/FILE_KEY/...?node-id=1-2
```

Or: *i18n stress test*, *font scaling QA*, *RTL layout check*.

## What you get back

| Deliverable | When |
|-------------|------|
| **Cursor report** | Always — start with **Action items (P0/P1)**, then failures with screenshots |
| **Figma matrix** | When `reportOnly: false` — linked after Phase 4 |

Read [how-to-read-results.md](how-to-read-results.md) to interpret the report.

## Common setups

| Situation | Config |
|-----------|--------|
| Full run (Figma + report) | `executionPath: "figma"`, `reportOnly: false` |
| App prototype, no Figma writes | `executionPath: "prototype"`, `reportOnly: true` |
| Prototype + Figma documentation | `executionPath: "both"`, `reportOnly: false` |
| Fast first pass | `localePack: "core"`, `fontScaleSweepLocales: "risk-set"` |

## If the run stops at Phase 0

The agent will report `CONFIG_INVALID`. Fix the listed field (empty locales, placeholder Figma keys, etc.) and re-run.
