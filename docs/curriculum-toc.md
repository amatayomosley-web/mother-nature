# Mother Nature — Solo Game Developer Curriculum (Plan)

> Status: planning artifact. The doc itself is not yet drafted; this is the synthesis + table of contents that the curriculum will be written against.
>
> Source: 17 multi-source research agents run 2026-05-08 covering academic curricula, postmortems, Godot official docs, ecosystem, design theory, math/physics, architecture patterns, four genre toolkits, art, audio, production, shipping, game feel, and an audit of the existing 736-line learning guide at `C:/Users/willi/Claude Flow/docs/game-dev-learning-guide.md`.
>
> **Open conflict with the actual Mother Nature design** (see `docs/design/survival-design.md`): this curriculum's TOC dropped multiplayer/networking to appendix mention. The Mother Nature design is multiplayer to its bones (solo / private / public-world tiers, persistent regional clocks, thousands of players). Genre toolkit also needs rework — none of the four (action / puzzle / RPG / roguelike) directly cover survival sim. **Decision pending**: rebuild TOC to fit Mother Nature specifically, or keep it generic with the design doc as a parallel reference.

---

## What this is

A page-by-page novice curriculum to take a complete game-dev novice from zero to a shipped 2D Godot 4 game. The reader directs Claude as the implementation engine; Claude writes most of the code. The reader's job is to understand enough to direct, evaluate, and debug.

Engine: Godot 4 (latest stable). Scope: 2D first; 3D / networking / multiplayer deferred.

Quality bar: every header and every word of advice in the final doc is backed to a first principle (computation, physics, perception, economics, market reality, or human cognition). No cargo-cult.

---

## Cross-cutting themes (anchors every chapter must hit)

Topics that recurred across 4+ research reports — the spine of the curriculum.

**Engine fundamentals.** Scene tree as composition. Signals as observer/event system. Resources for data outside the tree. `_process` vs `_physics_process`. Typed GDScript (28-59% perf + IDE help). InputMap (action names, not raw keys).

**Architecture.** Game loop + delta time. FSMs for player/enemies/UI. Save/load with version + migration (career-defining bug class). Composition over inheritance (Godot's tree IS this). Autoload sparingly (5-10 ceiling). Object pooling for high-spawn objects.

**Math.** Vectors (add/sub/scale/normalize/length²/dot). `atan2` for angle-of-vector. Lerp + framerate-independent damping. Semi-implicit Euler. ~15 concepts at conceptual depth, ~5 at hands-on.

**Game feel (load-bearing, not optional).** 100ms input-to-photon perceptual floor. Locus of control (cinematic ≠ game feel). Juice catalog: tween, particles, screen shake, hitstop, flash, squash/stretch, audio variation, number pop. Celeste forgiveness (coyote time, jump buffer, corner correction). Sensory layering within ~6 frames. Mute / no-art / 1-button diagnostic tests.

**Production.** Scope discipline (>70% of failed indies cite this). Vertical slice ≠ MVP. Git + LFS + Godot `.gitignore` from day one. 3-2-1 backups verified. Daily small step (Eric Barone's 10 hr/day is survivor bias). Public devlog as accountability + audience seed.

**Playtesting.** First-time-player observational protocol (silent, watch faces). Solver bias / "you can't unsee the solution." Cadence: vertical slice → +4 weeks → beta. Recordings beat surveys.

**Asset pipeline.** Aseprite + DB32 palette + Kenney CC0 starter. Pixel-perfect setup (Filter Off, integer scaling, snap-to-pixel). License hierarchy (CC0 > CC-BY > royalty-free). AI art net-positive for capsules/concept; net-negative for in-game sprites.

**Audio.** `AudioStreamRandomizer` + ±10% pitch + 3-5 sample rotation. 4-bus mix (Master/Music/SFX/UI). 50ms onset rule. OGG for music, WAV for SFX.

**Shipping.** itch.io first, Steam if earned. Capsule art = #1 marketing asset. Wishlists are leading indicator (3+ months pre-launch). $9.99 default for first 2D. 10-review unlock as urgent first goal.

---

## Top 10 unknown-unknowns (rarely in tutorials, recurrent in postmortems)

1. **Indie work is 90% non-development** — marketing, support, business, taxes.
2. **Steam page is a development asset, not a launch asset** — 6-8 months minimum runway.
3. **Loving to play a genre ≠ loving to make it** — validate developer-fit, not just player-fit.
4. **Polish doesn't broaden appeal — core concept does.** A muddy game with a sharp core outsells a polished derivative.
5. **Save/load is the career-defining bug class.** Schema versioning + migration from version 1.
6. **Burnout is statistically near-certain on multi-year solo projects.** Plan motivation as a system.
7. **Watch playtesters silently; ignore what they say, watch what they do.**
8. **Twitter/X is a dev echo chamber, not player reach.** Reddit + streamer outreach drive wishlists.
9. **Build the ending first** — forces a shippable shape.
10. **Median 2025 indie Steam game earned $174 net.** First-game realistic expectation: <100 sales. The win is shipping, not income.

---

## Audit verdict on the existing 736-line guide

Strong bones. Stages 2 (communicating with Claude), 3.3 (finishing discipline), 5 (game feel), 6.1 (project mgmt), 8.2 (bug categorization), 8.3 (destruction lenses), 10.2 (game jams) are worth porting verbatim. The rest is engine-agnostic, lacks any procedural walkthrough, and a complete novice cannot follow it page-to-page and ship a game from it.

**Verdict: hybrid rebuild.** New structure, new chapters, but port the strong sections from the existing guide as named imports.

---

## Proposed table of contents

7 parts, 25 chapters, 8 appendices. Multi-file: `INDEX.md` + per-chapter files in this `docs/` directory. Each chapter follows a fixed shape: concept → why-drilled-to-first-principle → Godot-specific implementation → checkpoint test → cross-references.

### Part 1 — Foundation (Before the First Pixel)
1. **The Honest Map** — scope, timeline, expectations, unknown-unknowns
2. **How Games Actually Work** — loop, dt, state, vectors, latency floor
3. **The Godot Mental Model** — nodes, scenes, scene tree, signals, Resources

### Part 2 — First Game (The Pipeline)
4. **Project Setup (Page-by-Page)** — install, git+LFS+gitignore, pixel-perfect, audio buses, InputMap
5. **GDScript Floor** — typed, lifecycles, `@export`, common gotchas, 3→4 translation
6. **Communicating with Claude About Game Code** [PORT existing Stage 2] — contract pattern, scene specs, bug categorization
7. **Your First Tiny Game** — Dodge-the-Creeps walked through end-to-end, exported, shipped to itch.io

### Part 3 — Building Real Things
8. **Game Design Fundamentals** — MDA, 8 Kinds of Fun, skill atom, loops/arcs, affordances, flow, scope discipline
9. **Game Feel and Juice** [EXTEND existing Stage 5] — Swink's pillars, juice catalog, Celeste forgiveness, diagnostic tests, juice checklist
10. **Architecture for Multi-System Games** — composition, FSMs, signals vs calls, autoload ceiling, Resources, save/load + versioning, object pools, dependency direction
11. **Bug Hunting and Destruction Testing** [PORT existing 8.2 + 8.3]
12. **Playtesting** — observational protocol, recruitment, cadence, recordings beat surveys

### Part 4 — Production
13. **Project Management for One** [PORT existing 6.1] — MVP/vertical slice, milestones, daily step, boredom trough, devlog, burnout
14. **Asset Pipeline (Solo, Non-Artist)** — Aseprite/DB32 workflow, Kenney + palette unification, autotile, 9-slice, AI art verdict
15. **Audio Pipeline (Solo)** — SFX vs music, tools, layering, `AudioStreamRandomizer`, licenses, AI audio

### Part 5 — Genre Toolkits (Pick What Applies)
16. **If You're Making a Platformer / Action Game** — coyote/buffer/corner, hitbox/hurtbox, frame data, Keren cameras, telegraphs
17. **If You're Making a Puzzle Game** — rule clarity, mechanic depth, instant undo, ABC ordering, fresh-tester rule
18. **If You're Making an RPG / Narrative Game** — stats, progression, save schema deep-dive, dialogue authoring (ink/Dialogic), branching, localization seam
19. **If You're Making a Roguelike / Sim** — BSP first, seed determinism, two-currency economy, smart RNG, Beehave ladder

### Part 6 — Shipping
20. **Building for Distribution** — export presets, code signing, web export quirks
21. **Steam (If You Earn It)** — Direct fee, capsule, wishlists, Next Fest, trailer, presskit, pricing, reviews, refund constraint
22. **itch.io (Default First Launch)** — butler CLI, page setup, devlog seeding
23. **Marketing Minimum** — Zukowski's data on what works, what doesn't

### Part 7 — Ongoing Practice
24. **Post-Launch** — day-one patch, community management, sales cycle
25. **After Your First Game** [PORT existing Stage 10] — critical play, jams, deferred domains

### Appendices
- **A.** GDScript reference floor
- **B.** Godot 3.x → 4.x translation cheatsheet
- **C.** Recommended addons (Phantom Camera, Beehave, Dialogic, GUT)
- **D.** Asset sources (Kenney, OpenGameArt, freesound, Lospec, Incompetech)
- **E.** Resources (books, talks, communities)
- **F.** 7-day prototype kill criteria
- **G.** Premortem template
- **H.** Glossary

---

## Decisions taken

1. **Hybrid rebuild**, not pure extension. New structure imports strong content from existing guide as ported sections.
2. **Multi-file** (`INDEX.md` + per-chapter files) under `docs/`. Existing 736-line guide stays in `Claude Flow/docs/` as separate artifact.
3. **2D Godot 4 only**; 3D / networking / multiplayer deferred to Appendix mention. 3D roughly doubles surface area with no shipping benefit on first game.
4. **Genre toolkits (16-19) are modular** — reader picks 1-2 based on their game.
5. **Fixed chapter shape**: concept → why-drilled-to-first-principle → Godot-specific implementation → checkpoint test → cross-references. This is the page-by-page novice contract.
6. **Front-loaded shipping**: project setup + GDScript floor + first-game walkthrough (chapters 4-7) so the novice has a shipped game by chapter 7. Existing guide deferred shipping to Stage 9; novices who don't ship by week 8 mostly quit.

---

## Drafting plan

Multi-session. ~25 chapters × ~3,500 words ≈ 90K words plus appendices ≈ 200-300 typeset pages. Per-chapter protocol: pull relevant research report(s); spawn drill-down agents for any why-chain that's shallow; draft prose with first-principles backing on every header; self-audit against chapter shape. Pace: 3-5 chapters per session, full doc ≈ 5-8 sessions.

---

## Open questions before drafting begins

- Sign-off on the 7-part / 25-chapter structure, or redirect.
- Are the four genre toolkits (action / puzzle / RPG / roguelike) the right four? Add a fifth (e.g., narrative-light arcade — Vampire Survivors-shape)? Cut one?
- Appendices A-H — right set, or add/cut?
- Should the 17 raw research reports be saved alongside this TOC as `docs/_research/`? Recommended yes (survives compaction, sourced citations for every claim) but adds ~17 files of ~2-3K words each. Default: save them.
