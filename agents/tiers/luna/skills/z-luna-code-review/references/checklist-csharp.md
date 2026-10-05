# C# and serialization

Use for changed runtime C#, compiler dependencies, Odin or serialized field contracts. For Animator and gameplay
API changes, also select their behavior-specific topic from the index.

## Compiler and dependencies

| Check | Decision and evidence |
| --- | --- |
| Compiler mode and language syntax | Do not enforce a universal C# 7.0 ceiling. V7 hints 919–920 (`Basic_ProjectSettings_*LunaCompilerV2_Hint`) describe lowering C# 8.0/9.0 before JavaScript compilation. Verify the project's mode and compile the actual feature; this is not blanket support for every library API. |
| DLL/native/SDK dependency | Identify the runtime implementation, source/stub mapping and export diagnostics. An arbitrary Unity DLL is not proof of a working JavaScript implementation. V7 hints 903 (`StubberTab_ThirdPartySdks_Hint`) describes stubbing nonessential libraries; 911 (`ExternalSourceTab_CsSources_Hint`) describes external C# sources. Do not ban every DLL, including vendor-supplied mapped assemblies. |
| Odin/editor references | Use `skill://z-luna-conventions/references/authoring-guards.md`. Keep cosmetic Sirenix attributes and imports under `UNITY_EDITOR && ODIN_INSPECTOR`; keep fields and Unity serialization attributes outside. Mixed `[SerializeField, Required]` must be split to guard only Odin. Runtime Odin serialization/base classes need a separate design, not cosmetic stripping. |
| Provider methods using UnityEditor | If the attribute and every caller are stripped by the same guard, the whole provider can be stripped. If callers remain, retain a runtime-safe method and guard its editor body. A removed `nameof` expression cannot require its target at compile time. |
| Generics and reflection | Inspect the concrete type/comparer and used reflection API; do not ban generic inheritance or all reflection from a name match. V7 hints 846 (`Advanced_GenericCheck_Hint`) documents an additional generic check, notably for DOTween. Reproduce the exact export path. |
| Incompatible identifiers, DOTS/ECS or other unsupported dependencies | Confirm against installed compiler/export diagnostics. Do not infer a specific `LP####` mapping, reserved-name list or package-wide failure from historical guidance. |

## Data correctness probes

These are verification leads, not established universal Luna defects:

-  **Struct-keyed Dictionary/HashSet:** insert a key, retrieve with an equal distinct value, and check
  hash/equality behavior with the actual comparer in the export. Do not assert that all lookups miss; class
  keys or O(n) iteration are not equivalent automatic fixes.
-  **SerializeReference:** verify derived type, field values, nulls and shared-reference behavior after
  export. Keeping the Unity attribute outside Odin guards preserves the declaration, not proof of Luna
  serializer support. Do not convert polymorphic data without a reproduced failure and an agreed migration.
-  **Deep reflection or generated code:** check retained types/members and Runtime Analysis exclusions as well
  as API availability. A stripped member and an unsupported API are different causes.

Search only scoped source paths for `Sirenix`, `UnityEditor`, `SerializeReference`, `Dictionary`, `HashSet`
and relevant reflection APIs. Inspect guards and call sites before reporting. A successful Unity compilation
does not establish Luna export compatibility.
