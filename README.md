# tiny-chess

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/chuxi041118/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/chuxi041118/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A 4KB Python chess engine for make.fun's
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty ($500).
`main.py` is the whole engine: 4,090 bytes as committed, Python 3.13,
standard library only. It reads, imports or execs no other file.

## How it plays

- 0x88 board, negamax with PVS, null-move pruning and LMR
- Quiescence search (captures, plus full evasions when in check)
- Transposition table (Zobrist keys), killer moves, MVV-LVA ordering
- Material + piece-square-table evaluation (mirrored tables)
- Fixed 80,000-node budget per move: deterministic, no wall clock,
  about 2 seconds a move (max ~3s), well inside the 10s/move cap
- Game-history repetition: an incremental hash table built in main()
  (no full replay), so the engine avoids threefold draws it would
  otherwise shuffle into; search-path repetition also scored 0

## Local practice results (6 games per trial)

| trial | Elo | W-D-L |
| --- | --- | --- |
| trial-0 | 1279 | 3-0-3 |
| trial-1 | 1279 | 3-0-3 |
| trial-2 | 1191 | 2-1-3 |
| **mean** | **~1250** | |

Mean practice Elo ~1250 clears the 1154 finals bar on every trial;
the sunfish baseline sits around 1325-1365. Perft verified on the
5 standard positions; no time losses or crashes in 18 practice games.

## Run

Set `MAKEFUN_ENV_URL` to a running tiny-chess-01 env server and run
`python3 main.py`. It loops over the env's games until the trial ends.
