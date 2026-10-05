# Exported assets

Use for prefab/mesh inclusion, TMP/font assets or other exported asset changes. For Animator/controller and
shader/material behavior, select their dedicated topics instead.

## Font and mesh checks

| Check | Decision and evidence |
| --- | --- |
| TMP atlas population | V7 hints 618–619 (`ProjectDiagnostics_LP_TMPUnsupportedAtlasPopulationMode_*`) explicitly says Dynamic is unsupported and directs Static. Inspect the actual font asset and required glyphs; confirm the installed version before reporting. |
| Glyph coverage/atlas size | V7 hints 894–896 (`FontTab_Alphabet_Hint`, `FontTab_TextureSize_Hint`) requires all needed characters and sufficient atlas size. A 256×256 atlas is a house reference, not a universal maximum. |
| Mesh limits | Inspect actual vertex/index data and exporter diagnostics for the installed runtime. Do not impose a universal 65k-vertex limit or invent LP1034 without confirming the mapping. |
| Included assets | Trace scenes, Resources, exclusions and references to the exported content. Vendor `config.json` exposes asset inclusion/exclusion controls; Editor visibility alone is not proof of export inclusion. |

## Prefabs

Inspect the resolved prefab instance and its references in the scene/export, including overrides. Keep variant
inheritance shallow for auditability as a house preference, not a proven Luna limit. Compare exported values
when inheritance or an override is implicated. Never hand-edit serialized prefab/scene data to make a review
pass.

## Verification boundary

Use available read-only asset/component inspection. If fields or asset contents cannot be inspected, report
precisely what remains unverified. Existing export diagnostics may cover some asset problems; their existence
does not prove that a particular check ran.

Texture/audio compression, shadows, scene-count policy and animation/mesh precision belong to
`skill://z-luna-build-check`. Size, memory and frame budgets need the target ad-network requirements and
actual build measurements.
