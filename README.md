# sakmad tiny-chess-01

A tiny original chess engine for the make.fun tiny-chess-01 competition.
One file, Python 3 stdlib only, fully deterministic (fixed node budget,
no wall clock, no randomness except the trial seed).

## Engine

- 0x88 board, alpha-beta + quiescence search
- Transposition table, killer moves, null-move pruning, LMR, PVS
- Aspiration windows at the root (+/-30 cp, depth >= 5)
- Evaluation: piece-square tables from Tomasz Michniewski's
  Simplified Evaluation Function (Chess Programming Wiki) for the
  middlegame, with a king endgame table blended in by game phase.
  (One SEF queen-table value symmetrised: h4 -5 -> 0, mirroring a4.)
- Fixed budget of 120,000 nodes per move; resigns never (plays on).

`main.py` is the packed entry (lzma + base85 payload, 4,026 bytes).
The readable source is golf5.py in the build notes; the packed file is
what was tested.

## Practice results (kit practice trials, 6 games each)

- trial-0: Elo 1545 (3W / 3D / 0L)
- trial-1: Elo 1545 (3W / 3D / 0L)
- trial-2: Elo 1279 (2W / 2D / 2L)
- mean: 1456 (target was >= 1450; margin is thin, trial-2 shows the
  6-game trials are noisy)

Sunfish baseline mean is ~1365; the prize needs a mean above baseline
with a 90% paired t-interval lead above 0.

## Run

The competition harness runs `python3 -P -s /bot/main.py` and talks to
it over HTTP (`$MAKEFUN_ENV_URL`): `POST /game`, `POST /move`,
`POST /resign`. No other files are read at runtime.
