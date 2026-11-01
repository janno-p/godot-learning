# 02 — Lights Out

## Status

Version one is complete, including Windows export, packaging, and a relocated-package launch by learner report. This was the first game, following the Basic Structure sandbox.

- Godot project name: `Lights Out`.
- Source: `/mnt/d/Janno/Godot/lights-out` (Windows: `D:\Janno\Godot\lights-out`).
- Source and export artifacts were inspected. Windows execution and gameplay checks were performed by the learner, not independently by the assistant.

## Agreed brief

- Board: a 3×3 grid of lights.
- Action: clicking toggles the selected cell and its existing neighbors above, below, left, and right. No diagonals or wrapping. Corners affect three cells, edge centers four, and the center five.
- Goal: turn every light off.
- Start: always the same solvable pattern, with only the top-left, center, and bottom-right lights on (`100 / 010 / 001`).
- State feedback: bright ON cells and dimmer OFF cells, retaining text as well as color differences. Hover and pressed appearances preserve the light state.
- Status: `Turn all lights off` during play, changing to `Solved!` on victory.
- Restart: always available; restores the starting pattern, play status, and cell input.
- After victory: the solved board remains visible and cell buttons are disabled until Restart.
- Excluded from version one: random puzzle generation, a timer, and level progression.
- Visual approach: built-in controls and simple styling rather than custom artwork.

The learner's persistent instruction text is:

> Click on the light to toggle it and its nearest top-bottom-left-right neighbours

It is separate from the changing status label.

## Technical decisions

- C# with a flat, row-major board model; UI labels and styles are outputs of that model.
- The learner intentionally chose a preview `LightState` union with `On`/`Off` records instead of the suggested booleans. Supporting compiler types are intentional learner work, not files to remove as unfamiliar scaffolding.
- The inspected project targets desktop `net8.0` with preview C# enabled. The SDK selected for this game's build was not independently checked; the sandbox's SDK-selection workaround is documented in [01 — Basic Structure](../01-basic-structure/LEARNING.md#project-generation-workaround).
- Scene setup and signal subscriptions happen once. A shared reset path restores state and refreshes the UI without reconnecting handlers.

## Verification and limitations

Source inspection and learner-reported exercises covered corner/edge/center moves without wrapping, applying a move twice to restore the board, model-based all-Off detection, post-win input blocking, and repeated Restart behavior.

The learner reproduced default gray hover and black pressed backgrounds, then added state-specific interaction styles and reported the styling exercise complete. Runtime visuals were not independently observed. If revisiting visual polish, check pressed-text contrast: it currently uses light text on moderately darkened backgrounds.

No unfamiliar tester was available. A solo screen audit identified the absent click-rule explanation, leading to the persistent instruction label. This is not equivalent to a fresh-eyes playtest.

## Export and package

- Inspected preset: Windows Desktop, x86_64.
- Export directory: `/mnt/d/Janno/Godot/lights-out/dist`.
- Required contents: `Lights Out.exe`, `Lights Out.pck`, and `data_Lights Out_windows_x86_64/`, kept together.
- Learner reports launching the export with the editor closed and checking play → win → restart.
- Learner zipped the dist folder, extracted it under `/mnt/c/Temp` (Windows `C:\Temp`), and reports the extracted application works.
- ZIP path/name and exact extracted executable subpath were not supplied. The assistant did not inspect the archive or execute the Windows binary.
- This was a same-machine portability check, not a clean-PC compatibility test. Nothing has been publicly published.

The existing dist folder is inside the source project. Prefer output outside the project for future exports, without requiring this successful build to be redone.

## Current step

None. Version one and its reflection are complete; further features are not required to count this project as finished. The next project is selected in the [workspace overview](../../LEARNING.md).

## History

Read [HISTORY.md](HISTORY.md) when tracing the original plan, milestone observations, or coaching decisions.
