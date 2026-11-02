# 03 — Crate-pushing

## Status

Version one and reflection are complete. The learner exported the standalone Windows build to `/mnt/d/Janno/Exports/Crate Pushing/` and reports the requested smoke-test results match requirements. The assistant inspected the Windows x86_64 preset and export layout, but did not launch the build or independently verify release/debug mode or rendered behavior. Local source commit `4997ff02afec126ff15ebf98f8242dace37fd332` is verified, and the source repository was clean at that inspection. No fresh-eyes playtest, clean-machine test, remote push, or public release is claimed. The learner chose [04 — Turn-based duel](../04-turn-based-duel/LEARNING.md) as the next project; no further crate-pushing feature is assigned.

The source project is `/mnt/d/Janno/Godot/03-crate-pushing/` in the local `godot-projects` checkout, explicitly supplied by the learner and inspected. The folder name is `03-crate-pushing`; the Godot application title is `Crate Pushing`. Files inspected include `Player.cs`, `project.godot`, `main.tscn`, and the project/shared C# configuration. The learner committed the project; no game-source edits, staging, or commits were performed by the assistant.

## Agreed direction

- A small Sokoban-style puzzle: move a character on a grid and push crates onto goal tiles.
- One handmade level with placeholder visuals.
- Learn keyboard input, positioned 2D world nodes rather than UI-only buttons, blocked movement/pushing rules, and small level design.
- Emphasize gameplay programming, building on the learner's enjoyment of coding while retaining design, feedback, and testing work.

## Working rules

The submitted level and solution are consistent with these proposed baseline rules; they were used for the independent solution check.

- Each action moves one tile up, down, left, or right.
- Walls block movement.
- A crate can be pushed only if the next tile beyond it is empty floor or an unoccupied goal.
- No pulling.
- Win when the crate reaches the goal.

## Checked level

```text
######
#    #
#@ $ #
## # #
#.   #
######
```

| Symbol | Meaning |
| --- | --- |
| `#` | Wall |
| Space | Floor |
| `@` | Player |
| `$` | Crate |
| `.` | Goal |

Winning sequence: `URRRDLULDDURRDDLL` (`U`, `D`, `L`, `R` mean up, down, left, right).

Checked by a standalone grid simulation on 2026-11-02: 17 legal moves, four pushes (left, down, down, left), crate ending on the goal. The level-sketch exercise is complete; shortest-path or difficulty analysis was not required or performed.

## Completed static-board exercise

Create and name a new Godot C# project in the separate `godot-projects` repository, then display the checked level as a static 2D board.

- Use a `Node2D` main scene rather than a UI grid.
- Use evenly spaced tiles and simple placeholder shapes/colors to distinguish walls, floor, player, crate, and goal. No art assets are needed.
- Match the submitted map; movement and pushing were outside this exercise's scope.
- The learner chooses the source-folder name and implementation details. No game source files have been edited by the assistant.

**Purpose:** move from a UI-based game to positioned 2D world elements and establish the coordinate layout before adding gameplay rules.

**Completion check:** running the project shows the same 6×6 layout with the player, crate, and goal in their starting cells. Complete by the supplied project name, editor screenshots, and the learner's report that it runs correctly without errors; not independently runtime-verified.

### Tile-placement guidance and progress

The learner requested more concrete instructions for tile creation and spacing. The static-board exercise was broken into smaller tile-placement steps.

- In the `Node2D` main scene, add a `Polygon2D` child with four local polygon points: `(0, 0)`, `(48, 0)`, `(48, 48)`, `(0, 48)`. Choose a visible wall color; keep scale at `(1, 1)`.
- Place the first tile at `(0, 0)`, duplicate it, and place the second at `(48, 0)`. Use Inspector → Transform → Position for exact placement, rather than dragging by eye.
- Extend this to six sibling tiles. Zero-based grid placement is `x = column × 48`, `y = row × 48`; Godot's positive Y points down. Polygon points describe the shape locally, while the node's position places it within its parent.
- No reusable tile scene, `TileMapLayer`, textures, scripts, or movement was required for the static-board step. This was a small hands-on introduction to positioned world nodes, not an agreed long-term rendering architecture.

The learner supplied editor screenshots showing the first wall row, then the first two rows, then the complete terrain. The latest screenshot visually matches all six wall/floor rows, including both interior obstructions, with aligned edges. The terrain step is complete by visual inspection. Same-colored adjacent squares naturally appear as one strip; individual node count and exact coordinates cannot be verified from these screenshots alone. No runtime screenshot was supplied.

### Marker-placement guidance and progress

- Create separate `Polygon2D` nodes for `Goal`, `Player`, and `Crate`, retaining the floor beneath them and using distinct colors.
- Suggested marker dimensions: player/crate 32×32, inset 8 pixels from each cell edge; goal 40×40, inset 4 pixels, so part of it remains visible when the crate later occupies it. These are placeholder visual suggestions, not fixed game requirements.
- The learner asked how to set “inset.” Clarified that it is a descriptive margin, not an Inspector property: edit the marker's local `Polygon` points to `(8, 8)`, `(40, 8)`, `(40, 40)`, `(8, 40)` for the player/crate, while leaving the node's grid-based `Position` unchanged. For the goal, use 4 and 44 instead. First check that the player has an 8-pixel floor border on all sides.
- Position markers using zero-based `x = column × 48`, `y = row × 48`, under the same parent as the terrain. Worked example: the player is in column 1, row 2, giving `(48, 96)`; let the learner calculate the other two.
- With equal Z indices, place terrain siblings first in scene-tree order, then `Goal`, then `Player`/`Crate`, so the objects render over terrain and the goal remains behind actors.
- Input, movement, pushing, and win detection were outside the marker-placement step's scope.

The latest editor screenshot shows a blue player at column 1, row 2; an orange crate at column 3, row 2; and a larger green goal at column 1, row 4, with floor visible around all three. Subsequent scene-file inspection confirmed marker positions, polygon insets, sibling ordering, and player-script attachment. A read-only checker also confirmed every terrain cell matches the original map.

### Runtime check

The learner reports the scene looks correct and has no errors after being asked to run it with F6. This is learner-reported runtime completion, not an independently observed run.

## Completed discrete-movement exercise

Make the player move exactly one tile per arrow-key press, without automatic repeat while a key is held.

- In Project → Project Settings → Input Map, add `move_up`, `move_down`, `move_left`, and `move_right`, bound to the corresponding arrow keys.
- Attach a C# script to the `Player` node.
- Keep the player's logical cell as a `Vector2I`, initially `(1, 2)`.
- Handle actions in `_UnhandledInput(InputEvent @event)` using `@event.IsActionPressed(...)`. This event-based approach, with the default `allowEcho: false`, avoids held-key stepping; do not poll movement every frame in `_Process`.
- For a matching action, change the logical cell by one cardinal direction and derive the node's pixel `Position` from the cell multiplied by 48. Keep the inset polygon points unchanged; do not add the inset to the node position again.
- Wall, boundary, and crate blocking are deliberately deferred for this isolated input/movement exercise. Passing through them temporarily is not a change to the game's rules. Do not move the crate or add win detection yet.

**Purpose:** learn Godot input actions and discrete input handling while keeping grid state separate from pixel presentation.

**Completion check:** from the starting cell, pressing up, right, down, left once each moves exactly one tile each time and returns to the starting cell. Holding a key produces only the initial step; releasing it produces none. Crate and goal remain stationary. The learner reports the exercise done. `Player.cs` source inspection supports the intended behavior, and `project.godot`/`main.tscn` inspection confirms all four arrow bindings and script attachment; no assistant-run runtime verification was performed.

## Completed wall-blocking exercise

Prevent the player from entering wall cells. Keep crate pushing and win detection out of scope for this step.

- Add all 22 wall tiles, and only wall tiles, to a scene group named `walls`.
- The learner asked how to create the named group. Clarified the editor workflow: select a wall node, open the Groups dock, click `+`, enter `walls`, leave Global unchecked, and confirm. The selected node is added automatically; enable the `walls` checkbox for the other wall nodes and save the scene. A group is a tag, not a new parent node. The workflow was checked against the [official Godot 4.7 groups documentation](https://docs.godotengine.org/en/stable/tutorials/scripting/groups.html). Subsequent scene-file inspection and a read-only checker confirmed exactly the 22 wall tiles are members.
- In the player's `_Ready`, collect the grouped nodes with `GetTree().GetNodesInGroup("walls")` and build a `HashSet<Vector2I>` of wall cells from each wall's local position divided by 48. The inspected terrain and player are siblings, so their local positions use the same coordinate space.
- In `_UnhandledInput`, calculate a candidate destination before changing `_gridCell`. If the destination is a wall, leave both logical cell and visual position unchanged. Otherwise accept the move and update the visual position as before.
- Use grid membership, not physics-body collisions, for this exercise. The enclosing wall ring also prevents leaving the board through ordinary movement; do not add unrelated systems.
- Preserve one step per press and no held-key repeat. During this exercise, the crate remained stationary and nonblocking; pushing was deferred.

**Purpose:** connect scene-authored terrain to gameplay data and validate a proposed move before committing state, without duplicating the wall layout in a second hand-maintained map.

**Completion check:** from the initial cell, left and down are blocked while up and right remain possible. In a fresh run, right → down → right stops on the last action at the interior wall. The player never enters outer or interior walls, valid floor moves still work, and blocked moves do not corrupt subsequent movement. The learner reports this exercise done. `Player.cs` inspection confirms cached wall membership and candidate-before-commit movement; group membership is independently checked at the scene-file level. The assistant has not run the game or independently verified runtime behavior.

## Completed crate-pushing exercise

Implement pushing the single crate while preserving the existing movement and wall rules. Win detection and a win message were outside this exercise's scope.

- Keep a reference to the existing `Crate` node and a logical crate cell derived from its initial local position. The crate and player are siblings; from the player, `GetNode<Node2D>("../Crate")` can obtain the reference. No new crate script or scene reparenting is required.
- If the player's candidate cell is a wall, reject the move as before.
- If it is free of the crate, move only the player.
- If it contains the crate, check the cell one step beyond the crate in the same direction. A wall there rejects the whole move, leaving both objects' logical cells and positions unchanged. Otherwise, push the crate one cell and move the player into the crate's former cell.
- Update both visuals from their logical cells using the same 48-pixel conversion. Do all legality checks before changing either object's state. Keep the inset polygons unchanged and do not add held-key repeat or pulling.
- Only one crate exists, so multi-crate rules and generalized crate infrastructure are outside this step's scope.

**Purpose:** coordinate a two-object move without partial state updates, applying the central Sokoban pushing rule.

### Resolved conversion finding

The first pushing submission called `ScreenToGrid` when assigning pixel positions in both movement helpers. After a diagnostic prompt, the learner acknowledged and fixed the issue. Source inspection confirms `MovePlayerToCell` and `MoveCrateToCell` now call the new `GridToScreen(Vector2I)` helper, which multiplies by 48. This confirms the saved-source change, not runtime correctness. The learner applied the fix; no game source was edited by the assistant.

### Post-fix regression checks

- In a fresh run, press `RRR`. The first press places the player at local pixel position `(96, 96)`; the second pushes the crate to `(192, 96)`; the third moves neither object because the wall blocks the push. Walking away should not pull the crate.
- Restart the scene, replay `URRRDLULDDURRDDLL`, and confirm the crate finishes on the green goal at local pixel position `(48, 192)`. No win message is expected yet.

**Purpose:** retest both conversion call sites and the original pushing scenarios after correcting a shared helper, before adding another feature.

**Completion check:** complete by the learner's explicit report that both scenarios work as required. The source fix is inspected; runtime success is learner-reported, not independently observed by the assistant.

## Completed win-feedback exercise

Detect when the crate reaches the goal and make completion visible to the player.

- Read the existing `Goal` node's position in `_Ready` and convert it to a logical goal cell with the same `ScreenToGrid` helper used for the crate and walls.
- Add a `Label` named `WinMessage` alongside the existing scene nodes. Give it completion text such as `Solved!`, position it below the board (for example `(0, 304)`), and hide it initially.
- After a successful push has updated both objects, compare the crate's grid cell with the goal cell. Do not compare the player's cell or poll every frame.
- On the first match, show the label and record a won state. For this exercise, ignore further directional movement after winning so the solved board remains visible. Restart controls, extra levels, and animation were outside this step's scope.
- Keep existing movement, wall, and pushing behavior unchanged before the win. Use F6 to start fresh runs for testing.

**Purpose:** connect a gameplay rule to clear player-facing feedback and an explicit completed state.

**Completion check:** in a fresh run, `RDDL` places the player on the goal without the crate and must not show a win. Restart and replay `URRRDLULDDURRDDLL`; the message should appear on the final push, not earlier. Subsequent arrow presses should move neither player nor crate. Complete by the learner's report that all proposed test outcomes match, plus source inspection of the push-only comparison, hidden label, and input guard. Runtime was not independently observed.

**Implementation note:** `LevelComplete` is currently derived from `WinMessage.Visible`, rather than stored as an independent boolean. This meets the small exercise's observed behavior; it couples gameplay state to UI visibility and may be revisited if the UI or state model grows. No refactor is assigned for this now.

## Completed restart exercise

Allow the player to restart the level from inside the game, both during play and after winning.

- Add an Input Map action named `restart`, bound to the physical R key (distinct from the `R` direction symbol, which means the right arrow in the recorded solutions).
- Handle the restart action before the existing `LevelComplete` early return. Winning should disable movement, not prevent restarting.
- Use `GetTree().ReloadCurrentScene()` to restore the saved scene rather than building a manual reset system for this step. Return from the old input handler after requesting a reload. Keep action handling event-based so held-key echo does not repeatedly reload.
- Add a small visible `R: restart` hint near the board, without overlapping the completion message.
- No undo system, new level, or broader architecture change is assigned.

**Purpose:** let players recover from irreversible crate mistakes and replay without relying on the editor; make the recovery control discoverable.

**Completion check:** push the crate toward the right boundary with two right-arrow presses, then press the R key: player/crate return to their original cells, the win message is hidden, and normal movement works. Solve the level, press R after winning, and verify the same reset and that the level can be solved again. The restart hint remains readable during play and after winning. Complete by the learner's report that it works as required and inspection of the restart binding, pre-win-guard reload/return, saved initial scene state, and separate hint label. Runtime and rendered readability were not independently observed by the assistant.

## Completed solo first-use clarity pass

Make the initial game screen understandable without the coaching conversation. This is a small solo clarity pass, not another mechanics feature.

- Add only the short on-screen instructions or simple visual cues needed to explain the objective and movement controls; choose the wording yourself.
- The initial screen should let a player identify the player, crate, and goal, understand that the crate must be pushed onto the goal, and discover arrow-key movement and R-key restart. Include the no-pulling rule if needed for clarity.
- Keep the existing placeholder visuals and level layout. Do not add levels, undo, animations, assets, or other mechanics for this step.
- Run at the intended window size and check that the instructions, restart hint, and win message remain readable and do not overlap or get clipped in either initial or solved state.

**Purpose:** shift from correct behavior to a player-facing explanation that does not depend on the coach, while keeping the one-level scope small.

### Screenshot and wording review

The learner supplied both requested views. The initial instructions identify the blue player, orange crates, green targets, arrow keys, and R-key restart. The solved view shows a clear separate completion message, a visible green border around the crate, and the same readable instructions. No text overlap or clipping is visible in these images. This is screenshot inspection, not an independently observed interactive run or evidence of understanding by a new player.

The initial objective said crates must be “moved,” without explaining pushing. The learner revised the instructions to:

> Use arrow keys to move blue player. Player can move the orange crate only by pushing it. Try to move the crate to the green target location.
>
> Press "R" to restart.

“Only by pushing it” conveys the push-only/no-pulling rule, and the singular wording matches this level. Inspection of `main.tscn` confirms this text is applied to the existing wrapped instruction label. The assistant did not edit game source.

**Completion check:** complete by the prior initial/solved screen review and the revised objective supplied by the learner and confirmed in source. The subsequent export-test report covers the revised text's readability; the assistant has not inspected a new rendered view of that revision. The learner's own audit and the coach's screenshot review are not equivalent to a fresh-eyes playtest; no new external-tester availability has been reported.

## Completed standalone export/test exercise

Export a standalone Windows desktop build and test it outside the Godot editor.

- In Project → Export, add a Windows Desktop preset. Install export templates matching the editor version if requested. Preserve the existing C#/.NET setup; no engine/language migration or web export is assigned.
- Use Export Project (not Export PCK/ZIP) to create a release build, with Export With Debug disabled, in a learner-chosen output folder separate from the source project.
- Keep the entire exported folder together, including generated .NET/data files; do not test or distribute only the executable.
- Launch the exported `.exe` directly with the editor closed. Check startup, the revised instruction text, normal movement, blocked pushing, winning, and restart during play and after winning.
- For a quick smoke test, use `RRR` for blocked pushing, restart with the physical R key, then solve using `URRRDLULDDURRDDLL`, confirm win feedback/input lockout, and restart again. Check that instructions and completion feedback remain visible without clipping.
- No public upload, distribution, new mechanics, or external playtest is assigned for this step.

**Purpose:** verify the actual deliverable and catch packaging or exported-build differences that editor testing cannot cover.

**Completion check:** complete by the learner's export-folder path and report that test results match requirements. Inspection confirms a Windows Desktop x86_64 preset and an output folder containing `Crate Pushing.exe`, `Crate Pushing.pck`, and `data_Crate Pushing_windows_x86_64/`. The data folder contains the game assembly, runtime configuration, GodotSharp, and bundled .NET runtime files. These are artifact checks, not an assistant-run build or execution test; actual runtime results remain learner-reported.

## Completed source-checkpoint exercise

Save a reviewed version-one source checkpoint in the local `godot-projects` repository.

- Before this checkpoint, `git status --short` reported `03-crate-pushing/` as untracked. The task was to review and stage this project's source, scene, settings, sidecars, and `export_presets.cfg` in the existing repository.
- Include the project's ignore/configuration files. Its existing `.gitignore` excludes `.godot/`; keep generated caches, build outputs, and exported binaries out of the source commit. The export folder is already outside the repository.
- Review the staged changes and create a commit, choosing the message yourself. Do not discard unrelated changes. No push, tag, or public upload is required.
- The learner performs the staging/commit. The assistant has not staged files, committed, or modified game source.

**Purpose:** preserve a recoverable source-history baseline for the working version before further changes or project closeout; a local commit is not a claim of off-device backup.

**Completion check:** complete. The learner supplied `4997ff02afec126ff15ebf98f8242dace37fd332`, message “Add grid-based crate-pushing puzzle project.” Read-only inspection verifies the commit contains the project source/configuration, scene, sidecars, export preset, and a README update, without generated caches or exported binaries. The source checkout's `git status --short --untracked-files=all` is empty. Remote push/public availability was not checked or requested.

## Completed reflection

Reflect briefly on the completed version before choosing another feature or project. A sentence or two per question is enough:

1. What is the most useful thing you learned in this project that you could not confidently do before?
2. What still feels unclear or needs more practice? “Nothing right now” is a valid answer.
3. Which part did you enjoy most, and what would you like more of next?

**Purpose:** consolidate learning and use the learner's actual experience to choose the next challenge, rather than automatically growing this small game.

**Completion check:** complete. The learner answered all three prompts:

- Godot node grouping was new; mapping between cell and screen coordinates was interesting.
- No current uncertainties were reported.
- The learner enjoys progression toward more complicated concepts. They did not select a specific next feature or genre in this reflection.

## Next-direction decision — complete

Recommended a tiny grid-based turn-based duel as a new learning project: one small room, one player, one enemy, and placeholder visuals. It builds on grid movement while introducing explicit turn ownership/state transitions, reusable actor scenes, and simple enemy decisions. The learner selected this direction; detailed combat/turn rules are being proposed in the new project's notes rather than treated as already agreed.

Adding undo to study state snapshots/action history was offered as an alternative, but was not selected. Version one remains complete, with no undo implementation assigned.

**Purpose:** choose a small next challenge with greater conceptual depth, guided by the learner's feedback rather than automatically expanding the finished game.

**Completion check:** complete. The learner chose the turn-based duel. Active coaching continues in [04 — Turn-based duel](../04-turn-based-duel/LEARNING.md); no new Godot source folder has been created by the assistant or reported by the learner.

## History

Read [HISTORY.md](HISTORY.md) when tracing concept selection or the notes-organization decision.
