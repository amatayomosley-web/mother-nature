# Postmortems + Survivor Accounts — Research Report

## Domain summary

This is the "I wish I'd known" knowledge — recurring lessons from devs who actually shipped. The pattern that dominates the 71% scope-failure rate across analyzed postmortems: novices think the bottleneck is technical skill or art quality; veterans say the bottleneck is finishing, scoping, and the 90% of work that isn't game development. The literature is consistent across two decades from Refenes/McMillen/Yu in the 2010s to Bucklew/Pope in the 2020s. The unknown-unknowns cluster cleanly into five themes: scope (you will overshoot), psychology (you will despair), production (placeholder-first, kill darlings), market (the Steam page is a dev-time asset), and post-launch (the work doesn't end at ship).

## Lesson catalog

### Scope is the silent killer

- **The lesson**: 71% of analyzed postmortems reported scope problems. Triple your estimate, then cut.
- **Quote(s)**: "17 postmortems, or 71 percent, reported scope problems where there was either not enough time or resources to complete the game" (gamedeveloper.com postmortem analysis). Wayline: "Over 70% of surveyed indie developers cited 'scope too large' as a significant factor in their game's failure." Solo dev cited: "9-month project ended up taking three times longer than expected (27 months)."
- **Why-drilled**: L1: Devs underestimate work → L2: Tasks have hidden subtasks (testing, integration, edge cases) → First principle: cognitive bias of planning fallacy is universal; only mechanical buffers correct it.
- **Novice failure mode**: Picks an MMO/Soulslike/open-world as first project; quits at month 18.
- **Recurrence count**: 8+ (Refenes implicit, Yu, McMillen, Barone, Hipster Whale, Tyroller, Pirate Software/Thor, postmortem meta-analysis)
- **Priority**: core
- **Confidence**: high

### Finishing is the skill, not coding

- **The lesson**: The hardest indie skill is consistently finishing games. It is its own discipline.
- **Quote(s)**: Derek Yu: "Finishing games gets easier with each game you finish." "If a person can get from the beginning of your game to the end of your game without the game actually crashing, your game is done." Yu also: "Until your game is finished, it doesn't really exist." Eric Barone: "I would've felt ashamed if I had gotten so far and then quit."
- **Why-drilled**: L1: Most projects die in the "Swamp" (middle 60%) → L2: Boredom and doubt peak before payoff → First principle: motivation is non-renewable without external commit forcings (deadlines, public commitments).
- **Novice failure mode**: Endless prototyping. Five "first levels" in a folder, no shipped games.
- **Recurrence count**: 6+ (Yu, Barone, Refenes, Pirate Software, Carotenuto, Last Humble Bee dev)
- **Priority**: core
- **Confidence**: high

### Indie work is 10% game dev, 90% everything else

- **The lesson**: Coding/art is a small fraction of shipping a game. Marketing, support, business, press, store pages, taxes — that's the bulk.
- **Quote(s)**: Attilio Carotenuto: "Being an indie developer is really 10% game dev, and 90% everything else." Tarn Adams: "A lot of work goes into sticking around, and almost everybody ends up needing to think of creative ways to continue to pay the rent."
- **Why-drilled**: L1: Shipping requires distribution, support, legal → L2: No employer absorbs that overhead for solo devs → First principle: a finished product is a business surface, not a code artifact.
- **Novice failure mode**: All time on engine; zero time on Steam page; no community; launches to silence.
- **Recurrence count**: 4 (Carotenuto, Adams, Last Humble Bee dev, How To Market A Game)
- **Priority**: core
- **Confidence**: high

### Build the ending first

- **The lesson**: Build the final boss or ending state before the rest. It forces commitment to a shippable shape.
- **Quote(s)**: gamedeveloper.com "Finish Your Game": "The strategy: build the final boss first, then expand backward with additional content."
- **Why-drilled**: L1: Without an end, every feature is justifiable → L2: An end-condition is a hard scope wall → First principle: closure points create constraint; constraint creates completion.
- **Novice failure mode**: Builds 12 systems, no win/lose state. Game has no shape.
- **Recurrence count**: 3 (Yu, "Finish Your Game" Gamasutra, Tynan Sylvester implicit in Designing Games)
- **Priority**: important
- **Confidence**: medium

### Loving the genre as a player ≠ loving to make it

- **The lesson**: Picking a genre because you love playing it can backfire. Designing it is a different job.
- **Quote(s)**: Carotenuto: "The fact that you enjoy playing a certain genre does not necessarily mean that you'll also enjoy working on it." (He loved Ikaruga; making bullet patterns wore him out.)
- **Why-drilled**: L1: Playing is consumption; designing is generation → L2: Tedium tolerance differs by task type → First principle: validate developer-fit by prototyping, not by player preference.
- **Novice failure mode**: Spends 2 years on a genre that bores them to make.
- **Recurrence count**: 1 explicit, but echoed across "make a small game first" pattern from Pirate Software, Yu
- **Priority**: important
- **Confidence**: medium

### Polish doesn't broaden appeal — core concept does

- **The lesson**: A bad core idea polished is still a bad game. A good idea unpolished still works.
- **Quote(s)**: Derek Yu: "Polish is not a great way to broaden the appeal of your game — comparatively, the core concept and basic execution are much more important." McMillen: "If you look back at [Isaac], it looks like mud." (Sold millions anyway.)
- **Why-drilled**: L1: Players can see through art to mechanics quickly → L2: Polish has diminishing returns; concept doesn't → First principle: distinguishing/load-bearing decisions live at the design layer.
- **Novice failure mode**: Polishes a derivative platformer for 3 years; core mechanic is forgettable.
- **Recurrence count**: 4 (Yu, McMillen, Pope, Hipster Whale)
- **Priority**: core
- **Confidence**: high

### Prototypes ask "should we?", vertical slices ask "can we?"

- **The lesson**: Prototypes are throwaway and answer design questions. Vertical slices are production-quality and answer execution questions. Don't confuse them.
- **Quote(s)**: Rami Ismail: "The Prototypes are to help you figure out whether you _should_ make a game, and the Vertical Slice is to help you figure out whether you _can_ make it." "Your Prototypes should not be made to be high-quality, structured well, or reusable... fast, cheap, and precise."
- **Why-drilled**: L1: Confusing them = polishing prototypes that should be thrown away → L2: Sunk-cost prevents killing bad ideas → First principle: separating discovery from production prevents premature commitment.
- **Novice failure mode**: Spends 6 months "polishing the prototype" instead of validating a different idea.
- **Recurrence count**: 3 (Ismail, "Polished Prototype Trap" Wayline article, Cook implicit)
- **Priority**: core
- **Confidence**: high

### Watch silent playtests; don't believe what players say

- **The lesson**: Observe playtesters silently. Their actions reveal real problems; their explanations don't.
- **Quote(s)**: Multiple sources: "Players are notoriously bad at diagnosing why something feels wrong, telling you combat is too hard when the real problem is unclear feedback." "Hand over the build and watch without explaining unless players are completely stuck, since you won't have the opportunity to explain in the future." Rami Ismail: "You can't make the game in your head."
- **Why-drilled**: L1: Players self-report inaccurately → L2: Behavior is the ground truth → First principle: what is observable beats what is asserted.
- **Novice failure mode**: Ships with tutorial confusion that 100% of testers hit but didn't articulate.
- **Recurrence count**: 5 (gdevelop, Filament, Vlambeer/Ismail, kokutech, indieop)
- **Priority**: core
- **Confidence**: high

### Game feel is non-optional, not "polish"

- **The lesson**: Game feel ("juice") is the difference between a game people play and one they bounce from. It's not optional polish — it's a load-bearing system.
- **Quote(s)**: Steve Swink: game feel is "realtime control of virtual objects in a simulated space, with interactions emphasised by polish." Vlambeer JW Nijman, "The Art of Screenshake": small additions like screen shake, hit-pause, particles "can make a game feel far more exciting and engaging."
- **Why-drilled**: L1: Players judge in 30 seconds → L2: That judgment is mostly tactile, not analytical → First principle: control loop fidelity is perceived as quality.
- **Novice failure mode**: Mechanically correct game that "feels dead." No screen shake, no hit-pause, no audio cues.
- **Recurrence count**: 3 (Swink, Nijman/Vlambeer, Pope implicit on constraints + feel)
- **Priority**: core
- **Confidence**: high

### Use placeholder art; do real art last

- **The lesson**: Final art before mechanics are validated wastes the art when mechanics change. Placeholder until the loop works.
- **Quote(s)**: gamedev.net forum consensus: "Using placeholders allows developers to build assets and levels fast, iterate without committing to anything, and avoid wasted hours trying to make old assets fit new mechanics." Pirate Software/Thor: chose tiny scope so the art/audio team could learn without locking final assets in.
- **Why-drilled**: L1: Mechanics drive art needs (hitboxes, anim frames) → L2: Final art locks the design → First principle: cheap iteration requires cheap representations.
- **Novice failure mode**: Six months of beautiful sprites for a mechanic that gets cut.
- **Recurrence count**: 4 (gamedev.net, Quora consensus, Pirate Software, "Polished Prototype Trap")
- **Priority**: important
- **Confidence**: high

### Use version control from day one

- **The lesson**: Git/source control is non-negotiable, even for solo devs. Lost work is the most preventable disaster.
- **Quote(s)**: Multiple: "Many indie game developers have lost weeks, months, or even years of work because a hard drive failed... with no backup system in place." "You should always use source control, even if you're a solo developer working on a hobby project, and even more so if you are a beginner."
- **Why-drilled**: L1: Solo dev with no VCS = single point of failure → L2: Failure is statistically certain over years → First principle: backups + reversibility are infrastructure, not optional.
- **Novice failure mode**: Drive failure or bad refactor wipes 3 months. Quits.
- **Recurrence count**: 5 (gamedeveloper.com, dev.to, virtualmaker.dev, multiple gamedev forums)
- **Priority**: core
- **Confidence**: high

### Steam page must go up 6+ months pre-launch

- **The lesson**: Wishlists compound. A page launched at "almost done" is too late.
- **Quote(s)**: Indie Launch Lab: "Creating your Steam page the week before launch is like trying to plant a tree the day you need shade." "Make a Steam page as early as possible and keep it up for a minimum of 6 months before you launch your game, and if you can wait, keep it up 8 months." Chris Zukowski: "~7,000 wishlists for Popular Upcoming"; "150 wishlists in first 2 weeks" as a viability signal.
- **Why-drilled**: L1: Steam algorithm rewards accumulated signal → L2: Launch email blast to wishlisters is the single biggest sales event → First principle: visibility on a saturated platform is path-dependent on accumulated history.
- **Novice failure mode**: Launches with 80 wishlists; 12 sales week 1; algorithm buries the game.
- **Recurrence count**: 4 (Zukowski, Indie Launch Lab, How To Market A Game, valadria)
- **Priority**: core
- **Confidence**: high

### Burnout is the second silent killer

- **The lesson**: Multi-year solo projects produce burnout, "dark nights of the soul," and project death by exhaustion.
- **Quote(s)**: Last Humble Bee dev: "repeated 'dark nights of the soul' while working solo for two years." Tanat Boozayaangool: "It's very hard to avoid some amount of burnout when working on indie games part-time." Last Humble Bee: "Nobody cares about your game as much as you do, and that's OK."
- **Why-drilled**: L1: Long solo projects have no external feedback → L2: Without dopamine inputs, motivation collapses → First principle: motivation is a system, not a trait; design for it.
- **Novice failure mode**: Year 2 of project, can't open project file, quits.
- **Recurrence count**: 4 (Last Humble Bee, Boozayaangool Medium, "The Fire Fades" Gamasutra, Pirate Software warning)
- **Priority**: core
- **Confidence**: high

### Set hard external deadlines; cut to fit

- **The lesson**: Set a deadline. Cut features to meet it. Don't extend the deadline to fit features.
- **Quote(s)**: Hipster Whale (Crossy Road): "Originally set out to make it in six weeks, which meant that if it didn't go well, they hadn't wasted much time." (Shipped in 12.) GameMakerBlog: "Set a deadline and cut your project to meet the deadline instead of pushing the deadline back."
- **Why-drilled**: L1: Open-ended scope = open-ended timeline → L2: Closure forcings work even when self-imposed if public → First principle: constraint-driven design beats vision-driven design for completion.
- **Novice failure mode**: "Just one more system." 5 years later, no game.
- **Recurrence count**: 4 (Hipster Whale, GameMakerBlog, Wayline, Sylvester)
- **Priority**: core
- **Confidence**: high

### Engine choice: pick the popular one

- **The lesson**: A solo dev should use a popular, proven, well-supported engine. The path of others is the path with answers on Stack Overflow.
- **Quote(s)**: Lucas Pope on Unity: "Particularly appropriate for a solo developer targeting multiple platforms and in need of a popular, proven, and well-supported engine." McMillen on Flash for Isaac: "The .FLA file corrupted regularly above 300MB, nearly preventing the DLC expansion entirely... Flash simply wasn't at all made to support a game of Isaac's size." (Cautionary tale of obscure-engine pain.)
- **Why-drilled**: L1: Engine bugs in obscure tools have no community fix → L2: A solo dev cannot afford to be the engine support team → First principle: leverage > novelty for solo work.
- **Novice failure mode**: Picks bespoke engine for "performance"; stalls at platform port stage.
- **Recurrence count**: 3 (Pope, McMillen retrospective, gamemaker.io)
- **Priority**: important
- **Confidence**: high

### Audio is not optional and routinely underbudgeted

- **The lesson**: Audio is among the highest-impact-per-effort additions to game feel and is consistently neglected by solo devs.
- **Quote(s)**: Krotos and consensus: "Audio budgets range between 5% and 10%" — but "for game jams, it's usually commonplace for game developers to just rip some sounds from freesound.org and that's it." Game-feel literature treats SFX as load-bearing for hit feedback.
- **Why-drilled**: L1: Players experience hits/feedback through sound as much as visuals → L2: Silent games feel dead → First principle: multimodal feedback is how brains confirm cause-effect.
- **Novice failure mode**: Ships with default GameMaker click sounds; reviews say "feels cheap."
- **Recurrence count**: 3 (Krotos, GameAnalytics, sound design literature)
- **Priority**: important
- **Confidence**: medium

### Marketing starts on day one, not month 12

- **The lesson**: Marketing begins when the idea begins. Building a community is a 12-18 month task, not a launch task.
- **Quote(s)**: Eric Barone on Stardew: "Launched a website for it a year into development and started talking about his project on Harvest Moon fan forums." How To Market A Game: "Start marketing the moment you have an idea for your game."
- **Why-drilled**: L1: Audience trust accrues slowly → L2: Launch-week visibility depends on pre-launch presence → First principle: distribution is a relationship; relationships need time.
- **Novice failure mode**: 200 Twitter followers, no Discord, launches to crickets.
- **Recurrence count**: 4 (Barone, Zukowski, Howtomarketagame, Carotenuto)
- **Priority**: core
- **Confidence**: high

### Twitter is not a marketing channel for indies

- **The lesson**: Twitter/X reaches devs, not players. It's an echo chamber.
- **Quote(s)**: Postmortem analysis 2021: "Twitter in particular seems to be an echo chamber of devs rather than useful wishlist generator." Reddit (specific subs) and TikTok consistently outperform.
- **Why-drilled**: L1: Algorithm favors engagement; dev followers engage with dev content → L2: Player audiences live elsewhere → First principle: channel choice ≠ audience location for niches.
- **Novice failure mode**: 5,000 dev followers, 0 player reach.
- **Recurrence count**: 3 (Howtomarketagame, multiple postmortems, Steam Dev Cheat Sheet)
- **Priority**: important
- **Confidence**: medium

### Kill your darlings — cut features that fight the game

- **The lesson**: A mechanic loved in isolation can ruin the larger game. Cut it.
- **Quote(s)**: Jonathan Blow on Witness: "I've cut hundreds of things from the game. Usually I cut them early — I start experimenting with something and decide I don't like it." Multiple sources: "A mechanic might work great by itself but fail as part of [the game]."
- **Why-drilled**: L1: Systems interact; local optima ≠ global → L2: Sunk cost biases against cutting → First principle: holistic fit beats individual feature merit.
- **Novice failure mode**: Keeps a stat system because they coded it; it confuses every playtester.
- **Recurrence count**: 4 (Blow, Extra Credits, Salvage Union devblog, Quirkworthy)
- **Priority**: important
- **Confidence**: high

### Post-launch is 10–20% extra dev cost and 5–15 hrs/week

- **The lesson**: Shipping is not the end. Plan for 3–6 months of patches, community management, and support.
- **Quote(s)**: Industry consensus: "Post-launch maintenance costs $10,000-$30,000 for a solo developer, requiring 3-6 months of active development time after launch." "Community management requires 5-15 hours per week during and after launch... during launch week this can consume 40+ hours." "10–20% of the budget after launch."
- **Why-drilled**: L1: Players find what testers missed → L2: Reviews drive revenue; bugs drive negative reviews → First principle: the product is a relationship, not an artifact.
- **Novice failure mode**: Ships, leaves for vacation, 200 negative reviews accumulate, sales tank.
- **Recurrence count**: 3 (Juego Studios, vsquad, multiple postmortems)
- **Priority**: important
- **Confidence**: high

### Make a deliberately small first game to learn the pipeline

- **The lesson**: Your first game is a training exercise. Pick something tiny; complete the full pipeline (build → ship → support).
- **Quote(s)**: Pirate Software/Thor: "Intentionally wanted to make something very simple and small so they could get their content pipeline down... the artist had never made pixel art before and the musician had never made game sound effects." Yu: "Release one sloppy, fun little game quickly, and then make a game that's slightly bigger and less sloppy to learn faster."
- **Why-drilled**: L1: The first time through any pipeline is the slowest → L2: Each tool has unknown unknowns that surface only at ship → First principle: experience is non-substitutable; minimize the cost of acquiring it.
- **Novice failure mode**: First project = dream project = unfinished forever.
- **Recurrence count**: 4 (Pirate Software, Yu, Refenes via Super Meat Boy origin, Barone via prior unfinished projects)
- **Priority**: core
- **Confidence**: high

## Top 10 unknown-unknowns

These rarely appear in formal tutorials but recur most across postmortems:

1. **Indie work is 90% non-development.** Marketing, support, business, taxes. Carotenuto, Adams.
2. **The Steam page is a development asset, not a launch asset.** 6-8 months minimum runway. Zukowski, Indie Launch Lab.
3. **Loving to play a genre ≠ loving to make it.** Validate developer-fit, not player-fit. Carotenuto.
4. **Polish doesn't broaden appeal; core concept does.** A muddy-looking game with a sharp core outsells a polished derivative. Yu, McMillen.
5. **Prototypes answer "should we?"; vertical slices answer "can we?"** Don't polish prototypes. Ismail.
6. **Watch playtesters silently; ignore what they say, watch what they do.** Behavior is ground truth.
7. **Build the ending first.** It forces a shippable shape.
8. **Burnout is statistically near-certain on multi-year solo projects.** Plan motivation as a system. Last Humble Bee, Pirate Software.
9. **Audio is high-leverage and routinely neglected.** Game feel is half-sound.
10. **Twitter is dev echo chamber, not player reach.** Reddit/TikTok/streamers actually drive wishlists.

## Cross-references

This domain overlaps with:
- **Engine/tooling choice** (Lucas Pope on Unity, McMillen Flash cautionary tale)
- **Game feel / juice** (Swink, Vlambeer)
- **Marketing/business** (Zukowski, Howtomarketagame)
- **Mental health and solo work** (Last Humble Bee, Boozayaangool)
- **Production discipline** (vertical slice methodology, version control)
- **Project management** (deadlines, scope cutting)

## Sources I used

- [Dissecting The Postmortem: Lessons Learned From Two Years](https://www.gamedeveloper.com/audio/dissecting-the-postmortem-lessons-learned-from-two-years-of-game-development-self-reportage)
- [The Last Humble Bee postmortem](https://www.gamedeveloper.com/business/the-last-humble-bee-postmortem-staying-sane-in-solo-development)
- [Postmortem: McMillen and Himsl's Binding of Isaac](https://www.gamedeveloper.com/business/postmortem-mcmillen-and-himsl-s-i-the-binding-of-isaac-i-)
- [How Stardew Valley creator Eric Barone coped with a four year dev cycle](https://www.gamedeveloper.com/production/how-i-stardew-valley-i-creator-eric-barone-coped-with-a-four-year-dev-cycle)
- [Derek Yu — Make Games (Death Loops)](https://www.derekyu.com/makegames/deathloops.html)
- [Derek Yu — Indie Archetypes](https://www.derekyu.com/makegames/archetypes.html)
- [Spelunky designer: learn how to finish games reliably](https://www.gamedeveloper.com/design/-i-spelunky-i-designer-want-to-be-an-indie-then-learn-how-to-finish-games-reliably)
- [Finish Your Game (Gamasutra)](https://www.gamedeveloper.com/business/finish-your-game)
- [Rami Ismail — Prototypes & Vertical Slice](https://ltpf.ramiismail.com/prototypes-and-vertical-slice/)
- [17 Lessons from Vlambeer's Rami Ismail](https://www.gamedeveloper.com/business/video-17-lessons-for-game-developers-from-vlambeer-s-rami-ismail)
- [Q&A: Tarn Adams on Dwarf Fortress](https://www.gamedeveloper.com/design/q-a-dissecting-the-development-of-i-dwarf-fortress-i-with-creator-tarn-adams)
- [Lucas Pope on Return of the Obra Dinn](https://www.gamedeveloper.com/design/for-lucas-pope-i-return-of-the-obra-dinn-i-was-a-bunch-of-appealing-design-problems)
- [Thor, Pirate Software: Leaving AAA to go Indie](https://www.gamedeveloper.com/business/thor-pirate-software-leaving-aaa-to-go-indie)
- [What worked in 2021 — postmortem summary](https://howtomarketagame.com/2021/12/27/what-worked-in-2021/)
- [Steam Dev Cheat Sheet (Zukowski)](https://www.valadria.com/steam-dev-cheat-sheet/)
- [Steam Page Timing Disaster — Indie Launch Lab](https://indielaunchlab.com/blog/the-steam-page-timing-disaster-why-coming-soon-isnt-soon-enough)
- [Postmortem of My First Indie Game (Carotenuto)](https://www.gamedeveloper.com/business/postmortem-of-my-first-indie-game)
- [Edmund McMillen indie advice — gamepressure](https://www.gamepressure.com/newsroom/the-first-thing-i-really-need-to-do-is-refocus-on-my-family-says/z08b0f)
- [Edmund McMillen interview — USC Annenberg](https://www.uscannenbergmedia.com/2024/10/11/edmund-mcmillen-indie-as-ever/)
- [The Polished Prototype Trap — Wayline](https://www.wayline.io/blog/polished-prototype-trap-indie-game-innovation)
- [Steve Swink — Game Feel](https://www.gamedeveloper.com/design/game-feel-the-secret-ingredient)
- [Vlambeer's Nuclear Throne / Performative Game Development (GDC)](https://www.gdcvault.com/play/1020034/Performative-Game-Development-The-Design)
- [Crossy Road postmortem (Hipster Whale, GDC Vault)](https://gdcvault.com/play/1021897/Crossy-Road-A-Whale-of)
- [Caves of Qud — Brian Bucklew interview](https://www.rpgsite.net/interview/20000-brian-bucklew-caves-of-qud-interview-expansions-switch-port-unity-controller-support-coffee)
- [Game Developer Podcast 36 — Chris Zukowski indie marketing](https://www.gamedeveloper.com/marketing/game-developer-podcast-36-indie-marketing-advice-from-chris-zukowski)
- [Tynan Sylvester — Designing Games](https://tynansylvester.com/book/)
- [The Fire Fades — burnout in game dev](https://www.gamedeveloper.com/production/the-fire-fades-dealing-with-the-scourge-of-burnout-in-game-dev)
- [Part-Time Indie Dev: Burning Out (Boozayaangool)](https://medium.com/@ttanatb/part-time-indie-dev-burning-out-before-you-even-make-it-c24d9041a89f)
- [Version control for gamedev](https://www.gamedeveloper.com/production/version-control-effective-use-issues-and-thoughts-from-a-gamedev-perspective)
- [Early Playtesting Lessons — GDevelop](https://gdevelop.io/blog/early-playtesting-lessons-from-astro-burn)
- [Toby Fox — Undertale Dev2Dev interview](https://meloshantani.wordpress.com/2013/05/25/toby-foxs-undertale-dev-2-dev-interview-1/)
- [Lostgarden — Daniel Cook game design essays](https://lostgarden.com/)
