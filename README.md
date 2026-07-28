# BANNERLINE

A 2D pixel-art **lane pusher** — not tower defense. Knights of a river keep against a goblin horde. Gold trickles in, you buy units that march right, and where the two armies collide *is* the front line. Ground behind it takes your colour, the banner posts flip as it passes, and **the ground you hold pays your wages**. Push the line onto their keep to win.

**[Play it →](https://bannerline.vercel.app)**

## Controls

| | |
|---|---|
| `1` / `2` / `3` or click | Buy Spearman / Knight / Archer |
| `↑` `↓` or click the field | Choose the deployment lane |
| `S` | Toggle 1× / 2× speed |
| `Esc` / `P` / gear icon | Pause & settings |
| `M` | Mute everything |
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

## Lanes

The field is five lanes deep and **you choose which one each unit marches into** — arrow keys, or click the lane directly. Melee only reaches its own lane and the two beside it, so the choice is real: concentrate to punch a hole, or spread to avoid being flanked. An empty lane is a free run at your keep.

The AI picks lanes too, and how well it does so is part of difficulty.

## Difficulty

Four tiers, set in Settings. They scale the AI's **generalship and purse — never unit stats** — so you're beating a better commander rather than smaller numbers.

| Tier | Enemy income | Decision interval | Counters at | Lane choice | Measured win rate |
|---|---|---|---|---|---|
| Recruit | 0.80x | 0.34s | 3 units | random | 95% |
| Soldier *(default)* | 1.00x | 0.20s | 2 units | least-crowded | 63% |
| Veteran | 1.12x | 0.14s | 2 units | tactical | 26% |
| Warlord | 1.26x | 0.10s | 1 unit | tactical | 11% |

Win rates are from 19 headless runs each against a player policy that mirrors the AI's own algorithm, including its lane logic. Median match length scales 43s to 110s across the tiers.

## Settings

Reachable from the title screen or the in-game gear icon (`Esc` / `P`). All options persist to `localStorage`:

- **Difficulty** — Recruit / Soldier / Veteran / Warlord
- **Music** and **Sound** volumes, on independent audio buses (0–100%)
- **Screen shake** — off for motion sensitivity
- **Damage numbers** — off for a cleaner field
- **Start at 2×** — skip the slow opening

The pause menu offers Resume / Settings / Restart / Quit to title.

## Art direction

Two **factions**, not a palette swap: blue-and-steel humans versus a green goblin horde — spear skirmishers with lashed flint, a hulking brute with a bone club and plank shield, and a hooded shaman carrying a burning skull-staff. The player defends a crenellated stone keep; the goblins hold a sharpened-log palisade crowned with a skull totem. Two silhouettes beat one silhouette in two colours for readability in a crowd.

**Depth by rank.** Back rows draw ~14% smaller from pre-dimmed sprite variants, front rows at full size and contrast. Without it a twenty-unit scrum reads as a flat sticker sheet.

**A 4-beat gait.** Units walk on a proper contact/passing/contact/passing cycle with body bob and arm swing, not a 2-frame shuffle — five baked frames per unit per side (four walk, one attack).

**Combat reads in motion.** Units topple and fall rather than vanishing, pivoting at the feet away from the blow. Melee swings draw an arc with a spark at the point of impact, cleaves get a bigger, hotter one, and arrows stick in their target for a moment.

**A living field.** Drop shadows on every unit, prop and keep; banded vertical shading plus a warm sun band; trees, bushes and banners swaying on individual phases via baked lean frames so the treeline never pulses in unison; birds, drifting leaves, churned earth and a dust haze over the clash. All of it is a pure function of a render-only `wind` clock, so ambient life costs the simulation nothing.

**Per-battle atmosphere.** Each battle draws the next sky in rotation — Clear Day, Dusk, Storm (rain and lightning), Deep Winter (snowfall) — via sky/water swaps and a full-field colour wash. Props are baked with fixed colours, so washing is far cheaper than re-baking the scenery every battle.

## Balance harness

Balance is tested headlessly, not eyeballed. In the console:

```js
__bannerline.sim(190, (G, buy) => { if (G.gold >= 30) buy("spear"); })
```

**Benchmark honestly.** The AI has counter logic *and* banks gold for the right unit, so a naive "random mix" policy losing to it proves nothing. The real fairness test is a player policy running the AI's *exact* algorithm — that should sit near 50%.

| Check | Target | Current |
|---|---|---|
| Player running AI's exact algorithm | ~50% | 47W/37L over 84 runs |
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

v0. One battle, three units, four difficulty tiers. Combat, pacing, presentation and player agency are settled — lane choice closed the "no placement" gap, and the atmosphere system is already campaign-shaped (each battle draws the next sky).

The remaining gap is **content**: it's still a single battle, so there's no reason to play a fifth time beyond climbing difficulty. Next step is a short campaign slice with real per-battle variation (a terrain or objective twist, then a boss unit) to test whether the core sustains repetition before committing to a full campaign.

Known rough edges: the unit sprites are improved but not exceptional and would benefit from real pixel-art iteration; there's no responsive layout, so phones are untested.
