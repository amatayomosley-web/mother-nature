# Roguelike / Sim / Strategy — Research Report

## Domain summary

Emergence-driven games — roguelikes, simulations, and systemic strategy games — succeed or fail on the quality of their *systems*, not their levels. The defining trait, articulated by Brian Bucklew (Caves of Qud), Tarn Adams (Dwarf Fortress), and Tynan Sylvester (RimWorld), is that the developer authors *rules and producers*, while the player generates the *experience* by colliding those rules. This shifts engineering effort from level design and asset production to procedural content generation (PCG), AI decision systems, RNG psychology, and run-economy tuning. For a 2D Godot novice, the discipline is unfamiliar: you stop hand-crafting and start instrumenting. The hardest skill to internalize is restraint — Dwarf-Fortress-tier complexity is a one-developer-decade game, and emergence is brittle when applied to systems that haven't been classified by problem shape.

## Mechanic + system catalog

### BSP (Binary Space Partition) dungeons
- **Brief**. Recursively split a rectangle into two sub-rectangles; stop at a target size; place a room in each leaf; connect siblings with corridors.
- **Why-drilled**. L1: produces non-overlapping rooms with predictable connectivity. L2: the recursion tree itself is the connectivity graph — no separate "are these reachable?" pass needed. First principle: a *space partition* is simultaneously a layout and a topology, so one data structure answers two questions for free.
- **Reference game**. Original Rogue (1980), most "traditional" roguelikes derived from it.
- **Novice failure mode**. Tuning split ratios so badly that every dungeon looks identical, or letting corridors get astronomically long.
- **Priority**. Must-know.
- **Source**. Red Blob Games (Amit Patel) — `redblobgames.com`; "Random Dungeon Generators" wiki.

### Cellular automata (CA) caves
- **Brief**. Random-fill a grid 45% wall, then iterate "wall if 5+ neighbors are walls" 4–6 passes.
- **Why-drilled**. L1: produces blob-shaped organic caves, not boxy rooms. L2: the rule is local but the result has global structure — large connected caverns form because the rule is a *threshold majority*, which is a smoothing filter. First principle: nature's "organic" shapes are characterized by local-rule equilibria, not global plans.
- **Reference game**. Dwarf Fortress's underground biomes; Terraria; many cave layers in modern roguelites.
- **Novice failure mode**. Forgetting connectivity — CA produces disconnected pockets. Always run flood-fill afterward and either dig connectors or discard small islands.
- **Priority**. Must-know.
- **Source**. Jeremy Kun, "The Cellular Automaton Method for Cave Generation"; Kodeco tutorial; converges within ~15 steps.

### Wave Function Collapse (WFC)
- **Brief**. From a sample image plus tile adjacency rules, generate larger maps where every NxN window is one that appears in the input.
- **Why-drilled**. L1: gives art-directed, locally-coherent output without hand-authoring rules. L2: it's a constraint solver — entropy-minimization at each step picks the most-constrained cell to collapse. First principle: it's local-pattern-statistics replicated globally; the algorithm is computationally expensive (NP-ish backtracking) because consistency is a global property.
- **Reference game**. Caves of Qud (ruins), Bad North, Townscaper.
- **Novice failure mode**. Using WFC as a hammer for problems BSP solves in 50 lines. Slow on large grids; backtracking can explode. Tile adjacency rules are infuriating to debug.
- **Priority**. Should-know (don't start here).
- **Source**. Maxim Gumin's repo (`github.com/mxgmn/WaveFunctionCollapse`); BorisTheBrave's "Wave Function Collapse Explained"; Bucklew/Grinblat GDC 2019 (`gdcvault.com/play/1026263`).

### Drunkard's walk
- **Brief**. Carve floor by random-walking from a starting cell until a target floor count is reached.
- **Why-drilled**. L1: trivially simple, organic-feeling. L2: produces twisty, always-connected layouts (the walker can't disconnect itself). First principle: a connected-by-construction algorithm needs zero post-processing for reachability.
- **Reference game**. Many minor roguelikes; mine layers in Terraria-likes.
- **Novice failure mode**. Output is mush — no discrete rooms, hard to author content into.
- **Priority**. Nice-to-have.
- **Source**. Procedural Content Generation Wiki.

### Room-and-corridor with prefab connectors
- **Brief**. Hand-author room templates (entrance/exit slots), procedurally place and connect them.
- **Why-drilled**. L1: combines author craft with procedural variety. L2: by constraining what tiles can attach where, you eliminate "looks generated" tells. First principle: human-authored content has higher information density than algorithmic output, so prefabs are an information-efficiency hack.
- **Reference game**. Spelunky (4×4 grid of rooms drawn from typed templates), Enter the Gungeon, Dead Cells.
- **Novice failure mode**. Authoring too few rooms — players notice repetition fast.
- **Priority**. Must-know for action roguelikes.
- **Source**. Derek Yu — Spelunky GDC 2011 postmortem (`youtube.com/watch?v=RiDy6CgBKqs`); Boss Fight Books "Spelunky" by Derek Yu.

### Cyclic dungeon generation (Dormans)
- **Brief**. Generate a *graph of cycles* first (nodes = rooms with roles, edges = connections, two paths between key nodes), then realize it spatially.
- **Why-drilled**. L1: produces dungeons with meaningful loops, locked-and-key puzzles, shortcut backtracks. L2: the cycle is the unit of design pattern application — "lock here, key earlier in the loop" is one rewrite rule. First principle: gameplay-meaningful structure comes from authoring the *graph topology* before the geometry, not the other way around.
- **Reference game**. Unexplored, Unexplored 2.
- **Novice failure mode**. Trying it before you've shipped a simple BSP dungeon.
- **Priority**. Should-know (advanced).
- **Source**. Joris Dormans, "Cyclic Generation" Chapter 11 in *Procedural Content Generation in Games Design* (CRC Press, 2017); BorisTheBrave "Dungeon Generation in Unexplored."

### Seed-based reproducibility
- **Brief**. Every run derives from a single integer seed; the same seed produces the same world.
- **Why-drilled**. L1: lets players share runs and lets you reproduce bugs. L2: requires that *every* RNG call descend from the seeded generator — incidental `Math.random()` calls poison determinism. First principle: a procedural game is a pure function `(seed, inputs) → world`; impurity here is a category error.
- **Reference game**. Every roguelike from NetHack to Spelunky.
- **Novice failure mode**. Using the engine's global RNG and wondering why bug reports can't be reproduced. In Godot, instantiate `RandomNumberGenerator` per system, seed it, hold the reference.
- **Priority**. Must-know.
- **Source**. Cogmind/Grid Sage Games "Working with Seeds" blog; RogueBasin "Random number generator".

### Run economy: currency layering
- **Brief**. In-run currency (gold, souls) resets at death; meta currency (darkness, cells, plasm) carries between runs into permanent upgrades.
- **Why-drilled**. L1: progress survives losing. L2: meta currency accrues even on bad runs, smoothing the engagement curve so failure isn't dead time. First principle: variable-ratio reinforcement (Skinner) requires *some* signal each session — pure permadeath punishes randomness too harshly to retain players.
- **Reference game**. Hades (Darkness/Keys/Gemstones via Mirror of Night), Dead Cells (Cells), Rogue Legacy.
- **Novice failure mode**. One currency that does both jobs, leaving players who lose feeling they got nothing.
- **Priority**. Must-know.
- **Source**. Hades Fandom Wiki "Mirror of Night"; Frostilyte "Arcana Cards & Meta-Progression in Hades 2"; Greg Kasavin GDC Podcast ep. 16.

### Difficulty scaling with depth
- **Brief**. Enemy HP/damage as a function of floor depth, run length, or kills.
- **Why-drilled**. L1: linear scaling bores, exponential scaling breaks. L2: a player's power tends toward step-function increases (item drops), while enemy power tends toward continuous; if curves don't meet, runs trivialize or collapse. First principle: scaling is a budget reconciliation problem between two heterogeneous distributions, not a single curve.
- **Reference game**. Slay the Spire (act-based discrete jumps); Binding of Isaac (fixed difficulty per floor with rare modifiers).
- **Novice failure mode**. Multiplying everything by `1.1^depth` and shipping. Always have *some* breakpoints.
- **Priority**. Must-know.
- **Source**. Anthony Giovannetti, "Slay the Spire: Metrics Driven Design and Balance" — GDC 2019 (`gdcvault.com/play/1025731`).

### Loot tables, weighted random, pity timers
- **Brief**. Pick from a weighted list; track recent draws to bias toward unseen items ("smart RNG"); guarantee rare items after N failures (pity timer).
- **Why-drilled**. L1: pure RNG produces streaks players experience as broken. L2: humans systematically misjudge independence — six common drops in a row feels rigged even if independent. First principle: perceived fairness ≠ statistical fairness; designers ship the perception, not the math (Jeong Hyeon-Uk's three-layer framing — fixed, semi-random, pure RNG).
- **Reference game**. Diablo III pity affixes; XCOM 2 hidden hit-chance bonuses; Hades guarantees one new boon variant before repeating.
- **Novice failure mode**. Naive `random.choice` everywhere; rare items literally never seen by 30% of playtesters.
- **Priority**. Must-know.
- **Source**. Medium "Designing Fair RNG in Roguelikes"; Studio Xehryn loot drops blog; Indieklem "How to design better randomness in video games."

### Utility AI
- **Brief**. Each possible action computes a numeric utility from world state via response curves; AI picks the highest-scoring action (or weighted-random among top-N).
- **Why-drilled**. L1: produces "smart-looking" NPCs without explosive state machines. L2: response curves smooth decision boundaries — the agent's behavior shifts gradually as inputs change rather than snapping at thresholds. First principle: it's mathematical modeling of preference (decision theory / utility theory from economics), so adding a new consideration is *one curve*, not N×M state transitions.
- **Reference game**. The Sims, RimWorld, Caves of Qud.
- **Novice failure mode**. Tuning curves blindly without inspecting them — utility AI demands debug visualization.
- **Priority**. Must-know for sims.
- **Source**. Dave Mark & Kevin Dill, "Improving AI Decision Modeling Through Utility Theory" GDC 2010 (`media.gdcvault.com/gdc10/slides/MarkDill_ImprovingAIUtilityTheory.pdf`); Mark, *Behavioral Mathematics for Game AI*.

### GOAP (Goal-Oriented Action Planning)
- **Brief**. Each action has preconditions and effects; A*-search backward from a goal state to find a valid action sequence.
- **Why-drilled**. L1: emergent multi-step plans (NPC opens door because path requires it). L2: separating *what NPCs want* from *what actions exist* lets designers add an action and have all goals automatically use it. First principle: planning is search over a state space — the same algorithm that solves pathfinding solves task sequencing if you abstract the graph.
- **Reference game**. F.E.A.R. (2005, Jeff Orkin).
- **Novice failure mode**. Using GOAP for behaviors a behavior tree solves in 5 nodes. The 80-page Orkin paper is a warning, not a tutorial.
- **Priority**. Should-know (don't start here).
- **Source**. Jeff Orkin, "Three States and a Plan" / "Applying GOAP to Games" — GDC 2006; "Building the AI of F.E.A.R. with Goal Oriented Action Planning" — Game Developer.

### Behavior trees
- **Brief**. Tree of nodes returning Success/Failure/Running; selectors try children until success, sequences require all children, decorators modify results.
- **Why-drilled**. L1: visual, debuggable, modular. L2: Running state lets multi-frame actions (move-to, animate) compose without callbacks. First principle: it's structured programming for AI (sequence = `;`, selector = `||`) rendered as data, so it's trivially serializable and tweakable.
- **Reference game**. Halo 2 introduced; ubiquitous since.
- **Novice failure mode**. Letting BTs grow into 200-node monsters when utility AI would handle the same problem with 12 considerations.
- **Priority**. Must-know.
- **Source**. Beehave addon (`github.com/bitbrain/beehave`) — Godot 4 native, MIT license, GDScript.

### Steering behaviors (Reynolds)
- **Brief**. Combine forces — seek, flee, separation, alignment, cohesion, wander, pursue, evade — to drive movement.
- **Why-drilled**. L1: cheap, frame-rate-independent, naturally smooth. L2: emergent flocking comes from three local rules per agent — no global plan needed. First principle: complex group motion is the *summation* of independent local force vectors; emergence is a closed-form consequence of vector addition.
- **Reference game**. Half-Life squad combat; basically every game with a swarm.
- **Novice failure mode**. Using steering for grid-based games where A* on tiles is correct.
- **Priority**. Must-know for action games with crowds.
- **Source**. Craig Reynolds, "Steering Behaviors For Autonomous Characters" GDC 1999 (`red3d.com/cwr/steer/gdc99/`).

### Pathfinding (A*, JPS, NavServer)
- **Brief**. A* finds optimal paths over weighted graphs using a heuristic; Jump Point Search prunes uniform-cost grids; navigation meshes abstract free space into polygons.
- **Why-drilled**. L1: NPCs navigate without scripting. L2: the heuristic admissibility property guarantees optimality — break it and you get fast but wrong paths. First principle: pathfinding is shortest-path on a graph; the engineering question is what *graph representation* fits your world (grid → A*/JPS, open space → navmesh).
- **Reference game**. Every game with NPCs.
- **Novice failure mode**. Re-pathing every frame, chewing CPU. Path once, repath on event.
- **Priority**. Must-know.
- **Source**. Godot 4 docs: `docs.godotengine.org/en/stable/tutorials/navigation/navigation_introduction_2d.html`; Red Blob Games A* interactive guide; KidsCanCode "Pathfinding on a 2D Grid."

### Influence maps
- **Brief**. Grid where each cell accumulates threat/desire/scent values; AI samples to make tactical choices.
- **Why-drilled**. L1: cheap area-control reasoning ("where is the front line?"). L2: temporal smoothing (decay rate) prevents AI thrashing at boundary cells. First principle: it's a discretized scalar field — gradient operations like "go where my-influence > threat" become sample-and-compare.
- **Reference game**. RTS games (Supreme Commander, StarCraft AI), kiting in MOBAs.
- **Novice failure mode**. Updating the whole grid every frame at the same rate. Update strategic maps at 1Hz, tactical at 5Hz.
- **Priority**. Should-know for strategy games.
- **Source**. Dave Mark, "Modular Tactical Influence Maps" — *Game AI Pro 2*, Chapter 30 (`gameaipro.com/GameAIPro2/GameAIPro2_Chapter30_Modular_Tactical_Influence_Maps.pdf`).

### Feedback loops (positive/negative)
- **Brief**. Positive loops amplify a state (snowball: kills → killstreak → more kills); negative loops dampen it (rubber-band: losing → better items → catch-up).
- **Why-drilled**. L1: positive ends games faster, negative ends them more fairly. L2: their *ratio* across phases is the pacing curve. First principle: feedback loops are control-theory feedback; positive feedback has unstable equilibrium, negative has stable.
- **Reference game**. Civilization positive (technology snowball) + negative (anarchy from over-expansion); Mario Kart blue shell.
- **Novice failure mode**. Pure positive feedback, snowballing the early lead so the rest of the run is autopilot.
- **Priority**. Must-know.
- **Source**. Machinations.io "Feedback Loops"; Wolfire Games "Feedback In Game Design."

### AI Storyteller (event director)
- **Brief**. A meta-agent reads world state and decides which events to fire to maximize narrative interest.
- **Why-drilled**. L1: produces narrative pacing without scripting. L2: divorces *what happens* from *when* — the same events differ in tension based on the player's current state. First principle: drama needs alternation (rest → tension → relief); a director enforces this where pure RNG would not.
- **Reference game**. RimWorld (Cassandra Classic / Phoebe Chillax / Randy Random), Left 4 Dead's AI Director (RimWorld's stated influence).
- **Novice failure mode**. Calling timed event spawns a "storyteller." It needs to *read state* (wealth, colony health, last-event-time) and adapt.
- **Priority**. Should-know.
- **Source**. Tynan Sylvester, "The Simulation Dream" (2013, `tynansylvester.com`); Sylvester, *Designing Games* (O'Reilly).

### Permadeath vs lives vs runs-as-context
- **Brief**. Pure permadeath (Rogue, NetHack) ends story on death. Lives (Spelunky's pseudo-permadeath) extend a single timeline. Hades-style "death is canon" continues the narrative.
- **Why-drilled**. L1: permadeath weights consequences; meta-context softens loss. L2: permadeath without meta retains hardcore audience but loses casuals; meta without permadeath loses tension. First principle: stakes and accumulation are tradeoffs on the same axis (loss aversion); the genre split is which side of the curve you target.
- **Reference game**. NetHack (pure), Hades (meta + narrative continuity).
- **Novice failure mode**. Pretending permadeath is a hardcore choice when really the game has meta progression — be honest about it.
- **Priority**. Must-know.
- **Source**. Greg Kasavin GDC Podcast ep. 16; Frostilyte "Arcana Cards" essay.

### Item synergies and broken combos
- **Brief**. Allow asymmetric power spikes when item combinations interact multiplicatively.
- **Why-drilled**. L1: makes every run different and creates "highlight reel" moments. L2: variance in run power *is the product*; without spectacular successes there's nothing to chase. First principle: information theory — each run's outcome must carry enough novelty (high entropy of trajectory) to remain rewatchable.
- **Reference game**. Binding of Isaac (Brimstone + Tammy's Head etc.), Slay the Spire (Snecko Eye).
- **Novice failure mode**. Balancing every item flat — "every run feels the same" review.
- **Priority**. Must-know.
- **Source**. McMillen interviews via GameGrin and Steam News; "Every Choice is a Gamble" — kokutech.com.

### Tick-based vs frame-based simulation
- **Brief**. Sims update on discrete ticks (fixed time-step world clock); action games update each render frame.
- **Why-drilled**. L1: ticks make save/load and determinism feasible. L2: serializing entity state at sim scale (DF: thousands of dwarves, organs per dwarf) requires fixed-step, grid-aligned data; floating-point per-frame state cannot be replayed. First principle: determinism requires discrete time and integer/fixed-point math; physics floats break replay.
- **Reference game**. Dwarf Fortress, Factorio (deterministic tick lockstep multiplayer).
- **Novice failure mode**. Deciding on tick-vs-frame after writing the simulation. It's not retrofittable.
- **Priority**. Must-know for sims.
- **Source**. Tarn Adams interviews — Game Developer Q&A; PC Gamer "42% towards simulating existence."

## Must-include shortlist

A novice attempting any emergence-driven 2D Godot game must internalize these:

1. **Seed-based determinism end-to-end.** Every RNG call routes through a seeded `RandomNumberGenerator`. No `randf()` shortcuts. Bug reports and run sharing depend on it.
2. **Pick one procgen approach and master it.** BSP for dungeons, CA for caves, prefab-grid for action roguelikes. Don't combine three before shipping one.
3. **Two currencies, two timescales.** In-run resource that resets, meta currency that persists. Hades is the textbook.
4. **Smart RNG with pity timers and "no repeats" buffers.** Players quit on perceived unfairness more than on actual difficulty.
5. **Behavior trees (Beehave) before utility AI before GOAP.** Climb the complexity ladder only when the previous tier breaks.
6. **Feedback loop awareness.** Identify your snowball; install at least one rubber-band so it isn't terminal.
7. **Tick-based simulation if it's a sim.** Save/load is a phase-1 requirement, not phase-3.
8. **Item synergy variance is a feature, not a bug.** Anti-balance is what makes runs memorable.

## Procedural-gen — recommended starting algorithm

**For a novice's first 2D roguelike: BSP dungeon with hand-authored room interiors.**

The recursion tree gives you connectivity for free, so you skip the failure mode that wastes a month — generating maps where rooms aren't reachable. You can tune split ratios and minimum room size as two scalars and watch results change predictably, which builds the *feel* for procgen tuning that no other algorithm teaches as cleanly. Each leaf rectangle is sized to fit one of a dozen hand-authored room templates, so the procgen system inherits the legibility of human design while the BSP supplies layout variety.

Cellular automata is the obvious second project — same skeleton (carve a grid, post-process for connectivity, populate) but the post-processing is what teaches you graph algorithms (flood-fill, largest connected component). Wave Function Collapse is the *third* algorithm, not the first; it's slow, debugging adjacency rules is miserable, and most novices don't have the art-direction sample inputs that make WFC pay off. Cyclic generation (Dormans) is the fourth — only when you have a clear gameplay reason to want loops and locks.

Spelunky's prefab-grid is the right choice for action-roguelike platformers specifically (Derek Yu's GDC 2011 postmortem walks through the 4×4 grid with typed templates), but it requires authoring 30+ rooms before the first dungeon plays well, so it's heavier upfront than BSP.

## Commonly oversold

- **Wave Function Collapse.** Beautiful demos hide that it's slow, brittle to rule conflicts, and overkill for most dungeon layouts. Use it for tile-art continuity, not whole-level structure.
- **GOAP.** Orkin's F.E.A.R. paper is famous because the implementation was hard, not because the result was unattainable other ways. A well-tuned behavior tree with utility selectors covers ~95% of games that "need GOAP."
- **Full Dwarf-Fortress simulation.** Adams has been at it since 2002. Complexity at that depth is a one-developer-decade project. Sylvester's RimWorld explicitly traded simulation depth for character-count and storyteller curation — and shipped.
- **Pure permadeath without meta.** Hardcore audiences love it; the median player needs accumulation. Hades-style meta + Spelunky-style room mastery is the dominant modern pattern for a reason.
- **Rubber-banding (negative feedback).** Overdone, it punishes skill and feels unfair. Mario Kart works because the game is short; in a 30-minute roguelike run, aggressive rubber-banding makes mastery meaningless.
- **Influence maps in small games.** They're an RTS tool. A 2D dungeon crawler with 5 enemies in a room doesn't need a tactical map.

## Cross-references

- **Agent 5 (game feel)**: smart RNG and pity timers are felt as fairness, not understood as math; perception is the deliverable.
- **Agent 7 (AI/animation)**: Beehave behavior trees, utility AI, and steering behaviors are the implementation layer for any reactive NPC.
- **Agent 9 (UI/UX)**: meta-progression UI (Mirror of Night) is the persistent menu where the run loop is tuned; deserves UX investment equal to combat.
- **Agent 13 (audio)**: AI Storyteller logic should drive music intensity (RimWorld's tension layers respond to storyteller state).
- **Agent 17 (shipping)**: seed sharing is a community feature — release a seed-of-the-day for retention.

## Sources

- [End-to-End Procedural Generation in 'Caves of Qud' — GDC Vault](https://www.gdcvault.com/play/1026313/Math-for-Game-Developers-End)
- [Tile-Based Map Generation using Wave Function Collapse in 'Caves of Qud' — GDC Vault](https://gdcvault.com/play/1026263/Math-for-Game-Developers-Tile)
- [Tapping into the potential of procedural generation in Caves of Qud — Game Developer](https://www.gamedeveloper.com/design/tapping-into-the-potential-of-procedural-generation-in-caves-of-qud)
- [Q&A: Dissecting the development of Dwarf Fortress with creator Tarn Adams](https://www.gamedeveloper.com/design/q-a-dissecting-the-development-of-i-dwarf-fortress-i-with-creator-tarn-adams)
- [Dwarf Fortress' creator on how he's 42% towards simulating existence — PC Gamer](https://www.pcgamer.com/dwarf-fortress-creator-on-how-hes-42-towards-simulating-existence/)
- [How RimWorld fleshes out the Dwarf Fortress formula — Game Developer](https://www.gamedeveloper.com/design/how-i-rimworld-i-fleshes-out-the-i-dwarf-fortress-i-formula)
- [Tynan Sylvester — Designing Games book site](https://tynansylvester.com/book/)
- [Red Blob Games — Amit Patel](https://www.redblobgames.com/)
- [WaveFunctionCollapse — Maxim Gumin GitHub](https://github.com/mxgmn/WaveFunctionCollapse)
- [Wave Function Collapse Explained — BorisTheBrave](https://www.boristhebrave.com/2020/04/13/wave-function-collapse-explained/)
- [Cyclic Dungeon Generation — Joris Dormans (procedural-generation tumblr)](https://procedural-generation.isaackarth.com/2016/12/22/joris-dormans-cyclic-dungeon-generation-ive.html)
- [Unexplored's Secret: 'Cyclic Dungeon Generation' — Game Developer](https://www.gamedeveloper.com/design/unexplored-s-secret-cyclic-dungeon-generation-)
- [The Cellular Automaton Method for Cave Generation — Jeremy Kun](https://www.jeremykun.com/2012/07/29/the-cellular-automaton-method-for-cave-generation/)
- [Procedural Level Generation Using a Cellular Automaton — Kodeco](https://www.kodeco.com/2425-procedural-level-generation-in-games-using-a-cellular-automaton-part-1)
- [The Spelunky HD Postmortem — YouTube](https://www.youtube.com/watch?v=RiDy6CgBKqs)
- [Spelunky — Boss Fight Books (Derek Yu)](https://bossfightbooks.com/products/spelunky-by-derek-yu)
- [Roguelikes and narrative design with Hades creative director Greg Kasavin — GDC](https://gdconf.com/article/roguelikes-and-narrative-design-with-hades-creative-director-greg-kasavin-gdc-podcast-ep-16/)
- [Mirror of Night — Hades Wiki](https://hades.fandom.com/wiki/Mirror_of_Night)
- [Improving AI Decision Modeling Through Utility Theory — Mark & Dill GDC 2010 PDF](https://media.gdcvault.com/gdc10/slides/MarkDill_ImprovingAIUtilityTheory.pdf)
- [Building the AI of F.E.A.R. with GOAP — Game Developer (Jeff Orkin)](https://www.gamedeveloper.com/design/building-the-ai-of-f-e-a-r-with-goal-oriented-action-planning)
- [Goal-Oriented Action Planning: Ten Years Old and No Fear! — GDC Vault](https://www.gdcvault.com/play/1022019/Goal-Oriented-Action-Planning-Ten)
- [Beehave — behavior tree AI for Godot](https://github.com/bitbrain/beehave)
- [Steering Behaviors For Autonomous Characters — Craig Reynolds GDC 1999](https://www.red3d.com/cwr/steer/gdc99/)
- [Boids — Wikipedia](https://en.wikipedia.org/wiki/Boids)
- [Godot 4 — 2D navigation overview](https://docs.godotengine.org/en/stable/tutorials/navigation/navigation_introduction_2d.html)
- [Pathfinding on a 2D Grid — Godot 4 Recipes (KidsCanCode)](https://kidscancode.org/godot_recipes/4.x/2d/grid_pathfinding/index.html)
- [Modular Tactical Influence Maps — Dave Mark, Game AI Pro 2](http://www.gameaipro.com/GameAIPro2/GameAIPro2_Chapter30_Modular_Tactical_Influence_Maps.pdf)
- [Slay the Spire: Metrics Driven Design and Balance — GDC Vault](https://www.gdcvault.com/play/1025731/-Slay-the-Spire-Metrics)
- [Designing Fair RNG in Roguelikes — Medium (Jeong Hyeon-Uk)](https://medium.com/@JeongHyeonUk/designing-fair-rng-in-roguelikes-balancing-luck-and-skill-7b967230e961)
- [Working with Seeds — Cogmind / Grid Sage Games](https://www.gridsagegames.com/blog/2017/05/working-seeds/)
- [Random number generator — RogueBasin](https://www.roguebasin.com/index.php/Random_number_generator)
- [Game systems: Feedback loops — Machinations.io](https://machinations.io/articles/game-systems-feedback-loops-and-how-they-help-craft-player-experiences)
- [Feedback In Game Design — Wolfire Games Blog](http://blog.wolfire.com/2010/04/Feedback-In-Game-Design)
- [Edmund McMillen Interview — GameGrin](https://www.gamegrin.com/articles/edmund-mcmillen-interview/)
- [Every Choice is a Gamble — kokutech.com (Binding of Isaac analysis)](https://www.kokutech.com/blog/gamedev/design-patterns/unique-mechanics/the-binding-of-isaac)
