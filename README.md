# Tiny Chess Engine (4 KB)

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Antrezur/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Antrezur/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An original 0x88-board chess engine in a single Python file (`main.py`,
3,822 bytes, standard library only). No model calls, no network, fully
deterministic: fixed 100,000-node budget per move, never a clock.

## Run it

```sh
python3 main.py
```

Plays the kit env protocol through `MAKEFUN_ENV_URL` (`POST /game`,
`POST /move`, `POST /resign`). Root moves come from the env's
`legal_moves`; the search only evaluates. Falls back to the first legal
move only if the search raises (it does not in testing).

## How it plays

- **Search:** negamax alpha-beta with iterative deepening, Zobrist
  transposition table (2^18 entries, always-replace, TT-move ordering),
  null-move pruning (R=3), PVS + late-move reductions, check extensions,
  quiescence search (captures, plus check evasions when in check),
  fail-soft, MVV-LVA capture ordering, and one depth-indexed killer move
  per ply for quiet-move ordering.
- **Evaluation:** material plus Michniewski piece-square tables with
  PeSTO phase interpolation for the king, pawn-shield king safety,
  passed pawns, pawn structure (doubled/isolated), the bishop pair, and
  central pawn breaks — a pawn on d5/e5 (or d4/e4) with a rook or queen
  behind it on the same file, in the style of Garry Kasparov's dynamic,
  initiative-first play (World Champion 1985–2000, peak rating 2851).
  The evaluation is incremental: the score is updated as pieces move
  instead of recomputed, so the search runs at ~35,000 nodes/s.
- **Move generation:** pseudo-legal with a king-safety filter; castling,
  en-passant, and all four promotions; verified by perft on 6 standard
  positions (all counts exact).

## Measured strength (kit practice trials, honest numbers)

- Trial 0: **1,654 Elo** (5W/0D/1L)
- Trial 1: **1,654 Elo** (4W/2D/0L)
- Trial 2: **1,099 Elo** (2W/0D/4L)
- Mean across the three practice trials: **1,469 Elo** (11W/2D/5L)

That is above the 1,154 practice floor by 315 and above the ~1,325
sunfish baseline by 144, so it qualifies for the final and can win a
prize if the final trials go well. It is 31 short of a 1,500 mean, and
the trial-to-trial spread is large (1,099–1,654), so the mean is
uncertain. Perft 6/6 exact, determinism verified (same position twice
gives the same move and node count), ~2.9 s/move (limit 10 s).

## Files

- `main.py` — the engine (3,822 bytes)
- `LICENSE` — MIT
- `.makefun/owner.json` — entry wallet

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
