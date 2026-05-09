# Game Design Theory — Research Report

## Domain summary

Game design theory is the body of frameworks designers use to predict and shape player experience before code exists. It sits one layer above mechanics ("how the dash works") and one layer below feel ("does the dash feel crunchy"). Five threads run through the literature: (1) games are systems whose emergent behavior is more important than their parts, (2) the player builds a mental model and the design's job is to teach that model legibly, (3) success is measured in player emotion, not feature count, (4) constraint and cutting are the engine of indie shippability, and (5) accessibility is design quality, not charity. This report compresses ~12 primary sources into a catalog tuned for a solo Godot 4 novice making 2D games.

## Concept catalog

### MDA Framework
- **Brief**: Mechanics (rules + code) produce Dynamics (run-time behavior) which produce Aesthetics (player emotion). Designers build left-to-right; players experience right-to-left.
- **Why-drilled**: L1 — separate "the rules" from "what happens" from "how it feels." L2 — the same mechanic can produce different aesthetics depending on context (shared resource = cooperation in one game, betrayal in another). First principle — emergent systems mean you cannot infer experience from inspecting rules; you must simulate the dynamics.
- **Novice failure mode**: Iterating on mechanics while expecting predictable aesthetics ("I added jump, why isn't it fun?"). The middle layer is invisible until playtested.
- **Priority**: core. **Confidence**: high.
- **Sources**: Hunicke, LeBlanc, Zubek, MDA paper (GDC 2001-04).

### Eight Kinds of Fun (Aesthetics taxonomy)
- **Brief**: Sensation, Fantasy, Narrative, Challenge, Fellowship, Discovery, Expression, Submission. A vocabulary for naming what your game is for.
- **Why-drilled**: L1 — "fun" is too coarse; eight target-emotions disambiguate. L2 — one game can hit multiple, but trying to hit all eight produces incoherence. First principle — design coherence emerges from a deliberate aesthetic target; mechanics that don't serve the target are noise.
- **Novice failure mode**: Designing without naming the target aesthetic, then confused why playtesters react inconsistently.
- **Priority**: core. **Confidence**: high.
- **Sources**: Hunicke et al., MDA paper; LeBlanc's "8 Kinds of Fun" rants at algorithmancy.8kindsoffun.com.

### Schell's Elemental Tetrad
- **Brief**: Every game has four constituents — Mechanics, Story, Aesthetics (look/sound/feel), Technology. All four must support the same experience.
- **Why-drilled**: L1 — game elements are interdependent; changing tech changes mechanics, changing aesthetics changes story tone. L2 — incoherence between any two breaks immersion. First principle — humans pattern-match holistically; one off-key element pollutes perception of the others.
- **Novice failure mode**: Treating art and code as separable production streams, then bolting them together at the end.
- **Priority**: important. **Confidence**: high.
- **Sources**: Schell, *The Art of Game Design: A Book of Lenses*, 3rd ed.

### Schell's Lenses (the 100+ tool)
- **Brief**: A library of question-sets, each a different angle for inspecting your design (Lens of Surprise, Lens of Fun, Lens of Curiosity, etc.).
- **Why-drilled**: L1 — designers get tunnel vision; rotating lenses surfaces blind spots. L2 — most lenses ask "what is the player feeling RIGHT NOW at this beat?" forcing experiential audit. First principle — design problems are perspective-bound; the same artifact looks different through different epistemic frames.
- **Novice failure mode**: Trying to "use all the lenses" — paralysis. Or ignoring them entirely.
- **Priority**: nice-to-have for novice; reference tool, not curriculum. **Confidence**: high.
- **Sources**: Schell, *Art of Game Design*.

### Affordances vs Signifiers (Norman)
- **Brief**: Affordance = what an object permits (a ledge can be climbed). Signifier = the perceptual cue telling the player it's climbable (yellow paint, chalk marks, lit edge).
- **Why-drilled**: L1 — players must perceive what they CAN do, not just have the option. L2 — Norman's 2013 distinction sharpened "affordance" because designers conflated capability with communication. First principle — perception is inferential; without a signifier the affordance might as well not exist.
- **Novice failure mode**: Building interactable objects with zero visual differentiation from scenery. "But the prompt appears when you walk up" — they never walked up.
- **Priority**: core. **Confidence**: high.
- **Sources**: Norman, *The Design of Everyday Things* (rev. 2013).

### Loops and Arcs (Daniel Cook)
- **Brief**: A loop is a repeatable mental-model-update cycle (act → observe → adjust). An arc is a one-shot delivery (cutscene, scripted reveal). Games nest loops at moment-to-moment, encounter, session, and meta-progression scales.
- **Why-drilled**: L1 — replayability and depth come from loops; story beats come from arcs. L2 — loops produce wisdom (transferable model), arcs produce knowledge (context-bound). First principle — neuroplasticity rewards prediction-error cycles, not unidirectional input.
- **Novice failure mode**: Building only arcs ("level after level of new content") then burning out at scope. Or building loops without arcs and producing texture-less grind.
- **Priority**: core. **Confidence**: high.
- **Sources**: Daniel Cook, "Loops and Arcs" (Lostgarden, 2012); "Skill Atom" essay.

### Skill Atom
- **Brief**: A unit of mastery: action → simulation update → feedback → mental model update. The smallest learnable thing.
- **Why-drilled**: L1 — design granularity at the skill atom forces "what am I teaching here?" per beat. L2 — once mastered, a skill stops being fun and must compose into the next. First principle — Koster's "fun is learning" — pleasure spikes at the moment of pattern-grok and decays toward zero.
- **Novice failure mode**: Adding mechanics that have no atom — they don't teach anything new because they're variants of an existing skill.
- **Priority**: important. **Confidence**: high.
- **Sources**: Daniel Cook, Lostgarden; Koster, *Theory of Fun*.

### Theory of Fun (Koster)
- **Brief**: Fun is the feedback signal of pattern-recognition during low-stakes learning. Once the pattern is mastered, fun drops; the brain demands a new variable.
- **Why-drilled**: L1 — explains why grinding stops being fun. L2 — explains why "easy" games bore you and "impossible" ones frustrate you (no pattern available to grok). First principle — evolutionary cognition rewards energy-efficient pattern compression.
- **Novice failure mode**: Designing one mechanic and stretching it for ten hours. Mastery hits at hour two; the rest is dust.
- **Priority**: core. **Confidence**: high.
- **Sources**: Koster, *A Theory of Fun for Game Design* (2nd ed., 2013).

### Flow Channel (Csikszentmihalyi → game design)
- **Brief**: A diagonal corridor on a challenge-vs-skill plot. Above = anxiety, below = boredom. Hold the player in the channel by ramping challenge as skill grows.
- **Why-drilled**: L1 — difficulty should rise with player capacity, not with clock-time. L2 — flow is bimodal: skill is acquired in steps, so the curve is stair-stepped, not linear. First principle — autotelic engagement collapses outside a narrow arousal band.
- **Novice failure mode**: Linear difficulty curve assuming linear skill acquisition. Late-game spikes either kill players or reveal the early game was undertuned.
- **Priority**: core. **Confidence**: high.
- **Sources**: Csikszentmihalyi, *Flow*; Jenova Chen MFA thesis (flOw, 2006).

### Risk/Reward and Faucet-Drain Economy
- **Brief**: Resources flow in (faucets) and out (drains/sinks). Decisions are interesting when each option has both upside and downside denominated in those resources.
- **Why-drilled**: L1 — without sinks, the economy collapses to stockpiling; without risk, choice collapses to dominant strategies. L2 — interesting decisions live where expected-value is close across options. First principle — utility theory: agents only meaningfully choose between roughly-equivalent expected payoffs.
- **Novice failure mode**: Building rewards without sinks (gold means nothing); building risks without compensating reward (pure punishment).
- **Priority**: important. **Confidence**: high.
- **Sources**: Daniel Cook, "Value Chains" (Lostgarden, 2021); Machinations.io economy primers.

### Signal-to-Noise in Feedback
- **Brief**: Every visual/audio cue competes for the player's attention. Important state (HP low, hit landed, danger imminent) must dominate; cosmetic flourish must recede.
- **Why-drilled**: L1 — players have ~7±2 attentional slots. L2 — when everything flashes, nothing reads. First principle — perceptual hierarchy is a finite resource; designers allocate it like a budget.
- **Novice failure mode**: "Juicing" everything equally — particles on every button press, screen-shake on every hit. The critical signal disappears in noise.
- **Priority**: core. **Confidence**: high.
- **Sources**: Norman; UX literature on signal-noise; cross-applies to game-feel literature.

### Ludonarrative Dissonance
- **Brief**: Conflict between what the game's story says and what its mechanics reward. (BioShock: story preaches free will; mechanics force a path.)
- **Why-drilled**: L1 — players believe what they DO over what they're TOLD. L2 — dissonance breaks the make-believe contract. First principle — cognition prioritizes proprioceptive evidence (my actions) over declarative input (cutscenes).
- **Novice failure mode**: Writing a peaceful protagonist whose primary verb is murder. Or a stealth game whose easiest path is combat.
- **Priority**: important. **Confidence**: high.
- **Sources**: Clint Hocking, "Ludonarrative Dissonance in Bioshock" (Click Nothing, 2007).

### Onboarding / Tutorialization (show-don't-tell)
- **Brief**: Teach mechanics through level geometry that forces the player to discover them, not through text dumps. Mega Man 1's Cut Man stage is the canonical novice example — it front-loads safe practice with each hazard.
- **Why-drilled**: L1 — discovered knowledge sticks; told knowledge evaporates. L2 — context-of-use teaching binds the rule to the situation it applies in. First principle — episodic memory encodes better than semantic when motor action is involved.
- **Novice failure mode**: A 90-second text tutorial nobody reads, then a level that assumes mastery.
- **Priority**: core. **Confidence**: high.
- **Sources**: Mark Brown, GMTK "Super Mario 3D World's 4-Step Level Design"; analysis of Mega Man 1 Cut Man stage.

### Failure Design (failing forward, soft fail)
- **Brief**: A failed attempt should advance state, not reset it. Soft fail = consequence + new option (lost stealth, now must fight). Hard fail = game over screen.
- **Why-drilled**: L1 — death-loops cause attrition; failure feedback sustains engagement. L2 — failure that teaches is fuel; failure that punishes is friction. First principle — operant conditioning: punishment without learnable signal extinguishes behavior, including "keep playing."
- **Novice failure mode**: Long retry loops between fail and resume. Loss of progress on death without compensating gain.
- **Priority**: core. **Confidence**: high.
- **Sources**: Josh Bycer, "How to Design Failure"; Game Wisdom "Hierarchy of Fail States."

### Scope Discipline (cut without losing identity)
- **Brief**: Most indie games die from scope, not bad design. The discipline is to identify the 1-2 mechanics that ARE the game and ship those at quality, cutting everything else.
- **Why-drilled**: L1 — every feature has a long tail of integration cost. L2 — features are not additive; each new system multiplies bugs in the others. First principle — Brooks's Law plus combinatorial complexity: cost grows superlinearly with feature count.
- **Novice failure mode**: MoSCoW with everything in "Must." Refusal to cut. "Just one more system."
- **Priority**: core. **Confidence**: high.
- **Sources**: Tynan Sylvester, *Designing Games* (RimWorld scoping); Wayline/Tono indie scope-creep essays.

### Constraint as Creative Driver
- **Brief**: Limitations (resolution, palette, single-screen, one-button, 48-hour jam) sharpen design. Removing the safety of "we could add anything" forces commitment to what matters.
- **Why-drilled**: L1 — infinite option-space paralyzes; bounded space forces invention. L2 — constraints become the game's identity (Pico-8, Game Boy, GameJam aesthetic). First principle — psychology of choice: cognitive load drops as the option space narrows, freeing capacity for depth-work.
- **Novice failure mode**: Treating their first project as their lifetime opus. Refusing the constraint of "small."
- **Priority**: core. **Confidence**: high.
- **Sources**: Anna Anthropy, *Rise of the Videogame Zinesters*; Ian Schreiber, *Game Design Concepts*.

### Theme as Design Constraint
- **Brief**: A theme isn't garnish — it's a filter that decides which mechanics are in or out. "Game about loneliness" → multiplayer is out, even if multiplayer would be fun.
- **Why-drilled**: L1 — coherence comes from a unifying lens. L2 — theme converts arbitrary mechanic choice into a binary "fits / doesn't." First principle — Gestalt: humans perceive unified meaning, so disunified parts produce noise.
- **Novice failure mode**: Picking a theme that's purely aesthetic skin. The gameplay would feel identical with any reskin.
- **Priority**: important. **Confidence**: high.
- **Sources**: Sylvester, *Designing Games*; Schreiber's course on themed rules.

### Player Types (Bartle, with caveats)
- **Brief**: Achiever (points/completion), Explorer (map/lore), Socializer (other players), Killer (dominance). 1996 MUD model — useful loose vocabulary, NOT a predictive taxonomy.
- **Why-drilled**: L1 — different players want different things from the same game. L2 — modern critique: types overlap, sample was MUD-specific, and most players are mixed. First principle — motivation is multidimensional; any 4-bucket model is a lossy projection.
- **Novice failure mode**: Designing for one type without acknowledging the spectrum, OR worshipping the taxonomy as if it were measured.
- **Priority**: nice-to-have. **Confidence**: medium (the types are folk-vocabulary; the empirical research is thin).
- **Sources**: Bartle 1996 MUD paper; critiques summarized at Tuts+ "Why It Doesn't Apply to Everything."

### Emergence (simple rules → complex behavior)
- **Brief**: Carefully chosen interactions between a few systems produce surprising stories. RimWorld's tantrum-cook-cooler chain is a designed emergence, not chaos.
- **Why-drilled**: L1 — depth from interaction beats depth from feature count. L2 — emergent designs are cheaper to build but harder to predict and balance. First principle — combinatorial state space grows multiplicatively; small rule sets can saturate human surprise.
- **Novice failure mode**: Building 50 disconnected mechanics that don't talk to each other, expecting emergence. Emergence requires intersection, not catalog.
- **Priority**: important. **Confidence**: high.
- **Sources**: Sylvester, *Designing Games* + "The Simulation Dream" essay; emergent gameplay literature.

### Accessibility (Game Accessibility Guidelines, Basic tier)
- **Brief**: A floor of must-haves: remappable controls, adjustable text size, colorblind-safe palette, captions/subtitles, separate volume sliders, no info conveyed by sound or color alone.
- **Why-drilled**: L1 — most accessibility wins benefit all players (controller remap, text size). L2 — basic-tier items are cheap if designed in early, expensive if bolted on. First principle — universal design: barriers don't sort cleanly into "disabled vs not"; they're a continuum.
- **Novice failure mode**: Treating a11y as a polish-pass feature. By then, hardcoded UI/font/keybind systems make it 5x more expensive.
- **Priority**: core (the floor, not the ceiling). **Confidence**: high.
- **Sources**: gameaccessibilityguidelines.com Basic tier.

## Frameworks — pick-one verdict

For a novice solo 2D Godot dev, **MDA wins**. Reasoning:

- **MDA** is one diagram. Mechanics (your code), Dynamics (what playtesters report happens), Aesthetics (the target emotion you named). Three boxes, one direction. It maps directly onto the iteration loop a solo dev actually runs: tweak code → run game → feel something. The 8 Kinds of Fun rider gives you a target vocabulary so playtest feedback isn't "fun / not fun" but "I aimed for Discovery, they reported Submission."
- **Schell's Lenses** are a reference library, not a method. 100+ lenses are paralyzing for a first project. Save them for project two, when you have a working artifact to interrogate. Use the Elemental Tetrad lens (#7) only — it covers ~80% of the value for novices.
- **Sylvester's framework** (mechanics-as-emotion-engine + production discipline) is excellent but presupposes you've already shipped a small game. The book's strongest material is on cutting and on emergent simulation, both of which require an in-progress codebase to apply.

Verdict: **Use MDA + 8 Kinds of Fun as your operating framework. Read Schell as reference. Save Sylvester for project two.**

## Must-include shortlist

The novice MUST internalize these before designing their first game:

1. **MDA**: name your aesthetic target, build mechanics for it.
2. **Theory of Fun / Skill Atom**: design teachable units; mastery decays so plan replacement.
3. **Affordances + Signifiers**: every interactable needs a visible cue.
4. **Flow Channel**: ramp difficulty with skill, not with time.
5. **Loops + Arcs**: most of the game is loops; sprinkle arcs for texture.
6. **Failing Forward / Soft Fail**: failure should reduce friction, not progress.
7. **Scope Discipline**: cut to the 1-2 mechanics that are the game.
8. **Constraint as Driver**: shrink the box deliberately; ship the small thing.
9. **Signal-to-Noise**: critical state must dominate the screen.
10. **Accessibility Basic Tier**: remap, text size, colorblind, captions, volume — design in from day one.

## Commonly oversold

- **Schell's full 100-lens battery**: hugely valuable as reference, paralyzing as method for a first project. Pick one or two lenses per design review.
- **Bartle's player types**: overcited folk vocabulary. The empirical evidence is thin (MUD-specific 1996 sample); single-player 2D games rarely benefit from quadrant thinking.
- **Ludonarrative dissonance**: matters when you have a story. A novice 2D platformer with no narrative cannot be ludonarratively dissonant; the concept is critic-tier, not first-game-tier.
- **Deep economy/faucet-drain modeling**: relevant for RPGs, idle games, F2P. A linear platformer or single-screen puzzler does not need a designed economy and the discipline of building one wastes weeks.
- **Multi-axis player taxonomies (BrainHex, Quantic Foundry)**: same critique as Bartle. Build for yourself first; segment when you have telemetry.
- **MDA as a complete theory**: Hunicke et al. acknowledged 8 Kinds of Fun isn't exhaustive. Don't twist your design to fit one of the eight.

## Accessibility — minimum bar

The novice's first game should meet, at minimum:

1. **Remappable inputs** (keyboard + controller). Godot's InputMap makes this nearly free if planned. Hardcoded inputs are the #1 a11y blocker.
2. **Adjustable text size** OR sufficiently large default (~24px+ at 1080p, scale with resolution). Use Godot's Theme system, not per-label font sizes.
3. **No info conveyed by color alone**. Pair color with shape, position, or icon. Test with a grayscale filter.
4. **Captions/subtitles** for any speech or critical sound. Even if your game has no dialogue, sound effects that gate puzzles need a visual equivalent.
5. **Separate sliders** for music, SFX, and (if applicable) voice. One master slider is not enough.
6. **No essential information by sound alone**. Critical alerts need a visual signal.
7. **Pause must actually pause everything** (timers, music, animations) — not just open a menu.
8. **A way to skip or shorten unskippable cutscenes** on subsequent plays.

This is the floor, not a full audit. Every item is cheap if designed in at hour one; each becomes 5-20x more expensive if retrofitted post-launch.

## Cross-references

- **Game-feel agent (16)**: signal-to-noise overlaps; this report covers semantic clarity, agent 16 covers haptic feel.
- **Genre agents (platformer/metroidvania/RPG)**: economy/loops/arcs apply genre-specifically; this report is engine-agnostic.
- **Onboarding overlaps with level design**: tutorialization is technique-here, geometry-elsewhere.
- **Production agent**: scope discipline is theoretical here, project-managed there.

## Sources used

- [Hunicke, LeBlanc, Zubek — MDA paper (PDF)](https://users.cs.northwestern.edu/~hunicke/MDA.pdf)
- [MDA framework — Wikipedia](https://en.wikipedia.org/wiki/MDA_framework)
- [Marc LeBlanc — algorithmancy / 8 Kinds of Fun rants](http://algorithmancy.8kindsoffun.com/)
- [Schell — *The Art of Game Design: A Book of Lenses* (3rd ed.)](https://www.routledge.com/The-Art-of-Game-Design-A-Book-of-Lenses-Third-Edition/Schell/p/book/9781138632059)
- [Elemental Tetrad — Skeleton Code Machine summary](https://www.skeletoncodemachine.com/p/elemental-tetrad)
- [Koster — *A Theory of Fun for Game Design*](https://www.theoryoffun.com/)
- [Theory of Fun summary (Shortform)](https://www.shortform.com/summary/a-theory-of-fun-for-game-design-summary-raph-koster)
- [Sylvester — *Designing Games: A Guide to Engineering Experiences*](https://tynansylvester.com/book/)
- [Sylvester — "The Simulation Dream"](https://tynansylvester.com/2013/06/the-simulation-dream/)
- [Norman — *The Design of Everyday Things* (PDF)](https://media.aanda.psu.edu/sites/media/aa/files/documents/norman_design-of-everyday-things.pdf)
- [Daniel Cook — "Loops and Arcs" (Lostgarden)](https://lostgarden.com/2012/04/30/loops-and-arcs/)
- [Daniel Cook — "Value Chains" (Lostgarden)](https://lostgarden.com/2021/12/12/value-chains/)
- [Csikszentmihalyi flow + game design (Game Developer mag)](https://www.gamedeveloper.com/design/the-flow-applied-to-game-design)
- [Jenova Chen — Flow in Games (MFA thesis PDF)](https://www.jenovachen.com/flowingames/Flow_in_games_final.pdf)
- [Hocking — "Ludonarrative Dissonance in Bioshock"](https://clicknothing.com/2007/10/07/ludonarrative-d/)
- [Bartle taxonomy — Wikipedia](https://en.wikipedia.org/wiki/Bartle_taxonomy_of_player_types)
- [Tuts+ — Bartle critique "Why It Doesn't Apply to Everything"](https://code.tutsplus.com/bartles-taxonomy-of-player-types-and-why-it-doesnt-apply-to-everything--gamedev-4173a)
- [Anna Anthropy — *Rise of the Videogame Zinesters*](http://triple-a-games.biz/books/videogame-zinesters/)
- [Ian Schreiber — *Game Design Concepts* course](https://gamedesignconcepts.wordpress.com/)
- [Game Accessibility Guidelines — Basic tier](https://gameaccessibilityguidelines.com/basic/)
- [Game Accessibility Guidelines — main](https://gameaccessibilityguidelines.com/)
- [Mark Brown / Game Maker's Toolkit](https://gamemakerstoolkit.com/)
- [Josh Bycer — "How to Design Failure in Video Games"](https://medium.com/@GWBycer/how-to-design-failure-in-video-games-d15713830d84)
- [Game Wisdom — "Hierarchy of Fail States"](https://game-wisdom.com/critical/hierarchy-of-fail-states-game-design)
- [Wayline — "Soft Fails" essay](https://www.wayline.io/blog/soft-fails-game-design)
- [Wayline — Scope creep in indie games](https://www.wayline.io/blog/scope-creep-indie-games-avoiding-development-hell)
- [Machinations — Game economy primer](https://machinations.io/articles/what-is-game-economy-design)
