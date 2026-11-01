---
name: game-design-fun
description: Make an AI-built game actually fun, not just a pretty tech demo. Covers core loop design, game feel/juice, balance and fairness, progression and meta, and playtest-driven iteration. Load when writing a brief, when a prototype "looks great but isn't fun", when balancing, or when the user asks how to give a game soul.
---

# Making it fun

The most common critique of the viral demos was "pretty concept demos, but where's the game?" One reply in Rimsha's thread put it as "how can I make the games actually fun and have some soul in it". Another: "just because you have all the parts doesn't mean you can assemble a Ferrari." Models are good at mechanics and visuals when given direction. **Fun comes from a tight loop, feel, fair challenge and the human playtesting it.**

## 1. Start from a proven loop and remix it

Models are strongest when an existing concept is mapped onto a new genre, setting or era. Sindre's version: Pixel Dungeon's bunkers mapped onto a Minecraft-like survival world. In the brief:
- "<Game A>'s core loop + <Game B>'s setting + one twist mechanic".
- Wildbrush: Zelda BotW + a paintbrush weapon, where each colour is an element (red fire, blue ice, yellow bounce, green vines). One twist, deeply integrated.
- Relic Eater: a bullet heaven + eat a boss to steal one of its body parts and abilities.

Write the loop at three timescales:
- **Moment** (≤10 s): verb, feedback, reward.
- **Session** (5–20 min): goal, escalation, climax.
- **Meta** (days): unlocks, records, collection. Examples: "max days survived", bunkers claimed.

## 2. Game feel checklist (ask for these explicitly)

- Input responds within 1 frame; buffer jump/attack inputs (~100 ms); coyote time on platform edges.
- Hit feedback: hit-stop (40–80 ms), screen shake (small, decaying), flash on hit, knockback, particles, a layered sound.
- Anticipation and follow-through on attacks; squash/stretch on stylised characters.
- Camera: smoothing, look-ahead in the movement direction, no clipping, framing of the targets.
- Juice for pickups and UI: tweens, number pops, sound pitch variation.
- Readability: enemies telegraph attacks, the player silhouette pops against the background, damage numbers or colours stay clear.

Install `game-feel` from `gamedev-skills/awesome-gamedev-agent-skills` for the full list.

## 3. Balance and fairness

From the group's own Fuse Bombers playtest (Richard): position on the map decided most wins, there was little room for skilful action, and the player was one-shot by multiplier chains without any counterplay. Generalise:
- **Agency over RNG.** Randomness should create situations, not decide outcomes. Give every random threat a counter: move, aim, block, dodge.
- **Positional and start symmetry.** Run bot-vs-bot simulations, e.g. 1,000 matches headless with the deterministic engine, and print win rates per slot or position. They should come out within ±5%.
- **Damage ceilings.** Cap burst damage so one combo can't remove a full-HP player without counterplay.
- **Roguelike/procedural fairness.** Validate generated runs. Topside bunkers use a solver per seed, so "survive longest" isn't a lottery. Also consider daily seeds so everyone plays the same world.
- Difficulty should scale with time or skill, and the player should understand why they died.

## 4. Progression and soul

- Give the world a reason to exist: lore fragments in logs (Topside bunkers have crew, dated logs and fates), environmental storytelling, and a named antagonist or mystery ("plane crash on an island with something in the woods").
- Ambient life: birds, NPC routines (Highrise Dynasty's tenants have commutes that affect satisfaction), weather and day/night.
- Audio is half the feel and the most common giveaway of vanilla AI games; see `game-audio`.
- Let players express themselves: building, loadouts, cosmetics.

## 5. Playtest-driven iteration

- After every feature, play it, and fix one thing at a time (rewind's step 7).
- Have kids or friends as product owners: they ask for what's fun (Danny's boys, Nathan's son drawing dungeon sketches to feed the AI).
- Keep a `docs/PLAYTEST.md` log with date, build, who played, the three best and three worst moments, and the next change.
- Gameplay PRs stay open until a human has played them ("green CI ≠ acceptance").
- Bots that use the same input as humans (Fuse Bombers) give automatic balance numbers and smoke tests.

## Verification

- A one-paragraph loop description per timescale exists in the brief.
- A balance sim prints win rates or time-to-kill distributions.
- The playtest log has at least one entry per milestone.
