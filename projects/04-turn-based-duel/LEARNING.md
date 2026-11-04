# 04 — Turn-based duel

## Status

Version one and reflection are complete. The learner exported to `/mnt/d/Janno/Exports/Turn Based Duel/` and reports the assigned smoke checks done. Inspection confirms a Windows x86_64 preset, executable, resource pack, and .NET data files; runtime success and release-mode selection are learner-reported, not independently verified. Local source commit `255f947349e55a79727827d3309451896155ec7f` remains the verified version-one checkpoint.

The focused snapshots/undo/redo extension is now complete by source review and learner-reported checks. Two histories preserve the state being left, restore snapshots directly, safely handle empty history, and discard redo only on new accepted gameplay actions. Restart clears both histories; presentation is derived from state. No independent runtime test, new export, or later commit of the extension has been verified.

Godot tweens and asynchronous turn sequencing are complete by source review and learner-reported follow-up completion on 2026-11-04. Each action now awaits its own movement before handing over the turn; blocked commands and immediate attacks create no tween. The nonqueueing semaphore gate spans the full player/enemy cycle, with exception reporting at the async callback boundary and release in `finally`. Idle undo/redo still restore snapshots immediately. No independent build/runtime test or newer export/checkpoint was verified. The learner has accepted parallel attack feedback as the next focused topic; its current exercise adds a composed lunge/hit flash and awaits its completion before handoff. Implementation and checks are pending. Retain the established exercise size and focus on new concepts; tactical design remains optional.

The learner created and supplied `/mnt/d/Janno/Godot/04-turn-based-duel/` in the separate `godot-projects` source repository. The source folder is learner-chosen and happens to match the numbered notes-folder label. The Godot application title is `Turn Based Duel`. Source reference: [04-turn-based-duel](https://github.com/janno-p/godot-projects/tree/main/04-turn-based-duel). The coach has inspected the local project but has not created or edited game source or verified a remote push.

## Agreed direction and learning focus

- A tiny duel in one small grid-based room, with one player, one enemy, and placeholder visuals.
- Build on cell/position conversion, positioned world nodes, discrete input, and rule checks from Crate-pushing.
- Introduce explicit turn ownership and state transitions, reusable actor scenes, and simple enemy decisions incrementally rather than increasing map size or content count.
- Keep inventory, procedural generation, and multiple levels outside the initial scope.
- Adapt to the learner's reported enjoyment of progression toward more complicated concepts, while continuing to include design, feedback, testing, and delivery.

## Design feedback and current progression

- On 2026-11-03, the learner observed that the baseline offers no apparent strategic element and asked whether its purpose is implementing turn-based mechanics. This is a design concern, not confirmation of the baseline as a finished game specification.
- The baseline intentionally isolates turn ownership, one-action responses, damage accounting, and terminal-state handling. The traced encounter has no meaningful tactical choice: once adjacent, repeated attacks win for the player who attacks first.
- The learner agreed to follow the initial mechanics-first plan, and the right-only baseline loop is now complete. The coach subsequently proposed a paper sketch for a telegraphed enemy attack; no sketch or detailed tactical rules were received.
- The learner then clarified: "You may ignore my remark about strategy if it wasn't the original goal of the exercise." Continue the original focus on turn ownership, state transitions, reusable actors, and simple enemy behavior, alongside player-facing feedback and testing. A tactical addition is optional, not a required next milestone or active assignment.
- At reflection, the learner requested stronger focus on each exercise's new topic, with less repetition of already-practiced work such as on-screen instruction polishing. Keep the established step size and use brief, relevant checks for familiar supporting work. This feedback governs future coaching; it does not mean skipping verification of new behavior.

## Agreed baseline for the initial mechanics milestone

The combat rules were proposed for the paper trace, then accepted for the initial mechanics milestone when the learner agreed to follow the initial plan on 2026-11-03. The room/input policies below were subsequently implemented in the completed movement exercise. The original trace remains a reference case for regression checks.

- The player acts first. Each actor begins with 3 HP, and an attack deals 1 damage.
- A player direction command targets the next orthogonally adjacent cell. If empty, move there; if occupied by the enemy, attack instead and stay in the current cell.
- After a successful player action, the enemy takes exactly one action, provided the duel has not ended.
- At the start of its action, the enemy attacks if already orthogonally adjacent to the player. Otherwise it moves one cell toward the player. Moving into adjacency does not also grant an attack that turn.
- When either actor reaches 0 HP, end the duel immediately. A defeated enemy does not get a response, and neither actor acts after the terminal result.
- The room is an empty 6×5 grid: `0 <= x < 6` and `0 <= y < 5`. Out-of-bounds player commands change nothing and consume no turn.
- Four arrow-key input actions each request one cardinal step per press; held-key echo is ignored.
- Enemy adjacency is Manhattan distance 1. Otherwise, pursuit steps horizontally toward the player whenever X differs, and vertically toward the player when X matches. Enemy resolution is immediate within the accepted input event.
- The physical R key reloads the scene during play or after a result, restoring the encounter's initial state.
- Obstacle/pathfinding rules and animated turn presentation are not part of the implemented baseline.

## Completed exercise — first turn trace

Trace a short encounter on paper or in a text table, using the baseline rules:

| Actor | Initial cell | Initial HP |
| --- | --- | --- |
| Player | `(1, 2)` | 3 |
| Enemy | `(4, 2)` | 3 |

The cells between them are empty. Issue four player **right-direction commands**, stopping early if the duel ends. For each individual actor action, record:

- Who acts and whether they move or attack.
- Both actors' cells and remaining HP afterward.
- Who acts next, or whether the duel has ended.

Do not combine the player and enemy actions into one row: the distinction is the learning target. No Godot project or code is needed yet, and no complete trace has been provided by the coach.

**Result (2026-11-03):** the learner corrected the HP accounting after the first enemy attack and completed a consistent seven-action trace. After the initial moves to player `(2, 2)` and enemy `(3, 2)`, both actors stay in place while alternating attacks. The fourth player command defeats the enemy; final HP is player 1, enemy 0. The learner correctly ends the duel immediately without an enemy response. The enemy's move into adjacency also correctly consumes its entire action.

**Purpose:** make turn ownership and the move-versus-attack distinction explicit, including the boundary between a successful player action, an enemy response, and immediate combat termination.

**Completion check — met:** the learner sent a consistent action-by-action trace, including separate actor turns, damage accounting, and immediate termination. Reviewed directly from the submitted text.

## Completed exercise — reusable actor scene and static starting layout

Create and name the new Godot C# project in the existing `godot-projects` source repository, then display the encounter's starting state using two instances of one saved actor scene.

- Use a `Node2D` main scene with a simple floor background large enough to show the encounter.
- Create a reusable actor scene with a `Node2D` root and one small placeholder shape. Suggested visual: a white `Polygon2D` square inset within a 48-pixel cell, building on the previous project's placement work.
- Instance that saved scene twice in the main scene, naming the instances `Player` and `Enemy`. Use each instance root's `Modulate` to give them distinct colors while sharing the placeholder shape.
- Use 48-pixel cell spacing and place the instances at the traced initial cells: player `(1, 2)` and enemy `(4, 2)`. Let the learner calculate pixel positions using the known cell-to-position conversion.
- Scope this exercise to the static starting layout and scene reuse. Gameplay state, movement, combat, room boundaries, and input policies will be introduced in subsequent exercises.
- As a reuse check, temporarily change the placeholder size in the saved actor scene and verify that both instances update, then restore the intended size.

### Shape setup guidance

The learner requested concrete guidance for the floor and actor placeholders:

- In the main scene, add a `Polygon2D` named `Floor`, at local position `(0, 0)`. Set its four polygon points to `(0, 0)`, `(288, 0)`, `(288, 240)`, `(0, 240)`, and choose a dark gray color. This is a single rectangle with a 6×5-cell visual footprint; gameplay boundaries were settled later in the room-movement exercise.
- Place `Floor` before the actor instances in scene-tree order so the actors draw above it with their default equal Z indices.
- In the saved actor scene, add a `Polygon2D` child at local position `(0, 0)`, with points `(8, 8)`, `(40, 8)`, `(40, 40)`, `(8, 40)`. Keep its color white. This gives a 32×32 square with an 8-pixel margin inside a 48×48 cell.
- Tint the `Player` and `Enemy` instance roots using `Modulate`, for example cyan and orange. The root's position denotes the cell's top-left corner; the inset is already encoded in the shape's local points.

**Purpose:** learn reusable scene instancing and per-instance variation while establishing the visual starting state for the turn loop.

**Completion check — met:** the learner reports the exercise done and supplied the source path and a running-game screenshot showing a cyan player and orange enemy over the gray floor. The requested no-error run and temporary shared-scene edit check are accepted as learner-reported completion; the coach has not independently exercised them.

### Source and screenshot review (2026-11-03)

- `Main.tscn` has a `Node2D` root with `Main.cs`, the 288×240 floor polygon first, and `Player`/`Enemy` instances referencing the same `Actor.tscn` resource. Their instance-root tints are cyan and orange.
- `Actor.tscn` contains a `Node2D` root and the expected inset polygon points `(8, 8)` through `(40, 40)`, with the default white color. No duplicated per-instance geometry is present.
- The learner initializes `_playerCell` to `(1, 2)` and `_enemyCell` to `(4, 2)` in `Main._Ready()`, then sets each node's position using `CellToScreen`, multiplying each coordinate by 48. Positions are set in code, not saved on the scene instances.
- At the setup review, `project.godot` pointed to the main scene and had no custom input actions; `Main.cs` had no gameplay input or turn logic. The screenshot was consistent with that source; no assistant-run build or game execution was performed.

## Completed exercise — one player move hands off the turn

Add explicit turn ownership to the existing `Main.cs` and use it to gate the first player move.

- Represent the current turn explicitly, for example with an enum containing `PlayerTurn` and `EnemyTurn`, starting on `PlayerTurn`. Keep this state in `Main`, which coordinates both actor instances.
- Add a `move_right` Input Map action bound to the right arrow. Use event-based `_UnhandledInput` and `IsActionPressed` as in Crate-pushing.
- Accept that action only during `PlayerTurn`. Move the player's logical cell one step right, update its position through `CellToScreen`, then hand ownership to `EnemyTurn`.
- Print the initial turn and the turn after the handoff to Godot's Output panel so the transition is observable.
- For this checkpoint, deliberately pause on `EnemyTurn`: later player commands are ignored and the enemy remains stationary. This temporary pause isolates the handoff; it does not change the agreed eventual one-action enemy-response rule. The tested move is from `(1, 2)` to empty `(2, 2)`; general movement legality and combat are subsequent work.

**Purpose:** make input permission depend on explicit game state and establish the boundary between a player action and an enemy response.

**Completion check — core behavior met:** the learner reports completion. Source inspection at this checkpoint confirmed a fresh run started in `PlayerTurn`, the first right-arrow press moved the player to `(2, 2)` and handed off to `EnemyTurn`, and the early guard ignored later input. The enemy remained at `(4, 2)`, and `_Ready()` initialized the starting cells and turn on a fresh run. The handoff was printed, but the requested startup turn message was absent. That observability omission was carried into, and resolved during, the automatic enemy-response exercise below. No assistant-run build or runtime test was performed.

### Implementation review (2026-11-03)

- `Scripts/GameTurn.cs` uses the learner's chosen C# preview union approach: `PlayerTurn` and `EnemyTurn` records, a `GameTurn` union, and a `ToString()` switch returning the state names. An enum was only an example, not a requirement.
- `Main._UnhandledInput` returns unless `_gameTurn is PlayerTurn`, accepts `move_right` through `IsActionPressed`, updates the logical cell and rendered position, then sets `EnemyTurn` and prints the transition. Unrelated input leaves the turn unchanged.
- `project.godot` binds `move_right` to the physical right-arrow key. `_Ready()` sets `PlayerTurn` but does not print it.

## Completed exercise — one automatic enemy response

Extend the right-arrow encounter so each player action is followed by one enemy action and control returns to the player. Represent attacks with output messages for this checkpoint.

- Add the outstanding initial-turn print after initialization in `_Ready()`.
- For a player right command during `PlayerTurn`, inspect the target cell. If it is empty, move there; if it contains the enemy, print a player attack and keep the player in place.
- Hand off to `EnemyTurn` and resolve exactly one enemy action immediately. If already orthogonally adjacent at the start of that action, print an enemy attack. Otherwise, move one cell toward the player along their shared row and update the enemy's visual position.
- Moving into adjacency consumes the enemy action; it must not also produce an attack. After resolving the action, return to `PlayerTurn`.
- Print each actor's action and the turn transitions so the sequence is inspectable. Keep action handling event-based and invoke the enemy response once from the player-action handoff.
- The exercised encounter remains on the initial clear row with the right-arrow command. Attack messages are a temporary step toward damage and termination; HP and terminal-state behavior will follow. General movement tie-breaking and room-boundary policies remain unassigned.

**Purpose:** implement a complete player/enemy action cycle, including occupancy-based move-or-attack decisions and the distinction between moving into attack range and attacking.

**Completion check — met:** the learner reports the exercise done. Source inspection supports the required two-press sequence: the first press moves player to `(2, 2)` and enemy to `(3, 2)`; the second prints one attack for each actor with positions unchanged. Each cycle returns to `PlayerTurn`. Event-based `IsActionPressed` retains default echo rejection, and startup/actions/handoffs are printed. Runtime checks, including held-key behavior, are learner-reported, not independently executed.

### Implementation review (2026-11-03)

- `Main._Ready()` now prints the initial turn, resolving the earlier omission.
- `_UnhandledInput` retains the player-turn guard, ignores unrelated input, then calls `PlayerAction(candidateCell)` followed by `EnemyAction()` once per accepted event.
- `PlayerAction` compares the candidate cell with the enemy cell and uses mutually exclusive attack-message/move branches before changing to `EnemyTurn`.
- `EnemyAction` checks the cell immediately to its left before acting, then either prints an attack or moves there, and returns control to `PlayerTurn`. This is sufficient for the assigned initial row with the enemy to the player's right; general pursuit/adjacency behavior is not implemented or claimed.
- `ChangeTurn` centralizes state assignment and transition output. The unconditional enemy call and unconditional normal-turn handoffs will need to respect terminal results in the next exercise.

## Completed exercise — HP and immediate duel termination

Turn the attack messages into damage and finish the baseline encounter when either actor is defeated.

- Initialize both actors to 3 HP. Each attack subtracts 1 HP from its target, leaving positions unchanged.
- Extend the explicit state model to represent a finished duel and its winner.
- Check for defeat immediately after damage. When a target reaches 0 HP, finish the duel before another action or normal turn handoff occurs. A defeated enemy receives no response, and player defeat must not return control to `PlayerTurn`.
- Ensure subsequent directional input leaves positions, HP, and the terminal result unchanged. Print the outcome once when the duel ends.
- Print both actors' HP after each individual action so the run can be compared with the completed paper trace.
- Coaching hint: `_UnhandledInput` currently calls `EnemyAction()` unconditionally after `PlayerAction()`. Reconsider that call when the player action can end the duel; also review the normal handoffs at the ends of both action methods.

**Purpose:** make defeat a terminal state, enforce action-boundary checks, and reproduce the paper trace in running gameplay.

**Completion check — met:** the learner reports the exercise done, covering the normal four-command win scenario, temporary 1-HP-player loss scenario, and post-result input checks. No individual runtime logs were supplied. Source inspection supports final HP 1/0 after the normal seven-action trace and final HP 0/2 after two commands in the loss setup, with terminal state retained in either case. `Actor._Ready()` sets HP to 3, confirming the normal starting configuration is restored. The coach did not independently build or run these scenarios.

### Implementation review (2026-11-03)

- Added `Actor.cs`, attached to the shared `Actor.tscn`; both instances now have independent `HealthPoints`, initialized to 3, and a `DealDamage()` method that subtracts one. `Main` obtains typed `Actor` references.
- Renamed the state model from `GameTurn` to `GameState` in `Scripts/GameState.cs` and added `FinishedDuel(bool PlayerWin)` alongside the two turn cases.
- Each attack checks the target's HP immediately after damage. At zero, it stores the winner and returns before the normal handoff.
- `_UnhandledInput` still calls `EnemyAction()` after `PlayerAction()`, but `EnemyAction` now immediately returns unless the state is `EnemyTurn`. This correctly prevents a defeated enemy from acting; a call-site conditional is not required for the current behavior.
- The input guard rejects later events in `FinishedDuel`. HP is printed after nonterminal actions; the terminal action's HP and winner are printed together at the end of the resolving input event. That result message occurs once under the current flow.

## Optional later proposal — tactical decision sketch

A telegraphed enemy attack was suggested as a way to explore target information, timing, and circumstances favoring attacking versus moving away. The proposed paper sketch is no longer the active assignment after the learner's clarification. No tactical rules have been agreed or implemented.

## Completed exercise — show HP and result inside the game

Add readable status text to the main scene so the encounter can be followed from the game window.

- Use one or more `Label` nodes below or beside the floor, with enough visible space to avoid clipping or overlap at the tested window size.
- Display clearly identified player and enemy HP, initially 3 each.
- Show an active-play status such as `Your turn`, then `You win` or `You lose` when the duel finishes.
- Derive the display from the actors' HP and the existing `GameState`. Game state remains authoritative; label text/visibility does not determine turn ownership or victory.
- Refresh the display after initialization and action resolution, including fatal actions that return early. A small shared display-update method can keep the formatting consistent.

**Purpose:** connect correct mechanics to visible player feedback and keep presentation derived from gameplay state.

**Completion check — met:** the learner reports completion and supplied starting/winning screenshots. Both show readable labels beside the floor without visible overlap or clipping: initially player/enemy HP 3/3 with `Your turn`, then HP 1/0 with `You win` and the actors adjacent. The loss scenario and post-result input checks are accepted as learner-reported; no loss screenshot or independent interactive run was supplied/performed. `Actor._Ready()` still initializes HP to 3.

### Source and screenshot review (2026-11-03)

- `Main.tscn` adds `PlayerHP`, `EnemyHP`, and `Status` labels at local x=300, beside the 288-pixel-wide floor, with distinct vertical positions.
- `Main._Ready()` obtains the label references and calls `UpdateLabels()` after initializing actor positions and the turn state.
- `_UnhandledInput` calls `UpdateLabels()` after the player/enemy action cycle. Early returns from fatal actions return to this caller, so the terminal HP and result are still rendered.
- `UpdateLabels()` reads the two actors' HP and switches over `GameState` to choose the status text, including both win and loss. Gameplay state remains independent of label contents. Updating once after the synchronous action cycle is sufficient for the current presentation.

## Completed exercise — four-direction room movement and enemy pursuit

Make the existing turn loop work throughout the empty 6×5-cell floor. The following boundary and pursuit policies extend the earlier row-only baseline and are now implemented, with runtime completion reported by the learner.

- Add left, up, and down input actions alongside right, using one cardinal step per discrete key press.
- Treat cells with `0 <= x < 6` and `0 <= y < 5` as the room. A player command outside those bounds changes no position, HP, or turn state and grants no enemy action.
- An in-bounds command moves into an empty cell or attacks an enemy occupying the target cell. Preserve immediate defeat handling and the existing HP/status display.
- At the start of the enemy action, test orthogonal adjacency using Manhattan distance: `abs(dx) + abs(dy) == 1`. If adjacent, attack once.
- Otherwise, move one cardinal cell toward the player: prefer the horizontal axis whenever X differs; when X matches, move vertically toward the player's Y. Choose the step's sign from the relative positions. Moving into adjacency still consumes the entire action.
- The empty rectangular room and this monotonic pursuit rule are sufficient for this exercise; obstacle/pathfinding rules remain undecided.

**Purpose:** generalize a fixed encounter into coordinate-based action decisions and make rejected input distinct from an action that consumes a turn.

**Completion check — met:** the learner reports completion of the original four-right win regression, the `Up, Up, Up, Right, Down` sequence, other directions/edges, and held-key checks. Source inspection supports the specified sequence: after two ups, player `(1, 0)` and enemy `(2, 2)` at 3/3 HP; the third up changes nothing; right leaves player `(2, 0)` and enemy `(2, 1)` at 3/3 HP; down attacks vertically and leaves positions unchanged at 2/2 HP. No new screenshots/logs or independently executed runtime tests were supplied/performed.

### Implementation review (2026-11-03)

- `project.godot` binds all four movement actions to their corresponding physical arrow keys. `_UnhandledInput` selects one cardinal vector using an `if`/`else if` chain and event-based `IsActionPressed`.
- `PlayerAction` rejects out-of-bounds candidates before any state change. The state remains `PlayerTurn`, so the existing `EnemyAction` guard suppresses the subsequent enemy call without movement, damage, or a handoff.
- `EnemyAction` tests Manhattan adjacency before choosing an action. Otherwise, it subtracts a signed horizontal or vertical unit step from the enemy cell, preferring X. This approaches the player from either side and moves along exactly one axis.
- Attack and movement branches remain mutually exclusive; HP, terminal-state guards, and label refreshes retain the previous behavior. `Actor._Ready()` still starts both actors at 3 HP.

## Completed exercise — restart the duel in-game

Make the encounter replayable during play and after a finished result.

- Add a `restart` Input Map action bound to the physical R key and handle it as a discrete press.
- Use the familiar scene-reload approach (`GetTree().ReloadCurrentScene()`) so actors, positions, HP, state, and labels are initialized together. Return from the old input handler after requesting the reload.
- Ensure restart input is reachable even when the state is `FinishedDuel`; the existing player-turn guard currently precedes all input handling and must not block restart.
- Add a visible hint explaining arrow-key movement/bump attacks and R-key restart. Keep it readable alongside the HP and result labels.

**Purpose:** make the duel replayable without editor controls and restore the whole encounter consistently across game states.

**Initial review (2026-11-03):** the learner reported the exercise done. Source inspection confirmed the physical R binding, `IsActionPressed("restart")` before the player-turn guard, scene reload, and immediate return from the old input handler. These support the requested restart behavior during play and after a result, with default echo rejection. Runtime reset/replay/held-key checks were accepted as learner-reported. The first hint submission contained only `Press "R" to restart`, so movement/attack wording was requested.

**Screenshot review (2026-11-03):** the learner supplied a revised initial-screen image showing 3/3 HP, `Your turn`, two lines explaining arrow-key movement/adjacent attacks, and a separate R-key restart line. All text is readable without visible clipping or overlap. `Main.tscn` confirms the revised text, the status moved to y=80, and the hint moved to y=130. The attack wording can be made more precise by saying to press toward the adjacent enemy; carry that small wording refinement into the export task. This is a coach screenshot review, not a fresh-eyes playtest.

**Completion check — met:** restart behavior is complete by learner report and source review; the revised controls layout is confirmed by screenshot and scene inspection. Final-state readability was previously learner-reported; no new final-state screenshot or independent interactive run was performed.

## Completed exercise — standalone Windows export and smoke test

Export the mechanics milestone as a standalone Windows release build and test it outside the editor, using the workflow already practiced in Crate-pushing.

- Before exporting, clarify that attacking requires pressing toward an adjacent enemy, and keep the hint readable.
- Add a Windows Desktop export preset. Use Export Project with Export With Debug disabled and a learner-chosen output folder outside the source project. No `export_presets.cfg` was present at assignment time.
- Keep the full exported folder, including the generated .NET/data files, together.
- Close Godot and launch the exported `.exe` directly. Check the board, HP, status, and controls text at the actual game-window size.
- From a fresh start, replay `Up, Up, Up, Right, Down`: the third up is blocked without an enemy action, and the sequence ends at player `(2, 0)`, enemy `(2, 1)`, with 2/2 HP. Press the physical R key and confirm initial positions, 3/3 HP, and `Your turn` are restored.
- Press right four times to win at 1/0 HP. Check that later arrows change nothing, then restart and confirm the duel can be played again.

**Purpose:** verify the actual deliverable, including packaging, readable presentation, and gameplay outside the editor.

**Completion check — met:** on 2026-11-04, the learner reported the exercise done and supplied `/mnt/d/Janno/Exports/Turn Based Duel/`. This is accepted as learner-reported completion of the assigned standalone smoke checks. Artifact and configuration inspection is recorded below; the coach did not build or launch the game or independently verify release/debug mode.

### Export and source inspection (2026-11-04)

- `export_presets.cfg` defines a Windows Desktop x86_64 preset with export path `../../Exports/Turn Based Duel/Turn Based Duel.exe` and an external PCK/data layout.
- The export folder contains `Turn Based Duel.exe`, `Turn Based Duel.pck`, and `data_Turn Based Duel_windows_x86_64/`. The data folder includes the game assembly and runtime configuration, `GodotSharp.dll`, `coreclr.dll`, and `hostfxr.dll`. These checks establish artifact presence, not independent runtime correctness or exhaustive package verification.
- `Main.tscn` now says: `Use arrow keys to move the player. / Move toward enemy to attack it / when adjacent to it. / Press "R" to restart`, resolving the pre-export wording follow-up. Revised-text readability in the exported build is learner-reported; no new screenshot was supplied.
- At the export review, the source repository's `git status --short --untracked-files=all` listed only the new `04-turn-based-duel/` project files as untracked. Its `.gitignore` excludes `.godot/`; the supplied export folder is outside the source repository. The subsequent source checkpoint is recorded below.

## Completed exercise — save a version-one source checkpoint

Review and commit the completed duel project in the existing `godot-projects` repository.

- At assignment, the project folder `04-turn-based-duel/` was untracked. The task was to review and stage its C# source, scenes, project/solution settings, UID/import sidecars, configuration/ignore files, and `export_presets.cfg`.
- Inspect the staged changes so the checkpoint contains the project's source/configuration rather than generated caches, build outputs, or exported binaries. Preserve unrelated work if any appears after the inspection above.
- Create the commit with a learner-chosen message. The learner performs staging and committing; the coach has not changed game source or the source repository's index/history.

**Purpose:** preserve a recoverable source-history checkpoint for the exported, tested version before project reflection.

**Completion check — met:** the learner supplied `255f947`. Read-only inspection resolves it to local commit [`255f947349e55a79727827d3309451896155ec7f`](https://github.com/janno-p/godot-projects/commit/255f947349e55a79727827d3309451896155ec7f), message `Add turn-based duel project`. Its 17 files contain the intended project source, scenes, settings, sidecars, ignore/configuration files, and export preset, with no generated caches or exported binaries. The source repository's `git status --short --untracked-files=all` is empty. Remote availability/public release was not verified. The learner performed the commit; the coach did not stage or commit source.

## Completed exercise — project reflection

Reply briefly to the following prompts; a sentence or two per item is enough:

1. What was the most useful new thing learned in this project?
2. What still feels unclear or needs more practice? `Nothing right now` is a valid answer.
3. Did the exercise steps feel too small, about right, or too large, and what would the learner like more of next?

**Purpose:** consolidate learning and use the learner's experience and pacing preferences to choose a useful next challenge.

**Completion check — met:** on 2026-11-04, the learner answered all prompts:

- Main learning takeaway: simple game-state management.
- Remaining uncertainties: nothing right now.
- Exercise sizing: about right.
- Desired coaching change: focus on the specific exercise topic and avoid repeating already-practiced tasks, especially polishing on-screen instructions across projects. Recorded this preference in the root learner notes as well.

## Completed checkpoint — choose the next focused topic

The learner accepted **state snapshots and undo** in the existing duel on 2026-11-04. Build directly on game-state management: capture an independent snapshot and restore a whole player/enemy action cycle, including HP, positions, and the turn/result state. Reuse the current project and presentation, with checks concentrated on the new behavior.

The source was re-read before assignment: `Main.cs` coordinated cells, actions, state, and labels; each `Actor` owned mutable HP; `GameState` contained the two turn cases and a finished result. No snapshot or undo implementation was present at that checkpoint.

**Purpose:** choose a focused new concept using the learner's reflection and reduce repeated supporting setup/polish work.

**Completion check — met:** the learner accepted the proposed topic. The first assignment introduced one-level undo; the submitted implementation and follow-up are recorded below.

## Reviewed exercise — snapshot-based undo

The original assignment was a single saved snapshot and a discrete `undo` action bound to U. One undo restores the state immediately before the last accepted player command, reversing that player's action and the resulting enemy response together. The learner submitted a multi-level stack instead; retain that useful extension for the follow-up rather than requiring a downgrade. The original single-level requirements/checks below record the assignment, not a claim that every check passed.

- Define an immutable snapshot containing both logical cells, both actors' HP, and `GameState`. Store independent data values rather than references to mutable `Actor` nodes; derive pixel positions and label text from restored gameplay data.
- Capture/replace the saved snapshot only after a player command is known to be valid, and before changing cells, HP, or turn state. Rejected boundary commands and unrelated input preserve the previous snapshot.
- On U, restore the saved data, rebuild actor positions through `CellToScreen`, and refresh the existing labels. Restoration itself grants neither actor an action.
- Consume the saved snapshot after restoring it. U with no saved snapshot does nothing; another accepted player command makes a new snapshot available. This first exercise has one level of undo.
- Undo must work after `FinishedDuel`, so consider input handling relative to the player-turn guard. A scene restart starts with no saved snapshot.

**Purpose:** capture a self-contained historical state and restore it consistently at a meaningful action-cycle boundary.

**Original one-level completion check:** use fresh runs for each scenario:

1. `Right, Right, U` restores player `(2, 2)`, enemy `(3, 2)`, HP 3/3, and `PlayerTurn`; a second U changes nothing. Then `Up, U` returns both actors to those same saved cells, confirming logical and rendered positions restore together.
2. Four right presses win; U restores the pre-killing-blow state at those adjacent cells with HP 1/1 and `PlayerTurn`. Right wins again.
3. `Up, Up, Up, U` restores the state after the first up: player `(1, 1)`, enemy `(3, 2)`, HP 3/3. The blocked third up must not have overwritten the snapshot. Then press Up to capture another noninitial snapshot, restart with R, and press U; the initial state remains unchanged.

### Submission review (2026-11-04)

- The learner reported `Done`. Inspected `Main.cs`, `Scripts/HistoryEntry.cs`, `Scripts/GameState.cs`, `project.godot`, and `Main.tscn`.
- `HistoryEntry` is a positional record containing the two value-type cells, two HP values, and the existing immutable phase/result representation. It does not retain actor references.
- `AddHistoryEntry()` pushes a snapshot after bounds validation and before any action mutation. `LoadLatestHistoryEntry()` pops and restores all five state values; `UpdateScreen()` derives both pixel positions and labels afterward. Undo returns before normal action resolution and is checked before the player-turn guard. `_Ready()` clears history, and the U action is bound to the physical U key with default echo rejection.
- The `Stack<HistoryEntry>` retains all older entries. Consequently, `Right, Right, U, U` restores the initial state rather than making the second U a no-op. The original single-level check is not met; the coach accepts the implemented multi-level direction for the next focused check. No claim of independently verified runtime completion is made.
- The learner also added `UndoLabel`. Its visibility is maintained in history push/pop methods and is used as the input condition for undo. The current code keeps these in sync in normal flow, but history availability should be authoritative and the label should be derived from it. This is a game-state responsibility exercise, not a request for text/layout changes.

## Completed exercise — make history authoritative for undo

Keep the implemented stack and make undo availability depend on history data.

- Use history availability to decide whether undo can restore anything. An empty history is a safe no-op, independently of label visibility.
- Derive `UndoLabel.Visible` from history availability in `UpdateScreen()` instead of maintaining it as a separate flag in push/pop operations.
- Preserve the existing immutable snapshots, action-cycle capture boundary, and terminal-state/restart behavior.

**Purpose:** establish one authoritative source for undo state and treat presentation as a view of that state, while validating multi-level restoration.

**Completion check — core behavior met:** the learner reports both focused checks done: four undos rewind the win through 1/1, 2/2, adjacent 3/3, and initial 3/3; a fifth U does nothing; hiding the hint does not prevent undo. Source inspection confirms `_UnhandledInput` always dispatches the undo action before the turn guard, `LoadLatestHistoryEntry()` uses `TryPop` and safely returns on empty history, and `UpdateScreen()` derives hint visibility from `_history.Count > 0`. No independent interactive run was performed.

**Cleanup resolved during redo:** the redundant `UndoLabel.Visible = true` assignment in `AddHistoryEntry()` was removed. Visibility now derives from history count in `UpdateScreen()`.

## Completed exercise — redo and branching history

Extend snapshot navigation with a discrete `redo` action bound to Y. Redo restores the outcome saved when undoing; a new accepted gameplay action after undo discards the old future.

- Preserve the current complete snapshot in redo history before undo restores an older snapshot.
- Redo restores the next saved future snapshot while preserving the state being left in undo history. Restore recorded state directly, including a finished result, without resolving actor actions again.
- Empty undo or redo history is a safe no-op. Both controls remain independent of the current turn/result and presentation visibility.
- A newly accepted player action clears redo history. Invalid boundary commands and unrelated input leave it intact; undo/redo navigation itself is not a new gameplay action.
- Restart clears both histories. Reuse the existing screen refresh to derive presentation from the restored state.
- Remove the redundant visibility assignment in `AddHistoryEntry()` as noted above.

**Purpose:** distinguish navigation through recorded states from taking a new action, and handle the point where a new choice replaces an abandoned future.

**Completion checks:** use fresh runs as appropriate:

1. Win with four rights, then U and Y. Redo returns to the same 1/0-HP win state; subsequent movement remains blocked. Undo/redo can repeat this round trip.
2. `Right, Right, U, Up, Y`: Y does nothing because Up created a new branch. U then Y should undo/redo that Up cycle, ending at player `(2, 1)`, enemy `(2, 2)`, HP 3/3.
3. `Up, Up, Right, U, Up, Y`: the final Up is blocked and must preserve redo; Y restores player `(2, 0)`, enemy `(2, 1)`, HP 3/3.
4. After `Right, Right, Right, U`, restart with R, then try U and Y. Each is a no-op at the initial state, checking that restart clears both histories while their top entries represent noninitial states.

**Result — met (2026-11-04):** the learner reports the checks done. Source inspection supports terminal-result round trips, new-branch invalidation, preservation of redo across invalid input, and reset of both histories. Runtime outcomes are learner-reported; the coach did not build or execute the game.

### Implementation review (2026-11-04)

- `Main` now has `_history` and `_revertedHistory`. Each navigation method first successfully `TryPop`s its destination, then saves the complete current snapshot to the opposite history before invoking shared `RestoreStateFromEntry()`. Empty navigation changes neither history nor gameplay state.
- `PlayerAction()` clears `_revertedHistory` only after bounds validation and capture of the pre-action state. Blocked commands and unrelated input therefore retain redo, while a new move or attack invalidates the abandoned future.
- Restore assigns both cells, both HP values, and `GameState` directly. Undo/redo handlers refresh the screen and return before player/enemy action resolution, so redo restores a saved finished outcome without running combat again.
- Both controls precede the player-turn guard, and `_Ready()` clears both histories. `UpdateScreen()` derives both hint visibilities from the corresponding stack counts; the old push-time visibility assignment is removed.
- `project.godot` binds redo to the physical Y key, and `Main.tscn` contains the referenced `RedoLabel`. No wording/layout follow-up is assigned.

## Completed checkpoint — choose the next focused topic

The learner accepted **Godot tweens and asynchronous turn sequencing** in the existing duel on 2026-11-04. The new concept is actions that take time: animate movement, wait for its completion before handing over the turn, and prevent overlapping actions while resolution is in progress.

**Purpose:** build on explicit turn state with a focused engine concept that introduces real completion boundaries across frames.

**Completion check — met:** the learner accepted this direction. The first exercise and its policies follow.

## Reviewed exercise — await animated movement through a whole turn cycle

Make each player/enemy move take approximately **0.2 seconds**, using a fresh Godot tween to animate the actor's `Position` to `CellToScreen(destination)`.

- Logical cells remain authoritative. Commit the destination when the move starts; the visual position catches up over the tween. Hand over the turn only after that actor's movement finishes: player movement, then one enemy action, then player input becomes available again if the duel continues.
- Attacks resolve immediately with the existing damage and terminal-result rules. Await actual tween completion for movement rather than a separate timer of matching duration.
- From acceptance of a player command until the full player/enemy cycle finishes, ignore arrows, R, U, and Y; discard these presses rather than queueing them. A resolving/busy guard must be active before the first await and cover the handlers that currently precede the player-turn check. Release it after resolution, including a terminal result. Bounds-rejected input must leave the game ready for the next command and preserve history.
- When idle, restart remains a scene reload, and undo/redo immediately restore snapshots and snap actors to the restored cells. Keep one history entry per accepted player command and restore only settled turn-cycle states; transient resolution bookkeeping is not snapshot data.
- Label refreshes must not overwrite positions while tweens are moving them. Initialization, restart, and snapshot restoration still need immediate placement at logical cells.

### Focused hints

- In `Main.cs`, `UpdateScreen()` currently assigns both actor positions as well as labels. Consider when each responsibility should run now that movement spans frames.
- [Godot Tween documentation](https://docs.godotengine.org/en/stable/classes/class_tween.html): use `CreateTween()`, `TweenProperty()` with `Node2D.PropertyName.Position`, and `await ToSignal(tween, Tween.SignalName.Finished)`. Create a fresh tween for each movement.
- Make awaitable movement/action helpers return `Task` and await them through the call chain. `_UnhandledInput` has a `void` override signature; an `async void` engine callback can be the boundary, while helper methods return tasks. An await yields to Godot's event loop, so the input handler can be invoked again before the earlier invocation has finished.

**Purpose:** coordinate logical state, visual completion, and input eligibility across frames while preserving whole-cycle history boundaries.

**Completion checks:** temporarily use approximately 0.8 seconds per move when checking timing, then restore 0.2 seconds:

1. From a fresh run, Right visibly moves the player first, then the enemy. It ends at player `(2, 2)`, enemy `(3, 2)`, HP 3/3. The enemy does not start early, and control returns only after its movement finishes.
2. Repeat while pressing arrows and R/U/Y during the player's animation, then during the enemy's animation. These inputs cause no action, reset, or history navigation, including after the cycle settles.
3. After a completed first Right, U immediately restores the initial positions; Y immediately restores the post-cycle positions. Neither navigation runs an enemy action or leaves a tween dragging an actor afterward.
4. From a fresh run, `Up, Up, Up, Right`, waiting after accepted commands: the third Up is blocked, and Right still works. End at player `(2, 0)`, enemy `(2, 1)`, HP 3/3.
5. From a fresh run, four Rights (waiting for each cycle) still win at HP 1/0 with no defeated-enemy response. U/Y can leave and restore the result; R works once resolution is finished.

### Async coaching clarification (2026-11-04)

The learner asked where signal awaiting fits, questioned `async void`, and checked their understanding of overlapping handler invocations. Explained that `ToSignal` makes tween completion awaitable, helpers return `Task`, and callers must await them through the chain. The required `void` Godot callback is the narrow async boundary: awaited helper failures can be caught there, while `finally` performs cleanup rather than exception handling. Godot can invoke the callback again during suspension; this tween flow normally involves main-thread reentrancy, not simultaneous execution on worker threads. A guard must exclude all relevant input throughout the cycle.

### Submission review (2026-11-04)

- The learner reported `Done`. Inspected `Main.cs`, `Actor.cs`, and input settings. No per-scenario runtime evidence was supplied, and the coach did not build or run the game. The submission does not yet meet the action-completion timing policy.
- `_UnhandledInput` uses a one-permit `SemaphoreSlim` and `WaitAsync(0)` before any input handling. The zero timeout rejects busy invocations without queueing; successful acquisition is paired with release in `finally`, including early returns. This is a valid implementation of the input gate; a boolean replacement is not required.
- `UpdateLabels()` is separate from `UpdateActors()`. Startup and idle undo/redo refresh both; normal action resolution refreshes labels without snapping the actors. History capture, branching, and restoration remain at the existing boundaries.
- `PlayerAction()` and `EnemyAction()` are still synchronous and both run before `await AnimateActors()`. Thus both logical actions and normal turn handoffs finish before either visual move starts. `AnimateActors()` appends player and enemy position tweeners to one default-sequential tween, so visual order alone can appear correct despite the premature gameplay handoffs.
- Both tweeners always use 0.2 seconds, even when their actors have not moved. A blocked directional command or an attack-only cycle therefore still incurs approximately 0.4 seconds of input exclusion. Invalid commands preserve gameplay/history but do not immediately leave input ready, as required.
- The callback has `try/finally` but no `catch`. The permit is released on an awaited failure, but the exception still escapes the `async void` boundary; this does not implement the exception-reporting guidance discussed above.

## Completed exercise — align action completion with animated movement

Revise the existing flow so each moving actor completes its animation before handing over the turn.

- Make the player/enemy action helpers awaitable and await them in order from the input callback.
- In each movement branch, commit that actor's logical destination, create and await its movement tween, then perform the normal turn handoff. A small single-actor movement helper can share the tween code. Replace the unconditional end-of-cycle two-actor animation with these movement-only waits.
- Bounds-rejected input and immediate attacks create no movement tween. Preserve terminal-result guards and whole-cycle history boundaries.
- Retain the working zero-timeout semaphore gate, its `finally` release, and immediate idle snapshot restoration. Catch/report awaited failures at the `async void` boundary; cleanup alone does not observe them.

**Purpose:** make an actor action's completion include its visual movement, keeping logical handoff and asynchronous completion aligned and avoiding artificial delays when nothing moves.

**Completion checks:**

1. Temporarily use 0.8-second moves. From a fresh run, Right must log the player-to-enemy handoff and `Enemy moves` only after the player arrives; the return to `PlayerTurn` must occur only after the enemy arrives. Inputs during both movements remain discarded. End at player `(2, 2)`, enemy `(3, 2)`, HP 3/3.
2. From a fresh run, use `Up, Up`, waiting for each cycle, then press blocked Up followed promptly by Right. Right must be accepted without a stationary-animation delay; end at player `(2, 0)`, enemy `(2, 1)`, HP 3/3. Attack-only cycles likewise introduce no stationary-animation wait.
3. Confirm idle undo/redo still snap immediately and the four-right win still allows undo/redo/restart after resolution. Restore 0.2-second movement duration.

**Completion check — met (2026-11-04):** the learner reported the follow-up done. Source inspection supports the corrected action/arrival order, rejection without stationary-animation delay, and preserved input/history/terminal behavior. Runtime completion is learner-reported; no per-scenario logs or independent build/run were supplied/performed.

### Follow-up implementation review (2026-11-04)

- `_UnhandledInput` now awaits `PlayerAction()` and then `EnemyAction()`. Both return `Task`; movement branches commit the actor's cell and await the shared `AnimateActor()` before calling `ChangeTurn()`.
- `AnimateActor()` creates a fresh single-actor tween and awaits its `Finished` signal. `AnimationSpeed` is restored to 0.2 seconds. The unconditional two-actor post-cycle animation is removed.
- Player bounds validation returns before history capture or any tween. The unchanged enemy-turn guard prevents a response to rejected input or a fatal player attack. Attacks complete synchronously with no animation wait.
- Both actions choose a normal next turn or `FinishedDuel` and apply it through `ChangeTurn()`, then print state and refresh labels. Fatal results therefore remain terminal and visible; no defeated-enemy response or return to player control occurs after defeat.
- The zero-timeout semaphore still covers restart/history handling and the full awaited cycle. A `catch (Exception)` reports action failures through `GD.PushError`; `finally` releases the permit. This confirms reporting/cleanup structure, not tested recovery from injected failures.
- Startup and idle undo/redo still snap actor positions through `UpdateActors()`. Snapshots contain logical cells, HP, and immutable turn/result data; resolution bookkeeping remains outside history. Reviewed `Main.cs`, `GameState.cs`, `HistoryEntry.cs`, and `Actor.tscn` without editing game source.

## Completed checkpoint — choose parallel attack feedback

The learner accepted parallel attack feedback on 2026-11-04. Introduce parallel versus sequential tween composition through a short attacker lunge and target hit flash that overlap, with the action awaiting completion of the full effect. This builds on sequential movement while adding readable combat feedback and temporary visual transforms separate from logical grid positions.

**Purpose:** choose a focused extension that teaches simultaneous animation tracks and separation of temporary presentation from grid placement.

**Completion check — met:** the learner accepted the proposal. Inspection confirms the existing actor scene has a `Polygon2D` child, and the two actor roots carry their cyan/orange tints. The assignment below reuses these nodes; no game source was edited by the coach.

## Current exercise — compose parallel attack feedback

Give each player/enemy attack a two-phase visual effect, using one fresh composed tween per attack:

| Phase | Attacker's `Polygon2D` | Target's `Polygon2D` |
| --- | --- | --- |
| First 0.1 seconds | Move its local position 12 pixels toward the target from its resting position | Tween `SelfModulate` toward a dark gray, for example `(0.3, 0.3, 0.3, 1)` |
| Next 0.1 seconds | Return to its original local position | Return to its original `SelfModulate` |

- Run both properties in each phase in parallel, and the two phases sequentially. Await the entire tween's `Finished` signal before the attack action completes. Use the same behavior for both actors.
- Animate the visible child for the lunge. The actor root remains at its cell position, and attacks change neither logical cell. Use the logical attacker-to-target direction so the effect works horizontally and vertically.
- Use the target child's `SelfModulate` for the flash. Its modulation multiplies the existing parent tint; a darkening pulse remains visible for both cyan and orange actors. Restore the original values so offsets/tints do not accumulate across attacks or enter history snapshots.
- Apply the one damage point at attack start and refresh the HP display. If fatal, record and display `FinishedDuel` immediately, then finish the effect under the existing input gate without a normal handoff or enemy response. If nonfatal, retain the acting turn until the full effect finishes, then hand over the turn. This extends the previous instantaneous-attack policy with awaited feedback while preserving immediate damage/defeat handling.
- Keep the semaphore held across the full player/enemy cycle, now including attack effects. Arrows and R/U/Y during effects remain discarded. Idle undo/redo restore snapshots without replaying effects, when both visuals are already at rest.

### Focused hints

- `Tween.Parallel()` adds the next tweener alongside the preceding one. Compose two parallel pairs in sequence; awaiting separate effects one after another would make them sequential.
- Useful property names are `Node2D.PropertyName.Position` and `CanvasItem.PropertyName.SelfModulate`. The existing `Polygon2D` can serve as the visual node.
- Capture resting visual values before constructing the tween. A shared `Task`-returning attack-feedback helper can keep the composition consistent between the two actors; damage and turn policy remain in the coordinating action logic.

**Purpose:** combine simultaneous visual tracks with sequential phases, await the whole composed effect, and keep temporary presentation independent of authoritative grid state.

**Completion checks:** temporarily use 0.4 seconds per phase for observation, then restore 0.1 seconds:

1. From a fresh run, Right moves into adjacency. A second Right makes the player lunge while the enemy darkens; both return to rest before the enemy's corresponding attack effect begins. Both actors stay in their logical cells, and the cycle ends at HP 2/2. Also check a vertical attack, for example Down after `Up, Up, Right` from a fresh run.
2. Press arrows and R/U/Y during each actor's attack effect, including the return phase. Inputs are discarded; repeated attacks leave no visual offset or tint drift. Four accepted Rights from a fresh run still win at HP 1/0, with the final player attack effect completing and no defeated-enemy response.
3. After the win settles, U restores the adjacent 1/1-HP state and Y restores the 1/0 win. Both visuals stay at rest with original tints, and neither navigation replays an attack effect.

Report the results for source review. Implementation and runtime completion are pending.

## History

Read [HISTORY.md](HISTORY.md) when tracing the project choice or earlier exercises. Shared learner/tooling context remains in the [root learning notes](../../LEARNING.md).
