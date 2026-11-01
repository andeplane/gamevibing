---
name: game-direction-workflow
description: How to direct a coding agent (Claude Code, Codex, Cursor, Gemini CLI, Copilot, OpenCode…) through building a game, from first prompt to polish. Load at the start of any new game, before a big feature, or before a long autonomous / multi-agent run. Covers the kickoff prompt, agent waves with critics, critique prompts, /goal recipes and the guard rails that keep a growing codebase sane.
---

# Game direction workflow

The viral AI games of Sept–Oct 2026 were not made with secret prompts. They were made with a **clear quality bar, a feature list, critics that compare the game against references, and a very picky human**. This skill packages what the authors themselves said they did.

## What actually worked (evidence)

| Game | What the author did | Cost/time |
|---|---|---|
| Minecraft test (Noah Wachnik) | 2-sentence prompt: "Create a playable version of Minecraft in my browser. Add advanced shaders that make it look as real and beautiful as possible." Effort **max** | 1h37m, ~$15 |
| Wildbrush, Zelda-like (Danny Limanseta) | "make a zelda-like game with paintbrush as a weapon … **wow me and use up to 6 agents**", then a day of iterating with his kids. Vanilla Claude Code, no skills, effort Extra High | ~30% of weekly limit |
| Forest-like survival (aipulsedaily) | "build me a first person survival game in three.js so realistic you cant tell it isnt earth", then "first pass is mediocre", and it pulled real reference photos. **"Waves of agents, one per domain, and critic agents that never touched the build checked the renders against real photos."** Then "improve the textures": 50 subagents | 1 day; stopped at 12h by usage |
| Tidewater (Dan Greenheck) | Kickoff: a long feature list plus "identify any other features i may have missed", "target is 60fps", "use webgpu and TSL", "iterate … refine against your references until it is perfect", "capture additional views … to verify AAA quality". Then dozens of short critique prompts. Effort max, parallel agents | $1,874 |
| Relic Eater (Graalito) | One prompt into his own "gauntlet loop" skill (orchestrating 10+ personal skills); Godot by editing .tscn files; runs the game for screenshots | 7h, one prompt |
| Lagoon Tree Village (cryptomanavan) | "simple 2 part prompt" plus his own concept drawings; no skills, no /goal; ran 7h straight | Max plan |
| Topside (Sindre) | Brief per feature (`docs/BRIEF-*.md`) with phased steps B.1–B.8; each step implemented, independently reviewed, committed; feature flag until promotion | ongoing |
| Fuse Bombers (Andreas) | "always create issues, spawn agents to implement, then new agents to review"; AGENTS.md rules; gameplay PRs stay open for a human playtest | one-shot plus iterations |

Takeaways:
1. **Quality bar + references beat prompt length.** The words "AAA", "Unreal-level", "can't tell it isn't earth" plus *reference photos* move the result more than any amount of spec.
2. **Separate builders from critics.** A critic agent that never edits code and only compares screenshots with references is the single biggest quality lever.
3. **Pickiness is the job.** Dan's critique prompts ("caustics are just noise", "rewrite from ground up. start over. find something better") are what turned a demo into Tidewater.
4. **Most advanced prototype first, then polish** (Graalito). Polishing a rich prototype is easier than growing a thin one.

## Phase 1: Brief (≤1 page, ~10 minutes)

Ask Claude: "Write a detailed one-page brief for a game like <X> but <twist>." Then edit it. It must contain:
- Fantasy and pitch in one sentence. Name the reference games ("Pixel Dungeon mapped onto a post-apocalyptic voxel world").
- Core loop (30 seconds, 5 minutes, session), win/fail, and the meta-progression.
- Camera, controls, target platform (desktop browser? mobile?), **performance target (60 fps on <machine>)**.
- Art direction: 3–6 reference images or photos plus 5 adjectives. Concept art from an image model is fine.
- Tech choice (see `game-engine-choice`) and asset strategy (see `game-asset-pipeline`).
- Out of scope.
- Feature list as **phased steps with acceptance criteria**. Each step must be independently testable.

Don't write a 30-document GDD. Claude-Code-Game-Studios measured that its 30-document build came last in blind review against a one-page-brief build.

## Phase 2: Kickoff prompt template

```
Build <game> as described in docs/BRIEF.md.
Quality bar: <AAA / "looks like real earth" / "as polished as <reference game>">.
Tech: <three.js r18x + WebGPU/TSL + Vite + TS> (see brief). Target 60 fps on <machine>; choose
resolutions, poly counts and texture sizes accordingly. Show FPS in the top-left.
Identify features I missed and implement them. If you find a better technique than the one I
named, use it. I'm not prescribing implementations.
Research references (real photos, papers, talks) for every visual system and refine against them.
Use up to <6> parallel agents, one per domain (world, rendering, gameplay, UI, audio), each owning
its own files. Run a separate critic agent that never edits code: it captures screenshots from
several views and compares them against references, then files concrete defects.
Keep docs/PROGRESS.md updated with what's done, what's next and known issues.
```

Settings: the highest reasoning effort for the kickoff and for rewrites (Claude Code: max/extra-high; Codex: `high`/`xhigh` reasoning); lower effort is fine for small fixes. "Parallel agents" means subagents where the agent has them (Claude Code subagents, Codex/Cursor multi-agent). Otherwise run each domain and the critic as separate sessions on separate branches.

## Phase 3: Agent waves (for long runs)

- **Lead** reads the brief, splits work by domain, and assigns **file ownership**: one writer per file, no overlapping edits.
- **Builders** implement one phased step each and finish with a verification artefact: screenshot path, test output, perf numbers.
- **Critics** run *after* each wave. They never touch the build. They compare captures with references and with the brief's acceptance criteria, and output a numbered defect list. See `game-art-review`.
- **One review round per step.** Fix, then move on; don't loop reviews recursively.
- Integrate behind a **feature flag** (`?bunkers`), and promote when it's done (Topside pattern).

## Phase 4: Critique prompts (the human's job)

Write short, specific and physical critiques. Good patterns, from Dan Greenheck's actual prompts:
- Name the defect precisely: "seeing tons of bright flickering spots", "clouds are pixelated", "shadows are really weak", "foam is getting stretched out".
- Demand a restart when a subsystem is fundamentally wrong: "caustics need to be rewritten from ground up. start over. find something better."
- Ask it to think through the physics or art: "think of where particles should spawn, how they should spawn, what they should look like, how lighting works."
- Guard performance continuously: "performance is starting to suffer. optimize as you go. don't let it get away from you."
- Ask for what "sells" realism: "irregular dark patches, world-placed patches of foam and streaks like you see in real water."

Avoid vague praise or "make it better". Every prompt should name what is wrong and where.

## Done-conditions (/goal recipes)

Claude Code's `/goal` keeps working until an evaluator judges the condition met from the transcript alone. Other agents don't have `/goal`; paste the same done-condition at the end of the prompt, or use the agent's own loop/autonomous mode. Either way the condition must be mechanically provable from printed output:
```
/goal Implement steps B.1–B.3 of docs/BRIEF-BUNKERS.md. Done when: `npm run build` exits 0,
`npm test` passes, `npm run playtest -- --scenario bunkers` saves screenshots to
artifacts/bunkers/*.png and prints their paths, and the seed sweep prints "256 seeds, 0 failures".
Stop after 30 turns if not done and summarise blockers.
```

## Guard rails for codebases that grow

Sindre noticed Opus slowing and losing the big picture as Topside grew. Counter it with:
- **AGENTS.md** with architecture rules, commands and definition of done. Codex, Cursor, Copilot, OpenCode, Amp and most other agents read it natively. For Claude Code add a `CLAUDE.md` containing `@AGENTS.md`; for Gemini CLI set `contextFileName` to `AGENTS.md` or add a `GEMINI.md` that points to it.
- **A deterministic simulation separate from rendering**, enforced by a test that fails on forbidden imports (Fuse Bombers: `src/engine` may not import Phaser/DOM/`Math.random`/`Date.now`). This keeps replays, bots, netcode and tests possible.
- **A seeded RNG everywhere.** Generators are pure functions of the seed. Golden hashes guard worldgen changes (bump `GEN_VERSION`).
- **docs/PROGRESS.md** as continuity between sessions; one brief per feature in `docs/BRIEF-<feature>.md`.
- **Test discipline:** unit-test pure logic only (sim, RNG, scoring, generators plus solvers). Run one smoke playthrough per iteration and full visual passes at milestones. Opus loves adding slow tests (Sindre: "so much time goes to waiting on tests"), so tell it the budget.
- **One PR per feature;** gameplay PRs stay open for a human playtest ("green CI ≠ acceptance").

## Verification

At the end of any session the agent must print:
- the build/test results;
- the paths to fresh screenshots from at least three views (or a short capture video);
- an FPS/frame-time readout;
- the updated PROGRESS.md diff.
