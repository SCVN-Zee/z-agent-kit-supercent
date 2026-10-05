---
description: "Apply Luna (Playwork) playable authoring constraints to C#, Odin fields, Animator controllers, prefabs, scenes, and materials. Guard editor-only Odin references; use focused skills for compatibility review and luna.json build settings."
globs: ["**/*.cs", "**/*.controller", "**/*.anim", "**/*.prefab", "**/*.unity", "**/*.mat"]
---

# Unity Luna Rules

Apply when the target is a Luna playable. Explicit Luna tier selection also enables Unity guidance.
Auto-detection uses a Luna package plus a playable target; the host's project-local `z-project.json`
`lunaPlayable` boolean overrides branch detection.

## Route by task

- **Authoring:** load `skill://z-luna-conventions`, then only the reference matching the changed behavior.
- **Compatibility review:** load `skill://z-luna-code-review`; it is report-only and selects topic checklists.
-  **Export settings:** load `skill://z-luna-build-check`; its gates are house policy, not universal
  Luna support limits.

## Odin editor-strip contract

`skill://z-unity-odin` owns general Odin policy. The canonical Luna guard and provider examples live in
`skill://z-luna-conventions/references/authoring-guards.md`.

For cosmetic Odin attributes in runtime source:
1. Guard both attributes and Sirenix imports with `#if UNITY_EDITOR && ODIN_INSPECTOR`.
2.  Keep Odin brackets separate from `[SerializeField]`; keep fields and Unity serialization attributes
   outside the guard.
3. Check provider callers before stripping the whole provider or only its editor body.

## Verify export behavior separately

This prevents cosmetic editor dependencies leaking into exported code; it does not establish that every
third-party DLL fails. Verify effective build symbols and package mappings. `SerializedMonoBehaviour`,
`SerializedScriptableObject` and `[OdinSerialize]` affect runtime serialization and need a separate
implementation decision, not automatic cosmetic guards.

Preserving `[SerializeReference]` outside guards does not prove Luna supports the required polymorphic
serialization. Confirm it with an export round trip. Version-specific runtime warnings require evidence from
the installed package/configuration or a reproducer; do not turn house preferences into compatibility bans.
