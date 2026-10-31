---
name: game-dev-index
description: Entry point for any game development work. Routes to the right game-* skill (direction, engine, world, buildings, voxels, physics, NPCs, characters, materials, VFX, audio, 2D, multiplayer, debug, performance, assets, art review, fun) and lists the MCP servers and external skill packs to have installed. Load first when starting or resuming a game project, or when unsure which game skill applies.
---

# Game dev index

Load the skills relevant to the task; don't load all of them. Always load `game-direction-workflow` when starting a project or a large feature.

| If the task is about… | Load |
|---|---|
| Starting a game, writing the brief, kickoff prompt, agent waves, /goal, keeping a big codebase sane | `game-direction-workflow` |
| Choosing three.js vs custom WebGPU vs Godot vs Unity vs Unreal vs Phaser; Godot without MCP | `game-engine-choice` |
| Making it fun: core loop, game feel, balance, progression, playtests | `game-design-fun` |
| Terrain, noise, erosion, biomes, rivers, scattering, chunk streaming, terrain LOD | `game-world-generation` |
| Buildings, towns, interiors, dungeons, WFC, lock-and-key levels, solvers | `game-building-generation` |
| Minecraft-like or smooth voxel worlds, meshing, voxel lighting, destructible terrain | `game-voxels` |
| Rigid bodies, character controllers, vehicles, ragdolls, determinism, physics workers | `game-physics` |
| NPCs, enemies, navmesh, crowds, behaviour trees/utility/GOAP, sim routines, LLM dialogue, ECS | `game-npc-ai` |
| Characters, rigs, Mixamo/Meshy/Tripo animation, blending, IK, third-person controller, 2D paper-doll | `game-characters-animation` |
| PBR, TSL/GLSL shaders, water, sky, fog, foliage, toon, post-processing, textures | `game-materials-shaders` |
| Particles, explosions, magic, trails, weather, Effekseer, GPU compute particles | `game-particles-vfx` |
| Music, SFX, spatial audio, adaptive music, ElevenLabs/Suno/ZzFX | `game-audio` |
| 2D games: Phaser/Pixi, tilemaps, pixel art pipeline, AI sprites | `game-2d` |
| Online play: Colyseus, PartyServer, WebRTC, prediction, rollback, lockstep | `game-multiplayer` |
| Letting the agent see and play the game: `window.__game` hooks, debug panels, Playwright loops | `game-debug-tools` |
| Low FPS, hitches, load times, draw calls, LOD, meshopt/KTX2, impostors | `game-performance-optimization` |
| Where models/textures/animations come from: procedural, CC0, Blender scripts, Tripo/Meshy, licences | `game-asset-pipeline` |
| "It looks AI-made / not AAA": references, captures, critic agent | `game-art-review` |

## Baseline setup for a game repo

1. **MCP servers:**
   - Playwright MCP (`claude mcp add playwright npx @playwright/mcp@latest`)
   - Chrome DevTools MCP (`npx chrome-devtools-mcp@latest`)
   - Context7 for current docs
   - blender-mcp, only when making assets
2. **External skill packs** (install alongside, don't duplicate):
   - `CloudAI-X/threejs-skills`: version-correct three.js APIs.
   - `dgreenheck/webgpu-claude-skill`: TSL/WebGPU.
   - `majidmanzarpour/threejs-game-skills`: game director, QA and Playwright templates.
   - `gamedev-skills/awesome-gamedev-agent-skills`: game-feel, engine-specific skills.
   - Godot: `jame581/GodotPrompter`. Unreal: Epic's official Claude Code plugin.
3. **Files:**
   - `AGENTS.md` with architecture rules and commands, plus `CLAUDE.md` containing `@AGENTS.md`.
   - `docs/BRIEF.md` and `docs/PROGRESS.md`.
   - `art/refs/`, `CREDITS.md`, and the debug hooks from `game-debug-tools`.
4. **Defaults:** three.js r18x + WebGPU/TSL + Vite + TypeScript; Rapier physics; a deterministic seeded simulation separate from rendering; FPS shown in dev builds.
