# Locale Registry

BCP-47 locales for global design stress testing. Three tiers control matrix size.

## Core languages (13) — default pack

These **13 languages** are stress-tested when `"localePack": "core"`:

1. German (`de-DE`)
2. French (`fr-FR`)
3. Italian (`it-IT`)
4. Dutch (`nl-NL`)
5. Spanish (`es-ES`)
6. Portuguese (`pt-PT`)
7. Arabic (`ar-SA`) — RTL
8. Chinese / Simplified (`zh-CN`)
9. Japanese (`ja-JP`)
10. Korean (`ko-KR`)
11. Dutch — Belgium (`nl-BE`)
12. Polish (`pl-PL`)
13. Norwegian (`nb-NO`)

## Extended pack — +17 locales (30 total)

Adds English (UK, Ireland, US, South Africa), Czech, German (Austria, Switzerland, Luxembourg), French (Switzerland, Luxembourg, Belgium), Italian (Switzerland), Danish, Finnish, Greek, Russian, and Swedish. Gulf markets use Arabic (`ar-SA`) as the RTL representative.

## Tiers

| Tier | ID | Count | Use when |
|------|-----|-------|----------|
| Core | `core` | 13 | Fast run — covers ~80% of i18n risk |
| Extended | `extended` | Core + 17 | Global markets (EU, Gulf, APAC additions) |
| Custom | `custom` | User-defined | Pass `customLocales` in config |

## Core tier (13 locales)

| Code | Short | Label | Market | RTL | 24h | Long string | Script | Sample location |
|------|-------|-------|--------|-----|-----|-------------|--------|-----------------|
| `de-DE` | de | German | Germany | — | ✓ | High | Latin | Frankfurt (FRA) |
| `fr-FR` | fr | French | France | — | ✓ | High | Latin | Paris (CDG) |
| `it-IT` | it | Italian | Italy | — | ✓ | Medium | Latin | Rome (FCO) |
| `nl-NL` | nl | Dutch | Netherlands | — | ✓ | Medium | Latin | Amsterdam (AMS) |
| `es-ES` | es | Spanish | Spain | — | ✓ | Medium | Latin | Madrid (MAD) |
| `pt-PT` | pt | Portuguese | Portugal | — | ✓ | Medium | Latin | Lisbon (LIS) |
| `ar-SA` | ar | Arabic | Gulf (representative) | ✓ | — | Medium | Arabic | Riyadh (RUH) |
| `zh-CN` | zh | Chinese | China | — | ✓ | Medium | Han | Shanghai (PVG) |
| `ja-JP` | ja | Japanese | Japan | — | ✓ | Medium | CJK | Tokyo (NRT) |
| `ko-KR` | ko | Korean | Korea | — | ✓ | Medium | Hangul | Seoul (ICN) |
| `nl-BE` | be | Belgian (Dutch) | Belgium | — | ✓ | Medium | Latin | Brussels (BRU) |
| `pl-PL` | pl | Polish | Poland | — | ✓ | High | Latin | Warsaw (WAW) |
| `nb-NO` | no | Norwegian | Norway | — | ✓ | Medium | Latin | Oslo (OSL) |

### Clock rules (Core)

- **24h clock:** all except `en-US` and `ar-SA` (12h AM/PM)
- **RTL:** `ar-SA` only in Core tier

## Extended tier additions (17 locales)

Adds to Core for `extended` pack:

| Code | Short | Label | Market | RTL | 24h | Long string | Script | Notes |
|------|-------|-------|--------|-----|-----|-------------|--------|-------|
| `en-GB` | gb | English (UK) | UK | — | ✓ | Low | Latin | Spelling, date order |
| `en-IE` | ie | English (Ireland) | Ireland | — | ✓ | Low | Latin | Same family as UK |
| `en-US` | us | English (US) | US (baseline) | — | — | Low | Latin | Default baseline locale |
| `en-ZA` | za | English (South Africa) | South Africa | — | ✓ | Low | Latin | Regional variant |
| `cs-CZ` | cs | Czech | Czech Republic | — | ✓ | High | Latin | Long compound words |
| `de-AT` | at | German (Austria) | Austria | — | ✓ | High | Latin | DE variant + formatting |
| `da-DK` | da | Danish | Denmark | — | ✓ | Medium | Latin | 24h, long labels |
| `fi-FI` | fi | Finnish | Finland | — | ✓ | High | Latin | Agglutination |
| `el-GR` | el | Greek | Greece | — | ✓ | Medium | Greek | Script + line breaking |
| `ru-RU` | ru | Russian | Russia | — | ✓ | High | Cyrillic | Case rules |
| `sv-SE` | sv | Swedish | Sweden | — | ✓ | Medium | Latin | 24h |
| `de-CH` | ch-de | German (Switzerland) | Switzerland | — | ✓ | High | Latin | Multi-locale market |
| `fr-CH` | ch-fr | French (Switzerland) | Switzerland | — | ✓ | High | Latin | Multi-locale market |
| `it-CH` | ch-it | Italian (Switzerland) | Switzerland | — | ✓ | Medium | Latin | Multi-locale market |
| `fr-LU` | lu-fr | French (Luxembourg) | Luxembourg | — | ✓ | Medium | Latin | Multi-locale market |
| `de-LU` | lu-de | German (Luxembourg) | Luxembourg | — | ✓ | High | Latin | Multi-locale market |
| `fr-BE` | be-fr | French (Belgian) | Belgium | — | ✓ | Medium | Latin | `nl-BE` already in Core |

### Gulf markets (regional note)

Bahrain, Kuwait, Lebanon, Oman, Qatar, Saudi Arabia, UAE — use `ar-SA` as RTL representative. Add frame footnote: *Regional copy may differ; layout/RTL stress applies to all Gulf markets.*

## Custom tier

Pass BCP-47 codes in config. **Required:** at least one code when `"localePack": "custom"`. An empty `customLocales` array is invalid — Phase 0 must block the run until the user supplies codes.

```json
{
  "localePack": "custom",
  "customLocales": ["he-IL", "tr-TR", "hi-IN"]
}
```

Agent resolves metadata from this table where possible; for unknown codes, infer RTL from script (`ar`, `he`, `fa`) and flag `[INFERRED]` in report.

## Risk locale set (font scaling default)

Used when `fontScaleSweepLocales` is `"risk-set"`:

| Code | Rationale |
|------|-----------|
| `de-DE` | Longest Latin strings |
| `ar-SA` | RTL + mixed script |
| `ja-JP` | CJK line breaking |
| Config `baselineLocale` | Reference (default `en-US` or `en-GB`) |

## Long-string priority (Phase 1 sampling)

When time-constrained, prioritize layout stress on:

1. `de-DE`, `de-AT`, `de-CH`, `de-LU`
2. `fi-FI`, `cs-CZ`, `ru-RU`, `pl-PL`
3. `fr-FR`, `fr-CH`, `fr-BE`

## Resolving locale list from config

```
if localePack == "core":
  locales = Core tier (13)
elif localePack == "extended":
  locales = Core + Extended additions (dedupe by code)
elif localePack == "custom":
  locales = dedupe(customLocales)   # must be non-empty — see validation below
```

### Phase 0 validation (mandatory — block Phases 1–4 if fail)

| Check | Rule | On fail |
|-------|------|---------|
| Custom pack | `localePack == "custom"` → `customLocales` must be a non-empty array of BCP-47 strings | **STOP.** Status `CONFIG_INVALID`. Ask user for codes or switch to `core` / `extended`. |
| Resolved count | `len(locales) >= 1` after resolution | **STOP.** Do not emit empty matrix or mark workflow complete. |
| Screen variants | `screenVariants` non-empty array | **STOP.** Ask user for at least one screen variant. |
| Matrix size | `len(locales) × len(screenVariants) >= 1` | **STOP.** Report expected cell count before Phase 1. |
| Figma targets | Required when Figma MCP will run — see **Figma target rules** below | **STOP.** Status `CONFIG_INVALID`. Ask for Figma URL or set `reportOnly: true` with `executionPath: "prototype"`. |
| Project name | `projectName` must not be template default `Your Project Name` | **STOP.** Ask user for a real project name (used in report and Figma section title). |

**Figma target rules** — validate `figmaFileKey` and `baselineNodeId` when **any** of:

- `executionPath` is `figma` or `both` (Phase 1 uses Figma MCP), **or**
- `reportOnly` is `false` (Phase 4 push or matrix upload)

Skip Figma target checks only when `executionPath` is `prototype` **and** `reportOnly` is `true`.

**Rejected placeholder values** (non-exhaustive — treat any obvious template copy as invalid):

| Field | Reject if |
|-------|-----------|
| `figmaFileKey` | empty; `YOUR_FIGMA_FILE_KEY`; `YOUR_FILE_KEY`; starts with `YOUR_`; shorter than 10 characters |
| `baselineNodeId` | empty; `0000:0000`; `0:0`; `YOUR_BASELINE_NODE_ID`; starts with `YOUR_`; does not match `^\d+:\d+$` (after normalizing `-` → `:`) |

Parse Figma URL **before** validation when the user supplies a link — overwrite placeholders in config with extracted `fileKey` and `nodeId`.

When `localePack` is `core` or `extended`, `customLocales` is **ignored** (may be `[]` in template). Only the custom pack reads `customLocales`.

Total extended unique codes: **30** (Core 13 + Extended 17, with `nl-BE` shared).
