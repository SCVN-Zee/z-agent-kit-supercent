---
name: z-luna-conventions
description: "Author Luna (Playwork) playable C#, Odin Inspector fields, Animator controllers, and prefab variants with editor-strip and export-safe conventions. Not for report-only compatibility review or luna.json build-settings validation."
---

# z-luna-conventions — Luna authoring rules

`rule://z-luna-rules` loads this skill for Luna playable authoring. This skill owns only constraints that
exist because the Luna/Playwork export target differs from the Unity player.

## Load only the matching reference

| Reference | Use |
| --- | --- |
| [`references/authoring-guards.md`](references/authoring-guards.md) | Odin guards, serialized fields, provider methods, and identifier pickers. |
| [`references/animator-prefab.md`](references/animator-prefab.md) | WriteDefaults and prefab-variant house preferences plus export verification. |
| [`examples/tabbed-component.md`](examples/tabbed-component.md) | Only when a larger Inspector needs a worked tabbed layout; not a routine prerequisite. |

## Workflow

1.  Apply the common Unity conventions as relevant: `skill://z-unity-code-conventions`,
   `skill://z-unity-asset-conventions`, or `skill://z-unity-odin`.
2. Apply `references/authoring-guards.md` to every Odin `using` and attribute in runtime-transpiled source.
3. Apply `references/animator-prefab.md` to Animator and prefab changes.
4.  Keep Unity serialization attributes and their fields outside Odin guards. For `[SerializeReference]`,
   separately verify polymorphic data survives Luna export; preserving the declaration does not prove support.
5.  Send export compatibility findings to `skill://z-luna-code-review`; this skill does not perform
   report-only review.

## Boundary

This skill applies only when the target is a Luna playable. It does not replace generic Odin presence
detection, common Unity serialization rules, or the Luna build-settings validator.
