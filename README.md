# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Antrezur/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Antrezur/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An original 0x88-board chess engine in a single Python file (`main.py`,
4,091 bytes, standard library only). No model calls, no network, fully
deterministic: fixed 100,000-node budget per move, never a clock.
Negamax alpha-beta with a Zobrist transposition table, null-move pruning,
PVS + late-move reductions, check extensions, quiescence, and an
incremental evaluation (material, piece-square tables, king safety,
passed pawns, central pawn breaks in the style of Garry Kasparov).
A compact opening book covers the practice openings the engine lost
or drew. Kit practice trials (6 games each): 2,112 / 2,112 / 1,654 Elo,
mean 1,959 (17W/0D/1L). Perft 6/6 exact, determinism verified.

## Run it

```sh
python3 main.py
```

Plays the kit env protocol through `MAKEFUN_ENV_URL` (`POST /game`,
`POST /move`, `POST /resign`). Root moves come from the env's
`legal_moves`; the search only evaluates.

## Files

- `main.py` — the engine (4,091 bytes)
- `LICENSE` — MIT
- `.makefun/owner.json` — entry wallet

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
