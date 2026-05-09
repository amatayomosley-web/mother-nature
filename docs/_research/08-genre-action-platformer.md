# Action / Platformer / Twin-Stick — Research Report

## Domain summary

Action games (platformers, twin-stick shooters, beat-em-ups, shmups, metroidvanias) live or die by a single observable: **does the character respond to my hands the way my body expects?** That feel is built almost entirely from a stack of small, invisible cheats — coyote time, jump buffers, corner correction, hitstop, screenshake, deadzones, telegraphed enemies — most of which exist to compensate for the gap between what the player intends, what the player actually inputs, and what the screen renders 50–150ms later. Maddy Thorson's framing is exact: every one of these mechanics widens a timing or positioning window in the player's favor, so the game feels *kind* even when it is brutally hard. The novice mistake is to treat "platformer physics" as a math problem; it is a perception problem, and the math serves perception.

## Mechanic catalog

### Input buffering
- **Brief**: A jump pressed within N frames before landing still triggers the moment the character touches ground.
- **Why-drilled**: L1 — players try to chain jumps and mash early. L2 — they will always be slightly too early or too late, never frame-perfect. First principle — human visual reaction time floors around 200ms and motor execution adds ~50ms; punishing 1-frame misses punishes biology, not skill.
- **Reference game**: Celeste (jump buffer; Maddy Thorson explicitly documents it).
- **Novice failure mode**: Treats "press jump on landing frame" as a fair contract. The contract is "press jump near landing."
- **Priority**: core.
- **Source**: Maddy Thorson, "Celeste & Forgiveness," maddymakesgames.com.

### Coyote time
- **Brief**: Jump still works for ~3–6 frames after the character has walked off a ledge.
- **Why-drilled**: L1 — players misjudge edges. L2 — display latency (often 50–100ms on TVs) means the character has *already* visibly walked off when the player commits to jump. First principle — Mark Brown notes that without coyote time, the player is statistically right when they say "I pressed jump before the cliff," because their image was delayed from game logic.
- **Reference game**: Celeste, Super Meat Boy.
- **Novice failure mode**: Considers it "a lie to the player." It is honesty about latency.
- **Priority**: core.
- **Source**: Mark Brown, *Platformer Toolkit* itch.io notes; Maddy Thorson, "Celeste & Forgiveness."

### Variable jump height (early-jump-cut)
- **Brief**: Holding jump = full arc, releasing early = cut arc short (typically by multiplying upward velocity by a fraction the moment release is detected).
- **Why-drilled**: L1 — same button does long and short jump, no separate "small hop" button. L2 — gives one input expressive depth, doubling the move-set without doubling controls. First principle — analog expressiveness from a digital input via *time*.
- **Reference game**: Super Mario World, Celeste.
- **Novice failure mode**: Implements only a fixed jump and then bolts on a "small jump" button.
- **Priority**: core.
- **Source**: GMTK *Platformer Toolkit*.

### Apex hang time (halved-gravity peak)
- **Brief**: Gravity is reduced (often halved) for the few frames around the top of a jump arc.
- **Why-drilled**: L1 — jumps look prettier. L2 — players need extra time to read the world and adjust at the riskiest point of the jump. First principle — perception is poorest during peak velocity reversal; slowing the apex matches the cognitive bandwidth budget.
- **Reference game**: Celeste (Thorson lists it explicitly), N++, Super Mario World.
- **Novice failure mode**: Uses linear gravity; jumps feel "stiff" and aerial corrections feel impossible.
- **Priority**: core.
- **Source**: Maddy Thorson, X thread (status/1238338574220546049): "If you hold the jump button, the top of your jump has half gravity applied."

### Fast-fall
- **Brief**: Pressing down (or releasing jump in air) ramps gravity above default, giving the player a "drop" command.
- **Why-drilled**: L1 — players want to commit to landings. L2 — closes the loop: jump = up commitment, fast-fall = down commitment, both expressive. First principle — direct control over the descent half of the arc.
- **Reference game**: Celeste, every platform fighter (Smash Bros, Rivals of Aether).
- **Novice failure mode**: Omits it; player feels "floaty" with no recovery option.
- **Priority**: important (less critical for slow-paced genres).
- **Source**: GMTK *Platformer Toolkit* presets reference Mario, Celeste behaviors.

### Acceleration vs instant velocity
- **Brief**: Choose between "snap to top speed" (Hollow Knight, Mega Man X) and "build to top speed with skid frames" (Mario, Sonic).
- **Why-drilled**: L1 — feels different. L2 — instant velocity rewards precision platforming and reactive combat; ramped velocity rewards momentum-based level design. First principle — the input curve dictates the level grammar; pick before designing levels, not after.
- **Reference game**: Hollow Knight (instant — Mega Man X lineage); Super Mario Bros (ramped).
- **Novice failure mode**: Tunes the curve in isolation, then designs levels that fight the curve.
- **Priority**: core (the *choice* is core; either choice works).
- **Source**: ResetEra Team Cherry interview thread; Wikipedia Hollow Knight: "movement based on Mega Man X… no acceleration when moving horizontally."

### Corner correction (head-bonk nudge)
- **Brief**: If the character's head clips a wall corner by ~1–4 pixels during a jump, the engine pushes them sideways around it instead of stopping their upward motion.
- **Why-drilled**: L1 — pixel-perfect ceiling edges feel cruel. L2 — players target a tile, not a sub-pixel; the engine should round in their favor. First principle — pixel boundaries are an implementation detail, not a contract.
- **Reference game**: Celeste (Thorson #4 in "Forgiveness").
- **Novice failure mode**: Lets a 1-pixel collision negate a perfect jump; players quit.
- **Priority**: core.
- **Source**: amano.games "How to correct a corner" devlog; Celeste source code (NoelFB/Celeste).

### Hitbox vs hurtbox separation
- **Brief**: "What hits the world" (hitbox, on attacks) and "what gets hit by the world" (hurtbox, on the body) are *different* shapes. Often the hurtbox is smaller than the sprite.
- **Why-drilled**: L1 — fairness. L2 — sprite silhouettes have hair, capes, weapons that visually overlap projectiles without the player feeling hit. First principle — visual fidelity and collision fairness are orthogonal; conflating them makes the game look like a lie.
- **Reference game**: Every fighting game; Celeste shrinks Madeline's hurtbox below her sprite.
- **Novice failure mode**: Uses the sprite rect for both. Death feels unfair, attacks miss visibly.
- **Priority**: core.
- **Source**: Standard practice; GMTK *Platformer Toolkit*.

### Frame data (startup / active / recovery)
- **Brief**: Every attack decomposes into startup (vulnerable wind-up), active (hitbox live), recovery (committed, vulnerable). Counted in frames at the game's tick rate.
- **Why-drilled**: L1 — gives attacks rhythm. L2 — recovery is the player's accountability for committing; without it, mashing dominates. First principle — combat is the dialog of commitment and counter-commitment; frames are the syllables.
- **Reference game**: Smash Ultimate, Rivals of Aether (frame data is published canon).
- **Novice failure mode**: Makes attacks instant (no startup) or cancellable at any frame; combat collapses into mash.
- **Priority**: core for combat-heavy action; genre-specific for shmups.
- **Source**: Kurogane Hammer frame data repository; Rivals of Aether wiki.

### Animation cancels
- **Brief**: A select set of moves can be interrupted into another move (attack → dash, dash → jump). Determined by design intent, not all transitions are open.
- **Why-drilled**: L1 — speeds up combat. L2 — creates a skill ceiling: novices play one action at a time, experts chain. First principle — emergent depth through grammar, not via more buttons.
- **Reference game**: Devil May Cry, Hollow Knight (pogo-cancel into double-jump), Hyper Light Drifter (slash into dash).
- **Novice failure mode**: Either cancels everything (combat feels unstructured) or nothing (combat feels stiff).
- **Priority**: important for action; genre-specific elsewhere.
- **Source**: Team Cherry interviews via Source Gaming.

### Hitstop (sleep)
- **Brief**: When an attack connects, both attacker and target *freeze* for a few frames before the target reacts.
- **Why-drilled**: L1 — feels punchy. L2 — separates "the hit happened" from "the consequences play out," giving the player's brain a beat to process impact. First principle (Nijman): perception of impact is signaled by a *discontinuity* in motion. Constant-velocity collisions read as "passing through"; a freeze reads as "collided with mass."
- **Reference game**: Vlambeer's Nuclear Throne; Hollow Knight on every nail strike.
- **Novice failure mode**: Smooth, continuous collision that feels like the sword passes through butter.
- **Priority**: core for any game with melee or projectile impacts.
- **Source**: Jan Willem Nijman, "The Art of Screenshake," Vlambeer (INDIGO 2013).

### Knockback / hitlag (separating hit from hurt)
- **Brief**: On hit, the target gets shoved (knockback) and may flash invulnerable; *being hit* and *taking damage* are separate state transitions.
- **Why-drilled**: L1 — communicates "I damaged you" clearly. L2 — lets the player verify their attack worked even when HP isn't visible. First principle — feedback loops require redundant channels (visual + motion + sound) because any single channel is unreliable.
- **Reference game**: Shovel Knight (Yacht Club explicitly tunes Plague Knight's knockback higher than Shovel Knight's to communicate fragility).
- **Novice failure mode**: Damage with no knockback; players don't realize they hit.
- **Priority**: core.
- **Source**: Yacht Club Games "Plague Knight Mobility Design" blog.

### Screen shake
- **Brief**: Translating the camera by small random offsets for a short window after impact / explosion / heavy step.
- **Why-drilled**: L1 — adds weight. L2 — perpendicular bias (shake along the impact axis only) reads as directional force. First principle — Itay Keren: motion-vestibular mismatch causes nausea; shake amplifies vection. Use sparingly, scale with impact magnitude, and ship a slider.
- **Reference game**: Vlambeer (Nuclear Throne, Luftrausers); Devil May Cry.
- **Novice failure mode**: Constant shake on every event; motion-sickness players bounce off the game in 5 minutes. ~1/3 of players have some sickness sensitivity (per madelinemiller.dev research).
- **Priority**: important; **always paired with an accessibility slider**.
- **Source**: Nijman's *Art of Screenshake*; Halo Infinite & RE4 Remake accessibility menus; Xbox Accessibility Guideline 117.

### Camera systems (Itay Keren framework)
- **Brief**: A library of techniques: position-locking (Rally-X), camera-window/deadzone (Jump Bug), platform-snapping (Mario), lerp-smoothing (Donkey Kong Country), dual-forward-focus (Mario world directional anchors), projected/predictive (Secrets of Rætikon), target-focus on stick input (Jazz Jackrabbit 2), cue-focus rings (Insanely Twisted Shadow Planet), and zoom-to-fit (Smash Bros).
- **Why-drilled**: L1 — keeps the player on screen. L2 — different verbs need different lookahead behaviors; ladders/drops/run/walk all want different camera laws. First principle (Keren): the fovea handles detail, periphery handles motion, vestibular handles balance — misalign these and you induce nausea.
- **Reference game**: Mario World for the platform-snap + dual-forward-focus combo; Hollow Knight for camera-window with subtle lookahead.
- **Novice failure mode**: A single rigid lerp-to-player-position regardless of action; reads as either too jittery or too sluggish; feels nauseating in vertical sections.
- **Priority**: core; **arguably the second most important system after movement**.
- **Source**: Itay Keren, "Scroll Back," gamedeveloper.com (originally Gamasutra) / GDC 2015.

### Controller deadzones (radial vs axial)
- **Brief**: Ignore stick inputs below a threshold. Choose *radial* (circular threshold, all directions equal) over *axial* (per-axis thresholds, cross-shaped dead region) for almost all action games.
- **Why-drilled**: L1 — sticks drift; ignore tiny jitter. L2 — axial deadzones produce rigid cardinal lock-in and bad diagonals; radial preserves direction integrity. First principle — the input is a 2D vector, not two 1D scalars; treat its magnitude and angle independently. Then *scaled* radial: remap [deadzone..1] to [0..1] linearly so there's no dead step at the threshold.
- **Reference game**: INVERSUS implementation (Ryan Juckett / Hypersect).
- **Novice failure mode**: Naive `if abs(x) < 0.2` per axis. Diagonals feel jagged; characters snap to N/S/E/W.
- **Priority**: core for any game with analog input.
- **Source**: Ryan Juckett, "Interpreting Analog Sticks," Hypersect blog.

### Dash / dodge with i-frames
- **Brief**: A short, fast directional move that grants invincibility frames (i-frames) for some or all of its duration, usually with a cooldown.
- **Why-drilled**: L1 — gives players an "out." L2 — the i-frame window is a *commitment cost*: you spend the dash, you can't do it again until the cooldown completes. First principle — invincibility without cost trivializes; cost without invincibility is just movement. The two together create a risk economy.
- **Reference game**: Hollow Knight Shadecloak (full i-frames, short cooldown); Hyper Light Drifter (originally no i-frames, patched to add a tight window).
- **Novice failure mode**: i-frames last too long, dash spam dominates; OR no i-frames, dash is movement-only and combat lacks defensive expression.
- **Priority**: core for combat-action; genre-specific for puzzle-platformer.
- **Source**: Hyper Light Drifter Steam announcement "The Invincibility Patch."

### Wall jump / ledge grab / climb
- **Brief**: Distinct mechanics for grabbing walls (Celeste 2-pixel detection), wall-jumping (push-off plus re-aim), and ledge-grabbing (snap to ledge from below).
- **Why-drilled**: L1 — players fail wall jumps because they're 1 pixel off. L2 — Celeste's "wide wall-jump window" extends the grab to 2 pixels, "super wall-jump window" to 5 pixels for harder maneuvers. First principle — same window-widening principle as coyote time, applied in space rather than time.
- **Reference game**: Celeste; Super Meat Boy.
- **Novice failure mode**: Implements wall-jump only on perfect adjacency; tutorial section becomes the hardest part of the game.
- **Priority**: genre-specific (climb-platformers, metroidvanias).
- **Source**: Maddy Thorson, "Celeste & Forgiveness."

### Death / respawn loops
- **Brief**: Cost of death, time between death and re-attempt, distance from death to checkpoint. Choose: instant respawn (Super Meat Boy <0.5s) vs. distance respawn (Hollow Knight, returns to bench, must re-trek and reclaim shade).
- **Why-drilled**: L1 — decides risk tolerance. L2 — instant respawn enables aggressive iteration (death is informative, not punishing); long respawn enables tension and strategic play. First principle — death cost calibrates the player's willingness to experiment, which calibrates the level-design grammar.
- **Reference game**: Super Meat Boy (instant); Hollow Knight (penalty + reclaim).
- **Novice failure mode**: Picks the punishing model for a precision-platformer; player rage-quits because the loop is too long for the failure rate.
- **Priority**: core.
- **Source**: Tommy Refenes interviews on Super Meat Boy/Forever; standard Souls-style critique.

### Enemy AI: aggro, telegraph, recovery
- **Brief**: Enemies have aggro radius (when they notice you), telegraph windows (visible wind-up before attacks, often paired with a sound or color change), and recovery windows (when *they* are vulnerable post-attack).
- **Why-drilled**: L1 — combat must be readable. L2 — telegraph time gives the player just enough to react if attentive; recovery gives them a *reason to attack* timed to the telegraph. First principle — combat is a dance of commitment; the enemy must commit publicly.
- **Reference game**: Hollow Knight (every enemy telegraphs distinctly); Souls series.
- **Novice failure mode**: Instant or invisible enemy attacks; no recovery windows; combat becomes an HP race.
- **Priority**: core for combat-action.
- **Source**: chaoticstupid.com "Enemy Attacks and Telegraphing"; Game Wisdom's "Three Components of Great Action Game Design."

### Pickup / power-up feedback
- **Brief**: Pickups magnetize toward the player at close range, play a distinct sound, animate (sparkle, pop), and pause briefly on grant.
- **Why-drilled**: L1 — players need to know they got something. L2 — magnetism handles near-misses, hides forgiveness as feel. First principle — closure feedback (Nielsen heuristic) for every player action; absence reads as a bug.
- **Reference game**: Vlambeer, Hades, every Metroidvania.
- **Novice failure mode**: Silent disappear-on-touch; players think it didn't register.
- **Priority**: important.
- **Source**: Nijman's *Art of Screenshake* (sound + permanence + feedback).

### Difficulty curve: density vs introduction
- **Brief**: Two ramping levers — "more enemies / faster patterns" (density) and "new mechanic introduced cleanly" (Nintendo Mario world structure: introduce → develop → twist → conclude). Use both, sequenced.
- **Why-drilled**: L1 — pure density without introduction feels arbitrary. L2 — pure introduction without density gets boring. First principle — humans learn under low load and stress under high load; alternate the two.
- **Reference game**: Super Mario World level design; contrast with Kaizo Mario (intentionally rejects this curve).
- **Novice failure mode**: Random monotonic difficulty ramp; first level is harder than first boss.
- **Priority**: core.
- **Source**: National Videogame Museum "Nintendo, Super Mario & difficulty"; gamedeveloper.com "An Examination into Kaizo Design."

### Metroidvania lock-and-key (Boss Keys framework)
- **Brief**: Map progression as a dependency graph of locks (gates) and keys (abilities). Hollow Knight uses many small locks per ability so each key feels recurrent.
- **Why-drilled**: L1 — open worlds need progression gates. L2 — placing the gates is the level design. First principle — the dependency graph is the level; the geometry merely renders it.
- **Reference game**: Hollow Knight, Metroid Prime, Symphony of the Night.
- **Novice failure mode**: One ability = one new region only; abilities feel single-use; backtracking is empty.
- **Priority**: genre-specific (metroidvania).
- **Source**: Mark Brown, "Boss Keys" series; GMTK Patreon "How I make a dependency graph."

## Must-include shortlist

Before scoping a first action game, internalize these eight:

1. **Coyote time + jump buffer** — the latency cushion. ~6 frames each is the standard.
2. **Variable jump height + apex hang time** — the expressiveness of jump.
3. **Corner correction** — pixel forgiveness; cheap to implement, transformational for feel.
4. **Hitbox vs hurtbox separation** — the contract for fair damage. Hurtbox smaller than sprite.
5. **Hitstop on impact** — Nijman's first lesson; freeze a few frames on hit. The biggest perceptual ROI for the least code.
6. **Camera deadzone + lerp smoothing (Keren)** — the second-biggest game-feel system after movement; pick from his framework, don't roll your own.
7. **Radial scaled deadzone** — if you support stick input, do it correctly first time.
8. **Telegraphed enemies + recovery windows** — combat readability; without it, every fight is an HP race.

## Commonly oversold

- **Screen shake as default polish** — over-applied by novices because it's cheap. Without an accessibility slider it makes the game unplayable for ~1/3 of players. Worth shipping; never the primary impact channel.
- **Frame-data depth on a first project** — published frame data matters in platform fighters and high-skill action; for a first 2D Godot game, "startup, active, recovery exist and feel right" suffices. Don't publish a frame chart before the game is fun.
- **Animation cancel grids** — flexible cancel systems are emergent in DMC/Bayonetta because the rest of the design supports them; tacked on, they just make combat feel unstructured. Build the base combat first.
- **Custom camera physics** — novices try to invent a camera. Pick one from Keren's catalog (camera-window + lerp + dual-forward-focus is the safe default for side-scrollers) and tune it. Don't ship a physics-coupled camera until you've shipped one that works.
- **Wall jump / ledge grab as default platformer kit** — Celeste needed it; many platformers don't. It's expensive in level design (every wall becomes potentially climbable, vastly expanding test cases). Skip it on first project.
- **i-frames on dodge** — easy to bolt on, easy to over-tune. Hyper Light Drifter shipped *without* i-frames originally and the combat worked; they patched them in later. Don't assume invincibility windows are required.

## Cross-references

- The schema discipline from architecture (define jump-state, hit-state as typed enums before writing the loop) applies directly here: input → buffered-input → state → physics-application is a pipeline of typed values, not an `if`-chain.
- Difficulty curve sequencing is the same problem as the metroidvania dependency graph at micro-scale: introduce a verb, develop it under low pressure, raise pressure, twist context, then chain. Apply at level scale, room scale, and encounter scale.
- Camera and screenshake share a substrate (the perception/vestibular interface). A motion-sickness slider should govern *both* systems together.
- Coyote time, jump buffer, corner correction, and wall-jump windows are four expressions of one principle: widen the player's window. Treat as a configurable bag of tunables, not four ad-hoc patches.

## Sources

- [Maddy Thorson — Celeste & Forgiveness (Medium)](https://maddythorson.medium.com/celeste-forgiveness-31e4a40399f1)
- [Maddy Thorson — Celeste & Forgiveness (canonical mirror)](https://www.maddymakesgames.com/articles/celeste_and_forgiveness/index.html)
- [Maddy Thorson — Celeste and TowerFall Physics (Medium)](https://maddythorson.medium.com/celeste-and-towerfall-physics-d24bd2ae0fc5)
- [Maddy Thorson — Game-feel Twitter thread](https://x.com/maddythorson/status/1238338574220546049)
- [Itay Keren — Scroll Back: The Theory and Practice of Cameras in Side-Scrollers (Game Developer / Gamasutra)](https://www.gamedeveloper.com/design/scroll-back-the-theory-and-practice-of-cameras-in-side-scrollers)
- [Itay Keren — GDC 2015 talk archive](https://archive.org/details/GDC2015Keren)
- [Mark Brown — Platformer Toolkit (itch.io)](https://gmtk.itch.io/platformer-toolkit)
- [Mark Brown — Boss Keys playlist](https://www.youtube.com/playlist?list=PLc38fcMFcV_ul4D6OChdWhsNsYY3NA5B2)
- [Mark Brown — How I make a dependency graph (Patreon public)](https://www.patreon.com/posts/how-i-make-graph-20631617)
- [Jan Willem Nijman — The Art of Screenshake, INDIGO 2013 (YouTube)](https://www.youtube.com/watch?v=AJdEqssNZ-U)
- [The Art of Screenshake — Internet Archive](https://archive.org/details/the-art-of-screenshake)
- [Ryan Juckett — Interpreting Analog Sticks (Hypersect)](http://blog.hypersect.com/interpreting-analog-sticks/)
- [amano.games — How to correct a corner](https://amano.games/devlog/how-to-correct-a-corner)
- [DeepWiki — Celeste Player Movement and Physics (NoelFB/Celeste source)](https://deepwiki.com/NoelFB/Celeste/2.3-player-movement-and-physics)
- [Yacht Club Games — Plague Knight Mobility Design](https://www.yachtclubgames.com/blog/plague-knight-mobility-design/)
- [Yacht Club Games — King Knight's Mobility](https://www.yachtclubgames.com/blog/king-knight-s-mobility/)
- [Yacht Club Games — Specter Knight Mobility Design](https://www.yachtclubgames.com/blog/specter-knight-mobility-design/)
- [Source Gaming — Team Cherry interview](https://sourcegaming.info/2025/04/09/straight-from-the-source-team-cherry/)
- [Steam — Hyper Light Drifter "The Invincibility Patch"](https://steamcommunity.com/games/257850/announcements/detail/594848467137959659)
- [chaoticstupid.com — Enemy Attacks and Telegraphing](http://www.chaoticstupid.com/enemy-attacks-and-telegraphing/)
- [Game Wisdom — Three Components of Great Action Game Design: Enemy](https://game-wisdom.com/series/three-components-of-great-action-game-design-enemy)
- [Kurogane Hammer — Frame Data Repository (Smash / Rivals)](https://kuroganehammer.com/)
- [Rivals of Aether Wiki — Frame data](https://rivals-of-aether.fandom.com/wiki/Frame_data)
- [Maddy Miller — Motion Sickness Accessibility in Video Games](https://madelinemiller.dev/blog/motion-sickness-accessibility/)
- [Microsoft — Xbox Accessibility Guideline 117 (camera shake)](https://learn.microsoft.com/en-us/gaming/accessibility/xbox-accessibility-guidelines/117)
- [Game Developer — An Examination into Kaizo Design](https://www.gamedeveloper.com/design/an-examination-into-kaizo-design)
- [National Videogame Museum — Nintendo, Super Mario & difficulty](https://thenvm.org/games/nintendo-super-mario-difficulty/)
- [The Game Developer — Enemy design and enemy AI for melee combat systems](https://www.gamedeveloper.com/design/enemy-design-and-enemy-ai-for-melee-combat-systems)
