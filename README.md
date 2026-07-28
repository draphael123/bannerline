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
- **Archer — 90g.** Ranged, frail. Holds at firing range and needs a line in front of it.

Battles run 3 minutes. If nobody's keep falls, whoever holds more keep HP wins.

## Four mechanics that are load-bearing

Each one fixes a failure the balance harness caught. Removing any collapses the game into a degenerate strategy — none of them are flavour.

**The lane has five rows.** On a single line, units queue single-file and only the leader ever fights — cheap swarms get no value from numbers, and cleave farms the whole queue. Knight-spam beat everything 11/0. Rows mean five fights happen abreast, so numbers matter and an outnumbered line gets flanked. Melee reaches its own row ±1; archers ignore rows.

**Units move 2.4× while behind their own front line.** Without this, a forward front line means *your* reinforcements walk further while theirs walk less — so pushing was punished and turtling was optimal. Fatal for a game about taking territory.

**Territory pays income.** Each banner you hold raises your gold rate (`TERR_K`, anchored so the starting 3-banner split is exactly 1.0×). Before this, banners were pure decoration and two even economies ground to a 180-second timeout **75% of the time** — measured, 21-sample mirror. Tying income to ground held turns a positional lead into an economic one, so leads convert instead of stalling. Stalemates went from 16/21 to 0/21.

**Ranged units hold at range and never retreat.** They stop advancing once a target is in reach. An earlier version had them walk backwards to avoid melee, which made your own line appear to rout — stopping is the fix, retreating never was. They also do 0.30× damage to keeps, so archers alone can't siege.

## The campaign

**Three worlds, three battles each.** A world has its own palette and its own standing rule; a level inside it varies by objective, terrain and who the horde fields.

| World | Rule |
|---|---|
| **The Green Marches** | Open country. |
| **The Frostreach** | Deep snow — every soldier marches 18% slower, both sides. |
| **The Ember Waste** | Scorched ground — gold flows 24% faster and the war burns hot. |

Levels differ by more than scenery, which was the actual reason they felt samey:

- **Objective** — `raze` their keep, `hold` five flags for eighteen seconds, or `survive` seventy seconds against a keep you cannot break.
- **Terrain** — marsh or a frozen river closing lanes, enforced for both sides.
- **Composition** — the horde fields a swarm, a heavy column, or a shaman line.
- **Boss** — every battle after the first has one. The **Chieftain** (500 HP) and the **Warchief** (1050 HP) are both `heavy`, so massed spears bring them down.

## Lanes

Five lanes deep, and **you choose which one each unit marches into** — arrow keys, or tap the lane.

⚠️ Lanes used to be nearly decorative. Melee reached its own lane **±1**, covering 3 of 5 lanes, and archers ignored lanes entirely. Measured: **84% of melee engagements and 97% of ranged ones crossed lanes.** Lane choice still moved win rates, but only indirectly — stacking one lane made units queue single-file and lose throughput — so lanes mattered for a reason the player couldn't see. Melee is now strictly same-lane (100%), archers spill exactly one, and the Knight's cleave and the Mage's blast are the deliberate lane-crossing exceptions.

An empty lane is now a genuine free run at your keep.

## Difficulty

Four tiers, chosen **on the title screen** or in Settings. **Recruit is the default.** They scale the AI's generalship and purse — never unit stats — so you're beating a better commander rather than smaller numbers.

Every tier also has a **muster period**: the enemy spends nothing for the first few seconds and their income ramps in gradually. Without it the opening was an ambush you couldn't learn from.

| Tier | Enemy income | Muster | Ramp | Decision | Counters at | Lanes |
|---|---|---|---|---|---|---|
| **Recruit** *(default)* | 0.62x | 10s | 28s | 0.46s | 4 units | random |
| Soldier | 0.82x | 6.5s | 22s | 0.30s | 3 units | least-crowded |
| Veteran | 1.00x | 3.5s | 15s | 0.18s | 2 units | tactical |
| Warlord | 1.10x | 2.5s | 12s | 0.15s | 2 units | tactical |

Measured win rates, 19 headless runs per cell, against three player policies of increasing skill:

| | Recruit | Soldier | Veteran | Warlord |
|---|---|---|---|---|
| Novice *(one lane, no counters, slow)* | 95% | 89% | 74% | 37% |
| Learning *(rough counters, spreads lanes)* | 100% | 100% | 79% | 32% |
| Fast *(tight cadence, tactical lanes)* | 100% | 100% | 84% | 5% |

⚠️ **This is the measurement I got wrong the first time.** The original tiers were tuned against an expert bot that already knew the counter-triangle and optimal lane play, which scored the default at a comfortable 63%. But a *novice* policy — the honest proxy for a first-time human — won **0 of 17 on every tier including the easiest**. The game was unwinnable for anyone who didn't already know it. Always tune against a policy that plays as badly as a real beginner.

Note the "Fast" row does *worse* than Novice at Warlord (5% vs 37%): dumping gold on a tight cadence starves you when the ramped wave lands. Banking beats spamming at high difficulty.

## Tutorial

The first ever battle is guided — nine steps that gate on what you actually do, not on timers alone: buy a unit, change lane, field four units, watch the line form. **The enemy is held off the field entirely until step 5**, so the opening is a lesson rather than an ambush. `Esc` skips it, and Settings has a **Replay Tutorial** action. Completion persists to `localStorage`.

## The campaign map, the store and the save

A parchment **campaign map** with three nodes shows what's cleared, what's next and what's locked; progress, renown and unlocks persist to `localStorage` automatically.

Winning a battle pays **renown** (120 first clear, 40 on a replay). Spend it with **The Quartermaster**, reached from the map:

| | |
|---|---|
| **Mage** — 120 | Blast damage across **every** lane. 38 HP — paper. |
| Reinforced Walls — 3 levels | +90 keep HP each |
| Deeper Coffers — 3 levels | +9% gold income each |
| Standing Army — 2 levels | +45 starting gold each |

The **Mage** is the deliberate lane-breaker: its blast ignores rows entirely, which is only meaningful *because* everything else no longer does. It is priced at exactly one first-clear (120 renown) so it is the reward for finishing battle I — it shipped at 200 and was silently unreachable until battle II. It also shipped underpowered at 110g/15dmg (it actually made you worse — Warlord 65% → 35%) and was corrected to 100g/21dmg, which puts it ahead: Veteran 76% → 94%.

## Home and bestiary

The home screen is a **menu**, not a match already in progress — a static scene of the two keeps facing each other, with Campaign / Bestiary / Settings and the difficulty picker. The unit glossary lives in the **Bestiary**, which lists both armies with stats and counters, and greys out what you haven't unlocked.

## Heroes

Once you buy one from the **Heroes** tab of the store, a hero marches in every battle for free. It fights on its own, and:

- **It walks to whichever lane you've selected.** One input does both jobs, it needs no extra controls (so it works on touch), and because it *walks*, switching lanes leaves it out of position for about a second — lane commitment still costs something.
- **One ability** on a cooldown — `Space` or the button bottom-left.
- **Death is a timed respawn** at your keep, not a permanent loss. A 60-second battle can't carry permadeath.

The first hero is the **Standard-Bearer**: tanky, modest damage, and RALLY hastens and strengthens the troops around him. It's deliberately an *anchor* rather than a carry — a duelist hero would turn your army into an escort, which is the failure mode worth avoiding.

**Pricing the hero.** A free permanent respawning body you can steer to the losing lane is worth a lot, and four separate levers barely touched it: three rounds of stat nerfs, adding bosses, and teaching the AI to counter-buy spears (heroes are `heavy`, which skirmishers already punish 3.2×). Bosses in particular made hero-less play *harder* without touching the hero — the hero counters bosses, not the reverse.

What finally scaled with the problem was pricing it directly: **the horde musters ~18% harder when a champion rides against them.**

⚠️ The hero is still an advantage at the top tiers, and the exact multiplier is tuned on a crude bot that uses a hero far worse than a person would. This one needs a playtest, not more simulation.

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

**Emissive light.** Nothing in the scene emitted light before — fire, gold and sparks were just coloured squares. Bright sources (shaman flames, gate torches, impact sparks, the front-line seam, lightning, a burning keep) now queue additive glows composited in a single pass, so overlapping sources stack the way real light does.

**Centre-weighted grade.** A vignette plus a warm centre. The frame was lit corner to corner, which is why it read flat even when every individual asset was fine.

**Crowd variation.** Three baked tint variants per unit, assigned at spawn. Twenty byte-identical sprites read as clone stamps; a subtle warm/cool shift per soldier makes a crowd read as a mob.

**Persistent battle scars.** Churn, scorch and dropped weapons accumulate permanently where fighting happened (capped at 340), so the ground shows where the battle has actually been rather than only where it currently is.

**Keeps that live and die.** Flying pennants, gate torchlight, smoke pouring from damage stages — and a real collapse: the keep sinks, throws debris, catches fire and piles rubble. The most important beat in the game previously had less feedback than a spearman dying.

**Per-battle atmosphere.** Each battle draws the next sky in rotation — Clear Day, Dusk, Storm (rain and lightning), Deep Winter (snowfall) — via sky/water swaps and a full-field colour wash. Props are baked with fixed colours, so washing is far cheaper than re-baking the scenery every battle.

## The campaign curve

Each level carries a `pressure` scalar that sits *underneath* the global difficulty tier, so level 9 is harder than level 1 whichever tier you picked. And the horde scales with your **empire** — `hordeResponse()` counts your hero, your unlocked units and every upgrade level, and musters accordingly.

⚠️ Both of those exist because measurement found the opposite of what I'd assumed:

- I built nine levels to be *different* and never checked they got *harder*. Swept at two tiers, the campaign was nine one-off skirmishes in random order — level 6 was harder than the finale, level 7 was a free win, level 8's survive objective could not be lost.
- Fully kitted (hero + mage + five upgrade levels) the campaign was **100% winnable on every level at every tier below Marshal**. Player power compounded faster than any per-level ramp, which is why scaling the horde to the whole empire — rather than to the hero alone — was the only lever that tracked the cause.

⚠️ **The absolute win rates below are a bot's, not a person's.** The harness policy counters perfectly, picks lanes optimally and buys the instant it can afford to. It reads ~100% on the normal tier where a human would not. The *shape* of the curve is trustworthy; the numbers need a human pass.

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

## Mobile

The game is **landscape by design** — the whole point is seeing the entire battle line at once, which a portrait phone cannot show. So on a narrow portrait viewport it asks you to rotate, with a tap-to-play-anyway escape.

In landscape it scales to fit any screen, taps work for everything (cards, lane selection, menus), and coarse pointers get a forgiving margin around every control. The tutorial swaps its keyboard prompts for tap instructions.

⚠️ The old `fit()` did `Math.max(1, ...)`, clamping the scale to at least 1× — which forced a 640px-wide canvas onto every phone and overflowed the viewport. The game was simply broken below 640px and nobody had looked.

## Running locally

Single file, no build step. Serve it — don't open as `file://`:

```bash
python -m http.server 5784
```

## Status

v0.5. Three battles, three units, four difficulty tiers, plays on a phone. Combat, pacing, presentation and player agency are settled — lane choice closed the "no placement" gap, and the atmosphere system is already campaign-shaped (each battle draws the next sky).

Queued next: a proper save system (campaign progress currently only advances within a session), a richer intro, and a unit viewer / bestiary.

Known rough edges: the unit sprites are improved but not exceptional and would benefit from real pixel-art iteration; there's no campaign map screen yet, so battles advance by restarting rather than through a chosen node.
