# How the vibe-coded games were built, and the skills to copy

Research date: 2026-10-31. Covers 25 game links from the #vibed-games Slack channel, plus a survey of the Claude Code skills, MCPs and asset tools available for game dev.

**A caveat on sources.** X/Twitter (and its nitter/xcancel mirrors) is blocked from the research environment. For most posts, the only sources were search snippets, aggregator write-ups, repos and articles. Every claim below is tagged ✅ confirmed (with a source), ~ inferred, or ? unknown. Games that published a repo (Tidewater, Fuse Bombers) are documented in far more detail than the rest.

---

## 1. TL;DR

1. **Most of the jaw-dropping three.js demos use no AI 3D assets at all.** Everything is procedural in code: shaders, erosion terrain, code-generated trees, synthesized audio. Examples are the Minecraft test, the Forest-like island, the "Claude Next" terrain demo, the Brawl Stars clone and Dev Ed's C++ falling-sand game. The best-documented one, Dan Greenheck's **Tidewater**, combines code-built geometry with CC0 photoscans from Poly Haven, free Rocketbox characters and Freesound audio. It uses no AI meshes.
2. **AI-generated 3D (Tripo/Meshy) appears mainly in the Godot/Unity pipelines.** Examples are Graalito's dark fantasy game (Tripo), Ferhat's winter survival game (Meshy plus Blender driven by GPT-6 Astra), rewind's cozy fishing game (Tripo Smart Mesh P2 → Blender → Unity 6) and Higgsfield's platform (its own image-to-3D and rigging tools).
3. **Two workflows dominate:**
   - **Short prompt plus a long autonomous run at high effort.** Noah's Minecraft came from a 127-character prompt at max effort in 1h37m. Graalito's took one prompt and 7 hours. The Forest clone took one day.
   - **Brief first, then phased implementation with independent review.** Sindre's Topside, Andreas' Fuse Bombers and Ferhat's "Fable plans, Astra models" split all work this way.
4. **What separates the good games from the rest is guard rails, not prompt length.** Typical guard rails are a deterministic engine/render split enforced by tests (Fuse Bombers), headless smoke tests (Tidewater), seeded generators plus a solver that checks every seed (Topside bunkers), and screenshot/video playtesting.
5. **The skills ecosystem already exists and is good.** Adopt `CloudAI-X/threejs-skills`, `majidmanzarpour/threejs-game-skills` and `dgreenheck/webgpu-claude-skill`. Write about four of our own: brief, playtest capture, visual verification and `/goal` recipes. See §5.

---

## 2. Overview table

| # | Game (author) | Engine | Model / harness | Assets | World | Workflow |
|---|---|---|---|---|---|---|
| 1 | Brawl Stars clone (Tim Jayas) | three.js ✅ | Fable 5.2 ✅ | procedural code ~ | handmade arena ~ | one-shot ~ (a similar demo by @notjazii took ~1h from "make a 3JS Brawl Stars clone, focusing on lighting and shadows" ✅) |
| 2 | Moonlit swamp (Chetan Ankola) | three.js ✅ | GPT-6 Astra "Ultra" ✅ (likely Codex ~) | procedural code ~ | handmade scene ~ | iterative series ~ |
| 3 | Winter survival (Ferhat Demir) | **Godot** ✅ | Fable 5.1 plans/codes, Astra does Blender ✅ | **Blender (model-driven) + Meshy** ✅ | ? | brief-first ✅ |
| 4 | Dark fantasy "gauntlet loop" (Graalito) | **Godot, no MCP** ✅ | Fable 5.1, later Opus 5.5 ✅ | **Tripo3D** ✅, mocap pipeline ✅ | ? | one prompt, 7h autonomous ✅ |
| 5 | Samurai demos (Higgsfield) | browser, three.js ~ | Opus 5.5 vs Astra via Higgsfield MCP ✅ | **AI 2D→3D + rigging via Higgsfield** ✅ | AI-authored ~ | "one prompt" ✅ |
| 6 | Dark Souls (Alex) | ? (three.js ~) | Opus 5.5 early access ✅ | ? | ? | ? |
| 7 | SkiFree 2026 (almonk) | ? | Claude ~ | ? | ? | ? |
| 8 | Minecraft "Lumen Vale" (Noah Wachnik) | three.js/WebGL ~ | Opus 5.5 **max effort** ✅ | procedural code ~ | procedural ~ | 127-char one-shot, 1h37m ✅ |
| 9 | Opus + three.js "7hr cook" (cryptomanavan) | three.js ✅ | Opus 5.5 ✅ | ? | ? | long autonomous ~ |
| 10 | Halloween game (Simon) | three.js → **custom renderer** ✅ | Codex ✅ | ? | ? | human-designed renderer, Codex implements ✅ |
| 11 | 50 games in 24h (Oguz) | ? | Opus 5.5 ✅ | ? | ? | batch/templated ~ |
| 12 | **Tidewater** (Dan Greenheck) | **custom WebGPU/WGSL, no framework** ✅ | Opus 5.5, $1,874 ✅ | **code + Poly Haven CC0 photoscans + Rocketbox** ✅ | handmade in code ✅ | many iterative prompts ✅ |
| 13 | Forest-like survival (aipulsedaily) | three.js ✅ | Opus 5.5 ✅ | **trees generated in code + photoscanned bark** ✅ | **erosion-simulated 2 km island** ✅ | 1 day continuous ✅ |
| 14 | 8-bit Rocket League (Robko) | ? | ? | pixel art | replays of an ML bot ✅ | ? (replay renderer more than game ~) |
| 15 | Sidewinder (Josh Clemm) | WASM + Cloudflare ✅ | Codex + GPT-6 Astra ✅ | **Blender** ✅ | ? | iterative, "a ton of human touch" ✅ |
| 16 | @threejs post | ? | ? | ? | ? | unknown (r186 added Gaussian splats ~) |
| 17 | Highrise Dynasty (Chris DiMarco) | ? (WebGL ~) | Opus ✅ | ? | simulation (NPC routines) ✅ | iterative ~ |
| 18 | Zelda BotW-like (Danny Limanseta) | three.js ~ (his previous work) | Opus 5.5 ✅ | ? | handmade ~ | **kids direct live, iterative, 1 day** ✅ |
| 19 | Pixel-sim game (Dev Ed) | **C++ + SDL, no engine** ✅ | ? | **zero files, everything simulated** ✅ | falling-sand sim ~ | ? |
| 20 | Terrain demo (Salio, "Claude Next") | single file ~ | "Claude Next" ✅ | **zero assets, music in code** ✅ | procedural + erosion ✅ | 3h, 1,135 LOC ✅ |
| 21 | Cozy fishing (rewind) | **Unity 6** ✅ | Opus 5.5 ✅ | **Tripo Smart Mesh P2** ✅ → Blender ✅ | handmade ~ | "one tool, one job" ✅ |
| 22 | ASCII MMORPG (Ansenjeo ~ / Smallzero ~) | custom ~ | ? (Smallzero's "vibe coded" ✅) | **text glyphs only** ✅ | ? | ? |
| 23 | **Fuse Bombers** (Andreas) | **Phaser 4 + TS + Vite** ✅ | Claude ✅ | reused Fuse Riders art (AI-made ~), SFX synthesized ✅ | seeded procedural terrain ✅ | **issue → agent implements → agent reviews** ✅ |
| 24 | **Topside** (Sindre) | Cavecraft (own voxel engine, JS) ✅ | Opus 5.5 extra effort ✅ | voxel templates from Node scripts ✅ | **seeded worldgen + templates + decay + bunker generator with solver** ✅ | brief per feature, phased steps, independent review per step ✅ |
| 25 | HoMM3 MCP (kvit's stream) | VCMI (C++) ~ | Claude Code plays ✅ | n/a | n/a | MCP exposing game state as text ✅ |

Not reachable: two Instagram links (blocked). Skipped as non-game: cabin, DLSS, Mac emulation, Snake Race, model and pricing links.

---

## 3. Categories

### 3a. AI-generated 3D assets
- **Graalito**: Tripo3D for all assets, plus a mocap pipeline for weapon animations. ✅
- **Ferhat Demir**: Meshy for some models; GPT-6 Astra built the rest in Blender. ✅
- **rewind (cozy fishing)**: Tripo Smart Mesh P2 for every model (palms, rocks, pier, boat, hut, character), then Blender, then Unity 6. ✅
  - A near-identical public tutorial by @Saccc_c spells out the full chain:
    - Fable 5.1 writes the PRD.
    - An image model makes reference and layout images.
    - Tripo makes the props, and Smart Mesh P2 multi-view-to-3D makes the character.
    - The character is rigged with Blender + Auto-Rig Pro and animated with Mixamo clips that Opus retargets.
    - Opus places props through the Unity MCP, and a separate agent reviews screenshots against the reference images. ✅ (for that tutorial)
- **Higgsfield**: its own MCP offers `image_to_3d`, `tripo_3d`, `sam_3_3d` and `3d_rigging`. ✅

### 3b. Blender driven by the model
- Ferhat (Astra writes bpy scripts/MCP calls ~), Josh Clemm's Sidewinder ✅, rewind's cleanup/rigging step ✅.
- Pattern: Blender is the **cleanup and kitbash station** (decimate, rig, export GLB), not where art gets made from scratch.

### 3c. Procedural in code (no external assets)
- Brawl Stars clone ~, Minecraft test ~, swamp ~, "Claude Next" terrain ✅, Dev Ed C++/SDL ✅, Forest-like island trees ✅, Fuse Bombers terrain and SFX ✅, Topside voxel templates ✅.
- **This is where the current models are strongest.** Sindre's experience backs this up: Opus 5.5 handles shaders much better than Astra.

### 3d. CC0 / photoscanned assets plus code
- **Tidewater**: Poly Haven photoscans decimated by scripts in `tools/`, Microsoft Rocketbox characters (MIT), a procedural whale traced from a NOAA illustration, and 42 Freesound CC0 recordings as positional audio. ✅
- **Forest clone**: real photoscanned bark wrapped on trees generated in code. ✅

### 3e. How worlds and levels were made
- **Simulation-based terrain**: hydraulic erosion (the Forest clone, the "Claude Next" demo).
- **Template + placement + decay** (Topside): voxel buildings are authored by Node scripts, placed on real terrain, then damaged by a `decay` pass. ✅
- **Grammar/graph generators with a solver** (Topside bunkers):
  - The generator makes a zone tree with locks and keys, then a story pass adds crew, logs and fates.
  - A solver must reach the vault from the airlock, and removing any key must keep its zone sealed.
  - 256 seeds were checked with 0 failures. ✅
  - This is the best "AI makes good levels" technique in the whole set: **generate, then verify mechanically**.
- **Seeded deterministic engine** (Fuse Bombers): `src/engine` may not touch `Math.random`, `Date.now`, Phaser or the DOM, and a test enforces it. ✅
- **Hand-directed in code** (Tidewater, most demos).

### 3f. Engines
- **three.js** is the default for browser demos: about 9 of the games. Tidewater and Simon both outgrew it with a custom renderer.
- **Godot** (Graalito, Ferhat): the model edits plain-text `.tscn`/`.gd` files and runs `godot --headless`. "Godot without MCP is the way." ✅
- **Unity 6** (rewind), **Phaser 4** (Fuse Bombers), **C++/SDL** (Dev Ed), **WASM** (Sidewinder), **VCMI** (HoMM3 agents).

### 3g. Audio
- Suno for music (Sidewinder) ✅, Freesound CC0 (Tidewater) ✅, synthesized in code (Fuse Bombers SFX, "Claude Next" music) ✅. ElevenLabs is available through MCP.

### 3h. Workflows that worked
1. **Short prompt plus max effort plus a long run** for showcase demos (Noah: 127 characters, max effort, 1h37m).
2. **Brief first** (Sindre: "write a detailed brief of a game like…", then `/goal implement brief in full`). Topside briefs (e.g. `docs/BRIEF-BUNKERS.md`) split each feature into steps B.1–B.8. Each step is implemented, reviewed independently and then committed, with a feature flag until promotion.
3. **Issue → implementer agent → reviewer agent** (Andreas' Fuse Bombers):
   - The rules live in `AGENTS.md`, and `CLAUDE.md` just contains `@AGENTS.md`.
   - One PR per issue; CI is checked with a single `gh pr checks --watch`.
   - Gameplay/visual PRs stay open with playtest instructions, because "green CI ≠ acceptance".
4. **Human designs, agent implements** (Simon's renderer, Josh's hand-tuned wheel feel, Danny's kids as product owners).
5. **Many small conversational prompts** (Dan Greenheck: "each '-' is a separate prompt… I don't correct my mistakes"), with an engine smoke test so the agent can check its own work.

---

## 4. Tools and MCPs worth knowing

| Job | Tool | Notes |
|---|---|---|
| See the game | **Playwright MCP** (microsoft/playwright-mcp) | screenshots, `browser_evaluate` to read game state or `renderer.info`. Set up first. |
| Profile | **Chrome DevTools MCP** | perf traces, heap snapshots, CPU throttling |
| Fresh docs | **Context7** MCP; threejs.org/llms.txt | three.js and Godot APIs change fast |
| Blender | **ahujasid/blender-mcp** (~32k★) | runs arbitrary bpy and pulls in Poly Haven, Sketchfab, Rodin, Hunyuan, Tripo. Keep it on localhost and turn telemetry off. |
| AI 3D, rigged/animated | **Meshy MCP** (official) | text/image→3D, rig, animate, retexture. Needs the Pro plan. |
| AI 3D, clean low-poly props | **Tripo** (Smart Mesh P2: quad meshes ≤25k faces); `npx @tripo3d/cli` skill | The Tripo MCP is alpha. |
| Self-hosted 3D | TRELLIS.2 (MIT, 24GB GPU) | Hunyuan3D's licence excludes the EU/UK. |
| Textures/HDRI | Poly Haven API (credit required), ambientCG | CC0 |
| Free models | Kenney, Quaternius, KayKit (CC0, no API, so download once and keep a manifest); Poly Pizza (API, mostly CC-BY) | |
| Concept art | nano-banana MCP, Meshy text-to-image | reference images for image-to-3D and visual review |
| SFX | ZzFX/jsfxr (code), ElevenLabs MCP | |
| Music | Suno (no official API, so generate by hand) | |
| Godot | no MCP: text files plus `godot --headless`. Coding-Solo/godot-mcp (~6k★) for editor control. | |
| Unity | CoplayDev/unity-mcp (~16k★), Unity's official MCP (pre-release), IvanMurzak/Unity-MCP | |

**"One tool, one job" pipeline:** reference image (image model) → hero mesh (Meshy/Tripo) → filler from the CC0 library before generating anything → cleanup, decimation and GLB export in Blender only → materials from Poly Haven → code in Claude Code + Context7 → verify with Playwright, then DevTools. Keep a `CREDITS.md` from day one.

---

## 5. Recommended skill set for the group

**Adopt (existing repos):**

1. **`CloudAI-X/threejs-skills`** (~3.7k★, 13 skills, r186). API knowledge with "Common Mistakes: outdated → r186 fix" lists, e.g. `three/nodes` → `three/tsl` and Clock → Timer. Fixes the model's stale three.js knowledge.
2. **`dgreenheck/webgpu-claude-skill`** (Tidewater's author). TSL, compute, WGSL, post-processing. Load it when using the WebGPU renderer.
3. **`majidmanzarpour/threejs-game-skills`** (~2.6k★), **forked and trimmed.**
   - Keep `threejs-game-director`: scope rules, a lead plus ≤2 workers with file ownership, one non-recursive review, and an `artifacts/game-progress.md` continuity file.
   - Keep `debug-profiler` and `qa-release`, which has Playwright smoke, visual-regression and bot playtest templates.
   - Drop the paid generators unless we have the keys.
4. **`obra/superpowers` → `subagent-driven-development` + `writing-plans`.** This is Andreas' issue → implement → review loop, already packaged.
5. **`gamedev-skills/awesome-gamedev-agent-skills`**: cherry-pick `game-feel`, `procedural-gen` and `audio-design`.
6. **Anthropic `skill-creator`** for writing and evaluating our own skills. Also `webapp-testing` and `frontend-design` for HUDs and menus.
7. **Engine add-ons as needed**: Godot → `jame581/GodotPrompter` + `godot-gdscript-headless-testing`; R3F → `EnzeD/r3f-skills`; Unity → Unity-MCP's generated skills.

**Write ourselves (the gaps, built from what worked above):**

8. **`game-brief`**: a one-page brief covering fantasy, core loop, controls, camera, art direction with reference images, win/fail, what's out of scope, and the feature list as phased steps with acceptance criteria (Sindre's Topside format). Deliberately not a 30-document GDD. Claude-Code-Game-Studios measured that its 30-doc build came last in blind review against a one-page brief.
9. **`playtest-capture`**: every game must expose
   - `window.__ready`, `window.__step(dt)` and `window.render_game_to_text()`, plus a seeded RNG.
   - A Playwright script with input bursts, a screenshot per state, an ffmpeg proof clip, and a check that the GPU is real (warn on swiftshader).
   - This lets the agent see and play its own game. The ideas come from godogen, openai `develop-web-game` and the HoMM3 MCP's "game state as text".
10. **`visual-verify`**: compare captures against reference images using a short defect list (readability, silhouette, lighting, HUD fit, motion). A reviewer subagent returns concrete defects, not scores. A visual change only closes with a screenshot.
11. **`procgen-with-solver`**: generators are pure functions of a seed. Every generator ships with a validator/solver run over N seeds, plus golden hashes, behind a feature flag until promoted. This is the Topside bunker pattern and the main technique for getting good AI-made levels.
12. **`goal-recipes` + `test-discipline`**:
    - `/goal` conditions must be mechanically provable from the transcript, e.g. "build exits 0, playtest passes, capture paths printed, or stop after N turns".
    - Unit-test only pure logic (sim, RNG, scoring). Run one smoke playthrough per iteration and full visual passes only at milestones. This fixes Sindre's "it waits on tests all the time" problem.
    - Enforce an engine/render boundary test like Fuse Bombers' `engine-boundary.test.ts`.

**Setup order:** Playwright MCP + Context7, then the adopted skills (1–4), then write 8–9. Add Blender MCP + Meshy/Tripo only when a game needs custom art; procedural + CC0 gets surprisingly far (see Tidewater).

---

## 6. Gaps
- About 10 posts could only be read through search snippets, because X is blocked: Alex, almonk, cryptomanavan, Simon, Oguz, Robko, @threejs, Chris DiMarco and parts of others. Pasting the thread text or replies would fill these in.
- Dan Greenheck's full prompt list is in his follow-up post (x.com/dangreenheck/status/2102911556296052788).
- The Instagram links couldn't be opened.

## Key sources
- Tidewater: https://github.com/dgreenheck/tidewater · demo https://dgreenheck.github.io/tidewater/
- Fuse Bombers: https://github.com/andreavs/fuse-bombers
- Minecraft test prompt: https://jasonzhu.ai/en/prompts/claude-opus-5-5/2102470200415166699
- Higgsfield Games/MCP: https://higgsfield.ai/blog/higgsfield-mcp-gpt6-astra-games-2 · https://www.developersdigest.tech/blog/claude-opus-5-5-3d-world-higgsfield-mcp
- Brawl Stars demo write-up: https://aiidelist.com/blog/claude-fable-5-2-brawl-stars-threejs-demo
- Sidewinder: https://runtimewire.com/es/article/sidewinder-josh-clemm-super-off-road
- Tripo → Unity tutorial: https://youmind.com/landing/x-viral-articles/ai-3d-game-dev-tutorial
- Godot + agents: https://dev.to/abagames/godot-is-well-suited-for-game-development-with-ai-coding-agents-2lcm
- HoMM3 MCP: https://github.com/andreinwald/heroes-3-mcp
- Skills: https://github.com/CloudAI-X/threejs-skills · https://github.com/majidmanzarpour/threejs-game-skills · https://github.com/dgreenheck/webgpu-claude-skill · https://github.com/gamedev-skills/awesome-gamedev-agent-skills · https://github.com/htdt/godogen · https://github.com/Donchitos/Claude-Code-Game-Studios · https://github.com/obra/superpowers · https://github.com/anthropics/skills · https://github.com/jame581/GodotPrompter
- MCPs: https://github.com/ahujasid/blender-mcp · https://github.com/meshy-dev/meshy-mcp-server · https://github.com/microsoft/playwright-mcp · https://github.com/ChromeDevTools/chrome-devtools-mcp · https://github.com/upstash/context7 · https://github.com/Coding-Solo/godot-mcp · https://github.com/CoplayDev/unity-mcp
