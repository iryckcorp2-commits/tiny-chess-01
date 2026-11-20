# tiny-chess-01 — a 4 KB chess engine

A single-file Python chess engine that fits in **4,096 bytes** (packed with `makefun compress`).

[![make.fun](https://img.shields.io/badge/make.fun-tiny--chess--01-blue)](https://make.fun/c/tiny-chess-01)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Engine

`main.py` is the whole engine. It speaks the make.fun game protocol over HTTP
(`MAKEFUN_ENV_URL`): it fetches the game in play, picks the best move with a
fixed 50,000-node search, and posts the move back.

Strength (local practice trials, 6 games vs the house gauntlet):

| trial | score | Elo |
|-------|-------|-----|
| 0 | 4.5 / 6 | 1,545 (v1) |

## Inside (packed source, ~9.9 KB unpacked)

- 0x88 board, piece lists, deterministic Zobrist hashing
- Tapered PeSTO evaluation (middlegame / endgame tables, king taper kept)
- Alpha-beta + transposition table + null-move pruning + LMR + killers +
  history + quiescence (captures / promotions / check evasions)
- Fixed 50,000 nodes per move (deterministic, ~1.5–2.5 s per move)

## Reproduce

```sh
node kit/cli/makefun.mjs compress src/compact.py --out main.py
ls -l main.py   # <= 4096 bytes
```

## License

MIT. The PeSTO evaluation tables are from the public-domain PeSTO
evaluation function (Ronald Friederich / RofChade). This engine is an
original implementation; no code is copied from any other engine.
