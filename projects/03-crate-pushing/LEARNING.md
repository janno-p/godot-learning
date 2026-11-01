# 03 — Crate-pushing

## Status

Active project, at the level-sketch stage. The game concept is agreed; no map or winning move sequence has been received.

`crate-pushing` is a notes-folder label, not a learner-chosen Godot project name. No new source project or source-folder name has been chosen or created by report. The learner will create and name the actual source folder when project setup is assigned.

## Agreed direction

- A small Sokoban-style puzzle: move a character on a grid and push crates onto goal tiles.
- One handmade level with placeholder visuals.
- Learn keyboard input, positioned 2D world nodes rather than UI-only buttons, blocked movement/pushing rules, and small level design.
- Emphasize gameplay programming, building on the learner's enjoyment of coding while retaining design, feedback, and testing work.

## Proposed baseline for the first sketch

These detailed rules and constraints were proposed for the exercise; they have not yet been separately confirmed through a submitted level.

- Each action moves one tile up, down, left, or right.
- Walls block movement.
- A crate can be pushed only if the next tile beyond it is empty floor or an unoccupied goal.
- No pulling.
- Win when the crate reaches the goal.

## Current exercise

Sketch a rectangular map, at most 6×6 including its enclosing walls, with one player, one crate, and one initially unoccupied goal.

| Symbol | Meaning |
| --- | --- |
| `#` | Wall |
| Space | Floor |
| `@` | Player |
| `$` | Crate |
| `.` | Goal |

Find a winning sequence using `U`, `D`, `L`, and `R`. The level need not be clever or difficult yet. No Godot project setup or source editing is assigned for this step.

**Purpose:** create a solvable example to build and test movement/pushing against, linking a small design task to the learner's coding interest.

**Completion check:** the learner sends the map in a code block and a legal winning move sequence. Review them together before assigning implementation. Both are still pending; the notes reorganization did not complete or replace this exercise.

## History

Read [HISTORY.md](HISTORY.md) when tracing concept selection or the notes-organization decision.
