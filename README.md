# BANNERLINE

A 2D pixel-art **lane pusher** — not tower defense. Gold trickles in, you buy units that march right, and where the two armies collide *is* the front line. Ground behind it takes your colour, the banner posts flip as it passes, and **the ground you hold pays your wages**. Push the line onto their keep to win.

**[Play it →](https://bannerline.vercel.app)**

## Controls

| | |
|---|---|
| `1` / `2` / `3` or click | Buy Spearman / Knight / Archer |
| `S` | Toggle 1× / 2× speed |
| `M` | Mute |
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

## Four mechanics that are load-bearing

Each one fixes a failure the balance harness caught. Removing any collapses the game into a degenerate strategy — none of them are flavour.

**The lane has five rows.** On a single line, units queue single-file and only the leader ever fights — cheap swarms get no value from numbers, and cleave farms the whole queue. Knight-spam beat everything 11/0. Rows mean five fights happen abreast, so numbers matter and an outnumbered line gets flanked. Melee reaches its own row ±1; archers ignore rows.

**Units move 2.4× while behind their own front line.** Without this, a forward front line means *your* reinforcements walk further while theirs walk less — so pushing was punished and turtling was optimal. Fatal for a game about taking territory.

**Territory pays income.** Each banner you hold raises your gold rate (`TERR_K`, anchored so the starting 3-banner split is exactly 1.0×). Before this, banners were pure decoration and two even economies ground to a 180-second timeout **75% of the time** — measured, 21-sample mirror. Tying income to ground held turns a positional lead into an economic one, so leads convert instead of stalling. Stalemates went from 16/21 to 0/21.

**Archers kite slowly, on a budget, and barely scratch stone.** They retreat at 0.68× speed with a 14-unit total budget, then stand and die — otherwise an unkillable archer ball forms. They also do 0.30× damage to keeps, so archers alone can't siege.

## Balance harness

Balance is tested headlessly, not eyeballed. In the console:

```js
__bannerline.sim(190, (G, buy) => { if (G.gold >= 30) buy("spear"); })
```

**Benchmark honestly.** The AI has counter logic *and* banks gold for the right unit, so a naive "random mix" policy losing to it proves nothing. The real fairness test is a player policy running the AI's *exact* algorithm — that should sit near 50%.

| Check | Target | Current |
|---|---|---|
| Player running AI's exact algorithm | ~50% | 24W/18L over 42 runs |
| Stalemates | none | 0/21 |
| Doing nothing | always loses | 0/17 |
| Any mono-unit strategy | loses | 0/17 each |
| Median match | 45–110s | ~57s |

`__bannerline.snapshot()` dumps live state; `render()` forces a frame; `setTerrK()` tunes the territory-income slope.

⚠️ Sample size matters. An earlier 13-run pass reported "mirror 5W/7L, median 70s" and concluded the pacing was fine. At 21 runs the same build was stalemating 75% of the time — the small sample simply missed it. Use ≥21 runs before trusting a pacing claim.

## Rendering

Every sprite is **baked once at boot** into an offscreen canvas with a dark outline burned in (`bake()`). Nothing draws raw rectangles at runtime — outlines are the biggest "real pixel art" signal and doing them per-frame would be far too slow.

Hit-stop lives in the main loop, never inside `update()`, so the headless harness stays a pure function of `dt`.

## Running locally

Single file, no build step. Serve it — don't open as `file://`:

```bash
python -m http.server 5784
```

## Status

v0, feel-first: one battle, three units, one AI. Combat, pacing and presentation are settled. Natural next steps are campaign scaffolding (battle 2+, unit unlocks) or a wider roster.
