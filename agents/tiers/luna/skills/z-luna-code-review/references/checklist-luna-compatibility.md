# Luna compatibility checklist index

Read only the topics matching the changed behavior and its dependencies. Do not preload all references. For a
whole-project review, identify used subsystems first, then cover every applicable topic. Multiple matches require multiple topics. Record skipped or inaccessible checks.

| Changed behavior or asset | Read |
| --- | --- |
| C# syntax, dependencies, Odin, generics, serialized fields | [C# and serialization](checklist-csharp.md) |
| Camera/input, physics, particles, networking, async, events, pooling | [Runtime APIs and lifecycle](checklist-runtime.md) |
| Animator calls, parameters, controllers, clips, avatars, skinned animation | [Animation](checklist-animation.md) |
| Shader source, material keywords, depth, render pipeline | [Shaders and rendering](checklist-shaders.md) |
| Prefabs, meshes, TMP/font atlases, exported asset inclusion | [Exported assets](checklist-assets.md) |

`luna.json` tuning alone goes to `skill://z-luna-build-check`, not all five topics. Review remains
report-only under the owning skill.

## Evidence standard

-  **Vendor evidence:** version-specific configuration, diagnostic or documentation. It proves the documented
  constraint, not that the current project reproduces a bug.
-  **Project evidence:** affected source/configuration plus a reproducible export result. Use this for runtime
  claims not established by vendor evidence.
-  **House policy:** intentional authoring or quality preference. Label deviations as policy, not unsupported
  Luna features.
-  **Unverified:** historical warning or opaque implementation. Request a focused build check rather than
  hard-failing or rewriting working code.

The audit baseline is Unity Playworks **7.2.0**, identified by `scripts/package.json` (version field). <!--
resource-link-example: scripts/package.json belongs to the external vendor package, not this skill. -->

`V7 hints` in topic references means the vendor file
`pipeline/resources/LunaPlayable Localisation - LunaPlayable.csv`, with line and stable localization key.
Paths are relative to the installed package, not this skill. Do not require that private package to be bundled
with the kit.

Check the actual project's version, compiler settings, scripting defines and Runtime Analysis exclusions
before applying a claim. The vendor `config.json` includes `unusedModules`, `unusedClasses`, `unusedMethods`
and forced inclusions; an API name alone does not prove it survives export. V7 hints lines 913–916
(`RuntimeAnalysisTab_*_Hint`) explain stripping and the need to exercise interactions. When source or live
Editor access is unavailable, label the evidence gap.

## Shared exclusions and verification

Suppress a source finding only after checking the effective compilation/exclusion path; do not assume
`!UNITY_LUNA` is inactive without verifying the define. Do not report editor-only code as exported runtime
code.

Export size, memory, frame time and network delivery limits need the actual ad-network contract and a target
build. Do not invent universal 2–5 MB, 256 MB, 60 fps or 1024-pixel limits. Export diagnostics are useful
evidence, not proof that all performance or compatibility checks ran.
