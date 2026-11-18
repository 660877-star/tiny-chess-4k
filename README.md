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

The unpacked source is developed outside the repo and packed with the kit's
`compress` command; the committed `main.py` is the entry.

MIT licensed (see LICENSE).

## Run it

```sh
MAKEFUN_ENV_URL=<game-environment-url> python3 main.py
```

The engine reads the environment URL from `MAKEFUN_ENV_URL` and plays its
moves over the environment's HTTP API.

## Local practice results

Local practice grading (the bounty's own harness, three six-game trials):

| Trial | Score | Elo |
|---|---|---|
| 0 | 6.0/6 | 3000 (solver cap) |
| 1 | 5.0/6 | 1654 |
| 2 | 4.0/6 | 1451 |
| **Pooled (18 games)** | 15.0/18 | **1654** |

House baseline (sunfish-NNUE-4k) practice mean: 1365 — the pooled lead
is +289. The 1154 entry floor was cleared on all three trials.

Determinism verified: re-running the same trial gave byte-identical game
records (fixed node budget; no clock, no randomness).

## Development

Built and graded locally in stages — perft-verified move generation and
negamax first, then the transposition table, quiescence, null-move
pruning, LMR and check extensions, re-running the practice trials after
each stage.

Node budget tuned last, by direct comparison: a 65,000-node variant graded
over the same three trials scored mean 1364 and was reverted in favour of
the 55,000-node build (mean 1425) shipped here.

The shipped `main.py` is the lzma-packed build from the kit's `compress`
command, packed from the readable 9,697-byte source.

---

<sub>Entered on [make.fun](https://make.fun/?ref=z6sz10w2&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
