# Mother Nature — Game Performance Estimation (v0, 2026-05-10)

> **STATUS**: ESTIMATE, NOT MEASURED. Numbers below are back-of-envelope projections grounded in Godot 4 documentation, published 2D-game benchmarks, and algorithm complexity. They are confidence-tagged. Real values require vertical-slice prototyping on target hardware. This document drives the **what-needs-prototyping-first** decision.
>
> Companion: [`perf-budget.md`](perf-budget.md) is the operational manifest tracking each system's cost class and ship-status.

---

## Target hardware

Mother Nature is a 2D top-down Steam-delivered survival simulation. The hardware tiers we're committing to:

| Tier | Spec | Target FPS | Frame budget |
|---|---|---|---|
| **Minimum** | Steam Deck OLED (4-core Zen 2 @ 2.4 GHz, 8 RDNA 2 CUs, 16 GB LPDDR5) | 30 FPS sustained | 33.3 ms |
| **Recommended** | 2022-era 6-core CPU (e.g., Ryzen 5 5600 / Intel i5-12400), 16 GB DDR4, GTX 1660 Super / RX 6500 class GPU | 60 FPS sustained | 16.7 ms |
| **Aspirational** | Modern desktop (8-core CPU + RTX 3060 / RX 6700 class) | 120 FPS | 8.3 ms |

**The build commits to the Recommended tier at 60 FPS as the design target.** Steam Deck at 30 FPS is the survivability floor (most Deck-class hardware must run at acceptable quality). Aspirational is for hardware-rich players; design does not chase it.

This means **~5-8 ms per frame is available for game logic** on Recommended hardware after rendering, physics, and OS overhead.

---

## Frame budget waterfall (Recommended tier, 60 FPS target)

```
16.7 ms total frame budget
  ├─ Rendering            ~6-8 ms   (2D sprites, lighting shader, particle effects)
  ├─ Physics + collision  ~2-3 ms   (player + active wildlife + projectiles)
  ├─ Game logic           ~5-8 ms   ← THIS is what design decisions consume
  └─ Buffer + variance    ~1-2 ms   (GC, OS interrupts, frame-pacing)
```

**Game logic budget (~6 ms target, 8 ms ceiling) is divided across the three tier categories:**

| Tier | Budget per frame | What runs here |
|---|---|---|
| **Always-on** | ≤3 ms total | Saliency shader on visible entities, body meters, character animation, UI updates, input processing |
| **Periodic** (server tick at 0.5-1 Hz) | ≤5 ms per tick, amortized to <0.5 ms per frame | Weather grid, scent dispersal, wildlife AI, decay processing |
| **Async** (on-demand) | ≤50 ms per call, cached aggressively | Trace queries on viewport entry, compendium lookups, shard browser, Field Notes generation |

On Steam Deck (Minimum tier, 30 FPS target with 33 ms budget), Always-on doubles to ≤6 ms and Periodic doubles to ≤10 ms per tick. Same SHAPE, slower target.

---

## Per-system estimates with confidence tags

Confidence legend:
- 🟢 **HIGH** — algorithm complexity is known; can be calculated from Godot 4 documented benchmarks
- 🟡 **MEDIUM** — based on patterns from similar games (PZ, Valheim, RimWorld); needs validation
- 🔴 **LOW** — novel-to-MN system; estimate is informed guess; needs prototype to verify

### Always-on systems (per-frame)

| System | Cost per frame | Confidence | Notes |
|---|---|---|---|
| Body meter updates (single player) | <0.01 ms | 🟢 | O(1) per meter × 6 meters. Trivial. |
| Character animation state | ~0.05 ms | 🟢 | Godot AnimationTree node, well-benchmarked. |
| Player input handling | <0.01 ms | 🟢 | Trivial. |
| **Saliency shader on visible entities** | ~1-3 ms | 🔴 | Wildcard. Cost depends on shader complexity × visible entity count (target ~20 saliency-relevant entities at any moment). Could be 0.5 ms with optimization or 5 ms naive. **PROTOTYPE FIRST.** |
| Saliency-degradation level computation | <0.05 ms | 🟢 | If cached and recomputed only on state-input threshold cross, this is trivial. |
| UI / HUD render | ~0.2 ms | 🟢 | Static UI, low-update. |
| Per-frame entity-state tick (visible apex + ambient) | ~0.3-0.8 ms | 🟡 | ~10 apex individuals + ~30 ambient prey in viewport. Each gets a light per-frame check (position, animation, behavior tick if needed). |
| **Subtotal** | **~1.5-4.3 ms** | | **Tight under naive saliency shader. Comfortable with optimization.** |

### Periodic systems (server tick, 0.5-1 Hz)

| System | Cost per tick | Confidence | Notes |
|---|---|---|---|
| Weather grid simulation (100 hex cells, semi-Lagrangian advection) | ~1-2 ms | 🟢 | Comparable to fluid sims in cellular automata; Godot's array math handles this well. |
| Scent dispersal on weather grid | ~0.5-1 ms | 🟢 | Same grid, additional pass with wind advection. |
| **Apex individual AI tick** (10 individuals × utility AI eval) | ~1-3 ms | 🟡 | Utility AI with ~30 inputs × 8 outputs per individual. Per-individual ~0.1-0.3 ms. |
| Ambient prey/predator population update (Lotka-Volterra per region) | ~0.5 ms | 🟢 | Bulk arithmetic, no per-entity simulation. |
| Track decay processing | <0.1 ms amortized | 🟢 | Batch every game-hour (~3 minutes real-time at 24:1). Spike cost ~50 ms. |
| Trace decay processing | <0.1 ms amortized | 🟢 | Same shape as track decay. |
| Stress/saliency state composition (per active player) | ~0.05 ms × 100 players = ~5 ms ⚠️ | 🟡 | Could be a problem at high CCU. **Cache aggressively, recompute only on state change.** |
| **Subtotal** | **~3-6 ms per tick** | | **Holds at 100 CCU if state-composition is cached well.** |

### Async systems (on-demand)

| System | Cost per call | Confidence | Notes |
|---|---|---|---|
| Trace query on viewport entry | ~5-15 ms | 🟡 | Spatial-index SQL query against per-shard DB. Cached after first query. **Worst case at year-old shard: needs prototype.** |
| Compendium lookup | ~1-5 ms | 🟢 | Indexed DB read. |
| Shard browser query | ~10-30 ms | 🟢 | Central account DB read. One-time at character creation. |
| Character creation | ~50-100 ms | 🟢 | Multi-table DB write. One-time. |
| Field Notes generation on death | ~10-20 ms | 🟢 | Multi-row DB writes + composition. |
| **Subtotal** | **Holds; cache discipline matters** | | |

### Multiplayer bandwidth (per player per second)

| Channel | Bandwidth | Confidence | Notes |
|---|---|---|---|
| World state delta (positions, animations) | 10-30 KB/s | 🟢 | Comparable to Project Zomboid; protocol-optimized. |
| Entity state sync (apex individuals, weather) | 5-10 KB/s | 🟢 | Periodic updates only. |
| Trace updates (significant actions) | ~1 KB per action (bursty) | 🟢 | Player-initiated; not constant. |
| Voice (proximity, opus codec) | ~24 KB/s when active | 🟢 | Industry-standard codec. |
| **Per-player total** | **~40-65 KB/s** | | |
| **Server total at 100 CCU** | **~4-6.5 MB/s** | | **Modest. Single-server bandwidth is not a bottleneck.** |

### Storage

| Asset | Size | Confidence | Notes |
|---|---|---|---|
| Per-shard DB (steady state, year 1) | ~150-200 GB | 🟡 | Estimate from §17.4 Trace as Object math (~14 GB/month raw + decay). |
| Per-account compendium | ~10 MB per character | 🟢 | Event-sourced log + compendium projections. |
| Per-shard installation | ~5-10 GB (assets) | 🟢 | Procedural map gen reduces baked-asset size. |
| **Total server-side per shard** | **~200 GB** | | **Per-shard VPS class, not exotic infrastructure.** |

---

## Total budget consumption summary

**Game logic per frame (Recommended tier):**

- Always-on: **1.5-4.3 ms** (target ≤3 ms — at risk if saliency shader runs naive)
- Periodic amortized: **<0.5 ms** (well under)
- Frame buffer for everything else: ~1-2 ms

**Total estimated game logic: ~2-5 ms per frame.** Fits the ~6 ms target with margin IF the saliency shader is implemented thoughtfully.

**Risk concentration: saliency shader (per-visible-entity cost).**

This single system accounts for ~60-70% of the always-on budget under naive implementation. Three optimizations would bring it well within budget:

1. **Domain-gated shader** — only apply saliency mod to entities relevant to character's skill domain (Hunter sees animal-cue shader; doesn't run shader on plant entities). Cuts ~70% of shader invocations.
2. **Distance LOD** — entities >30m from player get simplified shader. Cuts ~50% further.
3. **Cached level** — saliency-degradation level cached per character, recomputed only on state-input threshold cross. Already in spec.

With these optimizations, always-on budget settles at **~1-2 ms**, leaving comfortable headroom.

---

## What needs prototyping before commit

These are the **must-measure-before-build** items:

| Priority | What to prototype | Why |
|---|---|---|
| **P0** | Saliency shader cost at 20, 50, 100 visible entities | Biggest variance driver in the per-frame budget |
| **P0** | Apex AI utility-eval at 10 individuals with full input/output vectors | Validates the periodic-tick estimate; affects shard population cap |
| **P1** | Trace SQL query at simulated 1-year shard data (synthetic load) | Validates async-tier; could trigger redesign if query is slow |
| **P1** | Multiplayer bandwidth at 100 CCU with realistic action mix | Validates server-spec assumptions |
| **P2** | Saliency-degradation composition with 8 modulators × 100 players | Validates the state-composition caching strategy |

Each prototype is ~1-2 weeks of focused work. P0 items gate the v1 vertical slice; P1 gates beta scope; P2 is engineering hygiene during alpha.

---

## Cut hierarchy if budget overruns

Per the design discussion (2026-05-10), the cut order is:

1. **Cut uncommitted §13 brainstorm items first** — §13 has nothing load-bearing
2. **Defer v1.x candidates** — Wild-Tender sub-node (Run 16), managed-wild patch-state notebook, multiplayer settlement systems (Run 17 surfacings)
3. **Scope-cut** — fewer launch biomes (3→2), fewer archetypes (8→5), smaller shard radius (10→7 km), fewer apex individuals tracked per shard (10→6)
4. **Fidelity downgrade** — weather grid 100→64 cells, scent grid coarsened, saliency 5 levels →3 levels (Sharp/Narrowed/Tunneled only), shorter trace-decay timescales
5. **NEVER cut** — §1, §2, §2.5, §14, §15, §17 commitments are constitutional

This ordering protects design identity. We ship a smaller MN before a less-honest one.

---

## What I CANNOT estimate without prototyping

Some systems are genuinely novel-to-MN and can only be ranged with low confidence:

- **Saliency-shader cost at realistic entity density** — depends on Godot shader compiler + GPU performance under our specific use case. Could be 5x better or 2x worse than estimated.
- **AI utility-eval cost with full memory + behavioral state** — depends on data layout, cache behavior, and how complex the utility function ends up. Could be 2-3x higher than current estimate.
- **Server-side simulation overhead during low-population shard ticks** — depends on Godot's headless server cost. No public benchmark covers our specific shape.
- **Long-shard-age trace SQL query at scale** — depends on PostgreSQL/SQLite index strategy. Worst case could be 10x our current estimate.
- **Cross-platform parity** — Steam Deck SoC vs desktop GPU may behave very differently for our specific shaders. Need both targets profiled.

For these, the prototype work above is the answer. Until then, ranges are wide and decisions should preserve flexibility.

---

## What I'd need from you to do better than this

1. **Confirm hardware tiers** — is Steam Deck the minimum target, or do we target Steam Deck Lite / Windows handhelds too? Affects budget by ~50%.
2. **Confirm CCU target per shard** — 100 is current assumption (§12.1 indie-viable range, mid-32-128 expected). If you want higher (256? 512?), per-player budget tightens.
3. **Confirm v1 vs v1.x scope cuts you're comfortable with** — e.g., is multiplayer at launch mandatory, or could it be v1.5? Solo-only v1 cuts a huge chunk of cost.
4. **Engagement of a Godot engineer for the P0 prototypes** — to validate the riskiest assumptions before scope is locked. ~6-8 weeks of dedicated engineering effort.
5. **Specific Steam Deck performance access** — either own hardware or testing partnership.

Without those, this estimate is the best defensible projection given the data available. It says: **the design fits the target hardware IF the optimization patterns are applied, and the saliency shader is the principal risk.**

---

## Decision log

| Date | Decision | Reason |
|---|---|---|
| 2026-05-10 | Created initial estimate document | Per design discussion on perf-budget tracking and game-affordability question. Seeded with the ~30 currently-committed systems. |
