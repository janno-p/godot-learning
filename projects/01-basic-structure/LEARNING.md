# 01 — Basic Structure

## Status

Completed foundations sandbox. Source was inspected; successful builds, launches, and resize behavior are learner-reported, not independently run by the assistant.

- Godot project name: `Basic Structure`.
- Source: `/mnt/d/Janno/Godot/basic-structure` (Windows: `D:\Janno\Godot\basic-structure`).
- Purpose: refresh Godot's scene, scripting, input, and UI foundations before making a game.

## Completed learning

- Scenes, nodes, script attachment, and `_Ready()`.
- C# project generation and build/run setup.
- Node references and state-driven label updates.
- A button-driven counter with an editor-connected signal.
- Responsive layout using a full-rect CenterContainer and a VBoxContainer.

## Project-generation workaround

The learner intentionally chose the preview SDK recorded in the [shared tooling notes](../../LEARNING.md#tooling).

Initial C# project generation failed with .NET 11 by learner report. Installing .NET 10 allowed generation; afterward the learner selected .NET 11 through `global.json` and reported a successful launch. The generated desktop project targets `net8.0`, confirmed by source inspection.

The original generation error was not captured. Treat this as a project-specific workaround, not a general claim that Godot cannot build with .NET 11. Preserve the learner's intentional preview setup when investigating future problems.

## Current step

None. This sandbox is complete; the active project is selected in the [workspace overview](../../LEARNING.md).

## History

Read [HISTORY.md](HISTORY.md) when investigating the original setup or exercise sequence.
