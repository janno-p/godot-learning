# 04 — Turn-based duel

## Status

Active project, at the first turn-model exercise. The learner selected a tiny grid-based turn-based duel after completing Crate-pushing. The general direction is agreed; the detailed rules below are a proposed baseline for a paper trace, not yet a separately confirmed game specification. No turn trace or implementation has been received.

`04-turn-based-duel` is a numbered notes-folder label, not a learner-chosen Godot project/source-folder name. The learner will create and name the actual source folder when project setup is assigned. No Godot setup or game-source editing is assigned for this first exercise.

## Agreed direction and learning focus

- A tiny duel in one small grid-based room, with one player, one enemy, and placeholder visuals.
- Build on cell/position conversion, positioned world nodes, discrete input, and rule checks from Crate-pushing.
- Introduce explicit turn ownership and state transitions, reusable actor scenes, and simple enemy decisions incrementally rather than increasing map size or content count.
- Keep inventory, procedural generation, and multiple levels outside the initial scope.
- Adapt to the learner's reported enjoyment of progression toward more complicated concepts, while continuing to include design, feedback, testing, and delivery.

## Proposed baseline for the first trace

These rules make a small deterministic encounter to reason about before implementation. They are proposals for this exercise; review the learner's trace and any requested changes before treating the detailed rules as settled.

- The player acts first. Each actor begins with 3 HP, and an attack deals 1 damage.
- A player direction command targets the next orthogonally adjacent cell. If empty, move there; if occupied by the enemy, attack instead and stay in the current cell.
- After a successful player action, the enemy takes exactly one action, provided the duel has not ended.
- At the start of its action, the enemy attacks if already orthogonally adjacent to the player. Otherwise it moves one cell toward the player. Moving into adjacency does not also grant an attack that turn.
- When either actor reaches 0 HP, end the duel immediately. A defeated enemy does not get a response, and neither actor acts after the terminal result.
- The first trace uses a clear row with no obstacles. Wall/invalid-action policies, general movement tie-breaking, pathfinding, presentation timing, and input details are not assigned or settled by this trace.

## Current exercise

Trace a short encounter on paper or in a text table, using the proposed rules:

| Actor | Initial cell | Initial HP |
| --- | --- | --- |
| Player | `(1, 2)` | 3 |
| Enemy | `(4, 2)` | 3 |

The cells between them are empty. Issue four player **right-direction commands**, stopping early if the duel ends. For each individual actor action, record:

- Who acts and whether they move or attack.
- Both actors' cells and remaining HP afterward.
- Who acts next, or whether the duel has ended.

Do not combine the player and enemy actions into one row: the distinction is the learning target. No Godot project or code is needed yet, and no complete trace has been provided by the coach.

**Purpose:** make turn ownership and the move-versus-attack distinction explicit, including the boundary between a successful player action, an enemy response, and immediate combat termination.

**Completion check:** the learner sends a consistent action-by-action trace. Review it together, especially whether an enemy that just moved can also attack and whether a defeated enemy receives a final action. The trace and detailed-rule confirmation remain pending; only after that review should the next exercise be assigned.

## History

Read [HISTORY.md](HISTORY.md) when tracing the project choice or earlier exercises. Shared learner/tooling context remains in the [root learning notes](../../LEARNING.md).
