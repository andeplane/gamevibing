# gamevibing

Home for the Vibegames project: game experiments and the Claude skills that support them.

`.claude/settings.json` allows the common tools (shell, file edits, web fetch and search, MCP tools) so Claude sessions in this repo run without permission prompts.

Research behind the skills: [how the viral games were built (author replies)](docs/how-they-built-it-notes.md) · [first research report](docs/vibe-coded-games-report.md)

## Game skills for Claude Code

A set of 19 Claude Code skills for building high-quality games with agents. Most target three.js/WebGPU in the browser, with Godot, Unity, Unreal and 2D notes throughout. Each skill is a **decision guide** (what to use when, and when not to), followed by working recipes, the pitfalls models usually hit, and a verification step. Snippets were run or type-checked against three r187, Rapier 0.21 and the other current packages where possible; anything unchecked is marked *(unverified)* or *check docs*.

Sources:
- The 52 replies on Rimsha Bhardwaj's "10 repos for AAA-style games" thread and her earlier "10 agent game-dev repos" thread.
- The authors' own "how I did it" replies on the 25 games in our Slack channel. These were read in a signed-in browser.
- A survey of GitHub repos, skill packs and MCP servers, as of 31 Oct 2026.

## Install

Claude sessions opened in this repo pick the skills up automatically from `.claude/skills/`. To use them in another game repo:
```bash
# per game repo (recommended)
mkdir -p <game>/.claude/skills && cp -r .claude/skills/game-* <game>/.claude/skills/
# or for every project
cp -r .claude/skills/game-* ~/.claude/skills/
```
Then start with: *"Load game-dev-index and game-direction-workflow, then write docs/BRIEF.md for …"*.

Also install these external packs. We link to them rather than duplicating them:
```bash
npx skills add CloudAI-X/threejs-skills              # version-correct three.js (r186+)
npx skills add dgreenheck/webgpu-claude-skill        # TSL / WebGPU / compute
npx skills add majidmanzarpour/threejs-game-skills   # game director, QA, Playwright templates
npx skills add gamedev-skills/awesome-gamedev-agent-skills   # game-feel, engine-specific skills
# Godot: jame581/GodotPrompter · Unreal: EpicGames/unreal-engine-skills-for-claude-code-plugin
```
If `npx skills` isn't available, clone the repo and copy its `skills/` folders into `.claude/skills/`.

MCP servers:
```bash
claude mcp add playwright npx @playwright/mcp@latest        # see + play the game (essential)
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest   # perf traces, heap
claude mcp add --transport http context7 https://mcp.context7.com/mcp   # current docs
# assets, when needed: blender-mcp (uvx), Meshy MCP, Tripo CLI (npx @tripo3d/cli), ElevenLabs hosted MCP
```

## The skills

| Skill | Your category | Load when |
|---|---|---|
| [game-dev-index](.claude/skills/game-dev-index/SKILL.md) | — | Start of any game work: the router + baseline setup |
| [game-direction-workflow](.claude/skills/game-direction-workflow/SKILL.md) | workflow | New game or big feature: brief, kickoff prompt, agent waves + critics, critique prompts, /goal, guard rails |
| [game-engine-choice](.claude/skills/game-engine-choice/SKILL.md) | workflow | three.js vs custom WebGPU vs Godot (no MCP) vs Unity vs Unreal vs Phaser |
| [game-design-fun](.claude/skills/game-design-fun/SKILL.md) | workflow | Core loop, game feel, balance/fairness, progression, playtests |
| [game-world-generation](.claude/skills/game-world-generation/SKILL.md) | **1. World generation** | Terrain, noise, erosion, biomes, rivers, scattering, streaming, terrain LOD, heightfield colliders |
| [game-building-generation](.claude/skills/game-building-generation/SKILL.md) | **2. Building generation** | Buildings, towns, interiors, modular kits, WFC, BSP/caves, lock-and-key dungeons + solvers, templates + decay |
| [game-physics](.claude/skills/game-physics/SKILL.md) | **3. Physics** | Rapier/Jolt/Box2D choice, fixed step, character controller, vehicles, ragdolls, convex decomposition, determinism, workers |
| [game-materials-shaders](.claude/skills/game-materials-shaders/SKILL.md) | **4. Materials** | PBR + colour management, TSL vs GLSL, triplanar, splat terrain, toon, water/sky/fog, foliage wind, post-processing |
| [game-particles-vfx](.claude/skills/game-particles-vfx/SKILL.md) | **5. Particles & effects** | three.quarks/nebula/Effekseer, TSL compute particles, trails, flipbooks, explosions/magic/weather, overdraw |
| [game-voxels](.claude/skills/game-voxels/SKILL.md) | **6. Voxels** | Chunks, culled vs greedy meshing, AO, light propagation, workers, smooth voxels/transvoxel, .vox, destructible terrain |
| [game-npc-ai](.claude/skills/game-npc-ai/SKILL.md) | **7. NPCs** | Navmesh, crowds, steering, A*/flow fields, FSM/BT/utility/GOAP, perception, sim routines, LLM dialogue, ECS |
| [game-characters-animation](.claude/skills/game-characters-animation/SKILL.md) | **8. Characters** | Character sources, AnimationMixer blending, retargeting, IK, third-person controller + camera, mocap, 2D paper-doll |
| [game-performance-optimization](.claude/skills/game-performance-optimization/SKILL.md) | **9. Mesh optimisation / perf** | Profiling first; draw calls → instancing/batching; LOD (meshoptimizer); KTX2/meshopt; impostors; culling; budgets |
| [game-asset-pipeline](.claude/skills/game-asset-pipeline/SKILL.md) | assets | Procedural → CC0 → Blender scripts → Tripo/Meshy → blender-mcp cleanup; licences, folder layout |
| [game-art-review](.claude/skills/game-art-review/SKILL.md) | quality | References, deterministic captures, critic agent, common "AI-look" defects |
| [game-debug-tools](.claude/skills/game-debug-tools/SKILL.md) | quality | `window.__game` hooks, `render_game_to_text`, debug panels (ImGui-style), Playwright play harness |
| [game-audio](.claude/skills/game-audio/SKILL.md) | extra | Howler/Tone/ZzFX, spatial audio, adaptive music, ElevenLabs/Suno, unlocking audio on first input |
| [game-2d](.claude/skills/game-2d/SKILL.md) | extra | Phaser 4/Pixi 8/KAPLAY/Excalibur, Tiled/LDtk, pixel-art pipeline, AI sprites, 2D physics |
| [game-multiplayer](.claude/skills/game-multiplayer/SKILL.md) | extra | Colyseus/PartyServer/Geckos/Nakama, prediction + reconciliation, rollback, lockstep, Cloudflare |

## What the viral games actually did (from the authors' own replies)

| Game | Stack | How |
|---|---|---|
| [Noah Wachnik, Minecraft test](https://x.com/noahwachnik/status/2102470200415166699) | three.js | 2-sentence prompt, **max effort**, 1h37m, ~$15 |
| [Danny Limanseta, Wildbrush](https://wildbrush.vercel.app) | three.js, vanilla Claude Code, no skills | "make a zelda-like game with paintbrush as a weapon … **wow me and use up to 6 agents**", then a day of iterating with his kids. Extra High effort, ~30% of the weekly limit |
| [aipulsedaily, Forest-like island](https://x.com/aipulseda1ly/status/2103352533657719230) | three.js, CC0 textures + recorded sound | **"waves of agents, one per domain, and critic agents that never touched the build checked the renders against real photos"**. Offline erosion baked in; far trees as baked billboards; 50 subagents for the texture pass |
| [Dan Greenheck, Tidewater](https://github.com/dgreenheck/tidewater) | custom WebGPU/TSL, Poly Haven, Rocketbox | Max effort + parallel agents. [Full prompt list](https://x.com/dangreenheck/status/2102911556296052788): one feature-list kickoff, then dozens of picky critique prompts. $1,874 |
| [Graalito, Relic Eater](https://x.com/graalitoo/status/2102185653492764873) | **Godot, no MCP** (edits .tscn, runs the game for screenshots), Tripo, Mixamo, map in Blender, ElevenLabs | One prompt into his own "gauntlet loop" skill (10+ private skills), 7h. "SFX are the real bottleneck now" |
| [cryptomanavan, Lagoon Tree Village](https://lagoon-tree-village.netlify.app) | three.js only, "no assets no blender" | 2-part prompt + his own concept drawings; no skills, no /goal; ran 7h straight. Later: DYSTOPIA, 42 km², 37k procedural buildings, 2.9M rooms (WebGPU + TSL) |
| [Chris DiMarco, Highrise Dynasty](https://x.com/chrisjdimarco/status/2104231102412108258) | Canvas 2D + three.js | **"Every room, person and vehicle is generated by Python scripts in Blender."** Deterministic sim, being ported to C# for Unity/Steam |
| [The_Alex, "Dark Souls"](https://x.com/The_Alex/status/2102440678282412195) | **Unreal + Opus 5.5** | "Literally just asked it to make a dark souls game in unreal" |
| [rewind, cozy fishing](https://x.com/rewind02/status/2109585491280453986) | Tripo P2 → blender-mcp → Unity 6 | Every tool has one job: Opus sets up and animates models in Blender; systems are added one at a time; play after every step |
| [Simon, Halloween game](https://x.com/iced_coffee_dev/status/2102880675645071652) | three.js → custom WebGPU renderer (Codex) | Modified Quaternius assets + procedural grass; the custom renderer removed frame spikes and load time |
| [Chetan, moonlit swamp](https://x.com/chetanankola/status/2101595285675970853) | three.js + some Blender | "not a one shot … lots of reference images … and steering". Took several days, mostly Opus |
| Sindre, Topside | three.js voxel engine (Cavecraft) | Brief per feature, phased steps with independent review, seeded generators + a solver over 256 seeds |
| [Andreas, Fuse Bombers](https://github.com/andreavs/fuse-bombers) | Phaser 4 + TS | Issue → implementer agent → reviewer agent; AGENTS.md; a deterministic engine/render split enforced by a test |

Common pattern:
- **Set a quality bar and give references.**
- **Separate builders from critics.**
- **Be very picky in the critique prompts.**
- **Keep the simulation deterministic and separate from rendering.**
- **Reach for procedural code and CC0 before AI meshes.**
- **The agent must see the game:** screenshots, state hooks, debug panels.

From the Rimsha thread replies: "agents get a lot more useful when they can see the state they're changing instead of guessing from logs" (the Dear ImGui tip). Also: "meshoptimizer is what everyone skips". And: Godot now ships Jolt built in.

## Tool catalog (about 130 tools, grouped by your 9 categories plus extras)

Stars and licences checked on 31 Oct 2026. "When to use" is the short version; the matching skill has the full decision table.

### 1. World generation

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| FastNoise2 | https://github.com/Auburn/FastNoise2 | 1.4k | MIT | C++/WASM | SIMD node-graph noise + visual editor | native engines |
| FastNoiseLite (fastnoise-lite) | https://github.com/Auburn/FastNoiseLite | 3.5k | MIT | JS/C#/GLSL/Godot built-in | cellular, domain warp | CPU noise beyond simplex |
| simplex-noise | https://github.com/jwagner/simplex-noise.js | 1.9k | MIT | JS | 2D–4D simplex (v4 needs a PRNG) | default CPU noise |
| Terrain3D | https://github.com/TokisanGames/Terrain3D | 4.6k | MIT | Godot 4.4–4.6 | clipmap terrain, heightmap import, foliage | Godot terrain |
| Gaea | https://github.com/gaea-godot/gaea | 1.6k | MIT | Godot | graph-based grid/tile generation | 2D/grid worlds |
| three-geospatial | https://github.com/takram-design-engineering/three-geospatial | 1.7k | MIT | three.js | physically based atmosphere + clouds | sky |
| SebLague/Hydraulic-Erosion | https://github.com/SebLague/Hydraulic-Erosion | 1.0k | MIT | Unity | droplet erosion reference | porting erosion |
| Webgl-Erosion | https://github.com/LanLou123/Webgl-Erosion | 318 | MIT | WebGL | GPU grid erosion | GPU erosion reference |
| terrain-erosion-3-ways | https://github.com/dandrino/terrain-erosion-3-ways | 947 | MIT | Python | erosion methods compared | choosing a method |
| CDLOD | https://github.com/fstrugar/CDLOD | 240 | MIT | paper + DX9 | quadtree terrain LOD | huge terrains |
| ez-tree | https://github.com/dgreenheck/ez-tree | 1.7k | MIT | three.js | seeded procedural trees | vegetation |

### 2. Building, town and dungeon generation

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| WaveFunctionCollapse | https://github.com/mxgmn/WaveFunctionCollapse | 25.3k | check | C# | original WFC | reference |
| kchapelier/wavefunctioncollapse | https://github.com/kchapelier/wavefunctioncollapse | 526 | MIT | JS | 2D WFC port | quick 2D |
| DeBroglie | https://github.com/BorisTheBrave/DeBroglie | 526 | MIT | C# | constrained WFC with backtracking | Unity/C# |
| marian42 WFC city | https://github.com/marian42/wavefunctioncollapse | 4.9k | MIT | Unity | infinite 3D WFC city | 3D module reference |
| godot-constraint-solving | https://github.com/AlexeyBond/godot-constraint-solving | 466 | MIT | Godot 4 | WFC on TileMap/GridMap | Godot WFC |
| Sylves | https://github.com/BorisTheBrave/sylves | 229 | check | C# | irregular (Townscaper-like) grids | organic grids |
| Edgar-Unity / Edgar.Godot | https://github.com/OndrejNepozitek/Edgar-Unity · https://github.com/RickyYCheng/Edgar.Godot | 910 / ? | MIT (no resale) | Unity/Godot | graph → room-template layouts | room-template dungeons |
| rot.js | https://github.com/ondras/rot.js | 2.7k | BSD-3 | JS | roguelike maps, FOV, pathing | 2D roguelikes |
| ProbableTrain/MapGenerator | https://github.com/ProbableTrain/MapGenerator | 1.4k | LGPL/GPL-3 | TS | city street networks | reference only |
| watabou/TownGeneratorOS | https://github.com/watabou/TownGeneratorOS | 1.9k | GPL-3 | Haxe | medieval towns | reference only |
| Kenney / KayKit modular kits | https://kenney.nl · https://kaylousberg.itch.io | n/a | CC0 | any | modular building/dungeon pieces | kit assembly |

### 3. Physics

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Rapier (rapier3d / -compat / rapier2d / -deterministic) | https://github.com/dimforge/rapier | 6.0k | Apache-2 | three.js, 2D web | rigid bodies, character controller, raycast vehicle, CCD, snapshots | default web physics |
| react-three-rapier | https://github.com/pmndrs/react-three-rapier | 1.5k | MIT | R3F | declarative Rapier (pins 0.19.2) | R3F apps |
| JoltPhysics / JoltPhysics.js | https://github.com/jrouwe/JoltPhysics · https://github.com/jrouwe/JoltPhysics.js | 11.8k / 576 | MIT | C++, web; built into Godot 4.4+ | AAA-grade physics: ragdolls, vehicles, CharacterVirtual | heavy 3D, large worlds, Godot default for 3D |
| box2d3-wasm | https://github.com/Birch-san/box2d3-wasm | 66 (Box2D 10.5k) | MIT | 2D web | Box2D v3 in WASM | serious 2D physics |
| planck.js | https://github.com/piqnt/planck.js | 5.3k | MIT | 2D web | pure-JS Box2D | 2D without WASM |
| Matter.js | https://github.com/liabru/matter-js | 18.4k | MIT | 2D web | simple 2D | toys only |
| @babylonjs/havok | https://www.npmjs.com/package/@babylonjs/havok | n/a | MIT | Babylon | Havok physics | Babylon projects |
| cannon-es / ammo.js | https://github.com/pmndrs/cannon-es · https://github.com/kripken/ammo.js | 2.1k / 4.6k | MIT / zlib | web | legacy | avoid for new work |
| CoACD | https://github.com/SarahWeiii/CoACD | ~1.1k | MIT | offline | convex decomposition | dynamic concave meshes |
| V-HACD | https://github.com/kmammou/v-hacd | 1.7k | BSD-3 | built into Rapier | convex decomposition | quick in-engine |
| godot-rapier-physics | https://github.com/appsinacup/godot-rapier-physics | 1.0k | MIT | Godot | deterministic physics | Godot rollback |
| Box3D | https://github.com/erincatto/box3d | 6.5k | MIT | C17/WASM | new 3D physics by Box2D author (alpha) | evaluate |

### 4. Materials, shaders, sky and water

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| three.js r187 | https://github.com/mrdoob/three.js | 117.2k | MIT | web | WebGPURenderer, TSL, RenderPipeline | default |
| Material Maker | https://github.com/RodZill4/material-maker | 6.0k | MIT | tool | procedural PBR materials (free Substance Designer alternative) | tileable textures |
| pmndrs/postprocessing | https://github.com/pmndrs/postprocessing | 2.8k | Zlib | three WebGL | effect composer | WebGL post |
| N8AO | https://github.com/N8python/n8ao | 492 | CC0 | three WebGL | ambient occlusion | WebGL AO |
| three-custom-shader-material | https://github.com/FarazzShaikh/THREE-CustomShaderMaterial | 1.3k | MIT | three WebGL | extend stock PBR materials | GLSL tweaks keeping PBR |
| web-ocean | https://github.com/Noniv/web-ocean | 908 | MIT | WebGPU | 6-cascade FFT ocean | ocean reference |
| GodotOceanWaves | https://github.com/2Retr0/GodotOceanWaves | 3.3k | MIT | Godot | FFT ocean | Godot oceans |
| @pmndrs/sky | https://www.npmjs.com/package/@pmndrs/sky | 3 | MIT | three WebGPU | Hillaire sky + haze | drop-in sky (new) |
| webgpu-sky-atmosphere | https://github.com/JolifantoBambla/webgpu-sky-atmosphere | 35 | MIT | WebGPU | Hillaire LUTs | custom engines |
| sebh/UnrealEngineSkyAtmosphere | https://github.com/sebh/UnrealEngineSkyAtmosphere | 855 | MIT | reference | Hillaire 2020 reference code | learning |
| Quick_Grass | https://github.com/simondevyoutube/Quick_Grass | 224 | MIT | three | grass with wind | foliage reference |
| DeepBump | https://github.com/HugoTini/DeepBump | 1.4k | GPL-3 | tool | image → normal/height | offline only |
| ArmorPaint | https://github.com/armory3d/armorpaint | – | zlib source; paid binaries | tool | mesh painting (Substance Painter alternative) | texture painting |
| Poly Haven / ambientCG | https://polyhaven.com · https://ambientcg.com | – | CC0 | assets | HDRIs, PBR textures, scanned models | always first |

### 5. Particles and effects

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| three.quarks | https://github.com/Alchemist0823/three.quarks | 1.0k | MIT | three (WebGL only) | particle systems + quarks.art editor | authored effects on WebGLRenderer |
| three-nebula | https://github.com/creativelifeform/three-nebula | 1.2k | MIT | three WebGL + /webgpu | emitter/behaviour particles | CPU particle API on WebGPU |
| Effekseer | https://github.com/effekseer/Effekseer | 1.8k | MIT | editor + runtimes | anime/stylised effect authoring | artist/premade effects |
| EffekseerForWebGL / EffekseerForGodot4 / EffekseerForUnity | https://github.com/effekseer/EffekseerForWebGL · https://github.com/effekseer/EffekseerForGodot4 · https://github.com/effekseer/EffekseerForUnity | 136 / 114 / 65 | MIT | three WebGL / Godot 4.6+ / Unity | Effekseer runtimes | Effekseer in your engine |
| kanaworks-sakuga-vfx | https://github.com/kanaworksai-hue/kanaworks-sakuga-vfx | 4 | custom asset licence (<$1M revenue, credit) | web/Godot/Unity | 90 hand-drawn anime flipbooks | sakuga look, small studios |
| effekseer-vfx-skill | https://github.com/kanaworksai-hue/effekseer-vfx-skill | 0 | MIT | Claude plugin | agent-authored Effekseer effects | many effect variants |
| THREE.MeshLine | https://github.com/spite/THREE.MeshLine | – | MIT | three WebGL | trails/lines | projectile trails |

### 6. Voxels

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Zylann/godot_voxel | https://github.com/Zylann/godot_voxel | 3.9k | MIT | Godot | blocky + smooth voxels, streaming, VoxelTool | Godot voxels |
| voxelize | https://github.com/voxelize/voxelize | 686 | MIT | three.js + Rust | multiplayer voxel engine | web multiplayer voxels |
| noa / DivineVoxelEngine | https://github.com/fenomas/noa · https://github.com/Divine-Star-Software/DivineVoxelEngine | 688 / 264 | MIT | Babylon.js | voxel engines | Babylon only |
| binary-greedy-meshing | https://github.com/cgerikj/binary-greedy-meshing | 312 | MIT | C++ | 64³ bitmask meshing | native engines |
| Transvoxel | https://github.com/EricLengyel/Transvoxel | ? | MIT | any | seamless smooth-voxel LOD tables | smooth voxel LOD |
| vox-reader | https://www.npmjs.com/package/vox-reader | n/a | MIT | JS | read MagicaVoxel .vox | importing .vox |

### 7. NPCs, navigation, AI and ECS

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| recastnavigation | https://github.com/recastnavigation/recastnavigation | 8.0k | zlib | C++ core | navmesh + DetourCrowd | underlies bindings |
| recast-navigation-js | https://github.com/isaac-mason/recast-navigation-js | 428 | MIT | three.js | navmesh + crowds | default web navmesh |
| navcat | https://github.com/isaac-mason/navcat | 284 | MIT | web | pure-JS navmesh | no-WASM option |
| three-pathfinding | https://github.com/donmccurdy/three-pathfinding | 1.4k | MIT | three.js | simple navmesh A* | small static levels |
| Yuka | https://github.com/Mugen87/yuka | 1.4k | MIT | three.js | steering, FSM, perception | prototypes (last release 2022) |
| mistreevous / BehaviorTree.js | https://github.com/nikkorn/mistreevous · https://github.com/Calamari/BehaviorTree.js | 143 / 339 | MIT | web | behaviour trees | web BT |
| LimboAI / Beehave | https://github.com/limbonaut/limboai · https://github.com/bitbrain/beehave | 3.1k / 3.3k | MIT | Godot | BT + HSM | Godot AI |
| miniplex / becsy | https://github.com/hmans/miniplex · https://github.com/lastolivegames/becsy | 1.1k / 299 | MIT / check | web | object ECS / multithreaded ECS | mid-size games |
| bitECS / koota / flecs | https://github.com/NateTheGreatt/bitECS · https://github.com/pmndrs/koota · https://github.com/SanderMertens/flecs | 1.5k/746/8.7k | MPL-2/ISC/MIT | web/web/C | ECS | many entities |

### 8. Characters and animation

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Quaternius Universal Animation Library 1+2 | https://quaternius.com/packs/universalanimationlibrary.html | n/a | CC0 | any | 250+ clips on one humanoid rig | prototypes, stylised |
| KayKit Character Animations | https://kaylousberg.itch.io/kaykit-character-animations | n/a | CC0 | any | 133 clips + rigs | stylised casts |
| Microsoft Rocketbox | https://github.com/microsoft/Microsoft-Rocketbox | ~645 | MIT | any (FBX) | 115 realistic avatars, 417 animations | realistic humans |
| Mixamo | https://www.mixamo.com | n/a | free, Adobe ToS | any | auto-rig + mocap library (no API) | rigging your own humanoid |
| Meshy rigging/animation API | https://docs.meshy.ai/api/rigging-and-animation | n/a | paid | any | auto-rig + ~480 motions | AI humanoids |
| Tripo rig/retarget | https://developers.tripo3d.com/en/docs/animations-rig | n/a | paid | any | rigs incl. quadruped/serpent | AI creatures |
| pixiv/three-vrm | https://github.com/pixiv/three-vrm | ~2.2k | MIT | three.js | VRM avatars, WebGPU MToon | anime avatars (Ready Player Me shut down Jan 2026) |
| ahujasid/blender-mcp | https://github.com/ahujasid/blender-mcp | ~32.5k | MIT | Blender | agent-driven modelling, cleanup, rig, export; Poly Haven/Sketchfab/Rodin/Hunyuan | asset setup + cleanup |
| ozz-animation | https://github.com/guillaumeblanc/ozz-animation | ~2.9k | MIT | C++/WASM | SIMD sampling, blending, IK | custom engines, crowds |
| gkjohnson/closed-chain-ik-js | https://github.com/gkjohnson/closed-chain-ik-js | ~323 | Apache-2 | three.js | general IK | mechs/robots |
| sharpen3d/openvat | https://github.com/sharpen3d/openvat | ~288 | GPL-3 tool / MIT runtime | Blender→engines | vertex animation textures | crowds |
| LPC Spritesheet Generator | https://github.com/LiberatedPixelCup/Universal-LPC-Spritesheet-Character-Generator | ~1.6k | GPL-3 code; art CC0–GPL | 2D | paper-doll layered equipment | 2D equipment layering |
| freemocap | https://github.com/freemocap/freemocap | ~10.6k | AGPL-3 | offline | markerless mocap | free capture |
| GVHMR | https://github.com/zju3dv/GVHMR | ~1.9k | non-commercial | offline | video→motion | research only |
| aldegad/sprite-gen | https://github.com/aldegad/sprite-gen | (unverified) | ? | 2D | character image → animation frames + atlas, a Claude skill | 2D character animation |

### 9. Mesh optimisation and performance

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| agargaro/instanced-mesh | https://github.com/agargaro/instanced-mesh | ~460 | MIT | three.js | instancing with per-instance culling, BVH, LOD, skinning | thousands of objects/NPCs |
| agargaro/octahedral-impostor | https://github.com/agargaro/octahedral-impostor | ~66 | MIT (WIP) | three.js | octahedral impostors | far vegetation |
| gkjohnson/three-mesh-bvh | https://github.com/gkjohnson/three-mesh-bvh | ~3.5k | MIT | three.js | fast raycast/shapecast | picking, ground probes |
| zeux/meshoptimizer + gltfpack | https://github.com/zeux/meshoptimizer | ~8.3k | MIT | any/web | simplify, compress, LOD | always |
| glTF-Transform | https://github.com/donmccurdy/glTF-Transform | ~2.0k | MIT | glTF | inspect/optimize/simplify/KTX2 | every GLB before shipping |
| stats-gl | https://github.com/RenaudRohlinger/stats-gl | ~283 | MIT | three.js | FPS/CPU/GPU panels (for WebGPU r181+ use renderer.inspector) | dev builds |
| Spector.js | https://github.com/BabylonJS/Spector.js | ~1.6k | MIT | WebGL | frame capture | draw/state debugging |
| webgpu_inspector | https://github.com/brendan-duncan/webgpu_inspector | ~372 | MIT | WebGPU | capture + GPU-bound plot | WebGPU debugging |
| @needle-tools/gltf-progressive | https://www.npmjs.com/package/@needle-tools/gltf-progressive | n/a | MIT | three.js | progressive mesh/texture streaming | large scenes |
| dgreenheck/tidewater | https://github.com/dgreenheck/tidewater | ~1.4k | MIT | custom WebGPU | FFT ocean, impostors, dynamic resolution | reference implementation |

### 2D

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Phaser 4.2.1 | https://github.com/phaserjs/phaser | 40.5k | MIT | web 2D | 2D framework; ships its own agent skills | default 2D web |
| PixiJS 8.22 + pixijs-skills | https://github.com/pixijs/pixijs · https://github.com/pixijs/pixijs-skills | 48.0k / 356 | MIT | web 2D | WebGPU/WebGL 2D renderer | custom 2D architecture |
| KAPLAY | https://github.com/kaplayjs/kaplay | 1.8k | MIT | web 2D | tiny game lib | jams |
| Excalibur | https://github.com/excaliburjs/Excalibur | 2.4k | BSD-2 | web 2D | TS engine with Tiled/LDtk plugins | TS-first 2D |
| debuff (spryte, keiko…) | https://github.com/debuffjp/debuff | 2 | MIT | web | agent-first 2D toolkit; keiko runs web games deterministically in Node (alpha) | headless tests |
| LittleJS | https://github.com/KilledByAPixel/LittleJS | 4.4k | MIT | web 2D | tiny engine | size-limited games |
| Tiled / LDtk | https://github.com/mapeditor/tiled · https://github.com/deepnight/ldtk | 13.0k / 4.3k | GPL-2 (app) / MIT | all | tilemap / level editors | always for 2D levels |
| Aseprite | https://github.com/aseprite/aseprite | 39.5k | EULA | all | pixel art + CLI | sprite export (CI needs licensed binary) |
| PixelLab / Retro Diffusion | https://pixellab.ai · https://github.com/Retro-Diffusion/api-examples | – | paid | all | AI pixel art, hosted MCPs, palette forcing | sprites/tilesets |

### Audio

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Howler.js | https://github.com/goldfire/howler.js | 25.4k | MIT | web | audio playback | simple SFX |
| Tone.js | https://github.com/Tonejs/Tone.js | 14.7k | MIT | web | synth/music | procedural music |
| ZzFX / jsfxr | https://github.com/KilledByAPixel/ZzFX · https://github.com/chr15m/jsfxr | 795 / 460 | MIT / Unlicense | web | procedural SFX | placeholder/retro SFX |
| ElevenLabs MCP | https://github.com/elevenlabs/elevenlabs-mcp | 1.5k | MIT | any | SFX/music/voice; use hosted MCP | realistic SFX |

### Debugging and agent loops

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| lil-gui / Tweakpane / leva | https://github.com/georgealways/lil-gui · https://github.com/cocopon/tweakpane · https://github.com/pmndrs/leva | 1.6k/4.6k/6.2k | MIT | web/R3F | tuning panels | always |
| Dear ImGui / jsimgui / imgui-godot | https://github.com/ocornut/imgui · https://github.com/mori2003/jsimgui · https://github.com/pkdawson/imgui-godot | 76k/75/877 | MIT | C++/web/Godot | live debug inspectors | agents see state |
| Playwright MCP | https://github.com/microsoft/playwright-mcp | 38.9k | Apache-2 | web | drive and screenshot the game | always |
| Chrome DevTools MCP | https://github.com/ChromeDevTools/chrome-devtools-mcp | 52.9k | Apache-2 | web | perf traces, heap, console | perf work |
| godot-mcp / unity-mcp | https://github.com/Coding-Solo/godot-mcp · https://github.com/CoplayDev/unity-mcp | 6.3k/15.5k | MIT | Godot/Unity | editor control | engine loops |

### Multiplayer

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| Colyseus 0.18 | https://github.com/colyseus/colyseus | 7.5k | MIT | web | rooms + built-in prediction | realtime action |
| PartyServer | https://github.com/cloudflare/partykit | 1.3k | ISC | web/Cloudflare | Durable Object rooms (hosted PartyKit shut down Oct 23 2026) | turn-based/party |
| Geckos.io | https://github.com/geckosio/geckos.io | 1.5k | BSD-3 | web | UDP over WebRTC | fast action |
| Nakama | https://github.com/heroiclabs/nakama | 13.3k | Apache-2 | all | accounts, matchmaking | meta services |
| netfox | https://github.com/foxssake/netfox | 1.1k | MIT | Godot | rollback/prediction | Godot online |
| NetplayJS | https://github.com/rameshvarun/netplayjs | 575 | ISC | web | P2P rollback | fighting/party |
| Trystero | https://github.com/dmotz/trystero | 2.7k | MIT | web | serverless WebRTC | jams |
| Photon Fusion 2 / Quantum 3 | https://www.photonengine.com | – | commercial | Unity | netcode SaaS | Unity PvP |
| Multiplayer-LiveOps-Toolkit | https://github.com/Alchemishty/Multiplayer-LiveOps-Toolkit | 2 | MIT | Unity | wallet/shop/cosmetics reference | reference only |

### Engines, frameworks and Unreal

| Tool / repo | URL | ★ | Licence | Engine | What it is | When to use |
|---|---|---|---|---|---|---|
| s&box | https://github.com/Facepunch/sbox-public | 6.4k | MIT (+EULA bins) | Source 2/.NET | open-sourced sandbox engine | C# sandbox games |
| The Forge | https://github.com/ConfettiFX/The-Forge | 5.6k | Apache-2 | C++ | cross-platform rendering framework | custom native engines |
| openfluke/loom (welvet) | https://github.com/openfluke/loom | 135 | Apache-2 | WASM/WebGPU | deterministic NN VM | niche: in-game neural nets |
| Unreal MCPs | https://github.com/chongdashu/unreal-mcp · https://github.com/flopperam/unreal-engine-mcp | 2.1k/1.1k | MIT | UE5 | editor control | Unreal |
| Epic Unreal plugin for Claude Code | https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin | (Epic org; needs linked account) | Epic | UE5 | official Blueprints/materials/FX/anim/UI/testing tools | Unreal default |
| UE 5.8 built-in MCP server | https://www.unrealengine.com | – | Epic | UE 5.8 | experimental built-in MCP (Jun 2026) | Unreal |
| quodsoler/unreal-engine-skills | https://github.com/quodsoler/unreal-engine-skills | 335 | MIT | UE | 27 skills | Unreal |
| PlayableIntelligence/game-creator | https://github.com/PlayableIntelligence/game-creator | 349 | MIT | multi | skill pack | browse |
| thedivergentai/gd-agentic-skills | https://github.com/thedivergentai/gd-agentic-skills | 763 | ? | Godot | Godot skill pack | Godot |
| emalorenzo/three-agent-skills | https://github.com/emalorenzo/three-agent-skills | ? | ? | three/R3F | R3F skills (from Snyk Top 8) | browse |

### Skill packs, agent harnesses and AI-native game tools

| Repo | What it is | Verdict |
|---|---|---|
| [CloudAI-X/threejs-skills](https://github.com/CloudAI-X/threejs-skills) (~3.7k★) | 13 three.js API skills for r186+, each with a "Common mistakes" list | **Install** |
| [dgreenheck/webgpu-claude-skill](https://github.com/dgreenheck/webgpu-claude-skill) (~1.3k★) | TSL, compute, WGSL, post-processing, from Tidewater's author | **Install** for WebGPU |
| [majidmanzarpour/threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills) (~2.6k★) | Game director router, gameplay, graphics, UI, QA with Playwright templates | **Install**, trim the paid generators |
| [gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) (~5.3k★) | 77 skills: Godot, Unity, Unreal, Phaser, three.js, Bevy, Roblox; with a router | **Cherry-pick**: `game-feel`, `audio-design`, engine skills |
| [htdt/godogen](https://github.com/htdt/godogen) (~7.2k★) | Describe a game; the agent writes code, generates assets, runs the engine and checks via live game or recording (Godot, Bevy, Babylon.js, three.js) | **Learn from**: its capture/verify loop and `engines/threejs.md` |
| [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) (~27–30k★) | 49 agents organised as a studio | **Learn from**: too heavy; its own test found a 1-page brief beat 30 docs |
| [jame581/GodotPrompter](https://github.com/jame581/GodotPrompter) (781★) | 56 Godot 4 skills; validator against invented APIs | **Install** for Godot |
| [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | AI working inside the Godot editor: scenes, scripts, signals, animations, particles | Try if you want editor control *(unverified stars)* |
| [EpicGames/unreal-engine-skills-for-claude-code-plugin](https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin) | Epic's official Claude Code plugin: Blueprints, materials, FX, animation, UI, testing | **Install** for Unreal (needs an Epic-linked GitHub account) |
| [aldegad/sprite-gen](https://github.com/aldegad/sprite-gen) | Claude skill: one character image becomes animation states, transparent frames and an atlas, with a review UI | Try for 2D characters |
| [OpenDCAI/GameFactory-3A](https://github.com/OpenDCAI/GameFactory-3A) | Asset generation (images, 3D, motion, audio, cinematics) connected to game construction | Watch *(unverified)* |
| [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) | 7 skills: story → mechanics → world → art direction → build → test | Learn from it for narrative games |
| [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) | AI game-dev workspace: agents, Blender assets, publish to web (early, macOS/Linux) | Watch |
| [obra/superpowers](https://github.com/obra/superpowers) | `subagent-driven-development`, `writing-plans` | Use for the issue → implement → review loop |
| [anthropics/skills](https://github.com/anthropics/skills) | `skill-creator`, `webapp-testing`, `frontend-design` (HUD/menus) | Use `skill-creator` to evolve these skills |

### AI asset generation and MCP servers

| Tool | Job | Notes |
|---|---|---|
| [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) (~32k★) | Agent drives Blender: setup, cleanup, rig, animate, export; Poly Haven, Sketchfab, Rodin and Hunyuan built in | Keep on localhost; set `DISABLE_TELEMETRY=true`; split long jobs |
| Blender headless scripts (`blender -b -P x.py`) | Reproducible procedural assets in the repo | The Highrise Dynasty approach; no MCP needed |
| Tripo (Smart Mesh P2, [CLI skill](https://www.npmjs.com/package/@tripo3d/cli)) | Clean quad low-poly props and characters (≤25k faces) | Used by Graalito and rewind. The MCP is alpha |
| [Meshy MCP](https://github.com/meshy-dev/meshy-mcp-server) | Text/image→3D, retexture, remesh, **rig + animate** | Pro plan |
| [TRELLIS.2](https://github.com/microsoft/TRELLIS.2) | Self-hosted image→3D (MIT) | 24GB GPU |
| Hunyuan3D | Image→3D | Licence excludes the EU/UK/South Korea; check EEA/Norway |
| Higgsfield MCP | Image/multi-image→3D, rigging, game platform | All-in-one; demos were criticised as unplayable |
| [Context7](https://github.com/upstash/context7) | Current three.js/Godot docs | Add a rule in CLAUDE.md to use it |
| [Playwright MCP](https://github.com/microsoft/playwright-mcp) / [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp) | See and play the game; profile it | Essential |
| [Unity MCP (CoplayDev)](https://github.com/CoplayDev/unity-mcp) / [godot-mcp](https://github.com/Coding-Solo/godot-mcp) | Editor control | For Godot, prefer no MCP first |

## Version traps (things the model will get wrong)

- **three.js r18x:**
  - `PostProcessing` → `RenderPipeline`; `RGBELoader` → `HDRLoader`; `Clock` → `Timer`.
  - `renderAsync`/`computeAsync` are deprecated: use `await renderer.init()`.
  - stats-gl doesn't support WebGPU in r181+: use `renderer.inspector`.
  - `VOXLoader` returns `{chunks, scene}`. `BatchedMesh` requires all geometries indexed or all non-indexed.
  - Both water addons export a class named `WaterMesh`.
- **WebGPURenderer on the WebGL2 fallback** emulates compute with transform feedback: scatter writes (`element(i).assign`) land in the wrong slot. Write gather-style.
- **WebGL-only libraries:**
  - three.quarks, pmndrs/postprocessing (peer range excludes r187), N8AO, MeshLine, CSM and EffekseerForWebGL.
  - On WebGPU use TSL compute, three-nebula `/webgpu` and the RenderPipeline.
- **Rapier 0.20/0.21:**
  - New defaults: CCD on against fixed bodies, a velocity cap and new sleep defaults. 0.21 snapshots aren't readable by older versions.
  - react-three-rapier pins 0.19.2.
  - **The heightfield is column-major**: transpose your grid, or the result is 11 m off on asymmetric terrain.
- **Recast:** `walkableRadius/Height/Climb` are in **voxels**, not metres; `addAgent` returns −1 when the crowd is full.
- **Determinism:** `Math.sin/cos/exp/pow` can differ between browsers. Lockstep and rollback need lookup tables or fixed-point maths and Rapier's `-deterministic` build. Never hash GPU output.
- **Libraries:**
  - `fastnoise-lite` 1.1.1 spells it `DomainWrap`.
  - simplex-noise 4 needs a PRNG passed in.
  - 8-bit PNG/`Noise.get_image()` heightmaps terrace; use 16-bit or float data.
- **Services:**
  - The hosted PartyKit platform shut down on 23 Oct 2026: use `partyserver` + wrangler.
  - Colyseus 0.17/0.18 changed most APIs.
  - Phaser 4 removed preFX/postFX, BitmapMask and Mesh.
  - Ready Player Me shut down (Jan 2026): use VRM.
  - Suno has no public API.
  - ElevenLabs' local MCP is deprecated in favour of the hosted one.
- **Compression:**
  - WebP/AVIF save download size, not VRAM; only KTX2 saves VRAM.
  - `gltf-transform optimize` merges named nodes; pass flags to keep the ones gameplay needs.
  - gltfpack can't read Draco.
  - Quantised meshes break runtime simplification and reading collider data from positions.
- **Licences:**
  - kanaworks VFX: credit required, free only under $1M revenue.
  - DeepBump is GPL; Ubisoft CHORD is research-only.
  - Poly Haven's API needs a visible credit.
  - Poly Pizza is mostly CC-BY. GVHMR is non-commercial.

## Gaps and unverified

- "helium" and "amnicad" (recommended in the thread with TRELLIS.2 for asset creation) couldn't be identified.
- Graalito's "gauntlet loop" skill set is private. He describes it as one big skill orchestrating 10+ of his own skills.
- Effekseer node names in Godot/Unity, Effekseer/quarks inside a post chain, and texture sampling in compute on the WebGL2 fallback are marked *(unverified)* in the skills.
- Star counts are from 31 Oct 2026 and approximate.
