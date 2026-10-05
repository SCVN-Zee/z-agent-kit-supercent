---
name: z-luna-build-check
description: "Validate Luna (Playworks) luna.json export settings before a playable build; fix failing boolean gates after confirmation. Check six scene, AA, mesh, and animation gates; report four advisory groups without changing them. Not source or asset compatibility review."
---

# z-luna-build-check — Luna Build-Settings Validator + Auto-Fixer

Validate Luna Playworks `luna.json` export settings and, after confirmation, surgically fix failing gates.
Gates enforce the kit's house quality profile, not universal Luna support limits. Advisories are report-only
judgment calls. Unlike `skill://z-luna-code-review`'s
transpile-compatibility pass, this checks build-settings tuning.

## When it applies

Luna playable project only — a `luna.json` exists, or Luna/Playworks is in `Packages/manifest.json`
or `Assets/Luna/`. If none hold, say so and stop (nothing to validate).

## The bundled validator

A stdlib-only Node helper ships with this skill at `scripts/luna-build-settings.cjs` (relative to
this skill's base directory). It owns all parsing/validation/auto-fix — do not re-implement in prose.

```
node "<skill-base-dir>/scripts/luna-build-settings.cjs" validate <luna.json> [--json]
node "<skill-base-dir>/scripts/luna-build-settings.cjs" fix      <luna.json> [--write]
```

-  `validate` → prints the two-tier report; exit `0` = all gates pass, `1` = at least one gate fails, `2` =
  usage/read/parse error, `3` = incomplete (missing gates, no failures).
- `fix` (no `--write`) → **dry-run**: prints the batch diff (`path: from → to`) it *would* apply.
- `fix --write` → applies the gate fixes surgically (only the failing-gate value tokens change), then re-validates.

## Protocol

1. **Detect** Luna context (above). Not Luna → stop with that note.
2.  **Locate the source `luna.json`.** Search bounded source paths (e.g.
   `find <unity_project> -maxdepth 3 -iname luna.json`; widen only on a miss)
   and **exclude any path under `LunaTemp/`** — that is a generated build copy, never the source. 
   -  Multiple non-`LunaTemp` candidates → **ask the user which one** through the host's available question
     interface; never guess.
   - None found → **guided fallback** (step 6).
3. **Validate.** Run the `validate` command. Show its report to the user (gates table, advisories table, verdict).
4. **Auto-fix gates** — only if a gate failed. Run `fix` (dry-run) and show the batch diff + any `SKIP`
   lines (key-not-found / ambiguous → must be fixed manually). Then **ask the user to confirm** the
   write. On approval, run `fix --write`, then report the re-validation result and any residual failures.
5. **Scene gate is validate-only.** Enabled scenes = `unity.scenes` minus `unity.disabledScenes`; pass iff
   exactly 1 enabled. If not, report the enabled scenes and ask the user to fix it manually — the skill
   never auto-picks a scene.
6. **Guided fallback** (no `luna.json`): walk `references/luna-build-settings-checklist.md`; ask the user to
   paste the Luna Build-window settings (or screenshots of the Settings/Assets tabs); classify each
   PASS/WARN/FAIL by reasoning. **Report only — no writes** in fallback mode.
7. **Advisories are always report-only** (shadows, textures, sound, fonts): surface actual vs house
   reference with a "confirm intentional?" note. Do not write them — they are per-project judgment calls.

## Safety (must respect)

- **Editor-open hazard.** If Unity + the Luna window are open, an on-disk edit may be overwritten when
  Luna next serializes, or not picked up until reimport. Advise the user to close the Luna window (or
  reimport) before/after `--write`.
- **git is the undo net.** Only write after the confirm step; if the working tree is dirty, suggest the
  user commit/stash first so the change is reversible.
- **Never touch `overrides[]`.** Auto-fix only the listed boolean gates: `unity.disableAntiAliasing`
  and mesh/animation `*.default` values. Per-asset `texture.overrides[]` / `sound.overrides[]` are intentional tuning.
- **Missing keys are not created.** If a gate key is absent, report it `missing` and let the user add it
  in the Luna UI; the fixer skips it rather than inventing structure.

## Output format

Use the validator's report verbatim, then the verdict. Example:

```
## Luna Build-Settings Check — <luna.json>
### Gates
PASS Exactly 1 enabled scene — unity.scenes − unity.disabledScenes = 1 enabled
FAIL Mesh half-precision OFF — assets.rules.meshes.default.halfPrecision = true → should be false (auto-fixable)
...
### Advisories
WARNING Sound default bitrate (kb/s) = 64 (ref 96) — house reference 96; ...
### Verdict: FAIL 2 gate(s) failed
```

If all gates pass: report the verdict and list any advisory deviations for the user to confirm. Missing gates
mean incomplete verification, even when no present value fails. Exit 0 is house-profile compliance, not proof
that a Luna build succeeds.

## Workflow position

**Typically precedes:** a Luna build (Cmd+E export), shipping, or release.
**Related:** `skill://z-luna-code-review` (compatibility lens — complementary).
