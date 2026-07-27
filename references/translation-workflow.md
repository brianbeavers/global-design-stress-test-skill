# Translation Workflow

How to produce **real translated UI** in Figma and prototypes — not placeholder labels.

## Step 1 — String inventory

From baseline frame(s):

1. `get_design_context` with `fileKey` + `nodeId`
2. List every user-facing string:

| Key | Layer | English source | Node ref |
|-----|-------|----------------|----------|
| cta.primary | Button/Label | Book now | `123:456` |
| header.title | Title | Find a car | `123:457` |

Include: buttons, labels, placeholders, sheet titles, error messages, tab bar, calendar weekdays.

Exclude: decorative lorem, internal dev labels, component variant names.

## Step 2 — Resolve translations

Priority order:

1. **Project copy files** — `i18n/`, `locales/`, `widgetCopy.ts`, etc.
2. **Design system locale bundles** — if linked in codebase
3. **Machine translation** — translate string inventory; tag `[MT]` on frame names and in report

For MT: translate **the full string**, not a description of it.  
Wrong: frame named `"German CTA too long"`  
Right: button displays `"Jetzt buchen und sparen"`

## Step 3 — Figma text swap

**Ownership:** Phase 4 only when `reportOnly: false`. **Never** call `use_figma` when `reportOnly: true`.

Use `use_figma` with Plugin API:

```javascript
// Pseudocode — walk text nodes, set characters from inventory
const node = await figma.getNodeByIdAsync(TEXT_NODE_ID);
if (node.type === 'TEXT') {
  await figma.loadFontAsync(node.fontName);
  node.characters = translatedString; // actual DE/FR/ar text
}
```

Rules:

- Load font before setting `characters`
- Preserve text styles; allow auto-resize to surface layout breaks
- Do **not** shrink font size to force PASS
- For RTL: set text alignment + mirror parent auto-layout

After swap: `get_screenshot` → confirm target language visible.

## Step 4 — Prototype text swap

If prototype exists:

| Approach | When |
|----------|------|
| Scenario presets | Project has `i18n-de-widget` URLs — use them |
| Copy bundles | Import locale JSON/TS into prototype i18n layer |
| Temporary MT bundle | Agent writes `locales/de.json` + wires one run — document as `[MT]` |

Capture:

```
http://localhost:{port}/?scenario=i18n-de-widget
```

Screenshot `.phone-screen` or config `captureSelector`.

## Step 5 — Locale formats

After string swap, apply per [locale-registry.md](locale-registry.md):

- **Time:** 24h vs 12h AM/PM
- **Dates:** locale order
- **Numbers:** decimal/group separators
- **RTL:** `dir=rtl`, mirrored icons

Formats must appear in screenshots — not just strings.

## Step 6 — Verify before PASS

A cell is **not ready** until screenshot shows:

- [ ] Primary CTAs in target language
- [ ] Sheet/modal titles in target language
- [ ] No English fallback on user-visible chrome
- [ ] RTL mirrored (if `ar-*`)
- [ ] Layout evaluated against visible result

Incomplete translation → **FAIL** with screenshot showing the English fallback.
