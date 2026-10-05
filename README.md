# Selected agent guidance

Included tiers: **luna, supercent**. For **OMP, Pi, Codex and Claude Code**.

This is a content-only snapshot, not a standalone installer or release distribution.
No installer, package metadata, host projections or release workflows are included.
References to other tiers require separately available guidance; those tiers are not included.
See [LICENSE](LICENSE) for license notices.

## 1. Install

**Requirements:** Git. Node.js 18+ is needed only when running the included Node.js helpers.
These instructions install guidance in one project; they do not change agent model settings.

1. Clone this public repository outside your project:

   ```sh
   git clone https://github.com/SCVN-Zee/z-agent-kit-supercent.git "$HOME/z-agent-kit-guidance"
   ```

2. From your project root, copy complete selected skill directories into your agent's skill directory. Keep every resource file, not only `SKILL.md`.

   | Agent | Project skill directory |
   | --- | --- |
   | OMP | `.omp/skills/` |
   | Pi | `.pi/skills/` |
   | Codex | `.agents/skills/` |
   | Claude Code | `.claude/skills/` |

   Copy only the resources you need from this selection. If a destination already exists, compare or back it up first; do not overwrite your own changes.

   - [agents/tiers/luna/rules/z-luna-rules.md](agents/tiers/luna/rules/z-luna-rules.md)
   - [agents/tiers/luna/skills/z-luna-build-check](agents/tiers/luna/skills/z-luna-build-check)
   - [agents/tiers/luna/skills/z-luna-code-review](agents/tiers/luna/skills/z-luna-code-review)
   - [agents/tiers/luna/skills/z-luna-conventions](agents/tiers/luna/skills/z-luna-conventions)
   - [agents/tiers/supercent/rules/z-unity-sc-rules.md](agents/tiers/supercent/rules/z-unity-sc-rules.md)

3. Load the selected rules and restart or reload your agent:

   - **OMP:** copy selected rule files into `.omp/rules/`, keeping their names.
   - **Pi:** explicitly load selected rules or use an existing rule loader. This snapshot does not include the Pi rule-guidance extension; copying rules alone does not activate them.
   - **Codex / Claude Code:** merge selected rule text into your existing `AGENTS.md` / `CLAUDE.md`, or use your host's supported rule-loading mechanism. Never replace an existing instruction file wholesale.

Do not treat a missing referenced rule or skill as installed.

## 2. Update

Fetch the latest public guidance:

```sh
git -C "$HOME/z-agent-kit-guidance" pull --ff-only
```

Compare the updated checkout with the skill directories and rules you previously copied.
Manually merge local edits, then copy reviewed changes into the same project locations.
Do not blindly overwrite edited files. Remove retired files only after confirming you installed them from this repository.
Restart or reload your agent. Keep separately installed prerequisite guidance unchanged.

If the checkout has local changes or the pull cannot fast-forward, resolve them before retrying; do not use a hard reset to discard edits.

## 3. Uninstall

From the project where you installed this guidance:

1. Remove only the skill directories and rule files you copied from the selection above. Preserve any user edits you want to keep.
2. Remove only rule text or references you added to `AGENTS.md` / `CLAUDE.md` or your rule loader. Do not delete the entire instruction file or any agent directory.
3. Restart or reload your agent. You may separately remove the cloned checkout after saving local edits.

There is no installer-managed uninstall command or ownership lock in this content-only repository.
