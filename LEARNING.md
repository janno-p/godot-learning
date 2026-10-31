# Game development learning journal

## Starting point

- Goal: learn game development broadly, not just programming.
- Background: experienced programmer. Favorites are F# and Rust; also comfortable with C# and C++ (C++ not used recently). Has tried Python, Ruby, JavaScript/TypeScript, and Java, but these are not first choices.
- Engine experience: tried a game engine in the past, but expects to need a refresher.
- Development OS: Windows 11 (learner-reported).
- Engine: Godot 4.7.2 installed (learner-reported); proceeding with the C#/.NET route for the first build.
- .NET SDK: `11.0.100-rc.3.26522.148` (learner-reported). Preview SDK is an intentional choice. Learner installed .NET 10 to bootstrap Godot's C# project generation, then selected .NET 11 through `global.json`. The generated project targets `net8.0` for desktop (file inspected); successful launch is learner-reported, not independently run by the assistant.
- Interests: puzzle, turn-based strategy, and simulation games.
- Long-term project: open; no specific game in mind.
- Working arrangement: learner implements games in source folders they create; assistant guides and helps investigate problems. See `AGENTS.md` for the coaching agreement.
- Game project base: `/mnt/d/Janno/Godot` (Windows: `D:\Janno\Godot`), separate from this notes workspace.
- Current project: `/mnt/d/Janno/Godot/basic-structure` (Godot project name: `Basic Structure`).

## Tooling

Use Godot's .NET editor with C# to build on existing language familiarity while learning the engine. The [official prerequisites](https://docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/c_sharp_basics.html#prerequisites) describe the separate SDK requirement. Keep the learner's intentional preview SDK setup. Project generation and subsequent build/run behaved differently: the learner reports generation failed with .NET 11 but worked with .NET 10; switching back to .NET 11 afterward allowed the project to launch. Exact generation failure details were not captured, so this is a project-specific workaround, not a general compatibility conclusion.

Tradeoff: Godot 4 C# projects currently cannot export to the web. Prefer GDScript if browser-playable builds are important. F# or Rust integration can be explored later rather than adding it to the initial engine-learning task.

## Proposed direction — not yet agreed

Start with a small, finishable 2D game rather than a large strategy game or simulation.

Suggested first project: a Lights Out-style grid puzzle. Clicking a cell toggles it and its orthogonal neighbors; the goal is to turn every cell off. This offers a small scope for learning Godot's scene system, input, state, UI, feedback, and exporting without requiring much artwork.

Possible checkpoints, to break into individual exercises once the project is selected:

1. Get a minimal project running and understand scenes and nodes.
2. Make a board interactive and implement its rules.
3. Add a solvable starting board, win detection, and restart.
4. Make the rules and outcomes understandable through UI and feedback.
5. Playtest, fix problems, and export a playable build.
6. Reflect on what worked and choose the next project or a small design variation.

## Current step

Sandbox foundations complete by learner report and source inspection: scenes/nodes, script attachment, `_Ready()`, node references, state-driven UI updates, editor-connected signals, and container-based layout. The `CenterContainer` uses Full Rect, both controls are under the VBoxContainer, the label lookup matches the new hierarchy, and the button connection still targets `OnButtonPressed`. Runtime behavior has not been independently run by the assistant.

Next exercise: choose a tiny first game and describe its scope in a short brief in chat. Lights Out remains the recommendation, not an agreed selection. No new Godot project or implementation is needed for this exercise.

Purpose: practice game design and scope control before implementation. The learner's brief should specify the goal/win condition, the player's action and exact rules (including edge cases), the starting board, feedback for state/victory, restart behavior, and three deliberately excluded features.

Completion check: a game is selected and all six brief topics are answered with no unresolved core rules; review it together before assigning implementation. No source or project configuration has been changed by the assistant.

The first game's choice and rules are still unconfirmed.

## History

- 2026-10-31: Learner described their background, interests, and preference to do the work themselves. Established this workspace's coaching notes and journal. No game project has been created or inspected.
- 2026-10-31: Learner shared language preferences, prior engine exposure, and that Godot is not installed. Recommended C#/.NET, explained the web-export tradeoff, and assigned installation as the first exercise.
- 2026-10-31: Installation exercise completed by learner report: Windows 11, Godot 4.7.2, and .NET SDK `11.0.100-rc.3.26522.148`. Learner intentionally chose the preview SDK and plans to target `net8.0`. Assigned a minimal scene and C# build/run as the next exercise; its outcome is pending.
- 2026-10-31: Learner supplied the external project location and reported a missing `.csproj`. Read-only inspection confirmed the script, label, and script attachment, but no C# project file in the root. Proposed explicit solution generation as the next setup check; resolution is pending.
- 2026-10-31: Learner resolved missing C# project generation by installing .NET 10, then returned to .NET 11 through `global.json` and reported a successful launch. Inspection confirmed the generated project and SDK-selection file. Next exercise is a button-driven counter, conditional on confirming the original label/startup-output checks.
- 2026-10-31: Learner completed the counter and renamed the handler to `OnButtonPressed`. Source inspection confirmed the matching scene connection and counter/display logic. Assigned responsive container-based layout as the next exercise; outcome pending.
- 2026-10-31: Learner found the Full Rect preset and reported the layout exercise complete. Source inspection confirmed the container hierarchy, full-rect anchors, updated label path, and preserved signal connection. Assigned choosing and briefly scoping the first game as the next exercise; no game has been selected yet.
