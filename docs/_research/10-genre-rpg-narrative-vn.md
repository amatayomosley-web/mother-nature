# RPG / Narrative / VN — Research Report

## Domain summary

RPGs, narrative-heavy adventures, and visual novels all share one defining characteristic: the player's history matters. A platformer cares about the current frame; an RPG cares about whether you spared the dog in chapter one and how that affects the priest's dialogue in chapter twelve. This pushes every design problem onto a single substrate — *persistent state* — and that substrate is where novices drown. State explodes combinatorially (n binary flags = 2^n possible save shapes), schema changes silently corrupt months of player saves, branching dialogue O(2^n) eats writers alive, and "every choice matters" is a marketing slogan that engineering can never deliver. The novice trap is not learning Godot; it is underestimating that *every system you add multiplies into every other system*. Inventory + quests + dialogue + NPC schedule = a tesseract you cannot debug from any single angle. The hardest engineering problem is save/load reliability — bugs there are career-defining because they destroy player goodwill irrecoverably.

## Mechanic catalog

### Stat systems (attributes, derived, formulas)
- **Brief**: Primary attributes (Str/Dex/Int) drive derived stats (Damage = Str * 2 + Weapon). Formulas determine feel.
- **Why-drilled**: L1: Players need to see growth. L2: Numbers are the cheapest legible representation of growth. First principle: humans pattern-match progress through quantifiable change; without it, hours of play feel directionless.
- **Reference game**: Final Fantasy series (squared progression), Disco Elysium (24 skills as voices, not bars).
- **Novice failure mode**: Adding stats that don't actually drive any formula — decorative numbers. Or the inverse: stats so coupled (Str -> HP -> Defense -> Dodge) that one tweak rebalances the whole game.
- **Priority**: HIGH.
- **Source**: gabrielchauri.com Disco Elysium analysis; pavcreations.com level systems.

### Progression curves (XP)
- **Brief**: Linear (X per level), exponential (X * k^n), polynomial (Final Fantasy's 4th-order regression), sigmoid (front-loaded then plateau).
- **Why-drilled**: L1: Linear bores at endgame; pure exponential explodes into incomprehensible numbers. L2: Reward cadence must match content cadence. First principle: dopamine hits per hour-played should stay roughly constant from hour 1 to hour 60.
- **Reference**: SquareEnix uses squared formulas; many JRPGs cap at level 99 specifically to avoid 64-bit overflow theatre.
- **Novice failure mode**: Picking exponential because "RPGs use exponential" without computing the level-99 cost (often ends up being 10^15 XP — meaningless).
- **Priority**: MEDIUM.
- **Source**: davideaversa.it/blog/gamedesign-math-rpg-level-based-progression; onlyagame.typepad.com mathematics of XP.

### Save/load reliability
- **Brief**: Serialize game state to disk; deserialize back. Sounds trivial. Isn't.
- **Why-drilled**: L1: Saves are players' time. L2: A save file is a snapshot of a specific code version's data layout. L3: The *next* code version has different layout. First principle: saves are cross-version contracts, and contracts require schemas, versioning, and migration discipline.
- **Reference**: Stardew Valley's persistence model; Ren'Py's persistent object.
- **Novice failure mode**: Serializing live class instances directly (Godot resources, C# `[Serializable]`). Adding one field three months later breaks every existing save. Career-defining.
- **Priority**: CRITICAL.
- **Source**: forums.rpgmakerweb.com (save versioning best practices); developers.meta.com Save Game Best Practices.

### Dialogue trees
- **Brief**: Nodes of text + choices that link to other nodes; sometimes with conditions (`if relationship > 5`).
- **Why-drilled**: L1: Hand-coding dialogue in `if/else` collapses by hour 5. L2: Writers and programmers need separate workflows. First principle: dialogue is content, content needs a non-code authoring surface.
- **Reference**: Ink (Inkle), YarnSpinner, Dialogic (Godot), Ren'Py, Twine, Undertale (giant switch statement — viable only for one writer-coder).
- **Novice failure mode**: Storing dialogue in giant `match` statements like Undertale; works for Toby Fox, fails for everyone else when localizers and writers join.
- **Priority**: HIGH.
- **Source**: github.com/inkle/ink Documentation/WritingWithInk.md; nathanhoad/godot_dialogue_manager.

### Branching narrative architecture
- **Brief**: Linear, gauntlet, branch-and-bottleneck (hub-and-spoke), time-cave (full branching), quality-based.
- **Why-drilled**: L1: True branching scales O(2^n) — at n=10 that's 1,024 unique paths. L2: Indies cannot write 1,024 paths. First principle: convergence is mandatory at scale; the question is *where* you converge.
- **Reference**: Inkle's 80 Days uses branch-and-bottleneck. Failbetter rejects branching entirely for QBN.
- **Novice failure mode**: Promising "every choice matters" then writing endless dead-end branches you cannot finish.
- **Priority**: HIGH.
- **Source**: heterogenoustasks.wordpress.com Standard Patterns in Choice-Based Games (Emily Short).

### Quality-based narrative (storylets)
- **Brief**: Atomic story chunks gated by quality thresholds (`Shadowy >= 5`). Engine surfaces eligible storylets; player picks.
- **Why-drilled**: L1: Eliminates explicit linking — content is free-floating. L2: Adding new content doesn't require rewiring old content. First principle: "a flag-set is a story" — preconditions become narrative grammar.
- **Reference**: Failbetter's Fallen London, Sunless Sea, Sunless Skies; Cultist Simulator/Weather Factory's "resource narrative" extension.
- **Novice failure mode**: Treating QBN as "just gating dialogue with flags" without designing the *aesthetic* of discovery; ends up feeling like checklists.
- **Priority**: MEDIUM (high if your game design fits).
- **Source**: emshort.blog Beyond Branching; brunodias.dev/2017/05/30/an-ideal-qbn-system.html; weatherfactory.biz/qbn-to-resource-narratives/.

### Quest systems
- **Brief**: Linear quest log -> multi-stage quest -> quest dependency graph (DAG).
- **Why-drilled**: L1: Quests have state. L2: Quest state IS save state — every quest variable persists. First principle: quests are flag-machines wearing a UI. The flags are the contract.
- **Reference**: WoW's directed-gameplay model; Horizon Zero Dawn quest design (GDC).
- **Novice failure mode**: Storing quest state across global vars + scene properties + autoload singletons + quest UI strings. Save corruption follows.
- **Priority**: HIGH.
- **Source**: gamedeveloper.com The Quest for the custom quest system; gdcvault.com Horizon Zero Dawn.

### Inventory (stack vs slot, weight)
- **Brief**: Stackable consumables vs unique slot items; weight-based encumbrance vs slot count.
- **Why-drilled**: L1: Inventory looks like a list, isn't. L2: Equip/unequip mutates derived stats; stacking partially-full slots requires policy; UI drag-drop touches every system. First principle: inventory is a small ECS in disguise.
- **Reference**: Diablo grid inventory, Skyrim weight, classic JRPG slot count.
- **Novice failure mode**: Coupling inventory state to UI nodes (delete the panel scene, lose the items).
- **Priority**: HIGH.
- **Source**: scottlilly.com stackable inventory in C#; easybeak.itch.io isometric RPG dev log.

### NPC schedules
- **Brief**: NPCs have positions, dialogue, and behavior dependent on time-of-day, season, weather, festival, and friendship.
- **Why-drilled**: L1: A single NPC with 7 days * 4 seasons * 11 heart-levels = 308 dialogue contexts. L2: Multiply by 30 NPCs = 9,240 dialogue strings. First principle: scheduling is combinatorial; data-drive it or die.
- **Reference**: Stardew Valley.
- **Novice failure mode**: Hand-coding "if Monday and spring then..." instead of building a schedule data table.
- **Priority**: MEDIUM (high for life-sim/social games).
- **Source**: stardewvalleywiki.com Modding:Dialogue, Modding:Event_data.

### Dialogue presentation (portrait box, full VN, barks)
- **Brief**: Portrait + textbox (Stardew, Persona), full-screen VN (Ren'Py default), in-world barks (Undertale combat).
- **Why-drilled**: L1: Each style has different camera, layout, input flow. First principle: presentation is part of the contract — switching styles mid-project requires rewriting your dialogue system's render layer.
- **Reference**: Ren'Py for VN; Dialogic for Godot supports both.
- **Novice failure mode**: Hard-coding the textbox UI to specific portraits, then needing to add a third character.
- **Priority**: MEDIUM.
- **Source**: worldeater-dev.itch.io Bittersweet Birthday devlog.

### Variable interpolation / localization seam
- **Brief**: Replace `{player_name}` in dialogue strings at render time.
- **Why-drilled**: L1: Looks easy. L2: Localized text has different word order, gendered articles, plurals (`1 apple` vs `5 apples` vs Russian's three plural forms). First principle: string interpolation in monolingual code is concatenation; in localized code it's templating with grammar rules.
- **Reference**: Ren'Py's `[player_name]` substitution; ICU MessageFormat.
- **Novice failure mode**: `"You have " + str(n) + " gold"` — unfixable for German, Russian, Japanese.
- **Priority**: HIGH (if shipping outside one language).
- **Source**: gamedeveloper.com 9 Game Localization Best Practices.

### Choice consequences (immediate/deferred, real/illusory)
- **Brief**: Immediate (next line changes), deferred (Act 3 callback), surfaced (Mass Effect's wheel), hidden (Disco Elysium's quiet variables).
- **Why-drilled**: L1: True consequence requires every downstream scene to branch — combinatorial again. L2: Disco Elysium fakes it via *narrative superposition* — the same plot beat colored differently by accumulated state. First principle: agency is felt as *recognition*, not *divergence*. The game remembering you is more powerful than the game forking.
- **Reference**: Disco Elysium, Mass Effect.
- **Novice failure mode**: Promising consequence, delivering forked dialogue lines that re-converge identically two scenes later. Players notice.
- **Priority**: MEDIUM.
- **Source**: post45.org Disco Elysium and Narrative Superposition; firstpersonscholar.com Reconceiving Player Agency.

### Romance / relationship systems
- **Brief**: Point-based (heart counters, raised by gifts/dialogue) vs flag-based (specific scenes seen).
- **Why-drilled**: L1: Pure points feel transactional ("gift-grinding"). L2: Pure flags feel arbitrary. First principle: hybrid is correct — points gate flag-events, flag-events update points.
- **Reference**: Stardew Valley (heart points + heart events).
- **Novice failure mode**: Romance progressing to marriage via raw point count without any narrative event in between.
- **Priority**: MEDIUM.
- **Source**: stardewvalleywiki.com Friendship.

### Skill checks (deterministic vs random, hidden vs transparent)
- **Brief**: Deterministic threshold (Stat >= 5) vs dice roll (2d6 + Stat vs DC). Visible to player or hidden.
- **Why-drilled**: L1: Random checks add tension but punish unlucky players. L2: Disco Elysium runs *passive* skill checks invisibly to surface dialogue lines. First principle: checks shape *which content the player perceives exists*; hiding them hides the absent content too.
- **Reference**: Disco Elysium (2d6, passives hidden); Skyrim (deterministic Speech).
- **Novice failure mode**: Hidden random checks players can't reload to retry — feels arbitrary.
- **Priority**: MEDIUM.
- **Source**: discoelysium.fandom.com/wiki/Skills; awkwardmixture.blogspot.com Disco Elysium Skill Checks.

### Combat models (turn-based, ATB, RTwP, action)
- **Brief**: Turn-based (Pokémon), Active Time Battle (FF7), real-time-with-pause (Pillars of Eternity), action (Zelda).
- **Why-drilled**: L1: Each model demands different state granularity. L2: Turn-based serializes cleanly; action requires deterministic physics or live snapshots. First principle: combat model dictates save granularity inside combat (mid-fight saves, anyone?).
- **Reference**: Final Fantasy series across models.
- **Novice failure mode**: Picking action combat then realizing your novice physics layer can't be reliably saved/loaded.
- **Priority**: HIGH.
- **Source**: gamedeveloper.com Yasumi Matsuno design interviews context.

### Loot / drop tables
- **Brief**: Weighted random selection from a pool; pity timer prevents drought.
- **Why-drilled**: L1: Naive `random() < 0.05` produces visible streaks players blame on bugs. L2: Pity timer (probability rises after N failures) feels fairer without changing expected value much. First principle: humans cannot perceive true randomness as random; perceived fairness requires non-uniform sampling.
- **Reference**: Diablo, Genshin Impact (explicit pity).
- **Priority**: MEDIUM.
- **Source**: pulsegeek.com Loot Drop Rates Calculation Guide; gamedeveloper.com Defining Loot Tables.

### World state / reputation
- **Brief**: Persistent flags ("Killed mayor=true"), counters ("Renown=42"), regional reputation matrices.
- **Why-drilled**: L1: One flag is fine; 2,000 flags is a balance disaster. L2: Reputation matrices (faction × player) blow up storage and design space. First principle: state must be *queried often*, not just *written* — unread state is dead weight.
- **Novice failure mode**: Adding flags whenever convenient; never deleting; saves inflate; queries slow; nobody knows what `flag_173` means.
- **Priority**: HIGH.
- **Source**: forum.choiceofgames.com branching tips threads.

### Localization architecture
- **Brief**: All player-visible strings live outside source code in keyed files (.po/.csv/.json).
- **Why-drilled**: L1: Retrofitting localization touches every file. L2: Font support, RTL layout, text expansion (German +35%) are runtime concerns. First principle: localization is an architecture decision, not a content decision.
- **Reference**: Godot 4's CSV-based translation; ICU MessageFormat.
- **Novice failure mode**: Hardcoding strings, then trying to localize 18 months in. Almost never finished.
- **Priority**: HIGH (decide day one — even if you ship English-only).
- **Source**: gamedeveloper.com How to Prepare a Game for Localization.

### Text rendering (BBCode, wrapping, fonts)
- **Brief**: Variable-width fonts, word-wrap, in-line tags for color/speed/portrait. Godot 4's RichTextLabel uses BBCode.
- **Why-drilled**: L1: Looks easy. L2: Mid-text icons, wave effects, character-by-character reveal with skip — each feature crosses tokenizer boundaries. First principle: dialogue rendering is a small interpreter.
- **Reference**: Undertale's per-character timer + control tokens (color, sound, portrait).
- **Priority**: MEDIUM.
- **Source**: quora.com How did Toby Fox make dialogue boxes in Undertale.

### Auto-save vs manual save
- **Brief**: Auto-saves (every N minutes / on chapter), quicksaves (F5), manual slots (player names them).
- **Why-drilled**: L1: Auto-save mid-action can corrupt mid-write if the process dies. L2: Player needs N rolling autosave slots, not one (single overwriting auto-save is hostile). First principle: writes must be atomic — write to temp, rename. Lose a frame, never a save.
- **Reference**: Standard practice in roguelikes, JRPGs.
- **Novice failure mode**: One auto-save slot that overwrites every two minutes. Player dies mid-corruption, save is unrecoverable.
- **Priority**: CRITICAL.
- **Source**: developers.meta.com Save Game Best Practices.

## Must-include shortlist
1. **Save/load with versioning + migration** — non-negotiable, before save count > 10.
2. **External dialogue authoring (Dialogic / ink / YarnSpinner)** — never code dialogue in source.
3. **Localization seam (string keys, not literals)** — even for English-only games, costs little day-one.
4. **Quest state model = part of save schema** — quests aren't UI; they're persistent data.
5. **Branching strategy chosen explicitly (hub-and-spoke / QBN / linear with flags)** — not "we'll see."
6. **Stat system grounded in formulas** — every stat must drive at least one calculation.
7. **Atomic save writes (temp + rename)** — single biggest source of unrecoverable corruption.
8. **Inventory decoupled from UI** — model in a Resource/autoload, view in a scene.

## Save/load — separate deep-dive

Save/load is the single hardest engineering problem in narrative-heavy games and deserves its own chapter for three reasons.

**1. The schema-versioning trap.** A save file is a serialization of your data layout *at the version that wrote it*. Six months later, you've added a `quests_unlocked` field and renamed `hp` to `health`. Old saves now either crash on load (missing field) or silently load with garbage values. The fix is mandatory and well-known: every save file embeds a `version` integer, and the load path runs migration functions in sequence (`migrate_v3_to_v4(data); migrate_v4_to_v5(data);`) until the data matches the current version. RPG Maker's community calls this the "save version chain" — without it, every code change risks breaking every player's save.

**2. The atomicity trap.** A save write that's interrupted mid-flush leaves a half-written file. If you wrote directly to `save_1.dat`, the player loses everything. The standard pattern: write to `save_1.dat.tmp`, fsync, then rename atomically over `save_1.dat`. Every operating system guarantees rename atomicity. Skipping this on platforms where the player can pull the cartridge or kill the process (mobile, console) creates corruption events that destroy review scores.

**3. The reachability trap.** What is "the game state"? Naively: every variable. Realistically: only the variables that a future load needs. Confusing these means saves balloon (the entire scene tree pickled), or saves omit critical state (a counter local to a script that isn't persisted). Best practice: design an explicit *save schema* — a single class/Resource with named fields representing what gets persisted. Any state not in the schema is non-persistent by construction. This forces the question "is this state actually persistent?" to be answered when the field is added, not when a player reports a bug six months later.

**Common bugs**: Class references serialized directly (Godot's `Resource` save can pull dependencies you didn't intend). Save files containing absolute file paths from your dev machine. Saves that work in editor but fail in exported builds because resource paths differ. Saves that work on Windows but fail on Linux because of path separators. Saves that work today and fail tomorrow because daylight savings time changed a timestamp comparison.

The novice rule: write your save format, version it from `1`, never delete a migration. Test by saving in v1, loading in v5.

## Commonly oversold

- **"Every choice matters"**: Cannot scale; Disco Elysium's narrative-superposition trick is honest and rare.
- **Procedural quests**: Procgen makes hollow quests at AAA scale; indie procgen quests almost always feel like fetch-quest mad-libs.
- **Full voice acting**: Cripples iteration speed — locks dialogue text early. Indie games rewrite dialogue until launch.
- **Multi-language day-one ship**: Localization quality from a one-pass agency is poor; better to ship English well, then localize.
- **Mid-combat saves in action RPGs**: Implementation cost dwarfs player benefit; checkpoint-based saves are 90% as good.

## Cross-references

- **Save schema** (this report) ↔ **Quest data model** ↔ **Inventory model**: all must serialize to one schema with one version number.
- **Localization seam** ↔ **Dialogue trees**: dialogue files must be language-keyed from the start.
- **Stat formulas** ↔ **Combat model**: stats only mean what combat formulas use them for.
- **NPC schedules** ↔ **Dialogue trees** ↔ **World state**: NPC dialogue is conditioned on time + flags + relationship.
- **Choice consequence** ↔ **World state**: choices write flags; flags drive consequence; flags are saved.

## Sources

- [ink — inkle's narrative scripting language](https://www.inklestudios.com/ink/)
- [ink/Documentation/WritingWithInk.md (GitHub)](https://github.com/inkle/ink/blob/master/Documentation/WritingWithInk.md)
- [ink version 1.0 release](https://www.inklestudios.com/2021/02/22/ink-version-1)
- [Sunless Sea, 80 Days and the rise of modular storytelling — Game Developer](https://www.gamedeveloper.com/design/-i-sunless-sea-i-i-80-days-i-and-the-rise-of-modular-storytelling)
- [Attempted: Building a general-purpose QBN system — Bruno Dias](https://brunodias.dev/2017/05/30/an-ideal-qbn-system.html)
- [I've stopped talking about quality-based narrative — Weather Factory](https://weatherfactory.biz/qbn-to-resource-narratives/)
- [Beyond Branching: Quality-Based, Salience-Based, and Waypoint Narrative Structures — Emily Short](https://emshort.blog/2016/04/12/beyond-branching-quality-based-and-salience-based-narrative-structures/)
- [Storylets: You Want Them — Emily Short](https://emshort.blog/2019/11/29/storylets-you-want-them/)
- [Standard Patterns in Choice-Based Games — These Heterogenous Tasks](https://heterogenoustasks.wordpress.com/2015/01/26/standard-patterns-in-choice-based-games/)
- [Skills — Disco Elysium Wiki](https://discoelysium.fandom.com/wiki/Skills)
- [Disco Elysium: Skill Checks, Conversations, and Thoughts — Awkward Mixture](http://awkwardmixture.blogspot.com/2020/03/disco-elysium-skill-checks.html)
- [Reconceiving Player Agency with Disco Elysium — First Person Scholar](https://www.firstpersonscholar.com/reconceiving-player-agency-with-disco-elysium/)
- [Disco Elysium and Narrative Superposition — Post45](https://post45.org/2024/04/disco-elysium-and-narrative-superposition/)
- [Player agency, politics, and narrative design in Disco Elysium — Game Developer](https://www.gamedeveloper.com/design/player-agency-politics-and-narrative-design-in-disco-elysium-)
- [How did Toby Fox make dialogue boxes in Undertale? — Quora](https://www.quora.com/How-did-Toby-Fox-make-dialogue-boxes-in-Undertale)
- [Undertale was created using GameMaker — Facebook (GameMaker)](https://www.facebook.com/GameMakerEngine/posts/undertale-was-created-using-gamemaker-the-free-software-for-indie-game-developer/4359838180805503/)
- [Modding:Event data — Stardew Valley Wiki](https://stardewvalleywiki.com/Modding:Event_data)
- [Modding:Dialogue — Stardew Valley Wiki](https://stardewvalleywiki.com/Modding:Dialogue)
- [Friendship — Stardew Valley Wiki](https://stardewvalleywiki.com/Friendship)
- [How Stardew Valley creator Eric Barone coped with a four year dev cycle — Game Developer](https://www.gamedeveloper.com/production/how-i-stardew-valley-i-creator-eric-barone-coped-with-a-four-year-dev-cycle)
- [Persistent Data — Ren'Py Documentation](https://www.renpy.org/doc/html/persistent.html)
- [Saving, Loading, and Rollback — Ren'Py Documentation](https://www.renpy.org/doc/html/save_load_rollback.html)
- [Best Practices for Maintaining Save Compatibility Through Updates — RPG Maker Forums](https://forums.rpgmakerweb.com/threads/best-practices-for-maintaining-save-compatibility-through-updates.164582/)
- [Save Game Best Practices — Meta for Developers](https://developers.meta.com/horizon/documentation/unreal/ps-save-game-best-practices/)
- [Game Save Best Practices for Construct 3 — Bugnet](https://bugnet.io/blog/game-save-best-practices-construct3)
- [Dialogue Manager (Nathan Hoad) — GitHub](https://github.com/nathanhoad/godot_dialogue_manager)
- [Dialogue and Quest Systems in Godot 4 — StraySpark](https://www.strayspark.studio/blog/godot-4-dialogue-quest-systems-signals-resources)
- [HowTo: A Simple Dialogue System in Godot — World Eater Games](https://worldeater-dev.itch.io/bittersweet-birthday/devlog/224241/howto-a-simple-dialogue-system-in-godot)
- [The Quest for the Custom Quest System — Game Developer](https://www.gamedeveloper.com/design/the-quest-for-the-custom-quest-system)
- [GDC: Learning From World of Warcraft's Quest Design Mistakes — Game Developer](https://www.gamedeveloper.com/game-platforms/gdc-learning-from-i-world-of-warcraft-i-s-quest-design-mistakes)
- [Level Design Workshop: Balancing Action and RPG in Horizon Zero Dawn Quests — GDC Vault](https://www.gdcvault.com/play/1025177/Level-Design-Workshop-Balancing-Action)
- [How to Implement a Leveling System in RPG — How to Make an RPG](https://howtomakeanrpg.com/r/a/how-to-make-an-rpg-levels.html)
- [GameDesign Math: RPG Level-based Progression — Davide Aversa](https://www.davideaversa.it/blog/gamedesign-math-rpg-level-based-progression/)
- [Mathematics of XP — Only a Game](https://onlyagame.typepad.com/only_a_game/2006/08/mathematics_of_.html)
- [How to build stackable inventory for a game in C# — ScottLilly](https://scottlilly.com/how-to-build-stackable-inventory-for-a-game-in-c/)
- [Designing an Inventory System — easybeak (itch.io devlog)](https://easybeak.itch.io/isometric-rpg-initial-build/devlog/732711/designing-an-inventory-system)
- [How to Prepare a Game for Localization: 10 Basic Rules — Game Developer](https://www.gamedeveloper.com/business/how-to-prepare-a-game-for-localization-10-basic-rules)
- [9 Game Localization Best Practices — Game Developer](https://www.gamedeveloper.com/production/9-game-localization-best-practices)
- [Game Localization: 4 Best Practices for Indie Developers — LocalizeDirect](https://www.localizedirect.com/posts/indie-game-localization-best-practices)
- [Loot Drop Rates Calculation Guide — PulseGeek](https://pulsegeek.com/articles/loot-drop-rates-calculation-guide-numbers-to-feel/)
- [Defining Loot Tables in ARPG Game Design — Game Developer](https://www.gamedeveloper.com/design/defining-loot-tables-in-arpg-game-design)
- [Loot drop best practices — Game Developer](https://www.gamedeveloper.com/design/loot-drop-best-practices)
