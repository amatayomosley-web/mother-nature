# Game Math + Physics Fundamentals — Research Report

## Domain summary

A 2D Godot novice needs roughly fifteen mathematical concepts at conceptual depth and five at hands-on depth. The rest is overteaching. Modern engines absorb the heavy lifting (matrix composition, rigid-body integration, narrowphase contact resolution), so the novice's job is to read what the engine is doing well enough to debug and to reason about gameplay (movement, aim, easing, collision shapes, RNG). The most common failure mode is mistaking math anxiety for math need: solo 2D devs ship games with vectors, dot products, lerp, atan2, delta-time, and a probability sense. Calculus, quaternions, and matrix internals are not load-bearing for shipping a 2D game.

## Concept catalog

### 1. The 2D coordinate system (and why y points down)

- **Brief**: A point is `(x, y)`. In Godot and almost every 2D framework, `(0, 0)` is the top-left and `+y` goes down.
- **Why-drilled**: L1 — the engine just declares it. L2 — image and screen buffers are stored row-major from the top. First principle — CRT electron beams scanned left-to-right, top-to-bottom; framebuffer memory was laid out so that `offset = y * width + x`. Modern hardware keeps the convention out of decades of API lock-in (Learn365Project; The Hacks of Life). OpenGL inverted it later for 3D math reasons, but Godot's 2D world keeps the screen convention.
- **Concrete game-dev use**: A bullet moving "up" needs `velocity.y = -speed`, not `+speed`. Camera follows: a positive `position.y` means lower on screen.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Inverting the y-axis "to make math feel right" — fights the engine for the rest of the project.
- **Priority**: core.

### 2. Vectors: addition and subtraction

- **Brief**: A `Vector2` is two numbers treated as a unit. `a + b` adds component-wise; `b - a` produces the displacement from `a` to `b`.
- **Why-drilled**: L1 — "to move, add velocity to position." L2 — a vector represents either a position or an offset; subtracting two positions yields the offset between them. First principle — vectors are equivalence classes of displacements; arithmetic respects the geometric meaning (Lengyel; Godot vector_math doc).
- **Concrete game-dev use**: `direction = (target.position - self.position)` — the vector pointing from the entity to its target. `position += velocity * delta` — the canonical movement step.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Adding a position to a position. Geometrically meaningless; produces drifting bugs.
- **Priority**: core.

### 3. Scalar multiplication (scaling a vector)

- **Brief**: `v * s` multiplies every component by `s`. Direction unchanged unless `s < 0`; magnitude scales.
- **Why-drilled**: Same direction, different speed. Used everywhere movement combines an "intent direction" with a "how fast" scalar.
- **Concrete game-dev use**: `velocity = direction.normalized() * speed`. Knockback: `velocity += hit_normal * knockback_force`.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Forgetting to normalize before scaling — diagonal movement comes out √2 times faster than axis-aligned.
- **Priority**: core.

### 4. Magnitude (length) and normalization

- **Brief**: `length = sqrt(x*x + y*y)`. A normalized vector divides by its length, leaving direction at unit magnitude.
- **Why-drilled**: L1 — "needed for unit vectors." L2 — separates direction from magnitude so they can be mixed independently. First principle — Pythagorean theorem; Euclidean distance in 2D (Godot doc; Holmér Part 1).
- **Concrete game-dev use**: `if (player.position - enemy.position).length() < detection_range:` — proximity check. Use `length_squared()` when comparing distances to avoid the square root (Godot doc explicitly recommends this).
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Calling `length()` in a tight loop when `length_squared()` is sufficient; normalizing zero vectors and getting NaN.
- **Priority**: core.

### 5. Dot product

- **Brief**: `a.dot(b) = a.x*b.x + a.y*b.y`. For unit vectors, equals `cos(angle between them)`.
- **Why-drilled**: L1 — "tells you how aligned two vectors are." L2 — projection of one onto the other, scaled by lengths. First principle — Lengyel's framing: "a measure of how much one vector is like another." Sign tells you which half-space (in front / behind); magnitude tells you how aligned (1 = same, 0 = perpendicular, −1 = opposite).
- **Concrete game-dev use**: Field-of-view check: `enemy_facing.dot(to_player.normalized()) > 0.5` means the player is within ~60° in front. Dot also drives reflection: `R = I - 2*(I.dot(N))*N` for ball-off-wall bounces.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Comparing dot products of non-normalized vectors and treating the number as an angle.
- **Priority**: core.

### 6. Trig: sin, cos, atan2, radians vs degrees

- **Brief**: `sin/cos` parameterize circles. `atan2(y, x)` returns the angle of a vector (handling all four quadrants correctly, unlike `atan`).
- **Why-drilled**: L1 — `Vector2(cos(θ), sin(θ))` is a unit direction. First principle — the unit circle defines sin and cos; `atan2` is its inverse with quadrant disambiguation (GameDev Academy; Godot doc).
- **Concrete game-dev use**: Turret aim: `angle = atan2(target.y - self.y, target.x - self.x); rotation = angle`. Circular motion: `position = center + Vector2(cos(t), sin(t)) * radius`. Godot's trig functions take radians; expose degrees via `rotation_degrees` only in editor-facing code.
- **Depth a novice needs**: understand-conceptually for sin/cos, hands-on-comfort for atan2.
- **Novice failure mode**: Passing degrees to `cos()`; using `atan(y/x)` and getting wrong-sign results across quadrants.
- **Priority**: core.

### 7. Delta time

- **Brief**: `delta` is the seconds elapsed since the last frame. Multiply rates by it: `position += velocity * delta`.
- **Why-drilled**: L1 — "so movement looks the same on a 60Hz and 144Hz monitor." L2 — frames take variable time. First principle — OS scheduler jitter, GPU contention, vsync, and per-machine hardware variance mean no two frames last equally long; rate × time = displacement is the only invariant (Nystrom, Game Loop chapter; Gaffer On Games; Godot Recipes).
- **Concrete game-dev use**: `_process(delta)` for visual updates, `_physics_process(delta)` for fixed-step physics. Godot's `_physics_process` runs at a fixed 60 Hz by default; its `delta` is constant. `_process` `delta` is variable.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Forgetting `delta` entirely (everything tied to framerate); or applying it inside `_physics_process` thinking it's variable when it's fixed (harmless but reveals confusion).
- **Priority**: core.

### 8. Numerical integration: explicit vs semi-implicit Euler

- **Brief**: Two ways to advance position from velocity and acceleration. Explicit: position then velocity. Semi-implicit: velocity then position.
- **Why-drilled**: L1 — "Godot does this for you in physics_process." L2 — explicit Euler injects energy into oscillating systems and explodes; semi-implicit conserves energy on average. First principle — semi-implicit Euler is symplectic: it preserves the geometric structure of phase space, so springs and orbits don't spiral outward (Gaffer On Games).
- **Concrete game-dev use**: When you write a custom homebrew physics body, do `velocity += accel * delta; position += velocity * delta`, not the reverse. Knowing this exists helps debug when a hand-rolled spring "blows up."
- **Depth a novice needs**: just-the-name (you only need to recognize it when reading docs).
- **Novice failure mode**: Trying to write RK4 because a forum said so. RK4 has its own pathologies (energy loss in oscillators) and is overkill.
- **Priority**: nice-to-have.

### 9. Linear interpolation (lerp)

- **Brief**: `lerp(a, b, t) = a + (b - a) * t`. At `t=0` returns `a`; at `t=1` returns `b`; in between, blends.
- **Why-drilled**: L1 — animations smoothly transition values. L2 — every parametric tween, every blend, every "fade from x to y" reduces to lerp.
- **Concrete game-dev use**: `health_bar.value = lerp(displayed, target, 0.1)` for smoothed HUD; `Color.lerp(red, green, ratio)` for tinting; camera follow.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Treating `lerp(current, target, factor)` per-frame as framerate-independent — it isn't (Holmér's "Lerp Smoothing is Broken").
- **Priority**: core.

### 10. Easing and exponential decay

- **Brief**: Replace linear `t` with a curve (`smoothstep`, `ease_out`, exponential decay). Smoothstep is Hermite-interpolated S-curve.
- **Why-drilled**: L1 — "linear motion looks robotic." L2 — humans expect acceleration cues; constant-velocity motion reads as mechanical. First principle — biological motion in nature has inertia; perceptually, anything without acceleration profile reads as artificial (Smashing Magazine; Wikipedia/Smoothstep). Smoothstep was developed by Ken Perlin specifically to fix this in computer graphics.
- **Concrete game-dev use**: UI tweens via Godot's `Tween` node with `Tween.EASE_OUT`. For framerate-independent smoothing, use exponential decay: `current = target + (current - target) * exp(-decay * delta)` (Holmér, Driscoll).
- **Depth a novice needs**: understand-conceptually; hands-on for one easing call.
- **Novice failure mode**: Lerp-smoothing every frame at fixed `t` and shipping a game that feels different at 30 vs 144 fps.
- **Priority**: important.

### 11. Collision math: AABB, circle, raycast, normal, reflection

- **Brief**: AABB overlap = both x-ranges and y-ranges overlap. Circle-circle = distance ≤ sum of radii. Circle-AABB = clamp circle center to AABB, distance from clamped point ≤ radius. Raycast = parametric line vs shape. Reflection = `R = I - 2*(I.dot(N))*N`.
- **Why-drilled**: L1 — "Godot's CollisionShape2D handles this." L2 — Godot picks shapes for you, but knowing the math explains why a "stuck in wall" bug happens (penetration not resolved) or why a tunneling bullet at high speed misses (no swept test). First principle — every contact resolution computes a separation vector along a normal; the reflection formula is the dot-product projection mirrored.
- **Concrete game-dev use**: Use `Area2D` for triggers (overlap query), `CharacterBody2D.move_and_slide()` for movement with collisions. When a fast object tunnels, switch to `move_and_collide()` with safe-margin or Godot's continuous detection.
- **Depth a novice needs**: understand-conceptually (Godot does the work; you read errors).
- **Novice failure mode**: Reinventing collision detection in `_process` instead of using physics nodes.
- **Priority**: important.

### 12. Transformations: translate, rotate, scale (and order)

- **Brief**: Each is a matrix; composing them is matrix multiplication, which is non-commutative.
- **Why-drilled**: L1 — "rotate-then-translate ≠ translate-then-rotate." L2 — translation moves the local origin; subsequent rotation pivots around the new origin. First principle — matrix multiplication is non-commutative because each transform redefines the basis; (Wikipedia/Transformation matrix; Godot matrices_and_transforms doc).
- **Concrete game-dev use**: Godot's scene tree composes transforms automatically when you parent nodes — a `Sprite2D` child of a rotating `Node2D` orbits its parent. The default order Godot exposes is scale → rotate → translate (the standard SRT).
- **Depth a novice needs**: understand-conceptually.
- **Novice failure mode**: Manually composing matrices instead of parenting.
- **Priority**: important.

### 13. Pixel-perfect concerns

- **Brief**: Sub-pixel positions cause filtering blur; non-integer scale causes uneven pixel widths.
- **Why-drilled**: L1 — "pixel art looks bad without setup." L2 — texture filter `Linear` interpolates between source pixels; `Nearest` doesn't. Camera with float positions samples between texels. First principle — display pixels are a discrete grid; any continuous transform must commit to a snap rule (Godot pixel-perfect guides; GDQuest).
- **Concrete game-dev use**: Set `Texture > Filter = Nearest`; enable `Rendering > 2D > Snap 2D Transforms to Pixel`; use integer viewport scaling (Godot 4.3+).
- **Depth a novice needs**: hands-on-comfort if shipping pixel art; otherwise just-the-name.
- **Novice failure mode**: Wondering why the game looks blurry on every monitor.
- **Priority**: important (for pixel art) / nice-to-have (otherwise).

### 14. Polar coordinates

- **Brief**: A point as `(r, θ)` instead of `(x, y)`. Convert via `x = r*cos(θ); y = r*sin(θ)`.
- **Why-drilled**: Some problems are angular by nature: orbits, radial menus, bullet-hell shot patterns, AI scanning arcs.
- **Concrete game-dev use**: Radial menu: place each option at `(cos(i*step), sin(i*step)) * radius`. Spiral bullet pattern: increment `r` and `θ` together (CodePen radial menu math; gamemath.com).
- **Depth a novice needs**: understand-conceptually.
- **Novice failure mode**: Using polar where Cartesian would be simpler (e.g., a side-scroller).
- **Priority**: nice-to-have.

### 15. Random and noise

- **Brief**: Uniform RNG (`randi`, `randf`) is independent samples. Weighted RNG biases by table. Value/Perlin/simplex noise produces *coherent* random — adjacent samples are similar.
- **Why-drilled**: L1 — "random for variety, noise for terrain." L2 — uncorrelated random looks like static; coherent noise looks organic. First principle — Perlin's gradient noise interpolates between random gradient vectors at lattice points (Red Blob Games; Perlin's original work). Simplex is a faster, less-griddy successor.
- **Concrete game-dev use**: Spawn jitter: `pos + Vector2(randf_range(-5, 5), randf_range(-5, 5))`. Terrain: `FastNoiseLite` in Godot returns 2D noise; threshold it for cave/island maps.
- **Depth a novice needs**: hands-on-comfort for uniform; understand-conceptually for noise.
- **Novice failure mode**: Calling `noise.get_noise_2d()` on integer pixel coordinates and getting blockiness; not seeding the RNG and getting identical "random" runs.
- **Priority**: important.

### 16. Probability for games

- **Brief**: Expected value `EV = Σ (outcome * probability)`. Independent probabilities multiply for "all happen"; complement for "any happen": `P(at least one) = 1 - (1-p)^n`.
- **Why-drilled**: L1 — "loot tables and crits." First principle — every economy and progression curve compounds; getting EV wrong by 10% breaks the game (gamedeveloper.com; LootLocker).
- **Concrete game-dev use**: Cumulative-weight loot pick: pick `r = randf() * total_weight`, walk the table. Crit: `damage = base * (1 + crit_chance * (crit_mult - 1))` is the EV per swing.
- **Depth a novice needs**: hands-on-comfort.
- **Novice failure mode**: Designing rare drops at "1%" without computing how many runs to expected one drop (~70 runs for 50% chance of seeing it).
- **Priority**: important.

### 17. Big-O and broadphase

- **Brief**: Naive pairwise collision is O(n²). Spatial partitioning (grid hash, quadtree) reduces it.
- **Why-drilled**: L1 — "1000 bullets vs 1000 enemies = 1,000,000 checks/frame." L2 — most pairs are far apart and can be skipped. First principle — bin objects by location; only objects in the same/adjacent bins can collide (BuildNewGames; Newcastle physics tutorial).
- **Concrete game-dev use**: Godot's physics server already does broadphase; the relevance is recognizing the "frame stutter at high entity count" smell and reaching for `Area2D` carefully or pooling.
- **Depth a novice needs**: understand-conceptually.
- **Novice failure mode**: Hand-rolling `for a in entities: for b in entities: check(a, b)` and being surprised at 30 fps.
- **Priority**: nice-to-have.

## Must-include shortlist

These eight are non-negotiable. A novice who can't do them gets stuck on day-one tasks:

1. **Vector add/subtract** (movement and aiming).
2. **Scalar multiply + normalize** (frame-rate consistent direction).
3. **Magnitude / `length_squared` for distance checks**.
4. **Dot product** (FOV, alignment, reflection).
5. **`atan2`** (turret-style rotation toward a target).
6. **Delta time and `_process` vs `_physics_process`**.
7. **Lerp** (any smoothed transition).
8. **Random + weighted random** (loot, jitter, AI variance).

## Commonly oversold

- **Matrix internals beyond 2D parent transforms.** Godot's scene tree composes for you; manually multiplying `Transform2D` matrices is rarely the right tool.
- **Quaternions.** 3D-only. A 2D dev does not need them; `atan2` and `Vector2.from_angle()` cover all 2D rotation needs.
- **Full calculus.** Useful conceptually for understanding "velocity is the derivative of position" — note this fact and move on. You will not integrate symbolically.
- **RK4 and higher-order integrators.** Godot's physics is already stable; chasing "more accurate" Euler variants is a rabbit hole. Knowing semi-implicit beats explicit is enough.
- **Linear algebra (eigenvectors, basis change, SVD).** None of it is on the critical path of shipping a 2D game. 3blue1brown is a great optional curiosity for intuition only.
- **Manual collision algorithms (SAT, GJK).** Godot exposes these as nodes; reinventing them is a multi-week detour.
- **Bezier and spline math.** Godot's `Curve2D` covers it; deep theory comes only when authoring a custom path tool.

## Cross-references

- Pairs with **Engine concepts** (the Godot scene tree, `_process` vs `_physics_process`, `CharacterBody2D`).
- Pairs with **Physics body usage** (`move_and_slide`, `Area2D`).
- Pairs with **Animation systems** (Tween, AnimationPlayer use lerp under the hood).
- Pairs with **Procgen** (noise feeds terrain and dungeon generators).
- Pairs with **Game-feel/juice** (easing, screen shake — both built on lerp + noise).

## Sources

- [Godot Vector Math tutorial](https://docs.godotengine.org/en/stable/tutorials/math/vector_math.html)
- [Godot Matrices and Transforms (3.1 docs, still accurate for 2D)](https://docs.godotengine.org/en/3.1/tutorials/math/matrices_and_transforms.html)
- [Glenn Fiedler, "Integration Basics" (Gaffer On Games)](https://gafferongames.com/post/integration_basics/)
- [Robert Nystrom, "Game Loop", Game Programming Patterns](https://gameprogrammingpatterns.com/game-loop.html)
- [KidsCanCode, "Understanding delta" Godot Recipes](https://kidscancode.org/godot_recipes/4.x/basics/understanding_delta/index.html)
- [Freya Holmér, "Math for Game Devs Part 1: Numbers, Vectors & Dot Product"](https://www.youtube.com/watch?v=fjOdtSu4Lm4)
- [Freya Holmér, "Lerp smoothing is broken"](https://www.youtube.com/watch?v=LSNQuFEDOyQ)
- [Rory Driscoll, "Frame Rate Independent Damping using Lerp"](https://www.rorydriscoll.com/2016/03/07/frame-rate-independent-damping-using-lerp/)
- [Eric Lengyel, "Foundations of Game Engine Development, Volume 1: Mathematics"](https://lshoshia.science.tsu.ge/modeling/cignebi/Eric%20Lengyel%20-%20Foundations%20of%20Game%20Engine%20Development.%20Volume%201_%20Mathematics%20(2016,%20Terathon%20Software%20LLC)%20-%20libgen.li.pdf)
- [Fletcher Dunn, "3D Math Primer for Graphics and Game Development" — Polar chapter online](https://gamemath.com/book/polarspace.html)
- [Smoothstep, Wikipedia](https://en.wikipedia.org/wiki/Smoothstep)
- [Smashing Magazine, "Easing Functions for CSS Animations and Transitions"](https://www.smashingmagazine.com/2021/04/easing-functions-css-animations-transitions/)
- [Red Blob Games, "Making maps with noise functions"](https://www.redblobgames.com/maps/terrain-from-noise/)
- [adrianb.io, "Understanding Perlin Noise"](https://adrianb.io/2014/08/09/perlinnoise.html)
- [Build New Games, "Broad Phase Collision Detection Using Spatial Partitioning"](http://buildnewgames.com/broad-phase-collision-detection/)
- [LearnOpenGL, "Collision detection" (AABB and circle math)](https://learnopengl.com/In-Practice/2D-Game/Collisions/Collision-detection)
- [Mina Pêcheux, "Doing pixel-perfect in Godot the right way"](https://medium.com/codex/doing-pixel-perfect-in-godot-the-right-way-77cd39f8f23d)
- [GDQuest, "Setting up pixel art graphics in Godot 4"](https://www.gdquest.com/library/pixel_art_setup_godot4/)
- [Wintermute Digital, "Designing Luck — A Guide to Loot Distributions"](https://wintermutedigital.com/post/probcdf/)
- [LootLocker, "How to: Weighted Random Selections"](https://lootlocker.com/blog/random-with-weights)
- [Game Developer, "Loot drop best practices"](https://www.gamedeveloper.com/design/loot-drop-best-practices)
- [Heri Rodriguez, "The math core of a radial menu"](https://codepen.io/soybisonte/post/the-math-core-of-a-radial-menu)
- [Learn365Project, "Why computer coordinates start from the upper left corner"](https://learn365project.com/2015/08/01/why-do-computer-coordinates-start-from-the-upper-left-corner/)
- [The Hacks of Life, "Keeping the Blue Side Up: Coordinate Conventions for OpenGL, Metal and Vulkan"](http://hacksoflife.blogspot.com/2019/04/keeping-blue-side-up-coordinate.html)
- [GDC Vault, "Math for Game Programmers" tutorial series index](https://www.gdcvault.com/play/1017922/Math-for-Game)
