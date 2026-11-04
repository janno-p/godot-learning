# Game development learning

Active project: [04 — Turn-based duel](projects/04-turn-based-duel/LEARNING.md).

## Learner

- Goal: learn game development broadly, not just programming. The [coaching agreement](AGENTS.md#coaching-agreement) governs how we work together.
- Background: experienced programmer. Favorites are F# and Rust; also comfortable with C# and C++ (C++ not used recently). Has tried Python, Ruby, JavaScript/TypeScript, and Java, but these are not first choices.
- Engine experience: experimented with an engine previously, then refreshed Godot foundations in Basic Structure.
- Interests: puzzle, turn-based strategy, and simulation games. Long-term project remains open.
- After Lights Out: learned a few Godot usage details, reported no current uncertainties, and enjoyed coding most. Use that feedback to emphasize gameplay programming without dropping design, feedback, testing, and releasing.
- After Crate-pushing: node grouping was new, and mapping between grid cells and screen coordinates was interesting. Reports no current uncertainties and enjoys progression toward more complicated concepts. Build incrementally on established skills, emphasizing new conceptual depth rather than merely adding content.
- After Turn-based duel: simple game-state management was the main learning takeaway. Reports no current uncertainties, and the exercise steps were about the right size.
- Coaching preference (2026-11-04): keep each exercise focused on its specific new topic. Avoid repeatedly assigning already-practiced work, especially polishing on-screen instructions. Retain the current step size; treat familiar setup/polish as brief checks when relevant, and tie verification to the changed behavior. Dedicated exercises in these areas should introduce a new concept or address a concrete issue.
- Tester availability: no external tester available as of 2026-11-01. A solo clarity audit was used instead; it is not equivalent to fresh-eyes feedback.

## Tooling

- Learner-reported environment: Windows 11, Godot 4.7.2 .NET editor, and .NET SDK `11.0.100-rc.3.26522.148`.
- Learner clarification: the .NET 10 SDK is required to load and compile the C# projects in Godot, not just for initial project generation. Keep it installed alongside the .NET 11 SDK, which supplies the C# 15 preview language features used by the projects. These roles are learner-reported, not independently verified.
- Shared C# configuration is now in [godot-projects/Directory.Build.props](https://github.com/janno-p/godot-projects/blob/main/Directory.Build.props): preview language version, implicit usings, nullable reference checks, and SDK preview-message suppression. Source inspection confirmed both project files no longer duplicate these settings and no `global.json` remains in the source repository. The learner reports SDK pinning is unnecessary; effective SDK selection and builds have not been independently rechecked.
- At the learner's explicit request, union support types were consolidated unchanged into [godot-projects/shared/System.Runtime.CompilerServices/](https://github.com/janno-p/godot-projects/tree/main/shared/System.Runtime.CompilerServices). Root `Directory.Build.props` automatically includes them as linked source for inheriting projects; local duplicate definitions and unreferenced UID sidecars were removed. Static XML/path/source-uniqueness checks passed. Both build attempts were blocked before compilation because the `dotnet.exe` available to the assistant reports no installed SDKs; this does not supersede the learner's reported tooling setup.
- C#/.NET is the chosen engine-learning route. The preview SDK is intentional; preserve that choice. The initial project-generation workaround and its limits are in [01 — Basic Structure](projects/01-basic-structure/LEARNING.md#project-generation-workaround).
- The [official C# prerequisites](https://docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/c_sharp_basics.html#prerequisites) describe the separate SDK requirement.
- Recorded platform tradeoff: Godot 4 C# projects cannot export to the web. Recheck support before planning browser delivery; GDScript is an alternative. F# or Rust integration remains a possible later exploration, not an agreed next task.
- Repositories: [godot-learning](https://github.com/janno-p/godot-learning) holds coaching notes; [godot-projects](https://github.com/janno-p/godot-projects) holds the separate Godot source projects. The local source checkout is `/mnt/d/Janno/Godot`, supplied by the learner and inspected on 2026-11-02. Use GitHub links for cross-repository references and relative links within these learning notes.

## Project index

| Order | Project notes | Status |
| --- | --- | --- |
| 01 | [Basic Structure](projects/01-basic-structure/LEARNING.md) | Foundations sandbox complete |
| 02 | [Lights Out](projects/02-lights-out/LEARNING.md) | First game: version one and reflection complete |
| 03 | [Crate-pushing](projects/03-crate-pushing/LEARNING.md) | Second game: version one and reflection complete |
| 04 | [Turn-based duel](projects/04-turn-based-duel/LEARNING.md) | Version one, snapshot undo/redo, and awaited movement complete; parallel attack-feedback exercise assigned |

Each project's `LEARNING.md` owns its brief, decisions, progress, verification limits, and current exercise. Its `HISTORY.md` preserves past milestones for reference.
