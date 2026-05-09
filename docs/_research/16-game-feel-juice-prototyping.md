# Game Feel + Juice + Prototyping — Research Report

## Domain summary

Game feel is the polish layer where mechanics earn their right to be played a thousand times. Steve Swink defines it as "real-time control of virtual objects in a simulated space, with interactions emphasized by polish." Each phrase is load-bearing: real-time (under ~100ms perceptual floor); virtual objects (the avatar exists only because the simulation says so); simulated space (gives motion meaning); polish (the audiovisual scaffolding that makes the locus of control feel embodied rather than abstract). Feel is the hardest thing to teach because it is irreducibly perceptual — it must be felt through the controller, never specced in a doc — and because it emerges from a thousand uncredited micro-decisions whose individual contributions are imperceptible but whose absence is immediately felt as "this game feels dead." The novice trap: assuming feel comes after the mechanic ships. It does not. Feel is what determines whether the mechanic is worth shipping at all.

## Concept catalog

### Game feel — the Swink definition
- **Brief**: Real-time control of virtual objects in a simulated space, with interactions emphasized by polish. Six manipulable areas: input, response, context, polish, metaphor, rules.
- **Why-drilled**: L1 polish stacks on response stacks on input. L2 each layer is a feedback loop with the player's nervous system. First principle: human motor-perception requires the loop to close in <100ms or the brain stops attributing causation to the player's own action — locus of control collapses, the avatar feels "driven" not "embodied."
- **Reference**: Swink, *Game Feel: A Game Designer's Guide to Virtual Sensation* (2008), Ch. 1.
- **Novice failure mode**: Treats "feel" as a synonym for "polish" and bolts juice onto a mushy core. Polish cannot rescue an unresponsive simulation.
- **Priority**: Top.

### Locus of control
- **Brief**: The player's felt sense that *they* caused what happened. Cinematic feel ≠ game feel; cutscenes and scripted slow-mo move the locus *off* the player.
- **Why-drilled**: L1 input → response causality must be unambiguous. L2 every uncalled-for delay between press and effect transfers attribution from player to system. First principle: agency is constructed from temporal contiguity (under ~100ms causation reads as self-caused; over ~250ms it reads as system-caused).
- **Reference**: Swink Ch. 1; Joerg et al., *How Responsiveness Affects Players' Perception in Digital Games*.
- **Novice failure mode**: Adding tween-in animations to player movement (250ms ease-in to walk) — feels "smooth" to designer, feels "drunk" to player.
- **Priority**: Top.

### The 100ms input-lag floor
- **Brief**: Below ~100ms total input-to-photon latency, action feels self-caused. Above 100ms, performance measurably drops; above 150ms feels sluggish; over 200ms is openly distracting.
- **Why-drilled**: L1 fixed perceptual threshold. L2 budget every hop: input poll, physics tick, render, monitor scan-out. First principle: motor-prediction loops in the cerebellum operate on tight temporal contiguity windows; outside them, prediction error accumulates.
- **Reference**: Joerg et al. (Clemson); Wikipedia *Input lag*; Wayline blog.
- **Novice failure mode**: Building game at uncapped framerate on dev hardware, shipping with 60Hz lock and 80ms render queue.
- **Priority**: Top.

### Polish vs juice
- **Brief**: Polish = removing rough edges so nothing breaks the illusion. Juice = excessive, even gratuitous feedback per input — coins explode, screen quakes, numbers pop. Polish is hygiene; juice is character.
- **Why-drilled**: L1 polish prevents subtraction of feel; juice adds feel. L2 they compound: juice on top of unpolished input still feels broken. First principle: dopamine timing — clustered, immediate, novel feedback events spike reward; delayed or uniform ones flatten it.
- **Reference**: Jonasson & Purho, *Juice It or Lose It* (GDC); Mark Brown, GMTK *Secrets of Game Feel*.
- **Novice failure mode**: Juice without polish — particles on a 200ms-input-lag jump.
- **Priority**: Top.

### Tweening and easing (don't snap)
- **Brief**: Position, scale, color transitions over 4–12 frames with non-linear easing curves rather than instant snaps. Bounce, ease-out, elastic.
- **Why-drilled**: L1 the eye reads sudden snaps as glitches. L2 easing curves let the system convey weight, urgency, mood. First principle: real-world motion always has acceleration; instant velocity changes read as "broken simulation."
- **Reference**: Jonasson & Purho; Eiserloh, *Fast and Funky 1D Nonlinear Transformations* (GDC 2015).
- **Novice failure mode**: All transitions linear; UI snaps in/out without bounce or ease.
- **Priority**: High.

### Screen shake
- **Brief**: Camera offset by a small randomized vector for 4–12 frames after impacts/firing.
- **Why-drilled**: L1 conveys impact magnitude without changing simulation. L2 hijacks peripheral motion-detection. First principle: motion in peripheral vision triggers attentional spike from the visual cortex's magnocellular pathway — feels "loud" pre-cognitively.
- **Reference**: Jan Willem Nijman, *The Art of Screenshake* (INDIGO 2013).
- **Novice failure mode**: Shake on every event (motion sickness); or shake without falloff (jitters indefinitely); or constant amplitude regardless of action weight.
- **Priority**: High.

### Hitstop / freeze frames
- **Brief**: Game pauses 3–10 frames the instant a hit lands, on both attacker and target.
- **Why-drilled**: L1 gives the eye time to register the collision. L2 implies the struck object had to "stop the swing" — sells force. First principle: motion blur and saccadic suppression mean fast collisions are perceptually invisible without a freeze; the freeze is when the brain actually sees it.
- **Reference**: Critpoints, *Hitstop/Hitfreeze/Hitlag*; Sakurai, Famitsu Vol. 490; Smash Bros hitlag conventions.
- **Novice failure mode**: No hitstop (Dark Souls 2 problem — hits feel weightless); or hitstop only on target, breaking input symmetry.
- **Priority**: High.

### Flash on hit
- **Brief**: Sprite renders all-white (or full-color) for 1–3 frames at moment of damage.
- **Why-drilled**: L1 unambiguous "I was hit" signal. L2 visible even when sprite is occluded by VFX. First principle: high-contrast luminance change is the strongest single signal in the visual system; the cortex prioritizes it.
- **Reference**: Vlambeer (Nuclear Throne, Luftrausers); Jonasson & Purho.
- **Novice failure mode**: Damage feedback only via HP-bar number — player cannot localize where damage came from.
- **Priority**: High.

### Squash and stretch
- **Brief**: Sprite scales non-uniformly (e.g., 0.8×1.2 squash on land, 1.2×0.8 stretch on launch).
- **Why-drilled**: L1 conveys mass and acceleration. L2 a 2D shortcut for the physics the simulation isn't running. First principle: this is the first of Disney's 12 — the most fundamental cue that an object has weight rather than being a flat decal.
- **Reference**: Thomas & Johnston, *The Illusion of Life* (1981); Chris Totten, *12 Principles for Game Animation*.
- **Novice failure mode**: Pixel-perfect rigid sprites on a platformer — character feels like a sliding decal.
- **Priority**: High.

### Anticipation and follow-through
- **Brief**: A wind-up before action (anticipation) and a continued motion after (follow-through). In games, must be *short* — 2–6 frames, not animated-film length, to keep responsiveness.
- **Why-drilled**: L1 telegraphs incoming action so the player parses it. L2 follow-through sells inertia. First principle: predictive vision uses prior frames to anticipate next; motion that arrives without wind-up is read as teleportation.
- **Reference**: Disney 12; Totten; Swink Ch. on response.
- **Novice failure mode**: Borrowing 12-frame anticipation from animated film — adds 200ms input lag, kills feel.
- **Priority**: Medium-High.

### Audio variation (pitch + sample)
- **Brief**: Randomize pitch ±10% and rotate through 3+ samples per repeated SFX (footstep, gunshot, hit).
- **Why-drilled**: L1 prevents auditory fatigue. L2 makes the world feel non-deterministic. First principle: the auditory cortex habituates to identical stimuli within 3–5 repeats; randomized variation keeps it engaged.
- **Reference**: Vlambeer Luftrausers/Nuclear Throne post-mortems; Nijman *Art of Screenshake* audio sections.
- **Novice failure mode**: One footstep sample on a 0.4s loop — drives players insane within 30 seconds.
- **Priority**: High.

### Particles and trails
- **Brief**: Short-lived secondary objects emitted on events (impacts, jumps, deaths) with their own physics and fade-out.
- **Why-drilled**: L1 amplifies the event. L2 leaves a temporal smear so the eye reconstructs the action. First principle: particles bridge the gap between what the system emitted and what the retina integrates over a 50–100ms persistence-of-vision window.
- **Reference**: Jonasson & Purho; Vlambeer.
- **Novice failure mode**: Large opaque particles obscure gameplay; particle lifetimes too long, screen becomes soup.
- **Priority**: High.

### Number-pop damage indicators
- **Brief**: Damage values float upward from hit point, scale-bounce, fade.
- **Why-drilled**: L1 quantifies the hit immediately. L2 differentiates crit/normal/weak via size and color. First principle: numeric feedback closes the cognitive loop between input and reward, sustaining engagement on repeat actions.
- **Reference**: Jonasson & Purho; ARPG/roguelike conventions (Diablo lineage).
- **Novice failure mode**: Numbers spawn at fixed UI position rather than world-space — disconnects from the action.
- **Priority**: Medium.

### Coyote time and jump buffering
- **Brief**: Coyote time: jump remains valid 4–6 frames after leaving a ledge. Jump buffer: jump press 4–6 frames before landing still triggers a jump on land.
- **Why-drilled**: L1 player's intent registers despite minor mistiming. L2 widens timing windows in the player's favor invisibly. First principle: human motor-input variance is ±50ms even for trained players; perfect-frame requirements punish neuromuscular reality, not skill.
- **Reference**: Maddy Thorson, *Celeste & Forgiveness* (Medium / X thread).
- **Novice failure mode**: Treating "missed jump" as player error when it was a 1-frame timing window the player physically cannot hit.
- **Priority**: Top (for any platformer or precision-input game).

### Corner correction / wiggle nudges
- **Brief**: When player bonks a corner or dashes near a ledge, the system silently nudges them onto the platform.
- **Why-drilled**: L1 invisible forgiveness. L2 reduces failure cases that aren't actually skill-testing. First principle: reading exact pixel collisions is harder than reading approximate ones; the simulation should match what the player *meant*, not what they *did*.
- **Reference**: Maddy Thorson, Celeste design notes.
- **Novice failure mode**: Pixel-perfect collision for the sake of "fairness" — feels stingy and arbitrary.
- **Priority**: High.

### Camera tilt / zoom / lookahead
- **Brief**: Camera leans into player direction, zooms slightly on action, looks ahead of movement.
- **Why-drilled**: L1 keeps relevant content on-screen. L2 amplifies dynamic moments. First principle: the camera *is* the player's body in the world; static cameras feel observational, dynamic cameras feel embodied.
- **Reference**: Eiserloh, *Juicing Your Cameras With Math* (GDC 2016).
- **Novice failure mode**: Static centered camera in a fast-moving game; or jittery camera that fights player input.
- **Priority**: Medium-High.

### Slow-mo on dramatic moments
- **Brief**: Time scale drops to 0.1×–0.5× for 0.3–1.5 seconds on key events (final hit, near-death).
- **Why-drilled**: L1 emphasizes the moment. L2 lets the player witness an event their reflexes couldn't otherwise parse. First principle: the brain prefers high-information moments at lower temporal density; slow-mo raises information per second of attention.
- **Reference**: Vlambeer; *Super Hot* / *SUPERHOT* — full-game slow-mo example.
- **Novice failure mode**: Slow-mo on routine events, robbing the dramatic ones; or slow-mo that hands control off to a cutscene (locus of control collapse).
- **Priority**: Medium.

### Sensory layering
- **Brief**: Visual + audio + haptic stacked in a 2–6 frame window so they fuse into a single "impact" event.
- **Why-drilled**: L1 redundancy makes the event unmissable. L2 multimodal binding enhances felt magnitude. First principle: cross-modal stimuli within ~50ms fuse into one perceptual event with sum > parts (the McGurk-style binding window).
- **Reference**: Swink Ch. on polish; Nijman.
- **Novice failure mode**: Visual at frame 0, audio at frame 8, controller rumble at frame 14 — three weak events instead of one strong one.
- **Priority**: High.

### The mute test
- **Brief**: Play the game with sound off. What still feels alive? Where does it die?
- **Why-drilled**: L1 isolates visual+input feel from audio scaffolding. L2 reveals which feedback channels are load-bearing. First principle: each channel must independently sell the action; if any one is doing all the work, the game is brittle to context (deaf players, headphones off, noisy room).
- **Reference**: Nijman talk; Swink usability chapter.
- **Novice failure mode**: Game feels great in the dev's headphones, dead on a streaming clip.
- **Priority**: Diagnostic — apply weekly.

### The no-art test
- **Brief**: Replace all sprites with colored rectangles. Does the input still feel responsive?
- **Why-drilled**: L1 isolates response curve from art. L2 surfaces whether the mechanic is interesting. First principle: if the rectangle isn't fun, the sprite won't save it. Aesthetics multiply feel; they don't generate it.
- **Reference**: Gabler rapid-prototyping ("Build the Toy First"); Vlambeer.
- **Novice failure mode**: Treating art as the proof-of-concept gate, then discovering at week 8 the core is dead.
- **Priority**: Top (gate this before any art investment).

### The 1-button test
- **Brief**: Reduce a mechanic to one button. If it still feels good, the mechanic is sound.
- **Why-drilled**: L1 stress-tests whether feel is in the mechanic or hidden in input complexity. L2 forces clarity. First principle: complexity hides design weakness — the cleanest expression reveals whether the underlying loop is fun.
- **Reference**: *Canabalt*, *Flappy Bird*; Pittman & Lewis indie talks.
- **Novice failure mode**: Building a 12-button move-list before the 1-button base is good.
- **Priority**: Diagnostic.

### Tweakable parameters via @export
- **Brief**: All feel-relevant numbers (jump height, hitstop frames, shake amplitude, dash distance) exposed in the editor and adjustable while running.
- **Why-drilled**: L1 enables real-time tuning without rebuild. L2 the gap between "I think this might feel better" and "yes/no" is the iteration loop. First principle: every saved second of iteration → more candidate values tested → better local optimum found in feel-space.
- **Reference**: Godot 4 `@export` + script hot-reload; Eiserloh on tunable systems.
- **Novice failure mode**: Hardcoding magic numbers; rebuilding 30s for every tweak.
- **Priority**: Top (foundation for everything else).

### Watching faces, not surveys
- **Brief**: Record playtester video + audio. Watch where they hesitate, lean in, sigh, swear, or visibly enjoy. Surveys lie; faces don't.
- **Why-drilled**: L1 self-report is filtered through politeness and recall bias. L2 the moment of confusion happens in <1s and is forgotten. First principle: feel is pre-verbal; players cannot accurately retrospect on what they felt.
- **Reference**: Observational playtesting literature (Ludogogy); games-user-research conventions.
- **Novice failure mode**: Asking "did it feel good?" — getting "yeah it was fun" — and learning nothing.
- **Priority**: Top.

## Recommended novice prototyping methodology

**The 7-day prototype** (Gabler, *How to Prototype a Game in Under 7 Days*, GDC 2006). Adapted for a Godot 4 novice:

- **Day 0 (pre-prototype, half-day)**: Pick *one* core verb. Jump. Shoot. Slash. Stack. Define what one button does. Write it on paper: "Press X → Y happens." If you can't write it in one sentence, the prototype isn't ready.
- **Day 1**: Build the toy. No goals, no failure state, no UI, no art. Colored rectangles. The verb works at 60fps with a tweakable `@export` for every relevant number (speed, jump height, gravity, friction). Mute test: does it feel alive yet? If no, fix tomorrow.
- **Day 2**: Tune the toy. Eight hours of nothing but adjusting `@export` values and feeling the result. Add one second mechanic only if it serves the first. **Kill criterion**: if at end of day 2 the rectangle still doesn't make you smile when you press the button, kill the prototype. Move on.
- **Day 3**: Add minimum context — one obstacle, one goal. Still rectangles. Add the first juice pass: tween, screen shake, flash on hit, hitstop. Test mute and no-art.
- **Day 4**: Sensory layering. Audio with pitch variation. Particles. Camera lookahead. Stack visual+audio events into ≤6-frame fusion windows. Re-tune the toy now that juice is on top — the numbers that felt right naked may feel sluggish dressed.
- **Day 5**: Playtest with one human (not yourself, not a developer). Record their face. Watch where they hesitate, where they smile. Don't talk. Don't explain.
- **Day 6**: Fix the three biggest issues from Day 5. Resist adding new features. The prototype's job is to validate one idea, not to ship.
- **Day 7**: Decision day. The prototype is either clearly worth a full project or clearly not. **Kill criteria**: (a) you cannot honestly say "this is fun" while playing, (b) the playtester didn't lean in once, (c) the core loop doesn't survive the no-art and mute tests, (d) you're already bored playing it. Kill it without ceremony. The 7-day prototype's value is the *learning*, not the build.

**Eiserloh's amendment** (implicit in the Experimental Gameplay tradition): heavy theming will not save bad design. If a mechanic isn't fun by Day 3, do not attempt to rescue it with story or art on Days 4–7.

## Juice checklist

Before shipping any 2D action, work through this list in order. Each check should be feelable — if you can't tell whether you have it, you don't.

- [ ] Input → response under 100ms total (measure with high-speed camera or LDAT if possible).
- [ ] All transitions tweened with non-linear easing, not snapped.
- [ ] Squash and stretch on jump, land, and impact.
- [ ] Anticipation frames (≤6) before any heavy action.
- [ ] Hitstop on every impact (3–10 frames; longer for heavier hits).
- [ ] Flash on hit (target turns white 1–3 frames).
- [ ] Screen shake, scaled to event weight, with falloff.
- [ ] Particles or trails on impact, jump, death.
- [ ] Number-pop or visual confirmation of any quantity change.
- [ ] Audio with pitch ±10% and ≥3 sample rotation per repeated SFX.
- [ ] Camera lookahead / tilt / mild zoom on action.
- [ ] Coyote time + jump buffer (if platformer).
- [ ] Corner correction / wiggle nudges.
- [ ] Visual + audio + haptic events stacked within a 6-frame window.
- [ ] Mute test passes — game still feels alive without sound.
- [ ] No-art test passes — input still feels good with rectangles.
- [ ] 1-button test passes — core loop works at minimum input width.

## Must-include shortlist

1. **Swink's three building blocks** — real-time control, simulated space, polish — as the mental model for every feel decision.
2. **The 100ms perceptual floor** — hard latency budget every system must respect.
3. **Locus of control vs cinematic feel** — non-negotiable; cutscenes and forced animation move the locus off the player.
4. **The juice catalog** (tween, particles, screen shake, hitstop, flash, squash/stretch, audio variation, number pop) — the minimum kit; everything else is variation on these.
5. **Coyote time + jump buffering + corner correction** — the Celeste school of invisible forgiveness, applicable to any precision input.
6. **Sensory layering within ~6 frames** — the multimodal binding rule that makes impacts feel like one event instead of three.
7. **The 7-day prototype with kill criteria** — Gabler's method, adapted.
8. **The mute / no-art / 1-button diagnostic tests** — apply weekly to detect when feel is being carried by the wrong layer.

## Commonly oversold

- **Motion blur** in 2D — eats CPU, obscures pixels, rarely sells motion better than trails.
- **Long anticipation animations** borrowed from animated film — adds input lag, breaks responsiveness.
- **Constant-amplitude screen shake** — habituates within seconds; without falloff and event-weight scaling, it adds nothing.
- **Slow-mo on routine events** — devalues the dramatic ones, flattens pacing.
- **Cinematic cutscenes for "impact"** — moves locus of control off the player; juice that doesn't reinforce the player's own action is decoration, not feel.
- **Heavy theming without core verification** — Gabler's "you can't polish a turd"; aesthetic before mechanic is the most expensive failure mode for novices.
- **Surveys over recordings** — self-report is filtered through politeness and recall.
- **Ultra-realistic physics simulation in lieu of faked physics with polish** — physics realism rarely pays back the latency and tuning cost; squash/stretch and easing fake what the player wanted to feel anyway.

## Cross-references

- Genre agents (platformer, top-down shooter, etc.) define *which* specific mechanics need feel; this report defines the *general theory* of how to make any mechanic feel good.
- The depth-gate / iteration-discipline agent connects to the prototyping methodology — feel emerges from iteration count, which depends on tooling that supports fast iteration (`@export`, hot reload, scene reload).
- Audio agent and pixel-art agent: the mute/no-art tests are diagnostic boundaries — each layer must independently carry weight.
- Playtesting agent: observational protocol (recording faces, watching hesitation) is the empirical gate that prevents the dev from confusing their own muscle memory with universal feel.

## Sources

- [Game Feel: A Game Designer's Guide to Virtual Sensation — Steve Swink](https://archive.org/details/game-feel)
- [Defining Game Feel — Swink Chapter 1 PDF](http://mycours.es/gamedesign2014/files/2014/10/Game-Feel-Steve-Swink-chapter-1.pdf)
- [Game feel — Wikipedia](https://en.wikipedia.org/wiki/Game_feel)
- [Game Feel and Player Control: Lessons from Steve Swink — Manas Dhanait, Bootcamp Medium](https://medium.com/design-bootcamp/game-feel-and-player-control-lessons-from-steve-swink-beae0ea1987f)
- [The Art of Screenshake — Jan Willem Nijman / Vlambeer (INDIGO 2013)](https://www.youtube.com/watch?v=AJdEqssNZ-U)
- [The Art of Screenshake — Internet Archive](https://archive.org/details/the-art-of-screenshake)
- [Juice It or Lose It — Martin Jonasson & Petri Purho](https://www.youtube.com/watch?v=Fy0aCDmgnxg)
- [GDC Vault — Juice It or Lose It](https://www.gdcvault.com/play/1016487/Juice-It-or-Lose)
- [Making Games 'Juicy' — abagames](https://abagames.github.io/joys-of-small-game-development-en/make_game_juicy.html)
- [Celeste & Forgiveness — Maddy Thorson (Medium)](https://maddythorson.medium.com/celeste-forgiveness-31e4a40399f1)
- [Maddy Thorson — Celeste game-feel thread (X)](https://x.com/MaddyThorson/status/1238338574220546049)
- [Celeste Dev Explains How They Made Their Game Feel So Good — GameSpot](https://www.gamespot.com/articles/celeste-dev-explains-how-they-made-their-game-feel/1100-6474775/)
- [How to Prototype a Game in Under 7 Days — Kyle Gabler et al. (Game Developer)](https://www.gamedeveloper.com/game-platforms/how-to-prototype-a-game-in-under-7-days)
- [How to Prototype a Game in Under 7 Days — PDF](http://miami.lgrace.com/documents/How%20to%20Prototype%20a%20Game%20in%20Under%207%20Days.pdf)
- [GDC 2006: Kyle Gabler — How to Prototype a Game in Under 7 Days (Internet Archive)](https://archive.org/details/GDC2006Gabler)
- [Hitstop / Hitfreeze / Hitlag / Hitpause — Critpoints (Celia Wagar)](https://critpoints.net/2017/05/17/hitstophitfreezehitlaghitpausehitshit/)
- [Thinking About Hitstop — Sakurai's Famitsu Column Vol. 490, via Source Gaming](https://sourcegaming.info/2015/11/11/thoughts-on-hitstop-sakurais-famitsu-column-vol-490-1/)
- [Hitlag — SmashWiki](https://www.ssbwiki.com/Hitlag)
- [Game Maker's Toolkit — Secrets of Game Feel and Juice (Mark Brown)](https://www.designoriented.net/blog/2015/05/26/2015526reblog-game-makers-toolkit-secrets-of-game-feel-and-juice/)
- [Math for Game Programmers: Juicing Your Cameras With Math — Squirrel Eiserloh (GDC 2016)](https://archive.org/details/GDC2016Eiserloh)
- [Math for Game Programmers: Fast and Funky 1D Nonlinear Transformations — Squirrel Eiserloh](https://www.youtube.com/watch?v=mr5xkf6zSzk)
- [Squirrel Eiserloh — Bio](http://www.eiserloh.net/bio/)
- [How Responsiveness Affects Players' Perception in Digital Games — Joerg et al., Clemson](https://people.computing.clemson.edu/~sjoerg/docs/Joerg12_RespInGames.pdf)
- [Player Perception of Delays and Jitter in Character Responsiveness — Normoyle et al., Clemson](https://people.computing.clemson.edu/~sjoerg/docs/Normoyle14_DelayAndJitter.pdf)
- [Input lag — Wikipedia](https://en.wikipedia.org/wiki/Input_lag)
- [Input Latency Compensation: The Unsung Hero of Online Game Feel — Wayline](https://www.wayline.io/blog/input-latency-compensation-online-game-feel)
- [Disney's 12 Principles of Animation — Wikipedia](https://en.wikipedia.org/wiki/Twelve_basic_principles_of_animation)
- [12 Principles for Game Animation — Chris Totten (Medium)](https://totter87.medium.com/12-principles-for-game-animation-a9137ef44345)
- [Squash and Stretch in Animation — RebusFarm](https://rebusfarm.net/blog/squash-and-stretch-in-animation-the-essential-principle-for-lifelike-motion)
- [Vlambeer / Game Feel / Nuclear Throne — Thoughts of A Third World Filmmaker](https://thoughtsofathirdworldfilmmaker.wordpress.com/2016/10/15/vlambeer-game-feel-and-everything-in-between/)
- [Hot reload in Godot — Godot Forum](https://forum.godotengine.org/t/hot-reload-in-godot-instantly-see-changes-without-reloading-a-game/66089)
- [Playtest Like a Researcher: Observational Playtesting — Ludogogy](https://ludogogy.professorgame.com/article/playtest-like-a-researcher-basics-of-observational-playtesting/)
- [Daniel Linssen developer interview — itch.io](https://itch.io/blog/7/developer-interview-with-daniel-linssen)
- [Jonas Tyroller — YouTube channel](https://www.youtube.com/channel/UC_p_9arduPuxM8DHTGIuSOg)
- [Building a best-selling game with a tiny team — Jonas Tyroller / Pragmatic Engineer](https://newsletter.pragmaticengineer.com/p/thronefall)
