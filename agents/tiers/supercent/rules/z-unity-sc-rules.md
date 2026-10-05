---
description: "Apply Supercent's required [Dev] conventional-commit prefix and playable asset layout when Assets/Supercent/ is present. Not general Unity conventions."
alwaysApply: true
---

# Unity Supercent Rules

Supercent commit policy plus the Supercent-specific asset-layout overlay. Applies when `Assets/Supercent/` is present.

## Commit prefix (mandatory)

Every commit in this repo must start with `[Dev]` followed by a conventional-commit type, without exceptions.

- **Format:** `[Dev] <type>: <subject>`
-  **Examples:** `[Dev] feat: add player dash ability` · `[Dev] fix: stabilize attack transitions` ·
  `[Dev] chore: re-organize assets`

## Asset layout (Supercent playable ads)

Supercent projects place vendor packages beside the game-family root under `Assets/`:

```
Assets/
├── Plugins/
├── Resources/
├── Supercent/                         # Supercent project + shared code
│   ├── {Internal Submodules}/         # Shared code used across projects
│   └── {Game Name}/                   # Game family root
│       └── {Project Name}/            # Project-specific content
└── {Other Packages}
```

### Keep variants organized by asset type

Variants live at `Assets/Supercent/<GameName>/<PlayableXXX>/`. Keep content grouped by asset type, not by
Ingame/UI usage; do not use a mixed `Art/` folder. Editor-only scripts may use `Scripts/Editor/` for Unity
compilation scope.

The project-specific subtree follows the canonical default manifest linked from
`skill://z-unity-folder-setup/references/project-layout.md`.
