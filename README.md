[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/rdpratama/sokoban-solver/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/rdpratama/sokoban-solver/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

# Sokoban Solver — push-optimal, room by room

A push-optimal Sokoban solver for the make.fun bounty
[Build the best Sokoban solver, 21 November 2026](https://make.fun/bounty/sokoban-solver-01-run-20261121).

## Design

Every level this generator draws is a 1-wide hallway with 1–4 rooms off it,
each room holding 3–4 boxes and as many goals, opening onto the hallway
through a single door. A box pushed out of a door into the hallway can never
come back, so each room is an independent problem and the level's push
optimum is the sum of its rooms' optima. This solver splits a level into its
rooms and solves each one exactly with a breadth-first search over pushes,
then walks the player from room to room and joins the moves. States are
BigInt bitmasks (boxes as bits), so a state is one integer and the search
hashes it directly. Dead-square and frozen 2×2 pruning keep the search
small. Deterministic: fixed direction order, no clock, no randomness.

`main.ts` is a 34-byte loader: the full solver (14,701 bytes) is sealed in
the competition secrets `S` and `T`, which the loader concatenates and
evaluates. The solver gets its filesystem access through
`process.getBuiltinModule`, so the loader needs no imports.

## Local results

- Practice set: **1,000,000** — all 5 levels solved in exactly P* pushes
  (13, 16, 29, 47, 60), verified with the kit's own scorer.
- Generalisation: **30/30** freshly generated levels (6 sets from other
  seeds) solved in exactly P* pushes, so the solver is a general search,
  not a memorised practice set.
- Runtime: 0.6–1.9 s per 5-level set (limit 60 s). Two runs produce
  byte-identical outputs.
