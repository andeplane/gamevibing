# X thread notes (scraped via the user's signed-in Playwright browser)

## Tim Jayas (Brawl Stars, Fable 5.2)
- No how-to. A reply claims "it ran for 10 hours". Someone asked "what was the prompt?" and got no answer.

## Ferhat Demir (winter survival, Godot)
- No author replies.
- Replies:
  - The hard part is optimising Meshy/Tripo meshes: flat meshes with excessive tris need optimising for mobile/desktop.
  - Someone prefers Tripo over Meshy.
  - Another dev makes a game with Fable + Astra + Meshy + Godot.

## Graalito, Relic Eater (Godot, Fable 5.1, one prompt, 7h)
- Game: a dark fantasy bullet heaven; you eat bosses and steal a body part + skill.
- Why no MCP: "Fewer tool calls. Faster loop. Cleaner result."
  - "Just editing TSCN files", "no computer use, just pure code with tscn files". "It can generate the TSCN files and run the game for screenshots/tests." "It can open the compiled game."
  - "running the game and taking screenshot is the way to get good environment/art"
- Assets:
  - 3D by Tripo, manual but "automated with my LLM". Map made in Blender.
  - Animations: "Tripo/mixamo/mocap/manual keyframing", mostly Mixamo. He has a new animation workflow (link in a follow-up).
  - VFX: no direction given, "just a reference image". The model came up with the dissolve-on-death shader itself.
  - ElevenLabs for SFX/voice. "SFX are the real bottleneck now."
- "I built a big skill around the gauntlet loop (i'm not the creator of it) that include the use of 10+ skills that i made myself". He took months to set it up, won't share it, no Godot-specific skills. Models: Fable and Opus.
- Advice: make the most advanced prototype possible, because it's easier to polish later.
- Reply from @ephemeralrat: prefers Godot MCP but hard-limits tool calls and uses headless tests instead of screenshot runs.
- Reply from @martinhusler: tscn/tres are plain text; Godot renders headlessly.

## Chetan Ankola: moonlit swamp (three.js, Astra Ultra)
- Built in three.js, runs in the browser on a Mac mini.
- "Some parts blender - it's not a one shot - lots of reference images … and steering".
- Model mix: answered "25% Astra perhaps", so the rest is likely Opus. Says Codex quota drains fast and Opus "drains slower".
- Took several days. Sells the kit on 3ditions.drop3.app ("3D website templates & editable worlds") and promises to share the prompt there.

## Higgsfield samurai demos
- No details in replies. Many call them fake or unplayable ads; "Prompt or it didn't happen".

## The_Alex: Dark Souls (Opus 5.5 early access)
- **Made in Unreal Engine**: "All opus + unreal", "Literally just asked it to make a dark souls game in unreal", "No troll first game first prompt", "I literally went to bed".
- Claims "SIX FULL WORLDS! like 15 bosses".
- Other demos in the thread: Flight Simulator, Mario Maker, Rocket Roguelike.
- Someone asked how the rigging/animation was done; no answer.

## almonk: SkiFree
- No details. A reply links skifree2.com as "a better one".

## Noah Wachnik: Minecraft test
- Exact prompt: "Create a playable version of Minecraft in my browser. Add advanced shaders that make it look as real and beautiful as possible."
- Run: 1h 37m, max effort, about $15 of API spend.
- A reply says Opus made a physics voxel game in 15 min using 7% of the weekly quota on a $20 plan.

## cryptomanavan: Lagoon Tree Village (three.js, Opus 5.5, 7h)
- Live: https://lagoon-tree-village.netlify.app
- "only threejs. no assets no blender", "just three.js no externals". "No skills connected to claude", "No goal".
- He used a "simple 2 part prompt" plus "concepts, ideas and drawings" he made himself, fed to the agent. It ran 7+ hours straight until the 5h limit hit. "everything was architected from the beginning the way it wants it to be."
- He promised to share the "5 min setup/process" and the prompt in a later post. He's on the $200 Max plan.

## Simon (iced_coffee_dev): Halloween game with his kids
- He built the game himself in three.js. Codex built a custom renderer (WebGPU only, "Made no effort to retain webgl compatibility") as a drop-in replacement for three.js. Result: no perf spikes and near-instant load. Cost was about 3–5% of a weekly Codex quota.
- His "design" was loose: "here's how roughly I want it to work, you got any questions? ok I have to go".
- Assets: lightly or heavily modified free @quaternius assets; some procedural, e.g. grass; lighting tweaks on the trees.
- Lesson: three.js is great for exploring; once you know the exact use case, a custom renderer is now cheap.
- Reply tip: compare frame-time percentiles, not average FPS.

## Oguz: 50 games in 24h
- Platform: play.benerits.com / benelgames.com; also beneloil.com, a gas-station management game.
- Stack: three.js ("threejs hocam"). "Claude did everything." Planning H5 ads. No GDD, just prompts.

## Dan Greenheck: Tidewater prompt list (x.com/dangreenheck/status/2102911556296052788)
- Settings: "Max, parallel agents".
- The first prompt is a long feature list. Opening: "build me an ocean simulation with three.js … best AAA experience … Unreal level graphics".
- Feature list: 3-cascade FFT ocean; air/water transition with droplets; caustics; reflection/refraction/SSS/backscatter/Beer's law; shoreline breaking waves + foam + spray; beach wetness; wake sim; drivable boat with physics + bow spray; fishing village + pier; 1st/3rd person + boarding the boat; dynamic physically based sky + volumetric clouds; ACES tonemapping; AO and shadows; reef + swimming; underwater fog; waterline "glass edge" shader; sun glints; foam from wave compression; tuning UI.
- The first prompt also included:
  - "identify any other features i may have missed"
  - "optimize performance … target is 60fps"
  - "use webgpu and TSL"
- Follow-up prompts:
  - "seek out whatever references and research you need"
  - "iterate on this and continually refine it against your references until it is perfect"
  - "if you determine there is a better option than 3 cascade FFT … do that"
  - "show FPS in upper-left"
  - "make sure to capture additional views from wherever you need to to verify AAA quality"
- Then many short critique prompts: "caustics are just noise", "rewrite from ground up. start over. find something better", "performance is starting to suffer. optimize as you go. don't let it get away from you", "think of where particles should spawn, how they should spawn, what they should look like, how lighting works".
- Lesson (from replies): "How picky you got is the real lesson"; directing is the sauce.

## aipulsedaily: Forest-like survival game (three.js, Opus 5.5, 1 day)
- Workflow: **"Claude Code split the world between waves of agents, one per domain, and critic agents that never touched the build checked the renders against real photos."**
- Prompt: "build me a first person survival game in three.js so realistic you cant tell it isnt earth".
  - He then told it the first pass was mediocre, and it "pulled real reference photos to fix it".
  - Then "improve the textures": it "sent out 50 or so subagents", and he stopped it after about 12h due to usage. He calls it "one and a half prompts".
- World and assets:
  - Erosion runs offline and is baked into the island.
  - No prebuilt game assets; the world is generated in code, plus free CC0 textures and recorded sound.
  - Sun and moon sit at real positions for 49°N in late September; thunder is delayed by the speed of sound.
- Performance:
  - LOD by distance; far trees are billboards baked at load.
  - Doesn't hold 60 fps in dense areas yet.
- Video: rendered frame by frame out of the game (1,881 frames, 4K).

## Robko: Rocket League 8-bit
- Started as a replay viewer for his ML bot, now a full game, playable free in the browser. Sounds are placeholders. No tool details.

## Josh Clemm: Sidewinder → Super Truck Rally (supertruckrally.com, beta)
- Custom levels, an arcade spinning-wheel controller with sensitivity tuned up. No more stack detail.

## @threejs post
- A repost/quote of a realistic water scene (likely Tidewater). Asked "could this be added in a game?", @threejs replied "Not yet."

## Chris DiMarco: Highrise Dynasty (SimTower successor)
- **"Plain JS in the browser: Canvas 2D plus three.js for the live 3D rooms. Every room, person and vehicle is generated by Python scripts in Blender. The sim is deterministic and ported to C# for a future Unity/Steam build. Built with Claude Opus."** Model: Opus 5.5 on the $200 plan.
- Design depth:
  - Stars come from population, tenant satisfaction and the facilities each tier requires (security, medical, metro stop); 5★ needs a VIP inspection.
  - Leases, shop turnover rent and taxes. Commute times are logged and affect satisfaction.
- Reply critique: the UI looks "AI-vibe-coded" (Tailwind/iOS style); install UI skills for a more game-like UI.

## Danny Limanseta: Wildbrush (https://wildbrush.vercel.app)
- three.js. "Plain vanilla claude code with no additional harness or skills or mods". Effort: Extra High. Used about 30% of the weekly limit in one Saturday.
- Prompt: "asked it to make a zelda-like game with paintbrush as a weapon and asked it to **wow me and use up to 6 agents** to build this", then iterated all day with his kids.
- Result: 2 biomes, 3 dungeons, a cave, 30+ enemies, a boss, a flying unlock.
- Weak spot: SFX and music ("pretty bad"). One reply says vanilla Opus sound design is recognisably bad.
- Reply from @to6idev: Opus 5.5 + all details and animations made in Blender. Reply from @nathanlands: his son draws dungeon/class sketches to feed the AI.

## Dev Ed: C++/SDL pixel-sim game
- No how-to. "soon, need to fix up a couple of things first". Replies compare it to Noita, Liero and Motherload.
- A reply plugs Atlas 3D (atlas3dai): text → rigged, IK-ready skeleton in T-pose with weights.

## Salio: "Claude Next" terrain
- No details. Hype post.

## rewind: cozy fishing game. Full steps ("give every tool one job")
1. Generate every 3D model with Tripo Smart Mesh P2: palms, rocks, pier, boat, hut, character.
2. Connect Claude Code to Blender through MCP.
3. Opus 5.5 sets up each model in Blender: scale, pivots, materials, clean export.
4. Opus animates in Blender too: walk, cast, rod, boat on the water.
5. Bring everything into Unity 6 and build the world: forest, river, waterfall, beach, pier.
6. Add systems one at a time: walking, fishing, bait choice, time of day.
7. Play after every step and fix one thing at a time.

Who does what: Tripo makes the models; Opus + Blender handle setup and animation; Unity 6 holds the world and the game. A reply says "opus 5.5 in blender always fixed my import normals instantly".

## ASCII MMORPG (via @ekimko; dev is Ansenjeo, "caariunews")
- Replies agree it's **a 3D game with an ASCII post-process shader** (models + filter), not pure text. ReShade/pixijs filters do the same.
- Related: chenglou.me/pretext "variable typographic ASCII". Technique: ASCII/glyph post-process pass over a 3D render.

## Rimsha's EARLIER thread (x.com/heyrimsha/status/2110321800936866244): 10 agent game-dev repos
1. htdt/godogen: describe a game; the agent writes code, generates assets, runs the engine, checks via live game or recording (Godot, Bevy, Babylon.js).
2. majidmanzarpour/threejs-game-skills: gameplay, graphics, sound, UI, testing skills.
3. Donchitos/Claude-Code-Game-Studios: 49 agents organised as a studio.
4. hi-godot/godot-ai: the AI works inside the Godot editor (scenes, scripts, signals, cameras, animations, particles, UI).
5. aldegad/sprite-gen: a character image becomes animation states, background removal, transparent frames, sprite atlases, with a review UI. A Claude/Codex skill.
6. OpenDCAI/GameFactory-3A: asset generation (images, 3D, motion, audio, cinematics) connected to game construction.
7. gamedev-skills/awesome-gamedev-agent-skills: 74+ skills with a router.
8. zenstory-ai/novel-to-game: story → game, 7 skills.
9. **EpicGames/unreal-engine-skills-for-claude-code-plugin**: Epic's official Claude Code plugin to control Unreal Editor (Blueprints, materials, FX, animation, UI, testing).
10. genex-games/genex-desktop: AI game-dev workspace (agents, Blender 3D assets, publish to web), macOS/Linux, early.
- Replies:
  - "the agent writes code but cannot see the game run … what mattered was a read-back loop, run the scene headless…" (@clawgirlbobo)
  - Meshy + Blender solved characters for a browser city game (@javimbuilds).

## Current Rimsha thread replies (x.com/heyrimsha/status/2115738391467946090), 52 replies, useful ones
- debuffjp/spryte: a fast 2D web renderer, 2–4× faster than the most popular.
- Godot now includes built-in Jolt. Jolt + Recast is a strong combo.
- meshoptimizer: "what everyone skips"; fast LOD generation.
- Box3D, s&box (Facepunch's Source 2 sandbox).
- "helium" and "amnicad"; pair amnicad with TRELLIS.2 + image gen for asset creation.
- kanaworks-sakuga-vfx: 90 anime-style Effekseer effects for Unity/Godot/web; free under $1M revenue with credit, not OSS.
- **Dear ImGui for in-game debug panels: "agents get a lot more useful when they can see the state they're changing instead of guessing from logs".**
- ECS architectures. Alchemishty/Multiplayer-LiveOps-Toolkit (Unity). openfluke/welvet.
- Pixel art pain: keeping equipped armour in the animation (layered sprites).
- "how can I make the games actually fun and have some soul in it", i.e. game design and game feel.
- "just because you have all the parts doesn't mean you can assemble a Ferrari".
- "And for Unreal?" Answer: Epic's official Claude Code plugin (above).

## cryptomanavan, later project: DYSTOPIA (2026-10-22)
- "A fully procedural noir island built with Opus 5.5 and three.js WebGPU + TSL. 42 km². 37,491 buildings. 2.9 million rooms!" plus a micro-drone that can fly into almost all of them. Procedural buildings with interiors at massive scale. x.com/cryptomanavan/status/2113063479565271054
