# Animation compatibility

Use for Animator API calls, controller graphs, animation clips, avatars or animated skinned renderers. A C#
file changing Animator parameters belongs here even without a controller diff. Read only. Authoring fixes
belong to a separate pass.

## Source and controller checks

| Check | Decision |
| --- | --- |
| Parameter names, types and hashes | Verify every referenced parameter exists with the expected type in the assigned controller. A cached StringToHash does not create a missing parameter. Inspect actual controller assignments and overrides. |
| Play/CrossFade and direct-state selection | Distinguish state-name hashes from numeric indices and layer indices. Confirm intended transition/exit behavior. Prefer controller parameters for normal gameplay as a house design convention, not a Luna API ban. Verify special handling of zero and normalized time against the installed API/runtime before diagnosing the call. |
| HasState | Validate known-present and known-absent states in the export if the code relies on it. No V7 source evidence here establishes that it always returns true or false; toggling Animator.enabled is not a state-validation substitute. |
| Blend Trees, sub-state-machines and IK | Inspect usage and installed-version documentation/diagnostics. The audited readable package did not establish a universal ban. Keep them as targeted export checks; do not replace working graphs solely from this checklist. |
| Avatar/humanoid | V7 hints 235 (`Basic_ProjectSettings_MecanimWASM`) calls Mecanim Avatar experimental, while 633 (`ProjectDiagnostics_LP_AvatarAnimation_Description`) warns about unsupported avatar animation and suggests baking. Resolve the enabled mode and actual diagnostic before concluding support; neither text proves universal FK-only behavior. |

## Asset checks with a reachable Editor

Inspect controller parameters, transitions, layers, motion assignments and WriteDefaults through available
read-only animation/asset APIs or the Editor. If a field is inaccessible, report it unverified; do not call
the controller clean.

-  **WriteDefaults:** uniform OFF is the kit's authoring preference, not a demonstrated V7 export requirement.
  Check intended resets and transition behavior before changing an existing controller; mixed settings alone
  do not prove a Luna defect.
-  **Cross-fades:** compare bound properties/bones, reference poses, masks, root motion and sampling at the
  failing blend. Bone-set mismatch is a lead, not a proven universal cause of stretching.
-  **Null motion and speed flags:** inspect intended graph semantics. Null motion, an inactive speed parameter
  or non-unit clip time scale is not inherently invalid.
-  **Culling/offscreen updates:** inspect Animator.cullingMode and SkinnedMeshRenderer.updateWhenOffscreen
  against required gameplay updates and bounds. Measure before flagging a non-player AlwaysAnimate setting;
  CullCompletely can alter offscreen behavior.

Export/playback evidence is required for version-dependent behavior. No live Editor means
`luna asset-tier: skipped`; static source review can still finish with explicit coverage limits. Animation
compression settings belong to the build-settings skill, not this review.
