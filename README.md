# BANNERLINE

A 2D pixel-art **lane pusher** — not tower defense. Gold trickles in, you buy units that march right, and where the two armies collide *is* the front line. Ground behind it takes your colour and the banner posts flip as it passes. Push it to their keep to win.

**[Play it →](https://bannerline.vercel.app)**

## Controls

| | |
|---|---|
| `1` / `2` / `3` or click | Buy Spearman / Knight / Archer |
| `S` | Toggle 1× / 2× speed |
| `P` | Pause |
| `R` | Restart |

## The triangle

```
SPEAR ──beats──▶ KNIGHT ──beats──▶ ARCHER ──beats──▶ SPEAR
```

- **Spearman — 30g.** Cheap, fast, fragile. Braced spears gut armour.
- **Knight — 60g.** A wall: 300 HP, low damage. Holds frontage, doesn't farm swarms.
- **Archer — 90g.** Ranged, frail. Backs away rather than fight, so it needs a line in front of it.

Battles run 3 minutes. If nobody's keep falls, whoever holds more keep HP wins.

## Three mechanics that are load-bearing

These aren't flavour — each one fixes a failure the balance harness caught, and removing any of them collapses the game back into a degenerate strategy.

**The lane has five rows.** On a single line, units queue up single-file and only the leader ever fights — cheap swarms get no value from numbers, and cleave farms the whole queue. Knight-spam beat everything 11/0. Rows mean five fights happen abreast, so numbers matter and an outnumbered line gets flanked. Melee reaches its own row ±1; archers ignore rows.

**Units move 2.4× while behind their own front line.** Without this, a forward front line means *your* reinforcements walk further while theirs walk less — so pushing was punished, turtling was optimal, and the player lost even running the AI's exact policy (0W/11L in a mirror match). Fatal for a game about taking territory. Now ground you've taken feeds your army instead of stranding it.

**Archers kite slowly, on a budget, and barely scratch stone.** They retreat at 0.68× speed with a 14-unit total budget, then stand and die — otherwise an unkillable archer ball forms. They also do 0.30× damage to keeps, so archers alone can't siege.

## Balance harness

Balance is tested headlessly, not eyeballed. In the console:

```js
__bannerline.sim(190, (G, buy) => { if (G.gold >= 30) buy("spear"); })
```

Health checks that must hold:

| Check | Why |
|---|---|
| `mirror` policy ≈ 50% | Proves no structural asymmetry between sides |
| `idle` 0% | Doing nothing must always lose |
| No mono-strategy > ~55% | No dominant single unit |
| Median match ~70s | Pace fits the 3-minute cap |

Current: spear→knight 7/7, knight→archer 7/7, archer→spear 4/7 (soft counter). `__bannerline.snapshot()` dumps live state.

## Running locally

Single file, no build step. Serve it — don't open as `file://`:

```bash
python -m http.server 5784
```

## Status

v0, feel-first. One battle, three units, one AI. The question this build exists to answer is whether watching the line slide toward their keep is satisfying for three minutes. Campaign scaffolding and a wider roster come after that answer.
