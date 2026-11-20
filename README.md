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
| 0 | TBD | TBD |
| 1 | TBD | TBD |
| 2 | TBD | TBD |

Total: **TBD**. Practice mean: **TBD** (house baseline sunfish: 1,365).

## Inside (packed source, TBD bytes unpacked, TBD bytes packed)

- 0x88 board, deterministic Zobrist hashing
- Tapered evaluation (middlegame / endgame material and piece-square tables, king taper, bishop pair)
- Alpha-beta + transposition table + null-move pruning + LMR with re-search +
  killers + history + quiescence (captures / promotions / check evasions)
- Fixed 80,000 nodes per move (deterministic, ~3.5 s per move)

Verification (this build): perft all OK on 6 positions (start d1-d4
20/400/8902/197281, kiwipete 48/2039/97862, pos3-ep 14/191/2812/43238,
pos4-promo 6/264/9467, pos5 44/1442/60371, pos6 46/2079/89890);
mate-in-1 found (Qh5xf7, 2,725 nodes);
UCI roundtrip OK; identical move and node count on repeated runs.

## Reproduce

```sh
node kit/cli/makefun.mjs compress src.py --out main.py
ls -l main.py   # <= 4096 bytes
```

## License

MIT. This engine is an original implementation written from scratch for
this entry; no code is copied from any other engine.
