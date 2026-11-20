# tiny-chess-01 — a 4 KB chess engine

A single-file Python chess engine that fits in **4,096 bytes** (packed with `makefun compress`).

[![make.fun](https://img.shields.io/badge/make.fun-tiny--chess--01-blue)](https://make.fun/c/tiny-chess-01)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Engine

`main.py` is the whole engine. It speaks the make.fun game protocol over HTTP
(`MAKEFUN_ENV_URL`): it fetches the game in play, picks the best move with a
fixed 80,000-node search, and posts the move back.

Strength (local practice trials, 6 games vs the house gauntlet, this build):

| trial | score | Elo |
|-------|-------|-----|
| 0 | 5.0 / 6 | 1,654 |
| 1 | 3.0 / 6 | 1,279 |
| 2 | 5.5 / 6 | 1,809 |

Total: **13.5 / 18**. Practice mean: **1,581** (house baseline sunfish: 1,365).

## Inside (packed source, 10,147 bytes unpacked, 4,088 bytes packed)

- 0x88 board, piece lists, deterministic Zobrist hashing
- Tapered PeSTO evaluation (middlegame / endgame tables, king taper kept)
- Alpha-beta + transposition table + null-move pruning + LMR with re-search +
  killers + history + quiescence (captures / promotions / check evasions)
- Fixed 80,000 nodes per move (deterministic, ~2.6 s per move)

Verification (this build): perft all OK on 6 positions (start, kiwipete,
pos3-ep, pos4-promo, pos5, pos6); mate-in-1 found (Qh5xf7, 2,766 nodes);
UCI roundtrip OK; identical move and node count on repeated runs.

## Reproduce

```sh
node kit/cli/makefun.mjs compress src-engine2.py --out main.py
ls -l main.py   # <= 4096 bytes
```

## License

MIT. The PeSTO evaluation tables are from the public-domain PeSTO
evaluation function (Ronald Friederich / RofChade). This engine is an
original implementation; no code is copied from any other engine.
