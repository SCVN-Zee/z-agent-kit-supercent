# Luna Build-Settings Checklist

House rules for Luna Playworks export settings, mapped to their `luna.json` paths. These preserve the kit's
selected quality profile. Failing a gate is not automatically an unsupported Luna configuration. Vendor
evidence below explains the trade-offs, not mandatory values.

> **Source of truth:** the machine-readable rules live in `../scripts/luna-build-settings.cjs`
> (`BOOL_GATES` + `ADVISORIES`). This doc mirrors them for humans and for the guided fallback —
> keep the two in sync. The validator, not this table, is authoritative at runtime.

Settings come from the Luna Build window (**Settings** tab: Basic/Advanced; **Assets** tab:
Textures/Sound/Fonts/Meshes/Animations). The **Assets → Assets** sub-list is manual and out of scope.

## Gates (must pass — auto-fixable except scene)

| Setting | `luna.json` path | Required | Auto-fix | Why |
|---|---|---|---|---|
| Exactly 1 scene | `unity.scenes` − `unity.disabledScenes` | exactly **1 enabled** | no (validate-only) | House single-scene scope keeps content auditable. V7 exposes a list of build scenes; this gate does not prove all multi-scene use unsupported. Selection stays manual. |
| Force-disable Anti-Aliasing OFF | `unity.disableAntiAliasing` | `false` | yes | Keep AA so the playable preserves intended visual quality (no jagged edges in the ad). |
| Mesh half-precision OFF | `assets.rules.meshes.default.halfPrecision` | `false` | yes | House fidelity preference. Vendor half precision reduces stored mesh data size; measure precision and size before choosing a different profile. |
| Mesh reduce-complexity OFF | `assets.rules.meshes.default.useSimplification` | `false` | yes | House fidelity preference; simplification changes geometry and needs visual comparison. |
| Animation half-precision OFF | `assets.rules.animations.default.halfPrecision` | `false` | yes | House fidelity preference; precision reduction trades stored animation size against accuracy, not a guaranteed jitter defect. |
| Animation remove-redundant-keyframes OFF | `assets.rules.animations.default.stripCurves` | `false` | yes | House fidelity preference; compare exported motion before accepting curve reduction. |

## Advisories (report-only — judgment calls, never auto-set)

| Setting | `luna.json` path | House reference | Why / when to deviate |
|---|---|---|---|
| Realtime shadows | `unity.enableRealtimeShadows` | `false` (default OFF) | Realtime shadows are costly on WebGL playables; enable only if the creative genuinely needs them. |
| Sound default bitrate | `assets.rules.sound.default.bitrate` | `96` kb/s | Balances size vs quality. Lower to shrink the build, raise for quality-critical audio. Per-clip `sound.overrides[]` are intentional. |
| Font atlas size | `assets.rules.font.default.data.textureWidth` / `textureHeight` | `256` × `256` | Sufficient for typical short playable copy; enlarge only for many or large glyphs. |
| Texture default + overrides | `assets.rules.texture.default` (maxWidth/Height, format, compression, quality) + `assets.rules.texture.overrides[]` | default keeps build small | Global downscale/compression shrinks the bundle (textures dominate playable size); bump hero/high-detail textures via per-asset `overrides[]` for sharpness. The skill reports the default + override count and reminds you to override hero textures — it cannot know which textures "need" higher res. |

## Vendor evidence (7.2.0)

In `pipeline/resources/LunaPlayable Localisation - LunaPlayable.csv`, line 840 (`Basic_ScenesInBuild_Hint`)
describes the build-scene list; 847 (`Advanced_DisableAntiAliasing_Hint`) describes a performance reason to
disable AA; 898/900 (`Meshes_HalfPrecision_Hint` / `Animations_HalfPrecision_Hint`) describe 50% storage
reduction for those data, not the whole bundle. Lines 894–896 require enough font glyphs and atlas space, not
a fixed 256×256 atlas. Sound bitrate, simplification and curve-retention values remain kit policy.

## Notes

- **Never touch `overrides[]`.** Auto-fix only the listed boolean gates: `unity.disableAntiAliasing`
  and mesh/animation `*.default` values; per-asset overrides are intentional tuning.
-  **Missing keys** are reported as `missing` (not failed); the overall result is incomplete unless another
  gate fails. They are never created automatically — inspect the effective settings in the Luna UI, then
  re-run.
- **`LunaTemp/`** holds a *generated* `luna.json`; always validate/fix the **source** `luna.json` at the
  Unity project root, not the `LunaTemp/` copy.
