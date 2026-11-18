# tiny-chess-4k

Entry for make.fun bounty **tiny-chess-01**: the strongest chess engine that
fits in 4,096 bytes of Python.

`main.py` (4,057 bytes) is a self-contained lzma-packed build of a
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
