# Game development learning

Active project: [03 — Crate-pushing](projects/03-crate-pushing/LEARNING.md).

## Learner

- Goal: learn game development broadly, not just programming. The [coaching agreement](AGENTS.md#coaching-agreement) governs how we work together.
- Background: experienced programmer. Favorites are F# and Rust; also comfortable with C# and C++ (C++ not used recently). Has tried Python, Ruby, JavaScript/TypeScript, and Java, but these are not first choices.
- Engine experience: experimented with an engine previously, then refreshed Godot foundations in Basic Structure.
- Interests: puzzle, turn-based strategy, and simulation games. Long-term project remains open.
- After Lights Out: learned a few Godot usage details, reported no current uncertainties, and enjoyed coding most. Use that feedback to emphasize gameplay programming without dropping design, feedback, testing, and releasing.
- Tester availability: no external tester available as of 2026-11-01. A solo clarity audit was used instead; it is not equivalent to fresh-eyes feedback.

## Tooling

- Learner-reported environment: Windows 11, Godot 4.7.2 .NET editor, and .NET SDK `11.0.100-rc.3.26522.148`.
- C#/.NET is the chosen engine-learning route. The preview SDK is intentional; preserve that choice. The initial project-generation workaround and its limits are in [01 — Basic Structure](projects/01-basic-structure/LEARNING.md#project-generation-workaround).
- The [official C# prerequisites](https://docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/c_sharp_basics.html#prerequisites) describe the separate SDK requirement.
- Recorded platform tradeoff: Godot 4 C# projects cannot export to the web. Recheck support before planning browser delivery; GDScript is an alternative. F# or Rust integration remains a possible later exploration, not an agreed next task.
- Godot source base: `/mnt/d/Janno/Godot` (Windows: `D:\Janno\Godot`). This repository holds coaching notes; the source projects stay separate.

## Project index

| Order | Project notes | Status |
| --- | --- | --- |
| 01 | [Basic Structure](projects/01-basic-structure/LEARNING.md) | Foundations sandbox complete |
| 02 | [Lights Out](projects/02-lights-out/LEARNING.md) | First game: version one and reflection complete |
| 03 | [Crate-pushing](projects/03-crate-pushing/LEARNING.md) | Second game: concept agreed, level sketch pending |

Each project's `LEARNING.md` owns its brief, decisions, progress, verification limits, and current exercise. Its `HISTORY.md` preserves past milestones for reference.
