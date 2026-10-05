# Luna authoring guards

Keep cosmetic Odin dependencies out of runtime-transpiled C#. This is the kit's editor-only authoring policy,
not proof that every DLL is unsupported. Luna V7 `config.json` excludes `Editor/` source; its vendor hints
(`pipeline/resources/LunaPlayable Localisation - LunaPlayable.csv`, lines 901–903) also describe scripting
defines, source exclusion and SDK stubbing. Verify the project's effective defines.

## Guard shape

Use the combined guard exactly:

```csharp
#if UNITY_EDITOR && ODIN_INSPECTOR
using Sirenix.OdinInspector;
#endif
```

Guard each Odin attribute in its own region:

```csharp
#if UNITY_EDITOR && ODIN_INSPECTOR
[Title("Movement")]
#endif
[SerializeField] private float _speed = 5f;
```

Keep the field and its Unity serialization attribute outside the Odin guard. Stripping a field removes its
serialized contract. `[SerializeReference]` additionally needs a Luna export round-trip check; this guard is
not a serializer-support guarantee.

## Required references

Keep `[Required]` on its own line so it can be guarded without changing the serialized field:

```csharp
#if UNITY_EDITOR && ODIN_INSPECTOR
[Required]
#endif
[SerializeField] private Animator _animator;
```

For a prefab asset whose reference is supplied by the scene instance:

```csharp
#if UNITY_EDITOR && ODIN_INSPECTOR
[RequiredIn(PrefabKind.InstanceInScene)]
#endif
[SerializeField] private Camera _mainCamera;
```

## Member-referencing attributes

Check remaining callers before choosing a guard. If the attribute and all callers are editor-only, the entire provider may be editor-only too: preprocessing removes its `nameof(GetTags)` expression along with the
attribute. If runtime callers remain, keep the method and guard its editor-only body, as below. Use the same
rule for predicates and validation callbacks.

```csharp
private static IEnumerable<string> GetTags()
{
#if UNITY_EDITOR
    return UnityEditorInternal.InternalEditorUtility.tags;
#else
    return new string[0];
#endif
}
```

The same shape applies to `AnimatorController` and `AssetDatabase` providers. The serialized primitive remains
outside the guard:

```csharp
#if UNITY_EDITOR && ODIN_INSPECTOR
[ValueDropdown(nameof(GetTags))]
#endif
[SerializeField] private string _targetTag = "Untagged";
```

## Inspector composition

Prefer section-level `[Title]` and `[PropertyTooltip]` over per-field grouping when both express the same
structure. Fewer guarded attributes mean fewer export hazards. Editor windows and actions already live under
`Editor/` or `#if UNITY_EDITOR`; keep their editor-only APIs there.

## Fallback

If Odin is absent, use the common skill's built-in-attribute or runtime-assert fallback. Do not add an
unguarded Sirenix reference to make the Inspector prettier.
