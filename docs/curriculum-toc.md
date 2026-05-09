# Mother Nature — Curriculum (Plan)

> **Status**: rewritten 2026-05-08 against the design doc (`docs/design/survival-design.md`). Multiplayer survival sim is now the spine of the curriculum, not an appendix.
>
> **Source**: 17 multi-source research agents run 2026-05-08 (see `docs/_research/`). Supplemental research planned for gaps not covered by the original 17 — atmospheric simulation, Godot 4 multiplayer architecture, deeper wildlife/ecology, persistent shared world servers, survival-sim genre deep-dive, compendium/knowledge-progression systems. These are dispatched as a second wave; their reports will land in `docs/_research/18-23` when complete.

---

## What this is

A page-by-page novice curriculum to take a complete game-dev novice from zero to shipped Mother Nature. The reader directs Claude as the implementation engine; Claude writes most of the code. The reader's role is creative director / architect / evaluator.

**Mother Nature target shape** (from design doc):
- 2D top-down multiplayer survival simulation, Godot 4, Steam (PC) launch
- Three biomes (forest / desert / tundra), each its own survival problem
- Simulated weather (coarse-grid atmospheric model — emergent, not scripted)
- Day-night + four-season cycles with biome-modulated day length
- Wildlife as persistent agents — utility AI, sense systems (sight/hearing/smell), individual identity for apex predators, regional population dynamics
- Body simulation (cold/heat/hunger/thirst/exhaustion/injury) with wet-cold and frostbite as distinct
- Compendium with cross-tier persistence, tiered entries, low-confidence flags, upgradable
- Three multiplayer tiers — solo / private (2-10) / public-world (thousands, continuous regional clocks)
- 12-character roster with soft-locked crafting trees and "insight" mechanic
- Permadeath total; corpse persists with all carried items
- Genre comparables: *Project Zomboid* without zombies, *The Long Dark* with people, *Rust* without raid meta

Quality bar: every chapter, every header, every word of advice backed to a first principle — computation, physics, perception, economics, market reality, human cognition, ecology, atmospheric physics. No cargo-cult.

---

## Mother Nature design north stars

Drilled from the design doc. These are the lenses every chapter must respect.

1. **Variance, not randomness.** Every system that produces variance must also produce signals the player can learn to read. Random "the bear attacks 20% of the time" teaches nothing; variable "this bear is defensive because cubs are nearby and the player can see them" rewards learning. Drives wildlife state, weather predictability, terrain-as-microclimate, sign-reading.
2. **Animals are agents, not encounters.** Animals exist in the world whether or not the player is there. Most of what they do has nothing to do with the player. Drives utility AI per agent, persistent ecology, sense systems with wind, individual apex identity.
3. **Knowledge persists; nothing else does.** Permadeath is total. The compendium is the only meta-progression. Drives compendium engine design, permadeath UX, death-as-lesson reconstruction.
4. **Day is the game, night is recovery.** Real survivalists work in daylight. Drives day-night cycle balance, sleep-as-conditional-time-skip, season-modulated day length.
5. **Same systems, different survival problems.** Three biomes share simulation but feel categorically different. Drives biome-distortion architecture, character-biome balance, seasonal calendar synchronization.
6. **The world is readable to those who learn to read it.** Tracks, signs, weather precursors, animal posture all carry information. Drives tracks-as-physical-objects, sense system with environmental modifiers, weather forecast precursors.
7. **Multiplayer is jungle rules, not factions.** No mechanical alignment system, no factions. Trade, cooperation, conflict, avoidance all emerge from systems. Drives specialization-as-trade, character-locked crafting (knowing ≠ doing), no-PvP-system.
8. **Tiered access, earned progression.** Solo → private → public world. Each tier serves one need cleanly. New players don't enter world servers. Drives tier architecture, gating thresholds, account-vs-character meta.
9. **Death is a lesson, not a punishment.** The death screen reconstructs what killed the player and why — core temperature graph, last meal timestamp, wound timeline. Drives diagnostic UI, body-sim transparency, "that was bullshit" → "oh, I see what I did".

---

## Cross-cutting curriculum themes

Topics that recurred across 4+ research reports — the spine every chapter anchors against. (Preserved from v1; still valid.)

**Engine fundamentals.** Scene tree as composition; signals as observer/event system; Resources for data outside the tree; `_process` vs `_physics_process`; typed GDScript (28-59% perf + IDE help); InputMap (action names, not raw keys).

**Architecture.** Game loop + delta time; FSMs for player/enemies/UI; **save/load with version + migration** (career-defining bug class — and Mother Nature has more save state than most games via compendium + persistent world); composition over inheritance; autoload sparingly; object pooling.

**Math.** Vectors (add/sub/scale/normalize/length²/dot); `atan2`; lerp + framerate-independent damping; semi-implicit Euler. ~15 concepts at conceptual depth, ~5 hands-on.

**Game feel.** 100ms input-to-photon floor; locus of control; juice catalog; sensory layering within ~6 frames; mute / no-art / 1-button diagnostic tests.

**Production.** Scope discipline (>70% of failed indies cite this — and Mother Nature is *already* large); vertical slice ≠ MVP; git + LFS + Godot `.gitignore`; 3-2-1 backups; daily small step; public devlog as accountability.

**Playtesting.** First-time-player observational protocol; solver bias; cadence; recordings beat surveys.

**Asset pipeline.** Aseprite + DB32 (or biome-tuned palettes — see new chapter); Kenney CC0 starter; pixel-perfect setup; license hierarchy; AI art net-positive for capsules/concept.

**Audio.** AudioStreamRandomizer + ±10% pitch + 3-5 sample rotation; 4-bus mix; 50ms onset rule; OGG for music, WAV for SFX.

**Shipping.** itch.io first, Steam if earned; capsule art; wishlists 6+ months pre-launch; $9.99 default for first 2D — Mother Nature likely $14.99-$19.99 given scope.

---

## Top 10 unknown-unknowns (rarely in tutorials, recurrent in postmortems)

Preserved from v1; all still apply, several with extra weight given Mother Nature's scope.

1. Indie work is 90% non-development.
2. Steam page is a development asset, not a launch asset (6-8 months minimum runway).
3. Loving to play a genre ≠ loving to make it. **Especially relevant for survival sim — atmospheric/ecological simulation is years of work.**
4. Polish doesn't broaden appeal — core concept does.
5. **Save/load is the career-defining bug class.** Mother Nature has multiple save scopes (per-character compendium, world-server state, account meta) — this risk is amplified.
6. Burnout is statistically near-certain on multi-year solo projects. **Mother Nature is multi-year by design; plan motivation as a system.**
7. Watch playtesters silently; ignore what they say, watch what they do.
8. Twitter/X is dev echo chamber; Reddit + streamer outreach drive wishlists.
9. Build the ending first — for Mother Nature there's no ending; build the *first day's death loop* first as the equivalent shippable shape.
10. Median 2025 indie Steam game earned $174 net. First-game realistic expectation: <100 sales. **Survival sims like Long Dark and Project Zomboid took years to find their audience; expect a long tail, not a launch spike.**

---

## Audit verdict on the existing 736-line guide

Strong bones. Stages 2 (communicating with Claude), 3.3 (finishing discipline), 5 (game feel), 6.1 (project mgmt), 8.2 (bug categorization), 8.3 (destruction lenses), 10.2 (game jams) are worth porting verbatim. The rest is engine-agnostic, lacks any procedural walkthrough, and a complete novice cannot follow it page-to-page and ship a game from it.

**Verdict: hybrid rebuild.** New structure, new chapters, port the strong sections from the existing guide as named imports.

---

## Proposed Mother Nature Table of Contents

9 parts, 37 chapters + 8 appendices. Multi-file: `INDEX.md` + per-chapter files in `docs/chapters/`. Each chapter follows a fixed shape: concept → why-drilled-to-first-principle → Godot-specific implementation → Mother Nature application → checkpoint test → cross-references.

### Part 1 — Foundation (Before the First Pixel)
1. **The Honest Map** — scope, multi-year reality of survival sim, expectations, the unknown-unknowns, Mother Nature's design north stars
2. **How Games Actually Work** — loop, dt, state, vectors, latency floor
3. **The Godot Mental Model** — nodes, scenes, scene tree, signals, Resources

### Part 2 — First Game: The Pipeline (Pre-Mother-Nature ship)
4. **Project Setup (Page-by-Page)** — install, git+LFS+gitignore, pixel-perfect, audio buses, InputMap
5. **GDScript Floor** — typed, lifecycles, `@export`, common gotchas, 3→4 translation
6. **Communicating with Claude About Game Code** [PORT existing Stage 2] — contract pattern, scene specs, bug categorization
7. **Your First Tiny Game** — Dodge-the-Creeps walked through end-to-end, exported, shipped to itch.io. **Ships in months 1-3. Proves the pipeline before Mother Nature begins.**

### Part 3 — Building Real Things (Foundation for any 2D Godot game)
8. **Game Design Fundamentals** — MDA, 8 Kinds of Fun, skill atom, loops/arcs, affordances, flow, scope discipline. **Threaded with Mother Nature's design north stars.**
9. **Game Feel and Juice** [EXTEND existing Stage 5] — Swink's pillars, juice catalog, Celeste forgiveness, diagnostic tests, juice checklist
10. **Architecture for Multi-System Games** — composition, FSMs, signals vs calls, autoload ceiling, Resources, object pools, dependency direction
11. **Save/Load and Persistence** — schema versioning + migration, atomic writes, compendium-scale saves, per-character vs per-account vs per-server scopes
12. **Bug Hunting and Destruction Testing** [PORT existing 8.2 + 8.3]
13. **Playtesting** — observational protocol, recruitment, cadence, recordings beat surveys

### Part 4 — Survival Sim Systems (Mother Nature's Core)
14. **World, Biomes, and Map Architecture** — three-biome design, hand-authored geography + procedural overlays, world seed, terrain affecting microclimate
15. **Weather Simulation** — coarse-grid atmospheric model, pressure/humidity/wind/temp/cloud-density, fronts, mountains-and-rain-shadows, predictability for skilled players (the barometer mechanic)
16. **Day-Night and Seasonal Cycles** — two clocks, sleep-as-conditional-time-skip, biome-modulated day length, four seasons as distinct survival problems, season-as-curriculum
17. **Wildlife as Agents** — utility AI per animal, sense systems (sight + hearing + **smell with wind direction**), behavioral states, tiered danger, regional populations + apex individuation, migrations
18. **The Body Simulation** — cold/heat/hunger/thirst/exhaustion/injury as systems not meters; wet-cold vs dry-cold; frostbite as permanent injury; death-as-lesson reconstruction
19. **Tracks, Signs, and Reading the World** — physical track objects with substrate + age, scat + scratch + bedding, predator-tracks-player symmetry, blood trail mechanics

### Part 5 — Multiplayer (Mother Nature's Spine)
20. **Godot 4 Multiplayer Foundations** — high-level multiplayer API, ENet vs WebRTC vs WebSocket, MultiplayerSpawner, MultiplayerSynchronizer, RPC modes, authoritative server vs P2P
21. **Server Tier Architecture** — solo / private (2-10) / public-world (thousands), clock semantics (paused vs continuous), regional clock alignment for public servers
22. **World Server Engineering** — persistent world state across player absence, popular-area depletion, sharding-vs-single-world, server hosting cost models
23. **Networking Survival Systems** — wildlife replication strategy (which agents replicate?), weather grid sync, body-sim sync, save-state-on-disconnect, anti-cheat baseline
24. **Multiplayer Interaction Models** — "jungle rules" emergent design, specialization-as-trade, no-faction-system, voice/proximity chat (deferred to post-launch)

### Part 6 — Meta-Progression and Persistence (Mother Nature's Loop)
25. **The Compendium Engine** — entry tiers (unknown / glimpsed / novice / practiced / expert), low-confidence flags, upgradable entries, cost-of-consultation as game design
26. **Cross-Tier Sync and Account Persistence** — per-account vs per-server tradeoffs, compendium read-only thresholds on world servers, soft-pay-to-win risk
27. **Character Roster and Soft-Locked Crafting** — 12-character architecture, specialization trees, "insight" mechanic for adjacent-tree unlocks, character-vs-biome balance
28. **Challenge System and Custom Character** — universal challenges, custom-character unlock gating, spawn-window earnings (e.g., earned-winter-spawn)
29. **Permadeath, Corpses, and Death-as-Lesson** — corpse persistence, looting rules, decomposition + predator attraction, death screen reconstruction UI

### Part 7 — Production
30. **Project Management for One** [PORT existing 6.1] — multi-year scope, milestone architecture for Mother Nature (vertical slice = single biome, single season, single character, body sim + one apex predator), boredom-trough management
31. **Asset Pipeline** — biome-distinct palettes (forest greens, desert oranges, tundra blues), 12 character sprite sets, animal sprite library at scale, autotile for three terrains
32. **Audio Pipeline** — three-biome soundscapes, weather-driven dynamic audio (wind, rain, thunder), animal vocalizations, footstep substrate variants, AudioStreamRandomizer at scale

### Part 8 — Shipping
33. **Building for Distribution** — export presets, dedicated server build, code signing, web export feasibility (likely no for multiplayer scope)
34. **Steam (If You Earn It)** — Direct fee, capsule, wishlists, Next Fest, trailer, presskit, pricing ($14.99-$19.99 likely), reviews, refund policy and how survival-sim playtime sidesteps the 2-hour rule
35. **Marketing Minimum** — Mother Nature's specific positioning ("Project Zomboid without zombies, Long Dark with people, Rust without raid meta"), survival-sim community channels, devlog cadence

### Part 9 — Ongoing Practice
36. **Post-Launch** — patches, community management, sales cycle, the survival-sim long tail
37. **After Mother Nature** [PORT existing Stage 10] — game jams, deferred domains (3D, dogs/horses, mobile, VR), Mother Nature 2.0 vs new project

### Appendices
- **A.** GDScript reference floor
- **B.** Godot 3.x → 4.x translation cheatsheet
- **C.** Recommended addons (Phantom Camera, Beehave, GUT, multiplayer-ready)
- **D.** Asset sources (CC0 packs, biome-distinct)
- **E.** Resources (books, talks, communities, survival-sim-specific reference)
- **F.** 7-day prototype kill criteria
- **G.** Premortem template — Mother Nature-specific failure modes
- **H.** Glossary — including Mother Nature design terms (jungle rules, biome distortions, compendium tiers, etc.)

---

## Decisions taken (revised from v1)

1. **Hybrid rebuild against Mother Nature.** v1 was generic; v2 is Mother Nature-specific. Strong sections from existing 736-line guide still ported as named imports.
2. **Multi-file** under `docs/chapters/` with `INDEX.md` master.
3. **2D Godot 4. Multiplayer is now Part 5, not deferred.** This reverses v1 decision #3. Driven by `docs/design/survival-design.md` §9 making multiplayer load-bearing.
4. **Genre toolkit collapsed to one — survival sim — as Part 4.** v1's four-genre modular design (action / puzzle / RPG / roguelike) is removed; reader's game is Mother Nature, not a genre choice.
5. **Fixed chapter shape** with Mother Nature application section added: concept → why-to-first-principle → Godot implementation → Mother Nature application → checkpoint → cross-refs.
6. **Front-loaded shipping preserved**: the novice ships a tiny first game (Dodge-the-Creeps) by chapter 7 / month 3 before Mother Nature begins. Mother Nature is years of work; the first ship is months.

---

## Drafting plan

**Phase 1 — Supplemental research** (in progress, this session): 6 agents on gaps not covered by the original 17 — Godot multiplayer architecture, atmospheric simulation, deeper wildlife/ecology systems, persistent shared world server engineering, survival-sim genre deep-dive, compendium/knowledge-progression system patterns. Reports land in `docs/_research/18-23` as they complete.

**Phase 2 — TOC sign-off** (this session or next): you confirm the 9-part / 37-chapter structure, the design north stars list, and the gating decisions above. Or redirect.

**Phase 3 — Per-chapter drafting** (multi-session): ~3,500 words per chapter target. 37 chapters × 3,500 ≈ 130K words plus appendices ≈ 300-400 typeset pages. Pace: 3-4 chapters per session, ~10 sessions for full draft. Per-chapter protocol: pull relevant research report(s), spawn drill-down agents for shallow why-chains, draft prose with first-principles backing on every header, self-audit against chapter shape.

---

## Open questions before drafting begins

**Curriculum-side:**
- Sign-off on the 9-part / 37-chapter structure, or redirect.
- Should Part 4 (Survival Sim Systems) come *before* Part 5 (Multiplayer), as listed, or interleave? Argument for separation (current): reader builds the systems in solo first, networks them later. Argument for interleaving: networking shapes architecture from day one. I lean separation; reader can build single-player Mother Nature first and add multiplayer.
- Vertical slice scoping for Mother Nature: I propose single biome (forest), single season (summer), single character, body sim + one apex predator (bear), single-player only. Net result is a small Long-Dark-shaped artifact. Sign-off?

**Design-side (deferred from `docs/design/survival-design.md` §11)**:
- Character roster (12 names, kits, specializations)
- Crafting tree structure + insight mechanic
- Combat feel (twitchy vs deliberate)
- Building & shelter (modular vs freeform)
- Corpse rules
- Specific challenge thresholds for tier unlocks
- Compendium scope (per-account vs per-server on public worlds)
- Sound/light/tracking simulation depth
- Steamworks integration scope
