# 01 — Basic Structure

## Status

Completed foundations sandbox. Source was inspected; successful builds, launches, and resize behavior are learner-reported, not independently run by the assistant.

- Godot project name: `Basic Structure`.
- Source: [godot-projects/01-basic-structure](https://github.com/janno-p/godot-projects/tree/main/01-basic-structure).
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

That SDK-selection file is historical: the learner has since removed `global.json` as unnecessary. Source inspection confirmed its absence and the move of preview C#, implicit usings, nullable checks, and SDK preview-message suppression into the source repository's shared [Directory.Build.props](https://github.com/janno-p/godot-projects/blob/main/Directory.Build.props). Current SDK requirements and their learner-reported roles are recorded in the [shared tooling notes](../../LEARNING.md#tooling); the revised configuration has not been independently built or run.

At the learner's explicit request, `IUnion` and `UnionAttribute` now live unchanged in the source repository's [shared/System.Runtime.CompilerServices/](https://github.com/janno-p/godot-projects/tree/main/shared/System.Runtime.CompilerServices) folder and are included as linked source through root `Directory.Build.props`. Local copies and their unreferenced UID sidecars were removed. Static configuration/source checks passed; the build attempt could not start because the assistant-accessible `dotnet.exe` reports no installed SDKs.

The original generation error was not captured. Treat this as a project-specific workaround, not a general claim that Godot cannot build with .NET 11. Preserve the learner's intentional preview setup when investigating future problems.

## Current step

None. This sandbox is complete; the active project is selected in the [workspace overview](../../LEARNING.md).

## History

Read [HISTORY.md](HISTORY.md) when investigating the original setup or exercise sequence.
