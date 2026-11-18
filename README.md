# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/660877-star/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/660877-star/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Entry for make.fun bounty **tiny-chess-01**: the strongest chess engine that
fits in 4,096 bytes of Python.

`main.py` (4,002 bytes) is a self-contained lzma-packed build of a
sunfish-class engine written from scratch for this bounty:

- negamax + PVS, transposition table, quiescence search (captures,
  promotions, full evasions when in check)
- MVV-LVA + killer move ordering, null-move pruning, LMR, check extensions
- tapered middlegame/endgame piece-square-table evaluation
- fixed node budget (55k nodes/move, tapering late in the game) — fully
  deterministic: no wall clock, no randomness, replay-safe

MIT licensed (see LICENSE).

## Run it

```sh
MAKEFUN_ENV_URL=<game-environment-url> python3 main.py
```

It reads `MAKEFUN_ENV_URL` and plays over the environment's HTTP API.

## Local practice results

Local practice grading (the bounty's own harness, three six-game trials):

| Trial | Score | Elo |
|---|---|---|
| 0 | 6.0/6 | 3000 (solver cap) |
| 1 | 5.0/6 | 1654 |
| 2 | 4.0/6 | 1451 |
| **Pooled (18 games)** | 15.0/18 | **1654** |

House baseline (sunfish-NNUE-4k) practice mean: 1365 (pooled lead +289).
The 1154 entry floor was cleared on all three trials.

Determinism verified: re-running a trial gave byte-identical game records.

## Development

Built and graded locally in stages — perft-verified move generation and
negamax first, then the transposition table, quiescence, null-move
pruning, LMR and check extensions, trials re-run after each stage.

Node budget tuned last: a 65,000-node variant scored mean 1364 over the
same trials and was reverted in favour of the 55,000-node build.

Improvement round (2026-11-18): the evaluation was rebuilt on the full
PeSTO tapered tables — distinct endgame tables for every piece (the first
build reused the middlegame tables as endgame tables for N/B/R/Q) and
PeSTO piece values — plus a bishop-pair bonus. The tables are stored
left-right mirrored and expanded at startup, which makes the full set
fit. Repetition detection now also uses the game history, not just the
in-search path. Previous build: 1451 / 1545 / 1279, mean 1425.

The shipped `main.py` is the lzma-packed build from the kit's `compress`
command, packed from the readable 9,697-byte source.

---

<sub>Entered on [make.fun](https://make.fun/?ref=z6sz10w2&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
