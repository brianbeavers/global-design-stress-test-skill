# Cursor Adapter

Install the **global-design-stress-test** skill in Cursor.

## Install (all projects)

```bash
git clone https://github.com/brianbeavers/global-design-stress-test-skill.git ~/.cursor/skills/global-design-stress-test
```

Or copy the skill folder:

```bash
cp -R global-design-stress-test-skill ~/.cursor/skills/global-design-stress-test
```

## Project install

Commit or symlink into your repo:

```
your-project/
└── .cursor/
    └── skills/
        └── global-design-stress-test/
```

## Per-project config

Copy `stress-test-config.template.json` to your project root as `stress-test-config.json`.

## Invoke

```
Run global-design-stress-test on [Figma URL]
```

Triggers: *i18n stress test*, *font scaling*, *RTL layout*, *multilingual matrix*.

**Output:** report with **P0/P1 action items** at the top, then failure screenshots. Share [how-to-read-results.md](references/how-to-read-results.md) with stakeholders.

## MCP requirements

- Figma MCP connected in Cursor
- Optional: contrast and screen-reader companion skills if available in your environment

## Verify install

Ask the agent: *"List the phases in global-design-stress-test"*

Expected: Phase 0 config → Phase 1 i18n → Phase 2 a11y (scale-independent) → Phase 2.5 font scaling → Phase 2.6 a11y (post-scale) → Phase 3 report (branch template on reportOnly) → Phase 4 Figma (skip if reportOnly).
