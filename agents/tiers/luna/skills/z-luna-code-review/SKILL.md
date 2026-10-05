---
name: z-luna-code-review
description: "Review Luna (Playwork) playable C#, shaders, Animator assets, and other exported content for Bridge.NET and runtime compatibility, including Editor-only success or export regressions. Report findings only; not implementation or luna.json build-settings validation."
---

# Luna compatibility review

Review only the requested scope. Reuse `skill://z-unity-code-review` for input modes and report severity. This
skill adds Luna-specific evidence and checks.

## Workflow

1.  Confirm Luna is the requested target or is configured through `luna.json`, the installed package or
   project guidance. A `Playable` folder name alone is a clue, not proof. Without Luna context, use the
   generic review instead.
2.  Resolve the requested diff, commit, files or codebase. Read [the checklist
   index](references/checklist-luna-compatibility.md), then only its matching topic references. Route by
   behavior as well as extension: Animator calls in C# need the animation checklist.
3.  Inspect source and available export configuration. Record installed version, compiler mode, exclusions and
   relevant diagnostics. Search changed paths first; expand only for dependencies needed to establish a
   finding. Treat matches as leads, not failures.
4.  Inspect relevant assets through read-only Editor capabilities when available. Missing access or an
   unexposed field is unverified, not clean. Do not hand-edit serialized assets.
5.  Report findings supported by evidence separately from remaining verification. Read existing console/build/test results.
   Run covering tests only when authorized and they require no saving or other user-state mutation; dirty scenes block that step. Do not save scenes or enter Play Mode to complete a read-only review.

Bind Editor capabilities to the connected tools; do not assume a server or callable name.

## Boundary

Report only; do not edit code, assets, settings or Editor state. A fix belongs in a separately authorized
implementation pass. Build-settings house policy belongs to `skill://z-luna-build-check`; general
authoring conventions belong to `skill://z-luna-conventions`.

## Output

Use the generic review format and tag each finding `luna`. Include `file:line`, impact, one-line fix, and
evidence (installed version/configuration, vendor source or reproduced export behavior). Cite an `LP####`
number only when its mapping is verified for the installed version.

```text
Luna Review: N issues (X critical, Y informational)
- [file:line] Problem (lens: luna)
  Evidence: version/configuration or reproduction
  Fix: one line
Verify: compile <ok|errors|not run>; tests <pass N|fail N|not run>; luna asset-tier: <ran|partial|skipped>
Coverage: topics read; requested checks left unverified
```

If no findings are evidenced, say `No issues found in inspected scope`, not that an unbuilt playable is
compatible. List unresolved export checks without presenting them as defects.
