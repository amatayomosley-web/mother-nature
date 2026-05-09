# Curricula + Tutorial Creators — Research Report

## Domain summary

This domain covers how formal universities (DigiPen, USC IMD, NYU Game Center, UC Santa Cruz, MIT, CMU ETC), MOOC programs (Harvard CS50G/CS50's 2D Game Development, Coursera/Michigan State), and independent tutorial creators (GDQuest, Heartbeast, KidsCanCode, DevWorm, Bramwell, Game Endeavor, Game Development Center, Brackeys, Sebastian Lague, Clear Code) sequence the teaching of game development to true novices. Three orthogonal pedagogies surface: **project-spiral** (build progressively harder games — CS50G's Pong → Pokémon, Heartbeast's 4-game arc, Coursera's 4-game arc); **concept-ladder** (engine basics → physics → AI → graphics, used by DigiPen and most tutorial creators); and **design-first** (NYU/MIT lead with paper prototypes and game studies). For a solo 2D Godot novice, the reverse-engineered consensus path is project-spiral with concept ladders embedded — fundamentals only get drilled when a game-being-built demands them. Universities also teach C/C++ and CS theory the indie path skips entirely; the user's "creative director" role likely lets him skip those.

## Topic catalog

### Engine model: nodes, scenes, scene tree
- **Brief**: Godot's specific composition model — nodes are atomic objects, scenes are saved trees of nodes, the running game is one big tree. Every other engine has an analog (Unity GameObject/Component, Unreal Actor).
- **Why-drilled**: L1: every Godot tutorial opens here because the editor literally won't make sense otherwise → L2: because gameplay logic attaches to nodes via scripts and signals → L3: because game engines need a uniform addressable runtime graph to dispatch update/draw/input → first principle: **computation** — a game is a tree of stateful objects that must be traversed every frame at fixed budget.
- **Novice failure mode**: Building one giant scene with everything inline (the "monolithic scene" anti-pattern) instead of instancing reusable sub-scenes. Also: confusing nodes with classes.
- **Priority**: core
- **Confidence**: high
- **Sources**: GDQuest beginner path, Heartbeast 1-Bit course Section 1, Godot docs "Your First 2D Game" Ch. 1-2, KidsCanCode Godot 101.

### The game loop and delta time
- **Brief**: A game is a `while running: input → update(dt) → render` loop, ~60 times per second. `delta` is the seconds since last frame; multiplying movement by it makes speed framerate-independent.
- **Why-drilled**: L1: novices write `position += 5` and games run 4× too fast on 240Hz monitors → L2: hardware varies in frames per second → L3: physics requires time-derivative integration (`x = x + v·dt`) → first principle: **physics + computation** — discrete-time simulation of continuous motion.
- **Novice failure mode**: Skipping `delta` entirely; assuming one tick = one second; using `_process` for physics instead of `_physics_process`.
- **Priority**: core
- **Confidence**: high
- **Sources**: GameDev.net "Game Loop" tutorial, Math for Game Developers (YouTube), DEV Community "Understanding Delta Time", Game Development Center channel, "Game Programming Patterns" book introduction.

### Vectors and basic 2D math
- **Brief**: Vec2 for position/velocity/direction; addition for movement, subtraction for "from A to B", normalization for direction-only, dot product for "facing toward", length for distance.
- **Why-drilled**: L1: every movement script uses `velocity` → L2: input becomes a direction vector → L3: 2D space is R² and vector algebra is the native language → first principle: **mathematics of Euclidean space**.
- **Novice failure mode**: Diagonal movement is faster than cardinal because they didn't `.normalized()`; comparing distances by `length()` instead of `length_squared()` (perf trap).
- **Priority**: core
- **Confidence**: high
- **Sources**: GDQuest "Math for game devs", KidsCanCode "Gamedev Math" recipes, Game Development Center vectors series, GameLudere vector algebra, Godot Tutorials Ep. 5-7.

### GDScript syntax (or scripting language fundamentals)
- **Brief**: Variables, conditionals, loops, functions, classes, signals. GDScript specifically: `extends`, `_ready`, `_process`, `@export`, `@onready`.
- **Why-drilled**: L1: nothing happens without scripts attached to nodes → L2: gameplay = behavior over time = code → first principle: **computation** — Turing-complete behavior expression.
- **Novice failure mode**: Treating GDScript like Python and importing everything; not understanding `@export` exposes vars to the editor inspector; missing the difference between `func _ready()` and `func _process(delta)`.
- **Priority**: core
- **Confidence**: high
- **Sources**: GDQuest "Learn GDScript From Zero" (10-hour interactive), Heartbeast Rocket Launch section, Godot Tutorials Episodes -1 through 4, Coursera GDD1 Module 4.

### Input handling
- **Brief**: Polling (`Input.is_action_pressed`) vs events (`_input(event)` / `_unhandled_input`); the InputMap abstracts physical keys to action names ("move_right").
- **Why-drilled**: L1: a game that ignores the player isn't a game → L2: hardware-agnostic action names let you remap and support gamepad without rewrites → first principle: **human-computer interaction** — abstracted intent over hardware.
- **Novice failure mode**: Hardcoding `KEY_W` instead of using actions; mixing polled and event-based input and getting double-fires.
- **Priority**: core
- **Confidence**: high
- **Sources**: Godot docs first-2D-game Ch. 3, KidsCanCode Input section, DevWorm Part 2, Heartbeast Piggy section.

### Sprites, animation, AnimationPlayer/AnimatedSprite2D
- **Brief**: A Sprite2D draws a texture; AnimatedSprite2D cycles frames; AnimationPlayer keyframes any property on any node. Spritesheets pack frames into one image for atlas efficiency.
- **Why-drilled**: L1: characters need to look alive → L2: 60fps × N sprites means GPU prefers one big texture (atlas) over many small ones → first principle: **perception** (12-24 fps reads as motion to humans) + **GPU economics** (texture binds are expensive).
- **Novice failure mode**: One PNG per frame loaded separately (kills perf); using `_process` to manually flip frames instead of AnimationPlayer.
- **Priority**: core
- **Confidence**: high
- **Sources**: KidsCanCode Animation, Heartbeast Piggy, Godot docs first-2D-game Ch. 2, DevWorm Part 3, GDQuest beginner path.

### Collision detection (Area2D / CharacterBody2D / RigidBody2D / StaticBody2D)
- **Brief**: Three physics body types — kinematic (`CharacterBody2D`, manual control), dynamic (`RigidBody2D`, physics-driven), static (immovable). `Area2D` detects overlap without resolving forces. Layers and masks filter who-collides-with-whom.
- **Why-drilled**: L1: pick the wrong body and your character either passes through walls or behaves like jelly → L2: physics engines must trade simulation realism vs designer control → first principle: **physics simulation + perceptual control feel**.
- **Novice failure mode**: Using RigidBody2D for the player ("why does my character feel drunk?"); confusing layers (what I am) with masks (what I scan for).
- **Priority**: core
- **Confidence**: high
- **Sources**: KidsCanCode Physics, Heartbeast Piggy collision section, Godot docs first-2D-game Ch. 3, DevWorm, Game Dev Artisan "Collisions: Layers, Detections".

### Signals (event-driven communication)
- **Brief**: Godot's pub-sub system. Nodes emit signals; other nodes connect handlers. `body_entered`, `pressed`, `animation_finished`, plus custom `signal hit_point_changed(new_hp)`.
- **Why-drilled**: L1: without signals nodes call each other directly and break encapsulation → L2: tight coupling makes scenes non-reusable → first principle: **software architecture** — observer pattern decouples producers from consumers.
- **Novice failure mode**: Not using signals — instead poking at sibling nodes via `get_parent().get_node("X").do_thing()` (path coupling).
- **Priority**: core
- **Confidence**: high
- **Sources**: Heartbeast Piggy/Space Shooter, KidsCanCode "node communication", DevWorm signal_finished example, Godot docs, Wikibooks Godot signals guide.

### TileMap / TileSet
- **Brief**: 2D level construction by painting tiles from a TileSet onto a TileMap node. Modern Godot 4.3+ uses TileMapLayer. Auto-tiling, terrain layers, and tile collision shapes baked into the set.
- **Why-drilled**: L1: hand-placing every wall is impossibly tedious → L2: levels need to be authored, not coded → first principle: **economics** (designer time) + **memory** (one TileSet vs N sprites).
- **Novice failure mode**: Mixing physics layers between TileMap and other bodies; not using terrain bits for auto-tile.
- **Priority**: core
- **Confidence**: high
- **Sources**: Heartbeast Metroidvania section, DevWorm Part 4 "Level Creation", KidsCanCode 2D, Godot docs TileMap, Game Development Center Tilemap series.

### UI: Control nodes, anchors, themes
- **Brief**: Control is the UI base class with an anchor/margin system that survives resolution changes. Common: Label, Button, Panel, VBoxContainer/HBoxContainer, ProgressBar.
- **Why-drilled**: L1: every game needs a HUD/menu → L2: screen sizes vary so layout must be relative → first principle: **perception** (legibility) + **HCI** (layout invariants under resize).
- **Novice failure mode**: Hardcoded pixel positions that break on different resolutions; putting game logic in Control nodes instead of separate scenes.
- **Priority**: core
- **Confidence**: high
- **Sources**: Heartbeast Space Shooter, Godot docs first-2D-game Ch. 6 HUD, KidsCanCode UI, Game Development Center UI/inventory series.

### Audio (AudioStreamPlayer, buses)
- **Brief**: AudioStreamPlayer for non-positional, AudioStreamPlayer2D for spatial. Audio buses for routing/mixing master/music/SFX volumes.
- **Why-drilled**: L1: silent games feel broken → L2: humans process audio cues faster than visual ones → first principle: **perception** — auditory feedback closes the action-feedback loop.
- **Novice failure mode**: Restarting audio every frame (clicking); no separate music/SFX buses (can't mute one).
- **Priority**: important
- **Confidence**: high
- **Sources**: Heartbeast Space Shooter sound, Godot docs, KidsCanCode Audio Manager, Bramwell.

### State management: autoloads / singletons, save/load
- **Brief**: Autoload (singletons) for global state like score, settings, current level. JSON or `ConfigFile` or resource serialization for persistence.
- **Why-drilled**: L1: scenes are unloaded on transition; data must outlive them → L2: persistence is a separate concern from gameplay → first principle: **software architecture** (lifetime separation).
- **Novice failure mode**: Putting everything in autoloads (god-object); not handling missing/corrupted save files.
- **Priority**: important
- **Confidence**: high
- **Sources**: Heartbeast Space Shooter (highscores, autoloads), Godot Tutorials Ep. 8+, KidsCanCode Basics (file I/O), DevWorm.

### Custom Resources / data-driven design
- **Brief**: Godot `Resource` subclasses act as typed data files (item definitions, character stats, dialog trees). Drag into inspector, share between scenes.
- **Why-drilled**: L1: hardcoding stat tables in scripts means designers need a coder for tweaks → L2: tunable data should be authored not coded → first principle: **economics** (designer iteration speed) + **separation of data and behavior**.
- **Novice failure mode**: Storing all data in dictionaries with no schema, then losing track at scale.
- **Priority**: important (Godot-specific superpower)
- **Confidence**: medium (Heartbeast and GDQuest emphasize; not in Coursera/CS50G because Unity/LÖVE differ)
- **Sources**: Heartbeast Heart Platformer "custom resources" episode, GDQuest courses, Godot docs.

### Game design fundamentals (mechanic / dynamic / aesthetic)
- **Brief**: Verb-based design (what does the player *do*?), MDA framework, core loops, juice/feedback, playtesting.
- **Why-drilled**: L1: pretty assets on a boring loop fail → L2: fun is structural, not decorative → first principle: **human cognition** — flow theory, reward schedules, mastery curves.
- **Novice failure mode**: Designing systems before identifying the core verb; copying mechanics without understanding why they worked elsewhere.
- **Priority**: important
- **Confidence**: high
- **Sources**: NYU Game Center BFA "Games 101", MIT CMS.611J, USC IMD design courses, DigiPen DES 115, CMU ETC Intro to Game Design.

### Project structure / scene composition / inheritance
- **Brief**: When to make a separate scene vs adding child nodes; inherited scenes for variants; folder organization.
- **Why-drilled**: L1: 50-file flat folder becomes unmaintainable by week 3 → L2: reusable scenes = composition reuse → first principle: **software architecture** (DRY, modularity).
- **Novice failure mode**: One mega-scene with all enemies inline; no naming convention; mixing assets and scripts in same folder.
- **Priority**: important
- **Confidence**: high
- **Sources**: Heartbeast inherited scenes episode, GDQuest Godot best practices, KidsCanCode, Sandro Maglione intro.

### Cameras (Camera2D, smoothing, limits, shake)
- **Brief**: Camera2D follows player; limits constrain to level bounds; smoothing softens motion; screen shake for feedback.
- **Why-drilled**: L1: a stationary view in a moving world is unplayable → L2: the camera is also a designer tool — what you frame is what the player attends to → first principle: **perception + cinematography**.
- **Novice failure mode**: Snapping camera causing motion sickness; no level bounds so camera wanders into void.
- **Priority**: important
- **Confidence**: high
- **Sources**: KidsCanCode "cameras", Heartbeast all sections, Godot docs.

### AI / enemy behavior (state machines, navigation)
- **Brief**: Finite state machines (idle/patrol/chase/attack), NavigationAgent2D for pathfinding, line-of-sight via raycast.
- **Why-drilled**: L1: enemies that just stand there aren't enemies → L2: rule-based behavior over time = state transitions → first principle: **automata theory** + **economics** (FSMs are cheap and debuggable; full BTs/HTNs are not novice-priced).
- **Novice failure mode**: Giant if-elif tree pretending to be an FSM; using `_process` for AI when timer-driven would suffice.
- **Priority**: important
- **Confidence**: high
- **Sources**: KidsCanCode AI/Behavior, DigiPen CS 380, UCSC CMPM electives, Heartbeast Metroidvania bosses, Game Endeavor.

### Particles, juice, and game feel
- **Brief**: Screen shake, hit-pause, particle bursts, tween-based squash/stretch — the layer that makes mechanics feel good.
- **Why-drilled**: L1: identical mechanics with juice vs without can change a "boring" game to "addictive" → L2: feedback granularity matches human reaction-time perception (~100ms) → first principle: **perception + reward**.
- **Novice failure mode**: Skipped entirely until shipping; or overdone (every action shakes, screen-vomit).
- **Priority**: nice-to-have at first; important for shipping
- **Confidence**: high
- **Sources**: Heartbeast all sections, GDQuest courses, "Juice it or Lose it" (referenced widely).

### Export / publishing
- **Brief**: Godot export templates for Windows/Mac/Linux/Web/Mobile; signing on macOS; itch.io distribution.
- **Why-drilled**: L1: a game that doesn't run on someone else's machine is not a game → L2: each OS has security/packaging requirements → first principle: **distribution economics**.
- **Novice failure mode**: Forgotten until "I want my friend to play this"; missing export templates; broken paths in builds.
- **Priority**: important (terminal step)
- **Confidence**: medium-high
- **Sources**: Bramwell, GDQuest, Godot docs.

### Version control (git)
- **Brief**: Commits, branches, .gitignore (exclude `.godot/`, `*.import`), itch.io / GitHub distribution.
- **Why-drilled**: L1: undo runs out → L2: solo devs still need rollback and project history → first principle: **engineering hygiene**.
- **Novice failure mode**: No git at all; committing the `.godot/` cache; binary merge conflicts on `.tscn`.
- **Priority**: important
- **Confidence**: medium (universities mandate; tutorials skip)
- **Sources**: DigiPen, UCSC, NYU; absent from most Godot tutorial series.

## Consensus learning order

Synthesized from agreement across CS50G, Coursera GDD1-4, GDQuest beginner path, Heartbeast 1-Bit, KidsCanCode Godot 101, Godot docs, DigiPen freshman year:

1. **Engine install + editor tour** (open project, run a blank scene)
2. **Nodes, scenes, scene tree** (Godot's composition model)
3. **GDScript fundamentals** (vars, ifs, loops, functions, classes — taught alongside engine, not before)
4. **Game loop + `_process(delta)` / `_physics_process(delta)`**
5. **Input handling + InputMap** (move a sprite with arrow keys)
6. **Vectors / 2D math** (introduced when movement gets diagonal)
7. **Sprites + animation** (AnimatedSprite2D)
8. **Collision: Area2D, then CharacterBody2D** (pickup → wall-aware movement)
9. **Signals** (decouple after first ad-hoc coupling pain)
10. **A complete first game** (Pong / Dodge / top-down minimal — CS50G Week 0, Godot docs "Dodge the Creeps")
11. **TileMap / level building**
12. **UI / HUD / menus**
13. **Audio**
14. **Autoloads / save-load / scene transitions**
15. **A second, larger game** (platformer with juice — Heartbeast Section 2, Coursera GDD2)
16. **State machines / enemy AI**
17. **Custom Resources / data-driven design** (Godot-specific)
18. **Particles, shaders intro, polish**
19. **Export + publish**
20. **(advanced)** shaders, networking, procgen, 3D — outside novice scope

## Must-include shortlist

Every consensus curriculum agrees a novice must learn these first, ranked by how often they appear and how soon:

1. **Editor + nodes/scenes** — entry-cost, otherwise nothing makes sense
2. **GDScript basics** — variables, functions, classes, `_process`
3. **Game loop + delta time** — framerate independence
4. **Input handling** — make a sprite move
5. **Sprites + animation** — make it look alive
6. **Collision basics** — make it interact with the world
7. **Signals** — decouple
8. **Vectors / 2D math** — direction, normalization, distance
9. **TileMap** — level construction
10. **UI / HUD** — score, menus, game-over

A complete first shippable game (Pong/Dodge-the-Creeps clone) typically lands by topic 7-8.

## Commonly oversold

Topics that appear in some curricula but are not load-bearing for solo 2D Godot dev:

- **C/C++ proficiency** (DigiPen heavy, UCSC moderate). Not needed for Godot — GDScript is the native language. Justification: universities prep for AAA jobs where engines are C++; the user's role is creative direction.
- **Linear algebra as formal course** (DigiPen MAT 140, UCSC). Vector operations and basic transforms cover 95% of 2D needs; matrix algebra mostly hides behind `Transform2D` and shader-only contexts. Skip the course; learn vectors operationally.
- **Physics as a formal course** (DigiPen PHY 200 Motion Dynamics). Built into Godot's physics engine. Learn integration intuitively via `velocity += gravity * delta`.
- **Operating systems / Discrete math / Data structures** (DigiPen, UCSC). Pure CS-degree padding. Solo devs use the dictionary, the array, and the dynamic-typed list and never write a B-tree.
- **3D pipelines** (USC IMD, DigiPen, Bramwell focus). Out of scope for 2D-first. Defer.
- **Multiplayer / networking** (Game Development Center has whole series). Intermediate-only — beginner-tutorial roadmaps explicitly defer it (gamedev roadmap 2026).
- **Shaders** (multiple advanced channels). Polish layer, not load-bearing for first game.
- **Procedural generation** (KidsCanCode dedicates a section). Genre-specific (roguelikes); not core.
- **Game studies / game history** (NYU "Games 101", USC, MIT). Valuable for direction but pure-design — covered better by your design-theory research domain than by curriculum.
- **Agile / team production** (CMU ETC, USC, MIT CMS.611). Solo dev sidesteps this.

## Cross-references

- **Game design theory** (separate research domain): MDA, juice, flow, level design — universities lead with this; tutorial creators embed it implicitly. Overlaps with "Game design fundamentals" topic above.
- **Architecture patterns** (separate domain): "Game Programming Patterns" book — observer (signals), state (FSMs), component (composition), object pool. Overlaps with topics 9, 18.
- **Math for games** (separate domain): vectors, transforms, easing, interpolation. Overlaps with topic 4.
- **Asset creation** (separate domain): pixel art, audio, fonts. Tutorials assume given assets; universities add courses.
- **Production / shipping** (separate domain): playtesting, scope, marketing. Universities cover via studio courses; tutorials skip until "export".

## Sources I used

- [CS50's Introduction to 2D Game Development (Harvard, syllabus)](https://cs50.harvard.edu/games/)
- [GDQuest Godot beginner learning path](https://www.gdquest.com/tutorial/godot/learning-paths/beginner/)
- [GDQuest "Learn 2D Gamedev From Zero" course outline](https://school.gdquest.com/courses/learn_2d_gamedev_godot_4)
- [DigiPen BS in CS and Game Design — sample course sequence](https://www.digipen.edu/academics/game-design-and-development-degrees/bs-in-computer-science-and-game-design/sample-course-sequence)
- [USC IMD program catalog](https://catalogue.usc.edu/preview_program.php?catoid=16&poid=24903)
- [NYU Game Center BFA curriculum overview](https://gamecenter.nyu.edu/academics/game-design-bfa/)
- [UC Santa Cruz Computer Science: Computer Game Design BS catalog](https://catalog.ucsc.edu/en/current/general-catalog/academic-units/baskin-engineering/computational-media/computer-science-computer-game-design-bs)
- [MIT OCW CMS.611J Creating Video Games syllabus](https://ocw.mit.edu/courses/cms-611j-creating-video-games-fall-2014/pages/syllabus/)
- [CMU ETC academic curriculum](https://etc.cmu.edu/academics/curriculum/elective-courses)
- [Coursera Game Design and Development specialization (Michigan State)](https://www.coursera.org/specializations/game-design-and-development)
- [Coursera GDD1: 2D Shooter](https://www.coursera.org/learn/game-design-and-development-1)
- [KidsCanCode Godot 4 Recipes](https://kidscancode.org/godot_recipes/4.x/)
- [Heartbeast 1-Bit Godot 4 Course](https://www.heartgamedev.com/1-bit-godot-4-course)
- [Godot docs "Your First 2D Game" (Godot 4.5)](https://docs.godotengine.org/en/4.5/getting_started/first_2d_game/index.html)
- [DevWorm "Learn Godot 4 by Making a 2D Platformer" series (Christine Coomans)](https://dev.to/christinec_dev/learn-godot-4-by-making-a-2d-platformer-part-1-project-editor-overview-1ap4)
- [Bramwell Godot 4 Beginners](https://bramwell.itch.io/godot-4-beginners)
- [Game Development Center YouTube channel](https://www.youtube.com/c/gamedevelopmentcenter)
- [Godot Tutorials Basics Series](https://godottutorials.com/courses/godot-basics-series/)
- [Sebastian Lague Intro-to-Gamedev (Unity/C#)](https://github.com/SebLague/Intro-to-Gamedev)
- [Brackeys Unity Beginner Tutorials playlist](https://www.youtube.com/playlist?list=PLPV2KyIb3jR5QFsefuO2RlAgWEz6EvVi6)
- [GameDev.net beginner programming guide](https://gamedev.net/tutorials/programming/general-and-gameplay-programming/game-programming-beginners-guide-r906/)
- [Game Development Roadmap 2026 (gamedevreport)](https://gamedevreport.beehiiv.com/p/roadmaps-for-game-dev-success-in-2026-beginner-intermediate-marketing)
- [Game Programming Patterns book](https://gameprogrammingpatterns.com/introduction.html)
