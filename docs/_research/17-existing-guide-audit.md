# Existing Guide Audit — Research Report

## Document summary

The existing guide at `C:/Users/willi/Claude Flow/docs/game-dev-learning-guide.md` is 736 lines, structured as ten sequential stages from "How Games Work" through "The Ongoing Practice," with explicit time estimates per stage (4-6 months total to ship 2-3 small games). The tone is opinionated, conversational, and direct — it reads like an engineer's letter to a peer rather than a textbook. It addresses a specific reader: an experienced AI-directing dev with no game-dev background, who plans to use Claude as the primary implementation engine while developing taste, judgment, and architectural sense as the human-only contribution. The closing meta-note explicitly frames it as a "research-challenge cycle" output, with corrections already applied (domain count reduced from 70 to ~25, marketing demoted, "read, don't write" softened to 80/20). It is a learning roadmap, not a tutorial — it tells you what knowledge you need, not how to acquire it.

## Full structural map

**STAGE 1: How Games Work (Before You Touch Code)**
- 1.1 — The Game Loop
- 1.2 — State: The Memory of a Game
- 1.3 — Events and Input
- 1.4 — Coordinate Systems and Vectors
- 1.5 — How Game Engines Help

**STAGE 2: Communicating With Claude About Games**
- 2.1 — Prompt Specificity
- 2.2 — Evaluating Claude's Output
- 2.3 — Iterative Refinement
- 2.4 — Debugging Together

**STAGE 3: Your First Game**
- 3.1 — Project: Single-Mechanic Game
- 3.2 — Game Design Fundamentals
- 3.3 — Finishing Discipline

**STAGE 4: Systems That Talk to Each Other**
- 4.1 — Project: Multi-System Game
- 4.2 — System Decomposition (Learned Through Doing)
- 4.3 — Data-Driven Design
- 4.4 — Player Experience Design

**STAGE 5: Game Feel and Juice**
- 5.1 — What Game Feel IS
- 5.2 — Juice: Making Actions Feel Impactful
- 5.3 — The Taste Development Process

**STAGE 6: The Craft of Building Complete Games**
- 6.1 — Project Management for One Person
- 6.2 — Version Control
- 6.3 — Sound and Music
- 6.4 — Art Direction (Designing Within Your Limits)
- 6.5 — Testing and Quality

**STAGE 7: Specialized Knowledge Domains**
- 7.1 — AI and NPC Behavior
- 7.2 — Procedural Generation
- 7.3 — Narrative and Dialogue Systems
- 7.4 — Networking and Multiplayer
- 7.5 — 3D Graphics
- 7.6 — Economy and Progression Design

**STAGE 8: The Code Literacy Floor**
- 8.1 — What You Must Be Able to Write (Not Just Read)
- 8.2 — Debugging Code You Didn't Write
- 8.3 — Playtesting as Destruction: Breaking Your Own Game
- 8.4 — Reading Code Fluently

**STAGE 9: Shipping**
- 9.1 — Building for Distribution
- 9.2 — The Release Checklist
- 9.3 — Post-Release

**STAGE 10: The Ongoing Practice**
- 10.1 — Critical Play
- 10.2 — Game Jams
- 10.3 — Knowledge Domains to Explore As Needed

Plus: Learning Progression Map (visual), Resources (books, video, tools, communities), and meta-note on construction methodology.

### Stage 1: How Games Work

- **Coverage**: Game loop, state and state machines, events/input, coordinates and vectors, engines overview.
- **Depth**: Each subsection is a concept-level paragraph with bulleted "Learn:" lists. No code. No exercises. No diagrams beyond the pseudo-code loop.
- **Strengths**: The "why this first" rationale framing on each section is excellent — it ties abstract concepts back to "what a Claude conversation will assume." 1.4's "you don't need to solve equations by hand" is honest about depth.
- **Gaps**: No discussion of the scene graph or node tree (the actual mental model for Godot specifically). No mention of frame independence beyond delta time. State machines named but no concrete example. No mention of fixed timestep vs variable timestep. Vectors cover position/velocity/acceleration but not normalization, dot product, or angle math (used constantly in 2D).
- **Tone / pedagogical approach**: Explain-only, no show. Abstract-to-the-point-of-disembodied. A novice reading this will understand what state IS but cannot picture what state LOOKS LIKE in a project.

### Stage 2: Communicating With Claude About Games

- **Coverage**: Prompt specificity, output evaluation, iteration, debugging-with-Claude.
- **Depth**: Reasonably deep for a meta-skill section. 2.1 has a concrete bad/good prompt example (Godot Player node spec). 2.2 names red flags (dead variables, magic numbers, missing edge cases). 2.3 covers when-to-regenerate vs when-to-tweak.
- **Strengths**: This is the strongest stage in the guide. It treats AI-directing as the load-bearing skill and gives mechanical heuristics ("contract pattern," "expected vs actual," "describe data + behavior + interface + boundaries").
- **Gaps**: No example of a full multi-turn refinement transcript. No discussion of context management (when to start a fresh conversation, how to summarize state). No discussion of how to get Claude to produce a Godot scene tree spec rather than just code. No mention of artifact-based vs conversation-based workflows.
- **Tone**: Best balance in the guide between abstract and concrete.

### Stage 3: Your First Game

- **Coverage**: One project (clicker / dodge / shooter), game design fundamentals, finishing discipline.
- **Depth**: Project section is one-page-of-bullets thin. Names the choice but doesn't walk through any of them. Design fundamentals (core mechanic, feedback loops, risk/reward, pacing, hook) are paragraph-level definitions. Finishing discipline is the strongest part — Derek Yu's death loops, MVP, the last 10% rule.
- **Strengths**: "Ship it broken rather than never shipping it perfect" is the right anchor. Feature-cutting framed as primary skill is correct.
- **Gaps**: No actual walkthrough of any of the three suggested projects. No Godot project setup steps. No scene structure for a clicker. No script template for a dodge game. The novice is told "set up a Godot project from scratch" but the guide does not show how. This is the single biggest hole in a "page-by-page novice" guide.
- **Tone**: Aspirational, not procedural. Tells the reader what to learn, not how.

### Stage 4: Systems That Talk to Each Other

- **Coverage**: Multi-system project, system decomposition, data-driven design, player experience.
- **Depth**: Decomposition is paragraph-level concept (bounded systems, data contracts, integration tests). Data-driven design names the practice but no JSON/CSV example. Player experience covers flow state, difficulty curves, reward schedules, cognitive load, onboarding.
- **Strengths**: The "learn through building, not reading" frame is correct for this stage. "When to refactor vs push forward" gives a heuristic (5 files = restructure, 1-2 files = continue) that is actually mechanical.
- **Gaps**: No concrete example of a signal/event between systems in Godot syntax. No concrete data file example. No example of a save/load implementation. The integration test mentioned in 4.2 is named, not shown. Player experience section drifts toward design theory without a Godot anchor.
- **Tone**: Mostly explain. Hints at do-don't-read but provides nothing to do.

### Stage 5: Game Feel and Juice

- **Coverage**: What game feel is, juice techniques, taste development.
- **Depth**: Surprisingly concrete. Specific numbers (2-3px small shake, 8-12px large, 100ms responsive, 200ms laggy, 2-5 frame hit pause). Lists 6 juice techniques. Recommends specific games to study (Celeste, Hollow Knight, Dead Cells, Hades, Katana ZERO).
- **Strengths**: The most actionable stage. The A/B test habit and the parameter journal idea give the novice a concrete practice. Numeric thresholds are real and verifiable.
- **Gaps**: No Godot-specific implementation hints (Tween nodes, AnimationPlayer, ShaderMaterial for screen shake). Coyote time and input buffering named but not coded. No mention of how to actually wire screen shake in Godot 4 (Camera2D shake, RandomNumberGenerator, Tween).
- **Tone**: Show-tell hybrid. Best section for converting a novice into someone with taste vocabulary.

### Stage 6: The Craft of Building Complete Games

- **Coverage**: Project management, version control, sound, art direction, testing.
- **Depth**: Each subsection is one page. PM covers MVP, timeboxing, MoSCoW, kanban, weekly check-ins. Git covers basics, commit discipline, branching, what-not-to-commit. Audio covers sourcing and the "8 sounds for simple games" rule. Art covers achievable styles and consistency.
- **Strengths**: Pragmatic, opinionated, list-heavy in the right way. The 80/20 audio list (jump/land/attack/hit/death/pickup/UI/music) is exactly the kind of bound-the-scope advice a novice needs.
- **Gaps**: Git with Godot is named (.tscn and .tres files) but not shown — what actually goes in .gitignore for a Godot project? Art section has no asset-pack walkthrough. Testing section conflates QA testing with playtesting, then 8.3 covers playtesting in much more depth — content split awkwardly across stages.
- **Tone**: List-driven, practical, occasionally feels like an outline rather than prose.

### Stage 7: Specialized Knowledge Domains

- **Coverage**: Six optional domains — NPC AI, procgen, narrative, networking, 3D, economy.
- **Depth**: Each subsection is half a page. Names the techniques (state machines, A*, behavior trees, Perlin noise, Wave Function Collapse, client-server, prediction/interpolation, PBR, currency sinks). No implementation depth — explicitly framed as "concepts, Claude implements."
- **Strengths**: Honest about which domains are pits (networking "probably doubles or triples your development time"). 3D section's "start with 2D, seriously" is correct guidance.
- **Gaps**: For a 2D-Godot-first curriculum, this stage is over-broad. Networking and 3D belong in a much later document. Procgen and AI behavior could be deepened with Godot-specific examples (NavigationAgent2D, AStar2D classes). Economy section is the thinnest — spreadsheet modeling named but no example.
- **Tone**: Encyclopedic. Reads like a reference index, not learning material.

### Stage 8: The Code Literacy Floor

- **Coverage**: Minimum writing ability, debugging code Claude wrote, playtesting as destruction, reading fluency.
- **Depth**: 8.2 (debugging) and 8.3 (playtesting) are the longest, most detailed sections in the guide. 8.2 categorizes bugs into 4 types with Claude fix rates per category (90/60/40/0%). 8.3 has 5 destruction lenses and a per-system destruction questions table.
- **Strengths**: The bug categorization with fix-rate is genuinely novel and load-bearing. The "stupid player test" frame is honest about real-player behavior. The destruction table is a reusable artifact.
- **Gaps**: 8.1 is one page of trivial syntax examples (var, if, for, function call) — too thin to qualify as a "writing floor." No GDScript-specific syntax patterns. 8.4 is half a page and adds nothing 8.2 didn't already cover.
- **Tone**: 8.2 and 8.3 are the most show-don't-tell sections in the guide. 8.1 and 8.4 are filler by comparison.

### Stage 9: Shipping

- **Coverage**: Export, release checklist, post-release.
- **Depth**: One page total. Lists Godot export targets, distribution platforms (Steam/itch/GOG), release checklist (8 items), post-release habits (feedback, triage, post-mortem).
- **Strengths**: itch.io-first strategy is correct. Release checklist is concrete and verifiable.
- **Gaps**: No actual export walkthrough. No mention of code signing for Mac/Windows. No mention of HTML5 export quirks (CORS, audio context). No discussion of game pages, screenshots, gif marketing for itch.
- **Tone**: Checklist-driven. The thinnest substantive stage in the guide.

### Stage 10: The Ongoing Practice

- **Coverage**: Critical play, game jams, deferred domains table.
- **Depth**: Half a page. Recommends specific jams (Ludum Dare, Global Game Jam, itch.io jams) and lists 11 deferred domains (accessibility, localization, console porting, mobile, VR, anti-cheat, modding, analytics, business, tax, marketing).
- **Strengths**: Game jams as forcing function for finishing is correct. Deferred-domains table is the right way to handle the original 70-domain scope.
- **Tone**: Closing reference list.

## Engine-agnostic vs Godot-specific

The guide names Godot 12 times and Unity 7 times, plus Unreal twice and Phaser once. Concrete Godot anchors: pseudo-code "Player node" prompt example in 2.1 (mentions GDScript values), engine recommendation in 1.5, debugger reference in 8.2, .tscn/.tres in 6.2, export to .exe in 9.1. Everything else is engine-agnostic — the entire conceptual scaffolding (game loop, state, vectors, decomposition, juice, debugging) is engine-neutral and the Godot mentions are sprinkled in rather than load-bearing. Stages 1, 4, 5, 7, 8, 10 contain almost no Godot-specific content. For a Godot-first curriculum, every stage needs anchors: scene tree explanation in 1.5, signals as the events model in 1.3, Camera2D + Tween for screen shake in 5.2, AStar2D and NavigationAgent2D in 7.1, ResourceSaver/ResourceLoader for save/load in 4.3, and so on. The current guide could be retitled "Generic Game Dev Learning Guide With Occasional Godot Recommendation" without losing much content.

## Pedagogical strengths

What the rewrite should preserve: (1) The opinionated, direct tone — no hedging, mechanical thresholds and numbers throughout. (2) The "why this first" framing on each subsection — every concept is justified by what depends on it. (3) Stage 5's juice section with concrete numeric thresholds. (4) Stage 8.2's bug categorization with Claude fix rates per category. (5) Stage 8.3's 5 destruction lenses and per-system table. (6) The finishing-discipline framing in Stage 3. (7) The decoupling of "learning" from "doing" — calling it a to-learn list, not a to-do list. (8) Honest assessments of where Claude struggles (integration bugs, feel bugs, real-time multiplayer). (9) The explicit MVP and timeboxing discipline in Stage 6. (10) The deferred-domains table in Stage 10 — correct way to handle out-of-scope content.

## Pedagogical weaknesses / gaps

**Topics not addressed at all**: Godot scene tree as the central mental model. Signals as Godot's event system. Node lifecycle (_ready, _process, _physics_process). Resources as data containers. AnimationPlayer and AnimationTree. Tween and easing-by-example. Tilemap workflow. Particle systems (GPUParticles2D / CPUParticles2D). Shader basics (canvas_item shaders). Input map system. Autoloads / singletons. Groups for entity management. Physics bodies (CharacterBody2D, RigidBody2D, Area2D, StaticBody2D) and the choice between them. Collision layers/masks. Camera2D smoothing/limits/shake. UI (Control nodes, anchors, containers, themes). Localization basics. Accessibility basics for 2D (colorblind palettes, font sizing).

**Topics addressed but too shallow**: Stage 3's first project (named, not walked through). Stage 4's system decomposition (concept only, no Godot signal example). Stage 6's Git-with-Godot (named, not shown — `.gitignore` for Godot is non-trivial). Stage 8.1's writing floor (5 syntax examples is not a floor). Stage 9's export (no actual walkthrough). Stage 8.4's reading fluency (redundant with 8.2).

**Topics addressed in wrong order**: Playtesting split between 6.5 (testing/quality) and 8.3 (destruction) — should be co-located. Game design fundamentals (3.2) are introduced inside Stage 3 but apply earlier and throughout. State machines mentioned in 1.2 conceptually and 7.1 for NPCs but the canonical pattern is never shown. Code literacy floor (Stage 8) is parallel to 3-7 per the guide's own admission — should be earlier or distributed.

**Assumes prior knowledge novices won't have**: "Compiles and runs" (2.2) — GDScript is interpreted; this phrase confuses novices. "Stack trace" (8.2) — never explained. "Race conditions" (8.2) — never explained. ".tscn and .tres files" (6.2) — never defined. Pseudo-code in 1.1 uses C-style braces, not GDScript syntax. Vector/matrix math implied at 7.5 ("not optional") with no preparation.

**"Why" asserted but not drilled**: Why delta time multiplication matters mathematically (1.4). Why decoupling via events beats direct calls (1.3). Why state machines specifically (vs nested ifs). Why MVP economically vs psychologically (3.3). Why exponential decay on screen shake specifically (5.2). Why itch.io-first beyond "free and instant" (9.1).

## "Page-by-page novice" standard — does it meet?

No. This is a strong learning roadmap, not a buildable curriculum. A complete novice opening this guide and reading sequentially would understand what they need to learn but not be able to do any of it without leaving the document for tutorials. They would be stuck at Stage 1 needing a Godot install and project walkthrough that the guide does not provide. They would be stuck at Stage 3 because "set up a Godot project from scratch" is the title of a tutorial they don't have. They would be stuck at Stage 4 because data contracts and signals between systems are concept-named but not shown. They would arrive at Stage 5 with vocabulary but no implementation hooks. They would reach Stage 8.2 — the strongest section — and finally have something they can apply, but only retroactively to bugs they encountered while learning elsewhere. The guide's own framing as a "to-learn list, not a to-do list" is honest, but a novice reading it will need a parallel to-do document to actually ship anything. This guide is the syllabus; a novice needs the textbook plus the lab manual.

## Recommendation

**Hybrid (option 3).** Extract the strong sections — Stage 2 (entire, especially 2.1), Stage 5 (entire), Stage 8.2 (bug categorization), Stage 8.3 (destruction lenses), Stage 6.1 (project management), Stage 3.3 (finishing discipline), Stage 10.2 (game jams) — and treat them as load-bearing modules in the new curriculum. Rebuild Stages 1, 3, 4, 6.2-6.4, 7, 8.1, 8.4, 9 with Godot-specific anchors, code examples, and procedural walkthroughs. Restructure ordering so code literacy distributes across the first project rather than waiting until Stage 8, so playtesting consolidates in one place, and so each stage has a concrete Godot-first artifact (a scene, a script, a project) that the novice produces.

Rationale: The conceptual scaffolding is genuinely good. The opinionated tone, mechanical thresholds, and bug-categorization framework are the kind of unique contribution that took real research-challenge cycles to produce — discarding them would be wasteful. But the engine-neutral abstraction layer plus the absence of any procedural walkthrough makes a full supersession (option 2) tempting; what saves it is that the strong sections genuinely cannot be improved by rewriting from scratch. Pure extension (option 1) understates how thin Stages 1, 3, 4, 7, 9 are — they need rebuilding, not augmenting.

## Section-by-section gap list

- **Stage 1**: Add Godot scene tree mental model. Add node lifecycle (_ready, _process). Show state machines with concrete code. Cover normalize/dot product/angle for 2D. Distinguish fixed vs variable timestep.
- **Stage 2**: Add multi-turn refinement transcript example. Add context-management heuristics (when to start fresh). Add scene-tree-spec prompts, not just script prompts.
- **Stage 3**: Walk through one project end-to-end (recommend dodge-the-falling-objects). Show Godot project setup. Show first scene structure. Show Player script. Show export. Distribute code literacy across this stage rather than deferring to Stage 8.
- **Stage 4**: Concrete Godot signal example between two systems. Concrete Resource-based data file example. Concrete save/load using ResourceSaver. Concrete integration-test playthrough.
- **Stage 5**: Add Godot-specific implementations: Camera2D shake via Tween, AnimationPlayer for hit pause, GPUParticles2D for hit sparks, AudioStreamPlayer for impact sounds. Coyote time and input buffering as code.
- **Stage 6**: Show Godot `.gitignore` (.godot/, .import/, *.translation, export_presets.cfg). Walk through importing a Kenney asset pack. Consolidate playtesting from 6.5 with 8.3.
- **Stage 7**: For a 2D-first curriculum: cut 7.4 (networking) and 7.5 (3D) entirely or move to a "later" appendix. Deepen 7.1 (NavigationAgent2D, AStar2D). Deepen 7.6 with a spreadsheet example.
- **Stage 8**: Cut 8.1 and 8.4 — distribute their content earlier. Keep 8.2 and 8.3 as standalone load-bearing modules.
- **Stage 9**: Add full export walkthrough for Windows + HTML5. Add code signing notes. Add itch.io game-page setup walkthrough.
- **Stage 10**: Keep mostly intact. Strong closing.

## Sources

None — single-file audit.
