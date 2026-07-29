# BANNERLINE

A 2D pixel-art **lane pusher** — not tower defense. Knights of a river keep against a goblin horde. Gold trickles in, you buy units that march right, and where the two armies collide *is* the front line. Ground behind it takes your colour, the banner posts flip as it passes, and **the ground you hold pays your wages**. Push the line onto their keep to win.

**[Play it →](https://bannerline.vercel.app)**

## Controls

| | |
|---|---|
| `1` / `2` / `3` or click | Buy Spearman / Knight / Archer |
| `↑` `↓` or click the field | Choose the deployment lane |
| `S` | Cycle 0.5× / 1× / 2× / 4× speed |
| `Esc` / `P` / gear icon | Pause & settings |
| `M` | Mute everything |
| `R` | Restart |

## The triangle

```
SPEAR ──beats──▶ KNIGHT ──beats──▶ ARCHER ──beats──▶ SPEAR
```

- **Spearman — 6g.** Cheap, fast, fragile. Braced spears gut armour.
- **Knight — 12g.** A wall: 300 HP, low damage. Holds frontage, doesn't farm swarms.
- **Archer — 18g.** Ranged, frail. Holds at firing range and needs a line in front of it.

Battle gold is normalized to a **48g cap**. Income, starting gold and unit
prices are one-fifth of the original display values; purchase timing is
unchanged.

Battles run 3 minutes. If nobody's keep falls, whoever holds more keep HP wins.

## Four mechanics that are load-bearing

Each one fixes a failure the balance harness caught. Removing any collapses the game into a degenerate strategy — none of them are flavour.

**The lane has five rows.** On a single line, units queue single-file and only the leader ever fights — cheap swarms get no value from numbers, and cleave farms the whole queue. Knight-spam beat everything 11/0. Rows mean five fights happen abreast, so numbers matter and an outnumbered line gets flanked. Melee reaches its own row ±1; archers ignore rows.

**Units move 2.4× while behind their own front line.** Without this, a forward front line means *your* reinforcements walk further while theirs walk less — so pushing was punished and turtling was optimal. Fatal for a game about taking territory.

**Territory pays and rallies.** Each banner you hold raises your gold rate
(`TERR_K`, anchored so the starting 3-banner split is exactly 1.0×). Capturing
one immediately pays 2g, and friendly troops within its visible rally ring move
and fight 10% faster. Animated coins make the continuing income visible.

**Ranged units hold at range and never retreat.** They stop advancing once a target is in reach. An earlier version had them walk backwards to avoid melee, which made your own line appear to rout — stopping is the fix, retreating never was. They also do 0.30× damage to keeps, so archers alone can't siege.

## The campaign

**Seven worlds, four battles each.** A world has its own palette and its own
standing rule; its fourth battle is a dedicated boss encounter with no enemy
keep. Boss battles use normal unit deployment and end when the boss dies.

| World | Rule you can see | Terrain |
|---|---|---|
| **The Green Marches** | Open country — the baseline. | — |
| **The Frostreach** | **Black ice.** Soldiers carry their momentum and slide past where they meant to stop, so lines overshoot into each other. | A frozen lake across the middle third, where they slide further still. |
| **The Ember Waste** | **Both keeps burn** at 2.8 HP/s. Neither side can afford to wait. | Two geysers that erupt every 6s with a ~1.3s telegraph, for 46 damage to whoever is standing in that lane. |
| **The Skyward Steps** | **Vertical ascent.** The field turns bottom-to-top, with deployment columns replacing horizontal lanes. | Mountain stairs, high cloud, and updrafts that throw units into another column. |
| **The Crown Range** | **Elevation.** Each terrace slows the climb toward the enemy. | Siege gates, avalanche warnings, and a fortified summit. |
| **The Drowned Coast** | **Tides.** Low lanes flood and slow movement on a visible cycle. | Fog, drowned standards, a seawall, and the Tidefather. |
| **The Hollowwood** | **Living roots.** A different lane is choked as the forest shifts. | Poison seed-hearts, narrow paths, and the Thornmother. |

⚠️ The first version of these rules was *18% slower movement* and *24% faster gold*. Both were real and both were completely invisible in play — a world you can't perceive is a palette. Measured: on ice a unit now overshoots its stopping point by **10.6** world units against **6.8** on open ground.

Levels differ by more than scenery, which was the actual reason they felt samey:

- **Objective** — raze a keep, hold five flags, survive a siege, slay a boss,
  or demolish battlefield targets without an enemy castle.
- **Terrain** — marsh or a frozen river closing lanes, enforced for both sides.
- **Level mechanics** — war drums summon defenders until destroyed; blizzards
  reduce ranged damage by 42%; breaking ice warns before damaging an entire
  lane; and war pyres increase enemy income by 10% each until quenched.
- **Composition** — the horde fields a swarm, a heavy column, or a shaman line.
- **Enemy roster** — Runners punish open lanes, Shield Bearers absorb arrows,
  and Fire Bombers punish tightly packed troops after the opening levels.
- **Boss** — each world ends with a no-castle boss battle: the
  **Overchieftain** calls guards, the airborne **White Terror** changes lanes,
  and the **Ash Tyrant** periodically burns troops around it.

## Endless

Endless mode is available directly from the title screen and uses the player's
current warband, spells, training, and castle upgrades. A new wave arrives every
35 seconds, enemy pressure grows continuously, and every fifth wave brings back
a regional boss. The battlefield rotates through the four horizontal world
fronts every three waves, with their unique palette, weather, music, and ambient
motion. A run awards renown and permanently records the highest wave reached.

## War Modes

The War Hall contains eight modes that share the player's persistent army:

- **Endless** — escalating waves, rotating regions, and a boss every fifth wave.
- **Bounty Arena** — a repeatable renown grind where every kill pays a bounty;
  elite enemies and bosses are worth more.
- **Boss Rush** — an expandable boss registry with a recovery choice between encounters.
- **Siege Defense** — a two-minute castle-centered defense.
- **Daily Challenge** — a date-seeded field with fixed army, spells, and difficulty.
- **Vertical Ascent** — survival on Skyward's vertical battlefield.
- **Last Stand** — one 48-gold purse, no income, and seventy-five seconds.
- **Draft Mode** — three starting units and reinforcement choices between waves.

Best scores persist independently for every mode.

## Expanded army and Champions

Crossbowman, Priest, Pikeman, Engineer, Monk, Lancer, Falconer, War Wagon,
Shield Maiden, Alchemist, Duelist, Necromancer, and Banner Guard add armor
piercing, healing, boss stagger, repairs, cleansing, charge attacks, back-line
hunting, mobile projectile cover, interception, acid, elite hunting, temporary
thralls, and a non-Champion rally aura.

Champions are expensive mortal deployments—not free permanent heroes. Only one
Champion may be placed in a five-unit warband. Paladin, Standard-Bearer,
Beastmaster, Runesmith, Stormcaller, Royal Engineer, and Dragon Rider each have
an automatic signature ability and two ability-upgrade levels.

Every specialist has a distinct silhouette and equipment treatment. Core
knights, archers, goblins, castles, trees, and combat effects use selected art
from Pixel Frog's legacy **Tiny Swords** CC0 release. Source and license details
live beside the imported files in `assets/sprites/tiny-swords/SOURCE.md`.

## Boss encounters

Bosses use three health phases and a visible Stagger Gauge. Ballista bolts,
blasts, and Pikemen contribute extra stagger. Filling the gauge interrupts the
current ability, stuns the boss, opens a vulnerability window, and knocks it
backward. Temporary resistance after recovery prevents stun-locking.

The Tidefather telegraphs lane-wide tidal surges. The Thornmother telegraphs
root attacks and summons Rootlings. Both are dedicated castle-free fourth-stage
bosses and automatically appear in Boss Rush.

## Tactical command layer

- Order each lane to **Advance, Hold, or Retreat** (`F`).
- Purchase one unit into **Reserve** and release it later (`Q`).
- Select Vanguard, Bulwark, or Arcane doctrine during a briefing (`D`).
- Choose a safer or more rewarding route (`B`).
- Capture the Shrine and Mine for healing or extra battle gold.
- Read the next enemy deployment in the HUD.
- Earn two temporary veterancy ranks during battle.
- Hunt rare enemy field commanders that strengthen nearby troops.
- Complete optional objectives and choose post-battle spoils.
- Build **Morale** through kills, large targets, and captured ground. At 100%,
  trigger **War Cry** with `Space` (or the battlefield button) for a short
  army-wide rally and guard window.

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

A parchment **campaign map** with twenty-eight nodes shows what's cleared, what's next
and what's locked; progress, renown and unlocks persist to `localStorage`
automatically.

Winning a battle pays **renown** (120 first clear, 40 on a replay). Spend it with **The Quartermaster**, reached from the map:

| | |
|---|---|
| **Mage** — 120 | Blast damage across **every** lane. 38 HP — paper. |
| **Outrider** — 150 | Sprints past the fight entirely and runs at their keep. Send it down an empty lane. |
| **Sapper** — 180 | Feeble against troops, tears walls apart. Escort it. |
| **Keep Ballista** — 3 levels | Your keep shoots back, every 8s down to 6s |
| **Boiling Oil** — 2 levels | Burns whatever reaches your gate |
| Reinforced Walls — 3 levels | +90 keep HP each |
| Deeper Coffers — 3 levels | +9% gold income each |
| Standing Army — 2 levels | +9 starting gold each |

The Quartermaster has separate **Army**, **Champions**, **Spells**, **Castle**,
and **Armory** ledgers.
Every owned regular unit has five mastery ranks. Core unit training also adds
a visible battlefield behavior: brace, shield wall, volley, or arcane surge.
Champion abilities and spells have five ranks, while castle projects have four
or five ranks according to their safe gameplay limit. Castle construction adds
a visible slowing moat and an area-firing mage tower alongside the existing
walls, ballista, oil, coffers, and muster upgrades.

The Armory sells cosmetic army collections, banner treatments, castle themes,
deployment effects, and victory effects. Cosmetics never change combat stats.

## Spells

Spells are unlocked and upgraded through five ranks with renown. You begin with one command seal;
a second spell slot is a major store unlock, and two is the permanent maximum.
Both spells target the currently selected lane and recharge during battle:

- **Firestorm** burns every enemy in the lane.
- **Arrow Rain** delivers a concentrated volley.
- **Reinforce** calls free defenders into the lane.
- **Healing Light** restores and briefly guards friendly troops.

Keys `6` and `7` cast the equipped spells. Clicking an owned spell in the store
equips it; upgrading an unequipped spell equips it automatically.

## Expanding enemy roster

Each region introduces a signature enemy, while later worlds freely reuse
everything already encountered: Wolf Riders in the Green Marches, Ice Trolls
in the Frostreach, Salamanders in the Ember Waste, Harpies on the Skyward
Steps, and Siege Rams in the Crown Range.
The Drowned Coast adds Mire Witches and Bog Stalkers; the Hollowwood adds
Rootlings and Thorn Knights. Later regions continue to reuse earlier enemies.

The **Mage** is the deliberate lane-breaker: its blast ignores rows entirely,
which is only meaningful *because* everything else no longer does. It costs
120 renown to unlock and 20g to deploy.

## Home and bestiary

The home screen is a **menu**, not a match already in progress. Campaign, War
Modes, Bestiary, Settings, and the difficulty picker remain separate. The
paginated Bestiary covers the expanded army, Champions, regional enemies, and
bosses.

## Your warband

You own units; you **march with five**. Chosen before a battle on the Warband screen.

Before the cap, more units on the bar was strictly better, so the optimal play was "buy everything, always" and unlocks were a growing shopping list rather than a decision. Five forces a composition — and it keeps the HUD readable, which six cards would not.

The **Outrider** and **Sapper** exist because the other four all fight *the line*. Nothing changed **how** you win. One bypasses the fight; one exists to break walls. Measured: an outrider runs clean past a Knight blocking its lane, and a sapper does 2.3× a spearman's damage to a keep while doing barely a sixth of its damage to troops.

## Retired hero system

Permanent, free, respawning heroes remain retired because they added a separate
balance axis that unit prices could not control. Champions replace them as
expensive, mortal, normally deployed units. The Standard-Bearer has returned in
this form with a rallying ability.

## Settings

Reachable from the title screen or the in-game gear icon (`Esc` / `P`). All options persist to `localStorage`.

Battle speed cycles through **0.5x, 1x, 2x, and 4x**. New battles begin at
0.5x by default; the fast-start option begins them at 1x.

- **Difficulty** — Recruit / Soldier / Veteran / Marshal
- Independent **Master**, **Music**, **Sound**, and **Interface** volumes
- Screen shake, damage numbers, high contrast, and reduced motion
- Weather, particles, and screen-flash controls
- Automatic pause when focus is lost
- Fullscreen, save export, and save import
- Start at 1× or the default 0.5×

The pause menu offers Resume / Settings / Restart / Quit to title.

## Art direction

Two **factions**, not a palette swap: blue-and-steel humans versus a green
goblin horde. The expanded horde adds distinct runner, shield-bearer and bomber
silhouettes, plus seven oversized world bosses. Battlefields now carry
world-specific structures—war camps, ice spires and a living caldera—rather
than relying on palette changes alone.

**Depth by rank.** Back rows draw ~14% smaller from pre-dimmed sprite variants, front rows at full size and contrast. Without it a twenty-unit scrum reads as a flat sticker sheet.

**A three-phase attack from two frames.** Rather than draw a wind-up pose for all twelve unit types, the attack frame is drawn *pulled back* during the wind-up and *thrown forward* on the strike, then settles. It reads as one motion — guard, wind up, strike, recover — with no new art. Units also breathe on guard and recoil when struck.

**A 4-beat gait.** Units walk on a proper contact/passing/contact/passing cycle with body bob and arm swing, not a 2-frame shuffle — five baked frames per unit per side (four walk, one attack).

**Combat reads in motion.** Units topple and fall rather than vanishing, pivoting at the feet away from the blow. Melee swings draw an arc with a spark at the point of impact, cleaves get a bigger, hotter one, and arrows stick in their target for a moment.

**A living field.** Stronger contact shadows separate units from the ground.
Each world also carries a signature moving foreground layer: Marches reeds,
Frostreach ice streaks, Ember sparks, Skyward clouds, and Crown Range terraces.
Trees, bushes and banners sway on individual phases; birds, drifting leaves,
churned earth and a dust haze keep the field moving. All of it is a pure
function of a render-only `wind` clock, so ambient life costs the simulation
nothing.

**Emissive light.** Nothing in the scene emitted light before — fire, gold and sparks were just coloured squares. Bright sources (shaman flames, gate torches, impact sparks, the front-line seam, lightning, a burning keep) now queue additive glows composited in a single pass, so overlapping sources stack the way real light does.

**Centre-weighted grade.** A vignette plus a warm centre. The frame was lit corner to corner, which is why it read flat even when every individual asset was fine.

**Crowd variation.** Three baked tint variants per unit, assigned at spawn. Twenty byte-identical sprites read as clone stamps; a subtle warm/cool shift per soldier makes a crowd read as a mob.

**Persistent battle scars.** Churn, scorch and dropped weapons accumulate permanently where fighting happened (capped at 340), so the ground shows where the battle has actually been rather than only where it currently is.

**Keeps that live and die.** Flying pennants, gate torchlight, smoke pouring from damage stages — and a real collapse: the keep sinks, throws debris, catches fire and piles rubble. The most important beat in the game previously had less feedback than a spearman dying.

**Per-battle atmosphere.** Each battle draws the next sky in rotation — Clear Day, Dusk, Storm (rain and lightning), Deep Winter (snowfall) — via sky/water swaps and a full-field colour wash. Props are baked with fixed colours, so washing is far cheaper than re-baking the scenery every battle.

## The campaign curve

Each level carries a `pressure` scalar that sits *underneath* the global difficulty tier, so later levels are harder than earlier ones whichever tier you picked. The horde also scales with unlocked units and upgrade levels.

⚠️ Both of those exist because measurement found the opposite of what I'd assumed:

- I built nine levels to be *different* and never checked they got *harder*. Swept at two tiers, the campaign was nine one-off skirmishes in random order — level 6 was harder than the finale, level 7 was a free win, level 8's survive objective could not be lost.
- Fully kitted (hero + mage + five upgrade levels) the campaign was **100% winnable on every level at every tier below Marshal**. Player power compounded faster than any per-level ramp, which is why scaling the horde to the whole empire — rather than to the hero alone — was the only lever that tracked the cause.

⚠️ **The absolute win rates below are a bot's, not a person's.** The harness policy counters perfectly, picks lanes optimally and buys the instant it can afford to. It reads ~100% on the normal tier where a human would not. The *shape* of the curve is trustworthy; the numbers need a human pass.

## Balance harness

Balance is tested headlessly, not eyeballed. In the console:

```js
__bannerline.sim(190, (G, buy) => { if (G.gold >= 6) buy("spear"); })
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

Open `balance.html` through the local server to run a repeatable 240-match
campaign regression sweep. It reports termination, bot win rate, and median
duration for every battle and difficulty tier without changing the player save.

## Audio

Seven recorded CC0 tracks provide a dedicated main-menu theme, one theme for
each world, and a unique theme for each world boss. Source and license details
live in `assets/music/CREDITS.md`. Interface and combat effects are generated
in the browser with the Web Audio API. The procedural score remains as a
fallback when a browser cannot decode a bundled track.

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

The current prototype includes a persistent twenty-eight-battle campaign across seven
worlds, dedicated world bosses, eight level-specific battlefield mechanics, an
expanded regional enemy roster, two-slot spell loadouts, visible castle
construction, a campaign map, warband selection, a bestiary, unlockable units,
training upgrades, four difficulty
tiers, and desktop and landscape-mobile controls.

The next development phase should focus on maintainability and confidence:

- split the single-file source into focused game, rendering, UI, and data
  modules without changing gameplay;
- turn the existing balance harness into repeatable automated checks;
- add browser smoke tests for the title, campaign, shop, warband, battle, and
  save/load flows;
- continue tuning all twelve levels from player feedback;
- iterate on the new boss and enemy sprites with human feedback.
