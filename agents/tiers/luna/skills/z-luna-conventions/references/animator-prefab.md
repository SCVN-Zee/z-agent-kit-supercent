# Luna Animator and prefab rules

## Animator WriteDefaults

For new controllers, prefer uniform WriteDefaults `false` as a house authoring convention. The audited V7.2.0
package does not establish it as a universal export requirement. For existing controllers, inspect intended
resets, bindings and transitions before changing it; preserve working behavior. The common Animator skill owns
graph mechanics.

When the batched Animator edit cannot reach WriteDefaults, use its editor-side fallback recipe or set the
property in the Animator window. Never hand-edit `.controller` or `.anim` serialization.

## Prefab variants

Keep prefab-variant chains shallow and prefer one base with many variants for auditability. This is a house
maintainability preference, not a demonstrated Luna inheritance limit. Inspect the resolved scene instance and
verify exported overrides before declaring compatibility.

The common prefab skill owns the base-versus-variant decision and connected-instance creation workflow. This
file adds target-build verification and the house preference, not a new compatibility ban.
