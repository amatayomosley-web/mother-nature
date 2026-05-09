# Puzzle / Logic — Research Report

## Domain summary

Puzzle design diverges from action design at the level of what the player's brain is doing. Action games are nervous-system loops — perception, decision, motor output, all under continuous time pressure. Puzzle games are static cognition: time stops while the player builds a model of the rule system in their head and tests it against the level's constraints. This inverts almost every design instinct from action games. Difficulty isn't tuned by giving stronger enemies; it's tuned by what the player has not yet realised. Failure isn't death; failure is a flawed model of the rules. And the dopamine hit isn't from execution under threat — it's from the gamma-wave burst of insight when a held mental model snaps into alignment with the puzzle's hidden structure (Goldsmiths/Medical News Today, "aha moment" research). The designer's job is therefore epistemic: shape what the player understands, in what order, with what evidence.

## Mechanic + design-pattern catalog

### Rule clarity (the "why" contract)
- **Brief**: When a puzzle state changes, the player must understand *why* it changed. If they push a sausage and a tile flips for unclear reasons, the contract breaks.
- **Why-drilled**: L1 — opaque rules feel unfair. L2 — players solve by guessing rather than reasoning. First principle: a puzzle is a hypothesis-test feedback loop; if the test result is illegible, the loop stops producing learning, and the player is reduced to brute-force.
- **Reference game**: Stephen's Sausage Roll (every step is mechanically explicit), The Witness (panels solve or don't, no in-between).
- **Novice failure mode**: hiding state in invisible variables, or making one rule depend on an off-screen condition. Players cannot debug what they cannot see.
- **Priority**: core
- **Source**: Lavelle/Increpare PuzzleScript design, Blow's "Truth in Game Design".

### Mechanic depth over breadth
- **Brief**: One mechanic with 50 implications beats ten mechanics with five each. Stephen's Sausage Roll famously adds *no new mechanics* across its full runtime — it just unfolds existing ones.
- **Why-drilled**: L1 — players forget rules they've only seen once. L2 — shallow mechanics can't combine to produce genuinely novel situations. First principle: human working memory is ~4 chunks; the value of a mechanic is the size of the implication tree it spawns, not its surface novelty.
- **Reference game**: Stephen's Sausage Roll, Snakebird, Patrick's Parabox, Baba Is You.
- **Novice failure mode**: "feature creep as content" — each level adds a new wrinkle instead of recombining the old ones.
- **Priority**: core
- **Source**: Thinky Games on SSR's influence; Hempuli on Baba's emergent levels.

### Aha moment design
- **Brief**: Each puzzle should funnel the player toward a specific cognitive shift — the moment a piece of the rule system clicks. Multiple puzzles can share a target aha; ordering controls when it lands.
- **Why-drilled**: L1 — solved-by-luck doesn't feel good. L2 — the dopamine spike comes from insight, not output. First principle: fMRI/EEG studies show insight produces gamma-wave bursts in the right anterior superior temporal gyrus and dopamine release into the nucleus accumbens; design should aim *at* that neural event, not at "the puzzle is hard."
- **Reference game**: The Witness (perception-based ahas), Baba Is You (rule-rewriting ahas).
- **Novice failure mode**: stacking obstacles instead of revealing structure; difficulty without insight.
- **Priority**: core
- **Source**: Goldsmiths University dopamine research; Brainfacts.org "Aha! the eureka moment".

### Hint pacing (the subtle nudge)
- **Brief**: Show the *path* without solving. A doorway pointing at the relevant block. A panel placement that frames a needed mechanic in isolation. Counter-example: Snakebird shipped with no hints originally — Snakebird Complete (Switch) added a visual nudge system to widen the audience.
- **Why-drilled**: L1 — players who skip miss the aha. L2 — players who're stuck for an hour quit. First principle: the satisfaction-vs-frustration curve is sharp; frustration scales nonlinearly with time stuck, and a well-placed hint preserves the insight while short-circuiting the rage.
- **Reference game**: Snakebird Complete, The Witness's environmental layout.
- **Novice failure mode**: text hints that spoil; or no hint at all and player abandons.
- **Priority**: important
- **Source**: AppUnwrapper review of Snakebird; Nintendo's Snakebird Complete description.

### Failure design (undo as genre-definer)
- **Brief**: Instant unlimited undo (or autoreset) changes the felt genre. Without it, you're playing a memory-and-execution game; with it, you're playing pure logic.
- **Why-drilled**: L1 — punishment for failed hypotheses suppresses experimentation. L2 — experiments are the *only* way the rule model gets refined. First principle: in cognition-first games, the cost of trying a wrong move must be near-zero or the player stops generating moves; you cannot reason about a system you're afraid to poke.
- **Reference game**: Snakebird ("automatically undoes your last move instead of making you start over"), Baba Is You, Patrick's Parabox.
- **Novice failure mode**: copying action-game punishment (death, level restart) into a puzzle game.
- **Priority**: core
- **Source**: AppUnwrapper Snakebird review; Hempuli Baba devlog.

### Puzzle ordering / level-of-expertise curve
- **Brief**: Introduce mechanic in isolation → small variation → combination with a prior mechanic → twist that subverts the assumed model. This is the "ABC" or "introduction–development–reframing" pattern most thinky designers cite.
- **Why-drilled**: L1 — combined mechanics confuse if either piece is half-learned. L2 — players need a baseline mental model before twists land. First principle: learning is differential — a twist only registers as a twist if the prior expectation was solid.
- **Reference game**: The Witness (panels cluster by rule), Patrick's Parabox.
- **Novice failure mode**: introducing two mechanics in one puzzle — player can't tell which one caused the failure.
- **Priority**: core
- **Source**: Traynor GDC 2024 "System-Centric Puzzle Design"; Wikipedia/Game Developer Witness analysis.

### Discoverability without exposition
- **Brief**: The world signals there's more without saying so — a visible-but-unreachable area, a panel with a symbol you don't yet understand, a door that won't open with the rules you have. The Witness's island is the canonical example.
- **Why-drilled**: L1 — players who don't see hidden content miss most of the design. L2 — but explicit waypoints kill discovery joy. First principle: curiosity is dopaminergic in itself; an unexplained-but-visible affordance is a self-renewing motivator without breaking immersion.
- **Reference game**: The Witness, A Monster's Expedition (Hazelden).
- **Novice failure mode**: gating with explicit "you need item X" prompts.
- **Priority**: important
- **Source**: Marc ten Bosch / Blow IndieCade 2011 talk; Hazelden Draknek interview (TouchArcade).

### Puzzle vs puzzle-game distinction
- **Brief**: A *puzzle* is a single logic problem with a target solution. A *puzzle-game* is a system whose rules generate puzzles. Choose which you're making before content design; it changes everything.
- **Why-drilled**: L1 — hand-authored puzzles cost ~hours each; system-authored puzzles cost rule-design time amortized. L2 — players read tone differently between the two. First principle: a system with internal consistency invites exploration; a sequence of bespoke puzzles invites completion. They are different products.
- **Reference game**: Sokoban (system), Professor Layton (sequence).
- **Novice failure mode**: hand-authoring a system game and exhausting yourself; or system-authoring without testing implications.
- **Priority**: core
- **Source**: Blow & ten Bosch 2011 IndieCade — "Designing to Reveal the Nature of the Universe"; ten Bosch "Nature as Designer".

### Abstract vs themed puzzles
- **Brief**: Abstract = pure shapes and rules (Sokoban). Themed = same logic mapped onto a metaphor (Stephen's Sausage Roll: sausages need cooking). Theme makes mechanics memorable but constrains rule space.
- **Why-drilled**: L1 — players remember "the sausage that flips" better than "the L-shaped block." L2 — but theme also imports irrelevant intuitions ("can I eat the sausage?"). First principle: representation drives intuition; pick the representation whose intuitions match the rules you actually want the player to learn.
- **Reference game**: Sokoban (abstract), SSR / Snakebird / A Monster's Expedition (themed).
- **Novice failure mode**: theme that fights mechanics ("why does the brick float?").
- **Priority**: important
- **Source**: Lavelle on PuzzleScript & SSR.

### Solution uniqueness vs multiplicity
- **Brief**: Some puzzles have one valid solution path (most Witness panels); others admit many (open-ended Sokoban variants). Uniqueness sharpens the aha; multiplicity widens the audience.
- **Why-drilled**: L1 — single-solution puzzles feel "right" when solved but punish near-misses. L2 — multi-solution puzzles welcome diverse thinkers but dilute the insight payoff. First principle: a single solution is a stronger signal that the player understood the *intended* rule; multiplicity trades signal strength for accessibility.
- **Reference game**: The Witness (mostly unique), Baba Is You (often multiple paths).
- **Novice failure mode**: accidentally allowing trivial solutions ("I just brute-forced past your puzzle").
- **Priority**: important
- **Source**: Increpare/Lavelle on minimalism.

### Difficulty curves (rest beats and reflection)
- **Brief**: Linear difficulty curves fail in puzzle games because cognitive load doesn't recover with passive play. Insert intentionally easy puzzles after hard ones — the "rest beat."
- **Why-drilled**: L1 — three hard puzzles in a row burns players out. L2 — the recovery puzzle reaffirms competence and refreshes attention. First principle: cognitive endurance is bounded; sustained high difficulty produces failure-state exhaustion, not learning.
- **Reference game**: Patrick's Parabox post-iteration, Snakebird (criticized for *not* doing this — "stumped by level 20 of 50").
- **Novice failure mode**: ordering puzzles by chronological-design-order rather than difficulty; assuming linear ramp.
- **Priority**: important
- **Source**: Traynor GDC talk; AppUnwrapper Snakebird critique.

### Grid + polyomino logic
- **Brief**: Grid-based movement is the workhorse representation for 2D puzzles because it makes state discrete and undoable. Polyomino pieces (snakebirds, sausages) add positional constraint without continuous physics.
- **Why-drilled**: L1 — continuous physics introduces noise in the rule signal. L2 — players can't reason about state they can't enumerate. First principle: discrete state = enumerable state = reasonable state.
- **Reference game**: Sokoban, Snakebird, Tetris-as-puzzle, every PuzzleScript game.
- **Novice failure mode**: choosing free-physics in Godot when grid would do; underestimating tilemap setup; forgetting collision rules at grid boundaries.
- **Priority**: core (genre-defining for 2D)
- **Source**: Godot Recipes "Grid-based movement"; PuzzleScript Documentation.

### Hidden mechanics / invisible rules
- **Brief**: Some puzzle games reveal that a rule you didn't know existed has been there all along (Witness's environmental panels, SSR's late realisations). Powerful but high-risk: must be *fair on rediscovery*.
- **Why-drilled**: L1 — surprise rules feel like cheating. L2 — but rediscovered fairness produces the strongest aha. First principle: a hidden rule is fair iff (a) the evidence was always present and (b) the player can verify the rule mechanistically after the reveal. Otherwise it's a cheat.
- **Reference game**: The Witness (environmental puzzles), SSR (late-game revelations).
- **Novice failure mode**: hiding rules that depend on knowledge external to the level.
- **Priority**: genre-specific (advanced)
- **Source**: Game Developer "How the Witness breaks the rules of puzzle design".

### Onboarding without text (PuzzleScript-style)
- **Brief**: The first level of each mechanic is so constrained that only the intended interaction is possible. The level *is* the tutorial.
- **Why-drilled**: L1 — text tutorials interrupt the puzzle headspace. L2 — players who solved the rule themselves remember it better. First principle: encoding-by-discovery is deeper than encoding-by-instruction (memory consolidation literature on insight).
- **Reference game**: PuzzleScript example games, SSR's first sausage, Baba's first "Baba is You" panel.
- **Novice failure mode**: relying on tooltips; designing first-puzzle content without forcing the intended interaction.
- **Priority**: core
- **Source**: PuzzleScript prelude documentation; Lavelle increpare.com PuzzleScript announcement.

### Negative space in puzzle design
- **Brief**: Empty cells aren't filler — they're load-bearing. The shape of *where pieces aren't* defines what moves are legal.
- **Why-drilled**: L1 — novice designers fill levels until they look "interesting." L2 — clutter obscures the relevant constraint. First principle: a puzzle's information content is the difference between "what could be there" and "what is there"; negative space *is* signal.
- **Reference game**: SSR, Hazelden's Sokobond.
- **Novice failure mode**: filling levels with decorative obstacles; treating empty grid cells as unfinished.
- **Priority**: important
- **Source**: Lavelle PuzzleScript design notes.

### Procedural vs handcrafted puzzles
- **Brief**: Handcrafted = high quality per puzzle, finite content. Procedural = unlimited content, quality cap. Solvability guarantees (backward generation, constraint solvers) are non-trivial.
- **Why-drilled**: L1 — generators that produce unsolvable puzzles violate the "why" contract instantly. L2 — generators that produce trivial puzzles waste player time. First principle: a puzzle's quality lives in its *constraints' interaction*, which generators rarely tune; handcrafting is how a designer encodes the specific cognitive shift they want.
- **Reference game**: Most thinky games are handcrafted (SSR, Witness, Snakebird, Parabox); procedural attempts (Hexcells daily, etc.) are accessory content.
- **Novice failure mode**: trying to procgen as the main pillar to escape the cost of authoring.
- **Priority**: genre-specific
- **Source**: Game Wisdom "3 Failings of Procedurally Generated Game Design"; MIT Game Lab PuzzleDice research.

### Hint systems (linear, costed, optional skip)
- **Brief**: Three patterns: (1) linear escalating hints (Professor Layton), (2) in-game cost (currency for hints), (3) optional skip after N failures (modern accessibility). Each shapes player behaviour differently.
- **Why-drilled**: L1 — different players want different help levels. L2 — forcing one model alienates the others. First principle: hint design is audience design; the *shape* of help is content.
- **Reference game**: Professor Layton (linear cost), Snakebird Complete (visual nudge), The Witness (none — by design).
- **Novice failure mode**: bolting on a hint system at the end without thinking about what the hint *teaches*.
- **Priority**: important
- **Source**: Snakebird Complete Nintendo description; AppUnwrapper review.

### Playtesting puzzles (solver bias)
- **Brief**: The designer cannot test their own puzzle. Once you know the solution, you cannot un-see it. Fresh testers — who have never seen the level — are the only valid signal. Watch silently; if you help, you're not testing the product.
- **Why-drilled**: L1 — designers think their puzzles are clearer than they are. L2 — they ship puzzles that are obvious to them and impossible to anyone else. First principle: knowledge is asymmetric; the designer's mental model is contaminated by the solution and cannot be decontaminated.
- **Reference game**: documented by Traynor (GDC 2024) — Parabox shipped with smoother curve specifically because of fresh-tester rounds.
- **Novice failure mode**: solo-testing only; helping testers when stuck; ignoring the test because "they just didn't get it."
- **Priority**: core
- **Source**: Traynor GDC; Game Developer "Reflections on Playtesting and Puzzledorf".

## Must-include shortlist

A novice puzzle dev shipping their first 2D Godot game must internalise these or fail at the genre level:

1. **Rule clarity / why-contract** — every state change legible.
2. **Depth over breadth** — one mechanic, many implications; no feature creep.
3. **Instant undo** — the genre's defining affordance; build it in week one.
4. **ABC ordering** — introduce, vary, combine; never two new mechanics in one puzzle.
5. **Onboarding by level design** — first puzzle of each mechanic is a forced tutorial.
6. **Fresh playtesters** — solver bias is real; you cannot test your own design.
7. **Aha as design target** — design *toward* the cognitive shift, not just toward "harder."
8. **Grid first** — discrete state, enumerable moves, trivial undo.

## Commonly oversold

- **Procedural generation** — sold as a way to escape authoring cost; in thinky-puzzle, it caps quality and breaks the why-contract more often than it helps. Almost every great puzzle game is handcrafted. Skip until shipping a second project.
- **Story / narrative wrappers** — novices spend weeks on dialogue systems for puzzle games. Most of the genre's classics have minimal narrative (SSR, Snakebird, Parabox). The puzzles are the story.
- **Multiple solution paths as "freedom"** — sounds player-friendly but dilutes aha clarity and triples test surface. Single-solution puzzles are easier to design *and* hit harder.
- **Hint systems built in advance** — design the puzzles first; the hint system's shape depends on which puzzles people actually get stuck on, which you only learn from playtesting.
- **Branching level-select / hub-world** — adds scope and confuses curve design. Linear ordering with rest beats outperforms hub layouts for first projects.
- **Custom shaders / juice** — visual polish is action-game thinking; in puzzle games, clarity > flair, and over-animation hides state changes.
- **Tutorial dialog / text instruction** — the level should teach the rule. Text indicates a design failure.

## Cross-references

- **Action mechanics** (agent 8): puzzle games can borrow timing and execution but with caution — the moment execution failure punishes, the genre flips. See action-mechanics work for input handling primitives if the puzzle has any reactive component (rare in pure logic games).
- **Tilemap / grid systems** (engine-agnostic agents): grid math is the substrate of 2D puzzle design. See Godot 4 TileMap research.
- **Cognitive design / accessibility** (UX agents): hint design and undo design intersect with accessibility patterns.
- **Procedural generation** (PCG agents): only relevant for genre-specific applications (daily puzzles, infinite Sokoban variants); main-content procgen is generally a trap for novices in this genre.

## Sources

- [Designing to Reveal the Nature of the Universe – The Witness (Blow)](http://the-witness.net/news/2011/11/designing-to-reveal-the-nature-of-the-universe/)
- [Designing to Reveal the Nature of the Universe – Marc ten Bosch blog](https://marctenbosch.com/news/2011/11/designing-to-reveal-the-nature-of-the-universe/)
- [Jonathan Blow & Marc ten Bosch IndieCade 2011 — cai.zone summary](https://cai.zone/2012/01/jonathan-blow-marc-ten-bosch/)
- [Jonathan Blow — Truth In Game Design (YouTube)](https://www.youtube.com/watch?v=C5FUtrmO7gI)
- [The Truth in Game Design – Game Design Advance](https://gamedesignadvance.com/?p=2084)
- [IndieCade: Inside Jonathan Blow's Puzzle Design Process — Game Developer](https://www.gamedeveloper.com/design/indiecade-inside-jonathan-blow-s-puzzle-design-process)
- [The Witness (2016 video game) — Wikipedia](https://en.wikipedia.org/wiki/The_Witness_(2016_video_game))
- [Gaming Moments #01: How the Witness breaks the rules of puzzle design — Game Developer](https://www.gamedeveloper.com/design/gaming-moments-01-how-the-witness-breaks-the-rules-of-puzzle-design-and-gets-away-with-it)
- [The Methodology Behind The Witness — One More Continue](https://onemorecontinue.com/methodology-behind-the-witness/)
- [PuzzleScript announcement — increpare.com](https://www.increpare.com/2013/10/puzzlescript/)
- [Stephen's Sausage Roll — increpare.com](https://www.increpare.com/2016/04/stephens-sausage-roll/)
- [10 years of grilling: Stephen's Sausage Roll — Thinky Games](https://thinkygames.com/features/10-years-of-grilling-stephens-sausage-roll-remains-one-of-the-most-influential-puzzle-games-ever-created/)
- [Stephen's Sausage Roll — Wikipedia](https://en.wikipedia.org/wiki/Stephen's_Sausage_Roll)
- [Stephen Lavelle — Independent Games Wiki](https://tig.fandom.com/wiki/Stephen_Lavelle)
- [Increpare Games — Wikipedia](https://en.wikipedia.org/wiki/Increpare_Games)
- [PuzzleScript Rules — boristhebrave.com](https://www.boristhebrave.com/2024/06/10/puzzlescript-rules/)
- [PuzzleScript Documentation — Prelude](https://games.increpare.com/clone_avoid/Documentation/prelude.html)
- [Alan Hazelden — Games / draknek.org](https://alan.draknek.org/)
- [Alan Hazelden's articles — Thinky Games](https://thinkygames.com/contributors/alan-hazelden/)
- [Draknek Interview: Alan Hazelden — TouchArcade](https://toucharcade.com/2024/07/19/draknek-interview-lok-digital-thinky-puzzle-games-sokobond-express-mobile-steam-deck-apple-arcade/)
- [Marc ten Bosch — mtbdesignworks](http://marctenbosch.com/)
- [Nature as Designer — mtbdesignworks](https://marctenbosch.com/news/2014/10/nature-as-designer-there-was-only-one-way-to-design-miegakure/)
- [Miegakure — Wikipedia](https://en.wikipedia.org/wiki/Miegakure)
- [Designing Baba Is You's rule-writing system — Game Developer](https://www.gamedeveloper.com/design/designing-i-baba-is-you-i-s-delightfully-innovative-rule-writing-system)
- [Baba Is You — Wikipedia](https://en.wikipedia.org/wiki/Baba_Is_You)
- [Hempuli's blog — Sokpop recommendations](https://www.hempuli.com/blog/index.php?rule=id&ruleid=652)
- [Designing the mind-bending puzzles in Patrick's Parabox — Game Developer](https://www.gamedeveloper.com/design/patrick-s-parabox-)
- [System-Centric Puzzle Design in Patrick's Parabox — GDC Vault](https://gdcvault.com/play/1034415/System-Centric-Puzzle-Design-in)
- [System-Centric Puzzle Design in Patrick's Parabox — YouTube](https://www.youtube.com/watch?v=HAvS-RwkjdA)
- [Linelith devlog — Patrick Traynor](https://patricktraynor.itch.io/linelith/devlog)
- [Snakebird review — AppUnwrapper](https://www.appunwrapper.com/2016/09/10/snakebird-review/)
- [Snakebird — Thinky Games](https://thinkygames.com/games/snakebird/)
- [Tail Meets Head — Electron Dance (Snakebird analysis)](https://www.electrondance.com/tail-meets-head/)
- [Sokpop Collective — Wikipedia](https://en.wikipedia.org/wiki/Sokpop_Collective)
- [Indie troupe Sokpop Collective on releasing 100 games — Game Developer](https://www.gamedeveloper.com/design/indie-troupe-sokpop-collective-on-releasing-100-games-in-five-years)
- [Reflections on playtesting and Puzzledorf — Game Developer](https://www.gamedeveloper.com/game-platforms/reflections-on-playtesting-and-puzzledorf)
- [Puzzle Testing Process — Post Curious](https://www.getpostcurious.com/post/puzzle-testing-process)
- [Aha! moments linked to dopamine — Goldsmiths University](https://www.gold.ac.uk/news/aha-moment-dopamine/)
- [The Neuroscience of "Aha!" Moments — Inkwell Games](https://blog.inkwellgames.com/posts/the-neuroscience-of-aha-moments-in-puzzle-solving)
- [Aha! The Eureka Moment — Brainfacts.org](https://www.brainfacts.org/neuroscience-in-society/the-arts-and-the-brain/2019/aha-the-eureka-moment-and-creative-problem-solving-in-the-brain-022619)
- [What happens in the brain during a 'eureka!' moment — Medical News Today](https://www.medicalnewstoday.com/articles/321638)
- [3 Failings of Procedurally Generated Game Design — Game Wisdom](https://game-wisdom.com/critical/procedural-game-design)
- [Procedural Puzzles as a Design Tool — MIT Game Lab](http://gamelab.mit.edu/research/puzzledice/)
- [Grid-based movement — Godot 4 Recipes (KidsCanCode)](https://kidscancode.org/godot_recipes/4.x/2d/grid_movement/index.html)
- [Puzzle Game Design Principles — Game Design Skills](https://gamedesignskills.com/game-design/puzzle/)
- [Designing Video Game Puzzles — Game Developer](https://www.gamedeveloper.com/design/designing-video-game-puzzles)
- [Puzzle Design: a guide for game designers — Bruna Delfino, Medium](https://medium.com/@brdelfino.work/puzzle-design-a-guide-for-game-designers-72251d60a56a)
