# Production + Scope + VCS — Research Report

## Domain summary

Indie games almost never die from a missing rendering technique or a bad shader. They die from scope, then from data loss, then from burnout — a well-trodden order documented in postmortem after postmortem on r/gamedev, Game Developer's "Why Indie Games Fail" surveys, and decades of GDC vault talks. Eric Barone shipped Stardew Valley at 10 hours/day, 7 days/week for 4.5 years; Toby Fox shipped Undertale in 2.7 years only by delegating art to Temmie Chang; Tommy Refenes spent ~18 months in head-down crunch on Super Meat Boy. The constant across these timelines is that engineering effort scales linearly with the engine you pick, but production effort scales superlinearly with feature count. Production discipline — scope cuts, milestone gates, version control, backups, playtesting cadence — is what converts solo ambition into a shipped binary. For a novice solo Godot 4 dev, this is the curriculum chapter that earns its keep more than any other.

## Topic catalog

### Scope discipline as the master variable
- **Brief**: A game is whatever survives the cut list. ATNO's 2026 indie failure analysis reports >70% of failed-or-abandoned solo projects cited "scope too large" as a primary cause. Vampire Survivors shipped with one mechanic (walk + auto-attack) and earned $10M+.
- **Why-drilled**: L1 — scope creep produces missed deadlines and burnout. L2 — every feature you add costs implementation, debugging, asset, balance, playtest, and onboarding tax. **First principle**: solo dev hours are bounded; ambition is not. Hours scale linearly with calendar time; feature interactions scale combinatorially.
- **Reference**: ATNO Medium 2026; Wayline "Scope Creep in Indie Games"; Derek Yu on "scope honesty."
- **Novice failure mode**: "I'll add multiplayer later." Multiplayer is an architectural decision, not a feature.
- **Priority**: P0 (top of must-include).

### The "what's the smallest version of this game that's still YOUR game" diagnostic
- **Brief**: Derek Yu's scope honesty test: write down the 3 mechanics that, if removed, make this not your game. Everything else is on the cut list.
- **Why-drilled**: L1 — feature attachment is emotional, not analytical. L2 — pre-committing to a minimum core moves cuts from "loss" to "discipline." **First principle**: you cannot evaluate "is this feature worth it" against an unbounded vision; you can against a written core.
- **Reference**: Derek Yu, *Spelunky* (Boss Fight Books #11), "Finishing a Game" chapter.
- **Novice failure mode**: every feature feels essential because none have been written down as non-essential.
- **Priority**: P0.

### Cut by 90% / "the game gets smaller, then it ships"
- **Brief**: Most indie postmortems describe a moment of brutal cut where the project lost ~50-90% of its planned content and immediately got better. Tommy Refenes cut entire chapters of Super Meat Boy late in dev.
- **Why-drilled**: L1 — late-stage scope cuts feel like failure but are how shipped games happen. L2 — feature value follows a power law; the top 20% of features carry 80% of the player experience. **First principle**: economics of solo dev — bounded hours forces a feature ROI ranking that AAA can paper over with team size.
- **Reference**: Refenes/Indie Game: The Movie postmortem; Game Developer "How Spelunky got finished."
- **Novice failure mode**: cutting feels like quitting; it's the opposite.
- **Priority**: P0.

### Milestone architecture: greybox → vertical slice → alpha → beta → RC → ship
- **Brief**: Greybox (mechanics work, programmer art) → playable prototype (1 level fun for 5 min) → vertical slice (one slice at shipping quality) → alpha (all features in, content incomplete) → beta (content-complete, bug-fixing) → RC (no new bugs) → ship.
- **Why-drilled**: L1 — milestones force an artifact that proves a question. L2 — each milestone proves something *to a different audience*: yourself (greybox), playtesters (prototype), publisher/audience (vertical slice), QA (alpha/beta). **First principle**: undefined "done" allows infinite drift; defined milestones are the only mechanism that converts "this might ship" into "this will ship."
- **Reference**: Tumblr/AskAGameDev "Game Dev Glossary"; HacknPlan "Understanding vertical slicing"; Xsolla "Funding 101."
- **Novice failure mode**: jumping straight to "alpha" because greybox feels embarrassing.
- **Priority**: P0.

### MVP vs vertical slice — distinct artifacts
- **Brief**: MVP = thinnest playable feature set, programmer art OK, used to gather feedback. Vertical slice = one ~5-minute zone at shipping quality, used to evaluate "is this fun at full polish?" and to convince others.
- **Why-drilled**: L1 — confusing them produces gold-plated MVPs (slow) or content-thin vertical slices (uninformative). **First principle**: each artifact answers a different question; conflating them wastes the answers.
- **Reference**: Tumblr AskAGameDev glossary; Maurice Klimek, "The next thing after MVP"; Wikipedia "Vertical slice."
- **Novice failure mode**: spending 6 months polishing the MVP into a vertical slice without ever validating the loop.
- **Priority**: P1.

### Why git breaks on binary assets
- **Brief**: git's compression and merge model assumes line-diffable text. PNGs, .blend, .wav, .ogg, .fbx files are opaque blobs; every change rewrites the whole file into history.
- **Why-drilled**: L1 — repos balloon to many GB and clones become unusable. L2 — diff and merge are useless for binaries; conflicts must be resolved by lock-or-overwrite. **First principle**: git's diff model assumes text. Binary assets violate this assumption.
- **Reference**: Godot docs "Version control systems"; Anchorpoint "Git for Godot."
- **Novice failure mode**: committing 200MB of PSD source files to plain git, then discovering GitHub's 1GB and 100MB limits at the worst moment.
- **Priority**: P0.

### Git LFS — what it solves and what it doesn't
- **Brief**: Git LFS replaces large files with text pointers; the binary lives on a separate LFS server. Solves clone size and bandwidth. Does not solve: conflict resolution on binaries (still need locks), GitHub LFS bandwidth caps, or vendor lock-in to LFS hosting.
- **Why-drilled**: L1 — naive LFS still allows two devs to edit a .tscn simultaneously. L2 — solo doesn't need locks today, but discipline matters when collaborators arrive. **First principle**: LFS solves the storage problem, not the concurrency problem; locks solve the concurrency problem.
- **Reference**: Snopek Games "Godot and Git Part 8"; Anchorpoint Godot guide; godot-git-plugin issue #235.
- **Novice failure mode**: treating LFS as a magic bullet; not configuring `.gitattributes` until after binaries are already in history (too late — must rewrite history).
- **Priority**: P0.

### .gitignore for Godot 4
- **Brief**: Ignore `.godot/`, `.import/`, `export.cfg`, `export_presets.cfg` (or version with care), `*.translation` if generated. Use the GitHub `Godot.gitignore` template.
- **Why-drilled**: L1 — `.godot/` is per-machine cache; committing it produces phantom diffs every editor open. **First principle**: only commit authoritative source-of-truth artifacts; everything else is regeneratable.
- **Reference**: github/gitignore Godot.gitignore; Karbassi gist; Godot forums.
- **Novice failure mode**: committing `.godot/` and getting 5,000-line diffs on every commit.
- **Priority**: P0.

### Branching strategy for solo: trunk-based + WIP
- **Brief**: Solo dev should commit to `main` directly with frequent small commits, using short-lived WIP branches only for risky experiments. GitFlow's release/develop/feature topology is overhead with no payoff at n=1.
- **Why-drilled**: L1 — solo developers have no merge conflicts with themselves. L2 — long-lived branches accumulate drift; trunk-based forces small, integrated changes. **First principle**: branching strategies exist to coordinate humans; with one human, the simplest strategy wins.
- **Reference**: Atlassian "Trunk-based Development"; trunkbaseddevelopment.com.
- **Novice failure mode**: copying GitFlow from a tutorial and never touching the develop branch.
- **Priority**: P1.

### Plastic SCM / Diversion / Perforce — when overkill begins
- **Brief**: Plastic SCM (now Unity Version Control), Perforce Helix Core, and Diversion specialize in large binary repos with locking. Perforce is AAA standard; Diversion is the modern cloud alternative; Plastic ties to Unity's ecosystem.
- **Why-drilled**: L1 — these solve problems solo Godot devs rarely have (terabyte repos, 10+ artists locking the same scene). L2 — the cost is config overhead, vendor lock-in, server maintenance. **First principle**: classify before choosing the tactic — Perforce serves teams of 10+ artists pushing GB-scale uncompressible binaries daily.
- **Reference**: Anchorpoint "Best Version Control 2026"; Diversion blog "3 Reasons Devs Look for Perforce Alternatives"; Perforce vs Plastic comparison.
- **Novice failure mode**: setting up a Perforce server before shipping anything.
- **Priority**: P2 (mention, don't recommend yet).

### Backups: 3-2-1 rule
- **Brief**: 3 copies of the project, on 2 different media, 1 off-site. Plus the modernized 3-2-1-1-0: one immutable/offline + zero verified backup errors.
- **Why-drilled**: L1 — drives fail; SSDs degrade; clouds get account-locked. L2 — your local SSD + OneDrive are still 1 medium with correlated failure (sync deletes propagate). **First principle**: independent failure domains; correlated copies aren't backups.
- **Reference**: Backblaze "3-2-1 Backup Strategy"; Datto and Commvault explainers.
- **Novice failure mode**: relying on cloud sync alone — when OneDrive corrupts the `.godot/` lockfile, sync propagates corruption to every machine.
- **Priority**: P0.

### Cloud sync caveats with Godot
- **Brief**: OneDrive / Dropbox / iCloud sync the project folder live. Godot writes to `.godot/` constantly; sync conflicts and partial uploads can corrupt project state mid-edit.
- **Why-drilled**: L1 — sync is not version control. L2 — sync's "last writer wins" model is incompatible with editor lockfiles. **First principle**: sync optimizes availability; VCS optimizes integrity. Mixing them produces neither.
- **Reference**: Godot forums on `.godot/` directory; community reports on lost work via sync conflicts.
- **Novice failure mode**: project folder lives in OneDrive *and* git; weird breaks no one can debug.
- **Priority**: P1.

### Project structure: per-feature beats per-type
- **Brief**: Group folders by feature (`/player/`, `/enemies/goblin/`, `/ui/inventory/`) with each scene's exclusive resources alongside it. Reserve `assets/`, `shaders/`, `addons/` for cross-cutting shared resources.
- **Why-drilled**: L1 — by-type (`scenes/`, `scripts/`, `textures/`) requires hopping 3 folders to edit one feature. L2 — feature-folders match how you think about the game, not how files are typed. **First principle**: locality of reference for human cognition. The feature is the unit you reason about; co-locating files matches reasoning.
- **Reference**: Godot docs "Project organization"; abmarnie/godot-architecture-organization-advice repo.
- **Novice failure mode**: copying a Unity tutorial's `Scripts/`, `Prefabs/`, `Materials/` layout into Godot.
- **Priority**: P1.

### First-time-player observational playtesting
- **Brief**: Sit a fresh tester in front of the game with no instructions; do not coach; take notes on every confusion. Each tester is single-use for first-impression data.
- **Why-drilled**: L1 — first-time confusion is invisible to the developer ("you can't unsee the solution"). L2 — interrupting to help destroys the data point. **First principle**: bounded fresh attention — there are only ~N people who will ever see the game's intro for the first time, and you cannot unsee anything yourself.
- **Reference**: Steve Bromley user research blog; Ludogogy "Playtest Like a Researcher"; Game Developer "Playtesting for Indies"; Indieop "How to Run Your First Indie Game Playtest."
- **Novice failure mode**: reading instructions to the playtester, then concluding "the tutorial works."
- **Priority**: P0.

### Playtest cadence and recruitment
- **Brief**: Start at vertical slice (when there's something to evaluate), run testers in waves of 3-5, swap testers every milestone. Recruit via friends → indie communities (TIGSource, r/playmygame) → paid panels (Playtest Cloud) as quality demands.
- **Why-drilled**: L1 — same-tester twice produces familiarity bias. L2 — the question "is this good?" decomposes into onboarding, mid-game, late-game; each needs a different tester pool. **First principle**: bimodal — first-time-player feedback and experienced-player feedback measure different things; SLO them separately.
- **Reference**: Wayline "Effective Playtesting Strategies"; Game Developer Playtesting for Indies; Level Design Book.
- **Novice failure mode**: testing only with the discord that's been following dev for 6 months.
- **Priority**: P1.

### Bug tracking with reproduction steps
- **Brief**: GitHub Issues, HacknPlan, or a `BUGS.md` in the repo. Required field: reproduction steps. Optional: severity, expected, actual, build version.
- **Why-drilled**: L1 — bugs without repro steps decay into "vibe complaints." L2 — the act of writing repro steps fixes ~30% of bugs. **First principle**: observable success — a bug ticket without a falsifiable repro is decorative.
- **Reference**: HacknPlan "Best PM Tools for Game Dev"; Codecks 8 best PM tools.
- **Novice failure mode**: 47 untriaged "this feels off" issues, none reproducible.
- **Priority**: P1.

### Time management: 2-hour focused sessions, daily small step
- **Brief**: Most solo postmortems converge on small consistent sessions over heroic sprints. Eric Barone's 10-hour days are the survivor-bias outlier; he had a partner working two jobs.
- **Why-drilled**: L1 — long sessions degrade output quality. L2 — daily commits compound; sporadic crunches don't. **First principle**: solo dev is a marathon; pace is the rate-limiting variable, not peak intensity.
- **Reference**: Wayline "Solo Dev Survival Guide"; Ninichi "6 Ways to Manage Burnout"; Game Developer "How Eric Barone coped."
- **Novice failure mode**: 40-hour weekend, 0-hour Monday-Thursday.
- **Priority**: P1.

### The boredom problem (mid-project malaise)
- **Brief**: Months 4-9 of a 12-month project are the danger zone — novelty is gone, ship is far. Common collapse point.
- **Why-drilled**: L1 — motivation is finite; novelty was carrying the early phase. L2 — without external accountability, mid-project gets quietly abandoned. **First principle**: motivation is not a stable fuel; structure (milestones, devlogs, playtest dates) is what carries projects through the trough.
- **Reference**: Wayline "Top 7 Solo Game Dev Burnout Q&A"; Romain Mouillard Medium "Solo Game Dev: Lessons Learned."
- **Novice failure mode**: starting a "smaller side project" at month 6, abandoning the main one.
- **Priority**: P1.

### Public devlog as accountability
- **Brief**: Weekly post on TIGSource forums, r/gamedev's Screenshot Saturday, or Twitter/Bluesky. Cost: 1 hour/week. Benefit: external commitment device + early audience formation. Chris Zukowski reports email lists in the 5-25k range from sustained devlog presence (Tiny Glade case).
- **Why-drilled**: L1 — public stakes change behavior. L2 — devlogs double as marketing groundwork. **First principle**: accountability needs an audience; a private commit graph isn't an audience.
- **Reference**: Chris Zukowski howtomarketagame.com; GDC Podcast ep. 24; Zukowski/Tiny Glade case study.
- **Novice failure mode**: starting devlog, missing one week, never posting again.
- **Priority**: P1.

### AI-assisted dev: spec discipline + test-after
- **Brief**: When the AI writes the code, the spec becomes the load-bearing artifact. Code review still required even for code you didn't write. Tests written *after* AI-generated code function as a behavioral characterization, not a TDD spec.
- **Why-drilled**: L1 — AI generates plausible-looking code that doesn't match intent. L2 — without a written spec, "did the AI do the right thing?" has no ground truth. **First principle**: schema/spec is the contract; the implementation is one rendering of it. Without the schema you cannot validate any rendering.
- **Reference**: Refactoring CODE_AGE protocol (production vs never-deployed); LLM-authored test mutation-testing requirement.
- **Novice failure mode**: accepting AI code that compiles and runs once, never checking whether it does what was intended in edge cases.
- **Priority**: P0 (this user's specific risk).

### "Done is better than perfect"
- **Brief**: Jeff Vogel: "perfection is the enemy of completion." Define "done" up front and stick to it. Spiderweb Software's 30-year career rests on a back catalog of *finished* games.
- **Why-drilled**: L1 — polish has diminishing returns; ship has a step-function return (zero → revenue, audience, learning). **First principle**: an unshipped game produces no signal; shipping is the only learning event with empirical feedback.
- **Reference**: Jeff Vogel, "Six Truths"; Cobble.games "30-Year Indie Game"; Thomas Brush podcast ep. 46.
- **Novice failure mode**: chasing AAA-quality juice on a hobby project.
- **Priority**: P0.

## Recommended solo dev workflow

For a novice solo dev shipping a 2D Godot 4 game in ~12 months:

**Month 0 — setup (1 week)**
- Create GitHub repo, commit `Godot.gitignore` and `.gitattributes` BEFORE first asset.
- Configure Git LFS for `*.png *.jpg *.wav *.ogg *.mp3 *.fbx *.glb *.blend *.psd *.aseprite`.
- Set up 3-2-1 backup: local working drive + external drive (weekly mirror) + cloud (GitHub repo + a separate cloud archive of `.zip` releases). Verify a restore on day 7.
- Project on a *non-synced* path (e.g., `C:\dev\mygame`), not OneDrive.
- Write a 1-page design doc: title, 1-paragraph pitch, 3 core mechanics, 3 cuts you've already made.
- Trello/Notion/HacknPlan or a `TODO.md` in the repo. Pick whichever you'll actually open daily; switching tools is procrastination.

**Months 1-2 — greybox / playable prototype**
- Programmer art only. Goal: 5 minutes of fun in a single level with the core loop.
- First playtest at end of month 2, one tester, observational protocol.
- Daily commits to `main`. Tag `v0.1-prototype` when fun.

**Months 3-4 — vertical slice**
- One zone at near-final quality (art, sound, juice).
- Playtest with 3 fresh testers; do not assist.
- Tag `v0.2-vertical-slice`.
- Public devlog starts here. Weekly post.

**Months 5-8 — alpha (content build)**
- All systems implemented; content fills in. This is the boredom trough; lean on milestones and devlog cadence.
- Playtest every 4 weeks with new testers.
- Mid-month 6: scope review. Cut ~30% of remaining list.
- Tag `v0.5-alpha` when content-complete.

**Months 9-10 — beta (bug fixing)**
- No new features. Bug list only, with required repro steps.
- Playtest with 5+ fresh testers.
- Tag `v0.9-beta`.

**Months 11-12 — RC + ship**
- Last cut: anything still broken at month 11.5 gets cut, not fixed.
- Steam page live month 11; demo if appropriate.
- Ship.

## Must-include shortlist

1. Scope discipline + the "smallest-version-still-yours" diagnostic.
2. Milestone architecture (greybox → vertical slice → alpha → beta → ship).
3. Git + Git LFS + Godot `.gitignore` from day one.
4. 3-2-1 backups, verified.
5. First-time-player observational playtesting starting at vertical slice.
6. Public devlog as accountability + audience seed.
7. AI-assisted dev: spec-first, code review, test-after.
8. "Done is better than perfect."

## Commonly oversold

- **Sprints, scrum ceremonies, JIRA**: built for teams of 5-50 to coordinate. Solo dev with daily standups against a mirror is procrastination theater.
- **Perforce / Plastic SCM for solo Godot**: overkill until there's a second human pushing assets daily.
- **Elaborate Notion dashboards**: setup time is dev time stolen. A `TODO.md` in the repo is a feature, not a limitation.
- **Burndown charts and velocity tracking**: solo velocity is "did I work today." A streak counter beats a Gantt chart.
- **GitFlow with develop/release/hotfix branches**: zero payoff at n=1.
- **TDD on every feature**: TDD's predicate-stability precondition fails on game feel and mechanics. Use it for save/load, scoring, save-state migration; not for jump arc tuning.

## Cross-references

- **Engine selection (agent 1-3)**: Godot 4 keeps version control simple — text-format `.tscn` scenes diff in git, unlike Unity's binary scenes. Major reason to choose Godot for solo.
- **Audio/Art pipeline (agent 8-10)**: LFS configuration must include all source asset types (`.aseprite`, `.psd`, `.blend`, `.wav`).
- **Marketing/launch (agent 15-17)**: vertical slice doubles as Steam page trailer source; devlog cadence in months 5-8 builds the launch wishlist; Chris Zukowski's "build audience while you build the game" applies here.
- **Game design (agent 4-7)**: scope discipline upstream of mechanics — the design doc's 3-mechanic constraint is the input to mechanic design, not an afterthought.

## Sources

- [Tommy Refenes - Wikipedia](https://en.wikipedia.org/wiki/Tommy_Refenes)
- [Postmortem: Team Meat's Super Meat Boy](https://www.gamedeveloper.com/audio/postmortem-team-meat-s-i-super-meat-boy-i-)
- [Eric Barone - Wikipedia](https://en.wikipedia.org/wiki/Eric_Barone)
- [How Stardew Valley creator Eric Barone coped with a four year dev cycle](https://www.gamedeveloper.com/production/how-i-stardew-valley-i-creator-eric-barone-coped-with-a-four-year-dev-cycle)
- [Toby Fox - Wikipedia](https://en.wikipedia.org/wiki/Toby_Fox)
- [Undertale - Wikipedia](https://en.wikipedia.org/wiki/Undertale)
- [Inscryption - Wikipedia](https://en.wikipedia.org/wiki/Inscryption)
- [How a game jam on "sacrifices" became Inscryption](https://www.gamedeveloper.com/design/how-game-jam-sacrifices-became-inscryption)
- [Spelunky by Derek Yu – Boss Fight Books](https://bossfightbooks.com/products/spelunky-by-derek-yu)
- [How Spelunky got its procedural 'hook' & actually got finished](https://newsletter.gamediscover.co/p/how-spelunky-got-its-procedural-hook)
- [Jeff Vogel - The Bottom Feeder blog](http://jeff-vogel.blogspot.com/)
- [The 30-Year Indie Game: Jeff Vogel's Timeless Blueprint](https://cobble.games/wise-inspiring-smart/unsorted/the-30-year-indie-game-jeff-vogels-timeless-blueprint-for-sustainable-creativity)
- [Jeff Vogel: Making Games Alone For 30 Years (Thomas Brush ep. 46)](https://www.youtube.com/watch?v=F9zYiHllEcU)
- [Why 90% of Indie Games Fail (ATNO, 2026)](https://medium.com/@atnoforgamedev/why-90-of-indie-games-fail-and-the-5-things-successful-ones-do-differently-91922b4c09d3)
- [Scope Creep in Indie Games (Wayline)](https://www.wayline.io/blog/scope-creep-indie-games-avoiding-development-hell)
- [GameDev Protips: How To Kick Scope Creep In The Ass (Daniel Doan)](https://medium.com/@doandaniel/gamedev-protips-how-to-kick-scope-creep-in-the-ass-and-ship-your-indie-game-8fa3051500d1)
- [Game Dev Glossary: Prototype, Vertical Slice, MVP (AskAGameDev)](https://www.tumblr.com/askagamedev/746300998961741824/game-dev-glossary-prototype-vertical-slice)
- [Vertical slice - Wikipedia](https://en.wikipedia.org/wiki/Vertical_slice)
- [Funding 101: The impact of the vertical slice (Xsolla)](https://xsolla.com/blog/funding-101-the-impact-of-the-vertical-slice)
- [Best practices for setting up Git LFS - Godot Forum](https://forum.godotengine.org/t/best-practices-for-setting-up-git-lfs/75357)
- [Godot and Git: Git LFS and large assets (Snopek Games)](https://www.snopekgames.com/tutorial/2020/godot-and-git-part-8-git-lfs-and-dealing-large-assets/)
- [Git Version Control for Godot (Anchorpoint)](https://www.anchorpoint.app/blog/git-version-control-for-godot)
- [Version control systems — Godot docs](https://docs.godotengine.org/en/stable/tutorials/best_practices/version_control_systems.html)
- [github/gitignore - Godot.gitignore](https://github.com/github/gitignore/blob/main/Godot.gitignore)
- [Godot 4.3 gitignore + git lfs (Karbassi gist)](https://gist.github.com/karbassi/ce1f3cb68b3c6fc3c471cf992aed0053)
- [Best Version Control for Game Development 2026 (Anchorpoint)](https://www.anchorpoint.app/blog/version-control-for-game-development)
- [Perforce vs PlasticSCM for indie teams (Assembla)](https://get.assembla.com/blog/perforce-vs-plasticscm-indie-game-dev-teams/)
- [3 Reasons Game Developers Are Looking for Perforce Alternatives (Diversion)](https://www.diversion.dev/blog/3-reasons-game-developers-are-looking-for-perforce-alternatives)
- [Trunk Based Development](https://trunkbaseddevelopment.com/)
- [Trunk-based Development (Atlassian)](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development)
- [3-2-1 Backup Strategy (Backblaze)](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/)
- [3-2-1 Backup Rule (Datto)](https://www.datto.com/blog/3-2-1-backup-rule/)
- [Project organization — Godot docs](https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html)
- [godot-architecture-organization-advice (abmarnie)](https://github.com/abmarnie/godot-architecture-organization-advice)
- [Playtest Like a Researcher: Observational Playtesting (Ludogogy)](https://ludogogy.professorgame.com/article/playtest-like-a-researcher-basics-of-observational-playtesting/)
- [Some things I've learned about observing playtesting sessions (Steve Bromley)](https://www.stevebromley.com/blog/2015/03/19/some-things-ive-learned-about-observing-playtesting-sessions/)
- [Playtesting for Indies (Game Developer)](https://www.gamedeveloper.com/production/playtesting-for-indies)
- [How to Run Your First Indie Game Playtest (Indieop)](https://www.indieop.com/blog/how-to-run-your-first-indie-game-playtest-and-actually-get-useful-feedback)
- [How To Market A Game (Chris Zukowski)](https://howtomarketagame.com/)
- [Super practical indie game marketing with Chris Zukowski (GDC Podcast 24)](https://www.gamedeveloper.com/marketing/super-practical-indie-game-marketing-with-chris-zukowski---gdc-podcast-ep-24)
- [Solo Dev Survival Guide: First Indie Game (Wayline)](https://www.wayline.io/blog/solo-dev-survival-guide-first-indie-game)
- [Top 7 Questions About Solo Game Dev Burnout (Wayline)](https://www.wayline.io/blog/top-7-solo-game-dev-burnout-qa)
- [6 Ways to Manage Burnout as an Indie Game Developer (Ninichi)](https://ninichimusic.com/blog/6-ways-to-manage-burn-out-as-an-indie-game-developer)
- [Best project management tools for game developers (HacknPlan)](https://hacknplan.com/best-project-management-tools-for-game-developers/)
- [Project planning for solo game developers (HacknPlan)](https://hacknplan.com/project-planning-for-solo-game-developers/)
- [Solo Game Development in 2026: What Actually Works (Ziva)](https://ziva.sh/blogs/solo-game-development)
- [No More Robots - Wikipedia](https://en.wikipedia.org/wiki/No_More_Robots)
- [Data-Driven Indie Publishing with Mike Rose (Game Developer Podcast 13)](https://gamedeveloper.podbean.com/e/13-data-driven-indie-publishing-with-no-more-robots-mike-rose/)
