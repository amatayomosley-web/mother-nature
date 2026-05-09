# Architecture Patterns — Research Report

## Domain summary

Game architecture sits between two pressures: a tight per-frame budget (16.6 ms at 60 Hz) and runtime mutability (the player constantly changes what the program is doing). Most patterns in this domain exist to manage one of three first-principle costs — coupling cost (changing one system breaks another), simulation/render coupling (the loop must run reliably while the picture must look smooth), and allocation cost (creating objects mid-frame causes hitches). Robert Nystrom's *Game Programming Patterns* is the canonical taxonomy, and Godot 4 happens to encode several of his patterns directly in its node/scene/resource model — which means a 2D Godot novice gets composition, observer, and component patterns "for free" and should resist the urge to re-implement them. The verdict for solo 2D scope: master the eight patterns Godot already speaks, treat ECS and microservice-style decomposition as overkill, and reach for state machines, object pools, and the command pattern only when a concrete pain shows up.

## Pattern catalog

### Game loop (fixed vs variable timestep)
- **Brief**: The single while-loop that drives every frame: read input, simulate, render, repeat.
- **Why-drilled**: L1 — your game has to keep moving even when frame rate drops. L2 — if simulation runs in the same step as rendering, a slow frame produces a jumbo physics step and your character tunnels through walls. **First principle**: stable simulation requires a fixed-size physics step (determinism, numerical stability), but smooth rendering requires *interpolation between* whatever frames the GPU can produce. These are different clocks; conflating them is the foundational mistake.
- **Godot-native version**: `_process(delta)` runs every render frame (variable). `_physics_process(delta)` runs at a fixed rate (default 60 Hz) with a constant `delta`. Godot 4 added physics interpolation so visual position is smoothed between physics ticks at high refresh rates.
- **When to use**: Always. Put movement/collision in `_physics_process`; put input polling, animations, camera follow, UI in `_process`.
- **When to skip**: Never — but pure tween-driven games can put almost everything in `_process` if no physics is involved.
- **Novice failure mode**: Calling `move_and_slide()` from `_process`. Movement is now framerate-dependent; speed varies with monitor refresh rate.
- **Priority**: core
- **Source**: Nystrom ch. "Game Loop"; Godot docs *Idle and Physics Processing*.

### State pattern / finite state machine
- **Brief**: Player or enemy is in exactly one named state (Idle / Run / Jump / Attack); transitions are explicit edges.
- **Why-drilled**: L1 — without it, behavior code becomes nested `if` branches that interact in ways nobody can predict. L2 — gameplay logic is inherently mode-dependent (you can't shoot while reloading). **First principle**: complexity grows quadratically with implicit modes; making modes first-class collapses it to linear.
- **Godot-native version**: No built-in FSM, but `AnimationTree` with `AnimationNodeStateMachine` is one. Common pattern: a `State` node with `enter()`, `exit()`, `update(delta)`, plus a `StateMachine` parent that calls them.
- **When to use**: Player controllers, simple enemies (≤6 states), UI screen flow, dialog modes.
- **When to skip**: When sub-states share so much logic you copy it 5 times — promote to **hierarchical FSM** (HFSM). When state count exceeds ~10 and transitions form a dense graph — promote to behavior tree.
- **Novice failure mode**: Putting transition conditions inside the *current* state instead of a transition table; states then know about each other and the FSM stops being decoupled.
- **Priority**: core
- **Source**: Nystrom ch. "State"; The Shaggy Dev *Advanced state machine techniques in Godot 4*.

### Hierarchical state machine
- **Brief**: States contain sub-states. Police character's outer FSM: {Patrol, Chase, ArrestSuspect}; Chase contains {RunOnFoot, DriveCar}.
- **Why-drilled**: Flat FSMs explode: every sub-behavior has to handle "what if a higher-priority event interrupts?" HFSMs let parent states hold the interrupt logic once.
- **Godot-native version**: Compose state nodes — one `State` script on a parent node, child `State` nodes for sub-states; parent's `update` delegates to active child.
- **When to use**: Enemy AI with shared interrupts (every state can react to "took damage"). Player with movement modes (grounded vs aerial each containing idle/run/attack).
- **Novice failure mode**: Building an HFSM before a flat FSM has hurt. You'll end up with three states and four levels of nesting.
- **Priority**: nice-to-have (defer until flat FSM hurts)
- **Source**: Nystrom ch. "State" (concurrent/hierarchical sections); Game AI Pro 3 ch. 12.

### Behavior tree
- **Brief**: A tree of selector/sequence/action nodes that ticks each frame and returns SUCCESS / FAILURE / RUNNING. Unlike an FSM, the "current state" is implicit in tree position.
- **Why-drilled**: L1 — when you have 50+ behaviors, FSM transition graphs become unreadable. L2 — behavior trees encode priority via node order (selectors try children left to right) so adding a new high-priority behavior is local. **First principle**: trees are insertion-friendly; graphs aren't. Authoring cost grows linearly in BT, near-quadratically in FSM.
- **Godot-native version**: **Beehave addon** — adds `BeehaveTree` node with selector, sequence, condition, action nodes plus a Blackboard dict for cross-node state.
- **When to use**: 10+ enemy types with overlapping behavior; boss with multiple phases; stealth AI with priorities (flee > attack > patrol).
- **When to skip**: Single enemy type with three behaviors. A flat FSM is one third the LOC.
- **Novice failure mode**: Reaching for BT for a platformer player. Players are reactive single-mode actors; FSM is the right shape.
- **Priority**: nice-to-have for solo 2D novice; only earn it when you have >5 enemy types
- **Source**: Damián Isla, *Handling Complexity in the Halo 2 AI* (GDC 2005); Beehave docs at github.com/bitbrain/beehave.

### Composition over inheritance
- **Brief**: Build entities by *attaching* functionality (Sprite, CollisionShape, HealthComponent) instead of *inheriting* (FlyingFireBoss extends FlyingBoss extends Boss extends Enemy).
- **Why-drilled**: L1 — deep inheritance trees lock you in; orthogonal features (flying + on-fire) become a combinatorial mess. **First principle**: behavior is a *set* of capabilities, not a totally-ordered hierarchy. Sets compose; totally-ordered hierarchies don't.
- **Godot-native version**: This *is* Godot's node tree. A `CharacterBody2D` with child `Sprite2D`, `CollisionShape2D`, `AnimationPlayer`, `HitboxArea2D` is composition. You don't need to invent it — you need to *resist re-inventing inheritance on top*.
- **When to use**: Always default to it.
- **When to skip**: Two scenes share 90% structure with one parameter difference — scene inheritance is fine. A clean abstract base class for a known interface (e.g., `Pickup` with `on_collected()`) is fine.
- **Novice failure mode**: Building a 5-level GDScript class chain (`FireEnemy extends Enemy extends Actor extends Entity extends Node2D`) when you should have made one Enemy scene with swappable child component nodes.
- **Priority**: core (you're already using it; just don't fight it)
- **Source**: Nystrom ch. "Component"; Godot forum *Godot design flaw: inheritance VS composition*.

### Component pattern
- **Brief**: Subset of composition. An entity is a bag of components (HealthComponent, MoveComponent, AIComponent); each owns one slice of state and behavior.
- **Why-drilled**: Lets the same `HealthComponent` work for player, enemy, breakable crate. **First principle**: orthogonal slices of state shouldn't be entangled in one class.
- **Godot-native version**: Every Godot node *is* a component. Make a `Health` node script, drop it as a child of anything that should take damage, expose `damage(n)` as a method or signal.
- **When to use**: When two unrelated entity types need the same behavior (player + enemy both have health; cart + door both can be pushed).
- **Novice failure mode**: Over-componentizing — splitting "PlayerController" into MoveComponent + JumpComponent + DashComponent + InputComponent before you've shipped anything.
- **Priority**: important
- **Source**: Nystrom ch. "Component".

### Signal / observer / event bus
- **Brief**: Producer emits a named event; zero or more listeners react. They never reference each other directly.
- **Why-drilled**: L1 — direct calls hard-couple modules. UI listening to "score changed" via direct ref means UI knows about score system. **First principle**: information flow ≠ control flow. Most cross-system "knowledge" is *notification*, not *invocation*. Decouple them.
- **Godot-native version**: `signal` keyword on any node. For cross-tree comms: an autoload named `Events` (or `GameEvents`) with project-wide signals. UI connects to `Events.score_changed`; gameplay calls `Events.score_changed.emit(new_score)`.
- **When to use**: Cross-system events (UI ↔ gameplay, gameplay ↔ audio, gameplay ↔ save system).
- **When to skip**: Parent-to-child direct calls (parent already owns the child reference; signal would be ceremony). Tight inner loops (a player firing 60 bullets/sec via signals is fine, but profile if it shows up).
- **Novice failure mode**: Making *everything* a signal. Now nobody knows who listens to what. Or: emitting from a child to a parent that's right there — call up directly via signal, don't autoload it.
- **Priority**: core
- **Source**: Nystrom ch. "Observer"; *GDQuest — The Events bus singleton*; *Godot Signals Architecture* (Febucci 2024).

### Object pool
- **Brief**: Pre-allocate N reusable instances; "spawn" hands one out, "despawn" returns it. Never `new` mid-frame.
- **Why-drilled**: L1 — instantiating 20 bullets per shot causes a frame hitch. L2 — every allocation may eventually trigger garbage collection. **First principle**: GC is non-deterministic latency. A 10 ms pause inside your 16.6 ms budget is a missed frame. Pooling makes allocation cost zero at runtime.
- **Godot-native version**: An autoload `BulletPool` holding 100 pre-instantiated `Bullet` scenes; `get_bullet()` returns an inactive one and reactivates it; `release(b)` deactivates and re-parks it. GDScript does have a GC, so this matters.
- **When to use**: Anything spawned more than ~10 times/sec and short-lived (bullets, particles, pickups, damage numbers).
- **When to skip**: One-shot enemies, level objects, anything spawned <1×/sec. Pooling adds bug surface (forgotten releases leak to the GC anyway with no warning).
- **Novice failure mode**: Pooling everything, including the player and bosses. Or forgetting to *reset* state on retrieval — your reused bullet still has last frame's velocity.
- **Priority**: important once bullet hell or particle systems appear; otherwise nice-to-have
- **Source**: Nystrom ch. "Object Pool"; Jackson Dunstan *The Problems with Object Pools*.

### Service locator / autoload singleton
- **Brief**: A globally accessible object holding shared services (audio manager, save system, event bus).
- **Why-drilled**: Singletons are easy and *that's the trap*. **First principle**: global mutable state is an anti-pattern because change-locality is destroyed — anyone can mutate from anywhere, debugging becomes archaeology.
- **Godot-native version**: Project Settings → Autoload. The official docs explicitly call out anti-patterns: don't access in `_init()`, don't create circular deps, don't store scene-specific state, cap at 5–10.
- **When to use**: Truly cross-scene services with no good owner — event bus, audio manager, settings, save system. Read-mostly global data (item table, localization strings).
- **When to skip**: Anything that has a natural owner in the scene tree. Player references — get them via group lookup or signal, not `Globals.player`.
- **Novice failure mode**: `GameState` autoload that owns player, inventory, current scene, dialogue, score, and is read/written from 50 places. Welcome to the God Object.
- **Priority**: important (you'll use it; the discipline is *what NOT to put in it*)
- **Source**: Nystrom ch. "Service Locator" + "Singleton"; Godot docs *Autoloads versus internal nodes*.

### Dependency direction (UI → Game, not reverse)
- **Brief**: Gameplay code never imports or names UI code. UI listens to gameplay via signals and renders state.
- **Why-drilled**: L1 — if gameplay calls `health_bar.update(50)`, you can't run gameplay headless (tests, replay, server). L2 — UI changes constantly; gameplay shouldn't recompile when you reskin. **First principle**: the simulation is the source of truth; rendering is a projection of it. Projections depend on truth, not vice versa.
- **Godot-native version**: Player emits `health_changed(50, 100)`. HealthBar autoconnects in `_ready()` via `Events.health_changed` or via a direct connection set up by a parent scene. Gameplay nodes never call `get_node("../UI/HealthBar")`.
- **When to use**: Always. Same rule applies to save/load — game logic exposes serializable state; the save system reads it, not the other way.
- **Novice failure mode**: Player script calls `get_tree().root.get_node("UI/HealthBar").value = hp`. Now the player is inseparable from this exact UI layout.
- **Priority**: core
- **Source**: *Decouple game state from game UI* (Godot forum); Nystrom intro on architecture as decoupling.

### Save/load and schema versioning
- **Brief**: Game state serializes to a file; loading reconstructs it. Schema *will* change between releases.
- **Why-drilled**: L1 — naive `var_to_str` round-trips break the moment you rename a field. L2 — players have hour-long saves; breaking them loses their trust. **First principle**: save format is a *contract* with past versions of yourself. Like any contract, it needs versioning and migration. This is a first-class architectural concern, not an afterthought.
- **Godot-native version**: Two camps. (1) Custom `Resource` saves via `ResourceSaver.save(data, path)` — fast, typed, but couples save format to GDScript class layout (rename a field, break old saves). (2) JSON via dictionaries — schema-flexible, requires manual `Vector2`/`Color` conversion. Either way: store a `save_version: int`; on load, run migrations from older versions forward.
- **When to use**: Every game with progression. Even arcade scores deserve a version field.
- **Novice failure mode**: `ResourceSaver.save(player_resource)` with no version field. Six months later you add a stat, can't load any old save, players riot.
- **Priority**: core for any game beyond a session
- **Source**: GDQuest *Saving and Loading Games in Godot 4*; *How to load and save things with Godot* (forum tutorial).

### Scene composition vs scene inheritance
- **Brief**: Composition = embed scene A inside scene B. Inheritance = scene B is "a kind of" scene A with overrides.
- **Why-drilled**: Inheritance auto-propagates parent edits to children — great when 20 enemy types share a skeleton, painful when one diverges and you need to break the link. **First principle**: inheritance encodes "is-a" with mandatory propagation; composition encodes "has-a" with optional reuse. Wrong axis = friction.
- **Godot-native version**: Both supported. New Inherited Scene = inheritance. Drag a `.tscn` into another scene = instance/composition.
- **When to use inheritance**: Many entities sharing a stable skeleton (enemy roster). UI screens with a common header.
- **When to use composition**: Mixing orthogonal features (a flying enemy that also burns); features that recombine.
- **Novice failure mode**: Hundreds of monster types via deep inheritance. By monster #50 you'll wish you'd used a `Resource` data file driving one composed scene.
- **Priority**: important
- **Source**: Godot forum *Godot design flaw: inheritance VS composition*; UhiyamaLab *Editable Children vs Scene Inheritance*.

### Resource pattern (Godot-specific data-driven design)
- **Brief**: A custom `Resource` subclass with `@export` fields stored as a `.tres` file. Designers edit values in the inspector; code reads them as typed objects.
- **Why-drilled**: L1 — hardcoded enemy stats means programmer changes for every balance tweak. L2 — JSON works but loses type safety and editor integration. **First principle**: data and code change at different rates. Separating them so designers can iterate without recompiles is the whole point of data-driven design.
- **Godot-native version**: `class_name EnemyStats extends Resource` with `@export var hp: int`, `@export var speed: float`, `@export var sprite: Texture2D`. Save as `goblin.tres`. Enemy scene takes `@export var stats: EnemyStats` and reads from it.
- **When to use**: Enemy stats, weapon configs, dialogue lines, item definitions, level metadata. Anything tunable.
- **When to skip**: One-off values; truly procedural data (use generated dictionaries).
- **Novice failure mode**: Inlining `var hp = 100` in every enemy script. By the 5th enemy you're copy-pasting balance changes.
- **Priority**: core (Godot-specific superpower)
- **Source**: Godot docs *Resources*; UhiyamaLab *Custom Resources — Data-Driven Design*; Mayke Fernandes dos Santos *Resource-based architecture for Godot 4*.

### Command pattern
- **Brief**: Wrap "do X" as an object with `execute()` and `undo()`. Now you can queue, log, replay, or rewind actions.
- **Why-drilled**: L1 — undo/redo without command pattern means snapshotting full game state per action (huge). L2 — replays-as-recorded-commands are tiny because they only store inputs/intents. **First principle**: a method call is ephemeral; a command is reified, and reification unlocks queue/log/replay/network.
- **Godot-native version**: Plain GDScript `class_name Command` with virtual methods; no engine support needed.
- **When to use**: Turn-based games (full undo). Strategy/puzzle (rewind). Networked input (transmit commands, not state). Replay systems.
- **When to skip**: Action games where commands fire 60×/sec and undo is meaningless. Don't pay the indirection cost.
- **Novice failure mode**: Wrapping every input in a command "for flexibility" you'll never need.
- **Priority**: nice-to-have (overkill for first 2D platformer; essential for puzzle/strategy)
- **Source**: Nystrom ch. "Command".

### ECS (Entity-Component-System)
- **Brief**: Entities are IDs. Components are pure data. Systems iterate component arrays. Cache-friendly, mass-parallel.
- **Why-drilled**: ECS exists to solve a problem you don't have. **First principle**: ECS pays its complexity tax via cache locality at thousands-of-entities scale. With 50 entities, the cache cost is invisible; you've added a paradigm shift for nothing.
- **Godot-native version**: Godot is **not** ECS. Nodes are objects with state and behavior; that's OOP composition, not ECS. Third-party `godot-ecs` add-ons exist but force you to fight the engine.
- **When to use**: Simulation games with thousands of agents — RimWorld, Caves of Qud, Dwarf Fortress scale.
- **When to skip**: Solo 2D first game. Always.
- **Novice failure mode**: Reading a Unity DOTS article and concluding you need ECS to ship a platformer. You don't.
- **Priority**: overkill-for-novice
- **Source**: ecs-faq (Sander Mertens); Mojo Labs *ECS for Humans — When You Actually Need It*; *Do everything through ECS?* (gamedev.net).

### Layered architecture (input → game logic → presentation)
- **Brief**: Three loose layers — input (raw events → intents), game logic (intent → state change), presentation (state → pixels/sound).
- **Why-drilled**: Each layer changes for different reasons (rebinding keys, balancing damage, reskinning art). Mixing them means one change touches three concerns. **First principle**: change locality. Layers are thin walls keeping unrelated changes from leaking.
- **Godot-native version**: Input handled by `Input.is_action_pressed("jump")` (input layer reads raw, emits intent). Game logic in `_physics_process` mutates state. Presentation in `AnimationPlayer`/`Sprite2D` reflects state. Three layers, no enforcement — discipline only.
- **Priority**: important (lightweight version is enough)
- **Source**: Nystrom intro; general architecture canon.

### MVC / MVVM for game UI
- **Brief**: Separate Model (data), View (rendering), Controller/ViewModel (mediation).
- **Why-drilled**: MVC was designed for forms-over-data apps. Gameplay isn't that — it's a tight simulation loop. **First principle**: MVC's value is decoupling slow human input from displayed data. Gameplay's "input" is 60 Hz polling, not button clicks awaiting validation.
- **Godot-native version**: Apply only to *menus* — main menu, inventory, dialog UI. Treat in-game HUD as a passive observer (signals → labels). Don't MVC the gameplay loop.
- **When to skip**: The whole gameplay loop. The dependency-direction rule is enough.
- **Priority**: nice-to-have for menu-heavy games
- **Source**: general; Godot forum *Decouple game state from game UI (call it MVC or not)*.

### Update method
- **Brief**: Each entity has an `update(delta)` called every frame.
- **Why-drilled**: Without it, the game loop knows about every entity type — open/closed violation. **First principle**: polymorphic dispatch lets the loop iterate without caring what's inside.
- **Godot-native version**: `_process` and `_physics_process` *are* this pattern. Free.
- **Priority**: core (free in Godot)
- **Source**: Nystrom ch. "Update Method".

## Must-include shortlist

The five-to-eight a novice's first 2D Godot game absolutely needs:

1. **Game loop discipline** — `_physics_process` for physics, `_process` for everything else.
2. **Composition via the node tree** — don't fight Godot; build entities as scenes-of-nodes.
3. **Signals + one event bus autoload** — the cheapest decoupling you'll ever buy.
4. **Finite state machine for player and enemies** — flat, not hierarchical, until it hurts.
5. **Dependency direction (UI → Game)** — gameplay never imports UI; UI listens via signals.
6. **Resource pattern for data** — enemy stats, items, levels in `.tres` files.
7. **Save/load with a `save_version` field** — one int now saves a year of pain later.
8. **Object pooling** — only when bullets, particles, or pickups appear in volume.

## Commonly oversold patterns

- **ECS**. The single most-oversold pattern in 2026 game-dev media. Defended at length above. The honest verdict: ECS exists to make tens of thousands of homogeneous entities cache-coherent. Your platformer has one player and twenty goblins. Cache locality is invisible at that scale; what you'd buy is a paradigm shift, opt-out from Godot's node model, and a learning curve that delays shipping. The "leave-alone" condition: stay with composition unless you concretely measure CPU time dominated by entity update loops over thousands of homogeneous agents — and you won't, in a 2D first game.
- **Hierarchical state machines and behavior trees**. Premature for solo 2D scope. Earn them by feeling FSM pain first.
- **Command pattern everywhere**. Useful for puzzle/strategy/replay; pure overhead for action games.
- **MVC for the gameplay loop**. Designed for form apps. Apply to menus only.
- **Service locators for everything**. Cap at 5–10 autoloads; scenes own their own state.
- **Deep class inheritance**. Godot's node tree already gives you composition; building a 5-level GDScript class chain re-creates the problem the engine solved for you.
- **Bytecode/scripting subsystems**. Nystrom covers them, modders love them, you don't need them in your first game.

## Cross-references

- Loop discipline ↔ object pooling: both serve the 16.6 ms budget; pooling matters because allocations interrupt the loop.
- Signals ↔ dependency direction: signals are the *mechanism*; dependency direction is the *discipline* that decides which way they flow.
- Resource pattern ↔ save/load: `Resource` powers both data-driven design and save serialization, but the schema-versioning concern attaches to *both*.
- FSM ↔ behavior tree: same problem space (state-dependent behavior), different scaling regimes.
- Composition ↔ scene inheritance: inheritance is the special case of composition where propagation is mandatory.
- Service locator ↔ event bus: same autoload mechanism, different intent — services hold mutable state, buses route events. Don't merge.

## Sources

- Robert Nystrom, *Game Programming Patterns* (free): https://gameprogrammingpatterns.com — chapters Command, Observer, Singleton, State, Game Loop, Update Method, Component, Event Queue, Service Locator, Object Pool, Spatial Partition.
- Godot Engine docs: *Idle and Physics Processing* (https://docs.godotengine.org/en/stable/tutorials/scripting/idle_and_physics_processing.html); *Singletons (Autoload)*; *Autoloads versus internal nodes* (4.4); *Resources*; *Scenes versus scripts* (best_practices).
- Damián Isla, *Handling Complexity in the Halo 2 AI* (GDC 2005, Gamasutra): https://www.gamedeveloper.com/programming/gdc-2005-proceeding-handling-complexity-in-the-i-halo-2-i-ai
- Marc-Antoine Argenton, *Three Approaches to Halo-style Behavior Tree AI* (GDC 2007, GDC Vault).
- Beehave addon: https://github.com/bitbrain/beehave
- GDQuest: *The Events Bus Singleton* (https://www.gdquest.com/tutorial/godot/design-patterns/event-bus-singleton/); *Saving and Loading Games in Godot 4* (https://www.gdquest.com/library/save_game_godot4/).
- Febucci, *Godot Signals Architecture: Best Practices & Event Bus* (2024): https://blog.febucci.com/2024/12/godot-signals-architecture/
- The Shaggy Dev, *Advanced state machine techniques in Godot 4*: https://shaggydev.com/2023/11/28/godot-4-advanced-state-machines/
- Mayke Fernandes dos Santos, *Resource-based architecture for Godot 4*: https://medium.com/@sfmayke/resource-based-architecture-for-godot-4-25bd4b2d9018
- UhiyamaLab: *Custom Resources — Data-Driven Design in Godot Engine*; *Proper Usage of \_process and \_physics\_process*; *Editable Children vs Scene Inheritance*.
- Sander Mertens, *ecs-faq*: https://github.com/SanderMertens/ecs-faq
- Mojo Labs, *Entity–Component–System for Humans: When You Actually Need It*: https://mojolabs.nz/entity-component-system-for-humans-when-you-actually-need-it/
- Tynan Sylvester, *The Simulation Dream*: https://tynansylvester.com/2013/06/the-simulation-dream/ ; *Designing Games* (O'Reilly 2013).
- Jackson Dunstan, *The Problems with Object Pools*: https://www.jacksondunstan.com/articles/3829
- Game AI Pro 3 ch. 9 *Overcoming Pitfalls in Behavior Tree Design* (Anthony Francis): http://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter09_Overcoming_Pitfalls_in_Behavior_Tree_Design.pdf ; ch. 12 *A Reusable, Light-Weight Finite-State Machine* (David Graham).
- Kids Can Code, *Node communication (the right way)*: https://kidscancode.org/godot_recipes/4.x/basics/node_communication/
