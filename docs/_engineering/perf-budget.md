# Mother Nature — Performance Budget Manifest

> **OPERATIONAL DOCUMENT**. This is the live tracking of every committed system's cost class, estimated load, measured actuals (when profiled), and ship-status. Updates with every system spec or profile run.
>
> Companion: [`game-estimation.md`](game-estimation.md) is the analytical projection — budget waterfall, hardware targets, and confidence-tagged estimates.
>
> Authority: §18 Build Discipline — Profile-Driven Scope. The hard caps below are binding on v1 scope decisions per the design doc commitment.
>
> Status field values: `proposed` (back-of-envelope only) → `verified` (profile-tested, fits budget) → `shipped` (in v1 build) / `reformatted` (changed to fit budget) / `cut` (removed from v1) / `deferred` (held for v1.x or v2)

## Project Commitment (per §18, locked 2026-05-10)

This document is the binding rule the design respects for v1 scope decisions. Three commitments:

1. **Every committed system has an entry here.** No silent additions. New systems land as `proposed` with cost-class, estimated load, confidence tag, and cut alternative documented.
2. **No system ships as "I think it fits."** Status moves from `proposed` to `verified` only after profile-tested actuals on the binding hardware tier confirm the estimate.
3. **When a system overruns budget, the cut hierarchy is fixed:**
   - First: cut from §13 brainstorm (no load-bearing systems)
   - Second: defer to v1.x or v2 (Wild-Tender, managed-wild patch-state notebook, multiplayer settlement extensions, etc.)
   - Third: scope-cut (fewer biomes, archetypes, shard radius, apex individuals)
   - Fourth: fidelity downgrade (coarser grids, fewer saliency levels, simpler AI)
   - **NEVER**: cut constitutional pillars (§1, §2, §2.5, §14, §15, §17). If cost forces a pillar to change, the hardware target changes — not the pillar.

The pillars define what MN IS. Performance defines what's allowed to ship as MN. Reversing that ordering produces a different game.

### Binding hardware tiers

| Tier | Spec | Target FPS | Game logic ceiling |
|---|---|---|---|
| **Minimum** | Steam Deck OLED | 30 FPS | ~10-15 ms per frame |
| **Recommended** | 2022-era 6-core PC + GTX 1660 class + 16 GB | 60 FPS | ~6-8 ms per frame |

Every system must fit both. Steam Deck is the binding constraint for most CPU-heavy work (per-core perf); Recommended is the binding constraint for per-frame visual work (shaders, render). Aspirational (8-core + RTX 3060) is free headroom — not new content.

## Conventions

- **Cost class**: `always-on` (per-frame) / `periodic` (server tick at 0.5-1 Hz) / `async` (on-demand) / `bandwidth` (multiplayer) / `storage`
- **Estimated load**: back-of-envelope range with confidence tag (🟢 HIGH / 🟡 MEDIUM / 🔴 LOW)
- **Actual load**: measured value from profile test, with hardware tier annotation; blank until tested

## Frame budget summary (Recommended tier, 60 FPS)

| Tier | Budget per frame | Current estimate | Status |
|---|---|---|---|
| Always-on | ≤3 ms | ~1.5-4.3 ms (range) | At risk under naive saliency shader |
| Periodic amortized | ≤0.5 ms | ~3-6 ms per tick (well under) | Holds |
| Async | ≤50 ms per call | Holds with caching | Holds |
| Total game logic | ≤6-8 ms | ~2-5 ms expected | Fits with optimization |

---

## Always-on systems (per-frame)

| System | Cost class | Est. load | Confidence | Actual | Status | Notes |
|---|---|---|---|---|---|---|
| Body meter updates (§6) | always-on | <0.01 ms | 🟢 | — | proposed | Trivial; O(1) × 6 meters |
| Character animation (§5, §6) | always-on | ~0.05 ms | 🟢 | — | proposed | Godot AnimationTree, well-benchmarked |
| Player input handling | always-on | <0.01 ms | 🟢 | — | proposed | Trivial |
| **Saliency shader visible entities (§14.13)** | always-on | **1-3 ms** | 🔴 | — | **proposed — P0 PROTOTYPE** | Cost depends on shader × visible entity count. Domain-gating + distance LOD + cached level = ~70% reduction |
| Saliency-degradation level (§14.13 ext) | always-on | <0.05 ms | 🟢 | — | proposed | Cached; recomputed only on state-input threshold cross |
| UI / HUD render | always-on | ~0.2 ms | 🟢 | — | proposed | Static-heavy UI |
| Per-frame entity-state tick (visible apex + ambient) | always-on | ~0.3-0.8 ms | 🟡 | — | proposed | Light tick per visible entity (~40 total) |

**Always-on subtotal: 1.5-4.3 ms (target ≤3 ms). Tight under naive saliency shader; comfortable with optimization.**

---

## Periodic systems (server tick at 0.5-1 Hz)

| System | Cost class | Est. load | Confidence | Actual | Status | Notes |
|---|---|---|---|---|---|---|
| Weather grid simulation (§3.3, §12.8) | periodic | 1-2 ms | 🟢 | — | proposed | 100 hex cells × semi-Lagrangian advection |
| Scent dispersal on weather grid (§12.8) | periodic | 0.5-1 ms | 🟢 | — | proposed | Same grid, second pass with wind advection |
| **Apex individual AI tick (§12.8, bear-ai.md)** | periodic | **1-3 ms** | 🟡 | — | **proposed — P0 PROTOTYPE** | 10 individuals × utility AI (~30 inputs × 8 outputs) |
| Ambient prey/predator population (§5.5) | periodic | 0.5 ms | 🟢 | — | proposed | Lotka-Volterra per region, bulk math |
| Track decay (§13.3, §12.8) | periodic | <0.1 ms amortized | 🟢 | — | proposed | Batch every game-hour (~3 real-min at 24:1); spike ~50 ms |
| Trace decay (§17.4) | periodic | <0.1 ms amortized | 🟢 | — | proposed | Same shape as tracks; per-shard isolation |
| **Stress/saliency state composition (§14.13 ext)** | periodic | **~5 ms at 100 CCU ⚠️** | 🟡 | — | **proposed — needs caching design** | Per-active-player × 8 modulators; must cache, recompute on state change |
| Carrion Chain state update (§13.2) | periodic | <0.5 ms | 🟢 | — | proposed | Per-active-carcass scent emission update |
| Apex behavioral memory updates (§17.6 DS) | periodic | <0.2 ms per event | 🟢 | — | proposed | Event-driven, not per-tick |
| Plant growth tick (§3 biomes, §17.3) | periodic | <0.5 ms per region | 🟡 | — | proposed | Coarse-grain region-level update |
| Day-night cycle update (§4) | periodic | <0.1 ms | 🟢 | — | proposed | Single time tick |
| Seasonal transition logic (§4.5) | periodic | <0.1 ms | 🟢 | — | proposed | Day-boundary only |

**Periodic subtotal: ~3-6 ms per tick at 100 CCU. Holds if state composition is cached.**

---

## Async systems (on-demand)

| System | Cost class | Est. load | Confidence | Actual | Status | Notes |
|---|---|---|---|---|---|---|
| **Trace query on viewport entry (§17.4)** | async | **5-15 ms** | 🟡 | — | **proposed — P1 PROTOTYPE** | Spatial-index SQL query; cached after first query. Worst case at year-old shard. |
| Compendium lookup (§8, §12.5) | async | 1-5 ms | 🟢 | — | proposed | Indexed DB read |
| Shard browser (§12.9) | async | 10-30 ms | 🟢 | — | proposed | Central account DB; once at character creation |
| Character creation (§12.9) | async | 50-100 ms | 🟢 | — | proposed | Multi-table DB write |
| Field Notes generation on death (§14.12) | async | 10-20 ms | 🟢 | — | proposed | Multi-row DB writes + composition |
| Forest Signs read/write (§13.1) | async | 1-3 ms | 🟢 | — | proposed | Per-shard DB, indexed by location |
| Compendium event-log append (§12.8) | async | <1 ms | 🟢 | — | proposed | Append-only |

**Async holds with normal caching discipline.**

---

## Bandwidth (multiplayer, per-player per second)

| Channel | Bandwidth | Confidence | Status | Notes |
|---|---|---|---|---|
| World state delta (positions, animations) | 10-30 KB/s | 🟢 | proposed | Comparable to PZ; protocol-optimized |
| Entity state sync (apex, weather) | 5-10 KB/s | 🟢 | proposed | Periodic only |
| Trace updates (significant actions) | ~1 KB per action (bursty) | 🟢 | proposed | Player-initiated |
| Voice (proximity, opus codec) | ~24 KB/s when active | 🟢 | proposed | §17.5 proximity-only |
| **Per-player total** | **40-65 KB/s** | | | |
| **Server total @ 100 CCU** | **4-6.5 MB/s** | | | Modest; single VPS handles easily |

---

## Storage

| Asset | Size | Confidence | Status | Notes |
|---|---|---|---|---|
| Per-shard DB steady state (year 1) | 150-200 GB | 🟡 | proposed | From §17.4 math (~14 GB/month raw + decay) |
| Per-account compendium | ~10 MB per character | 🟢 | proposed | Event-sourced log + projections |
| Per-shard installation assets | 5-10 GB | 🟢 | proposed | Procedural map gen reduces baked assets |
| **Total server-side per shard** | **~200 GB** | | | Per-shard VPS class |

---

## Risk register

| Risk | Severity | Mitigation | Status |
|---|---|---|---|
| Saliency shader cost on Steam Deck under naive impl | HIGH | Domain-gated shader + distance LOD + cached level (~70% cost reduction) | Designed; needs prototype validation |
| Apex AI utility-eval at full input vector | MEDIUM | Per-individual eval at 0.5 Hz, not per-frame | Designed; needs prototype |
| Stress/saliency composition at 100 CCU | MEDIUM | Cache + threshold-driven recomputation | Designed; needs implementation discipline |
| Trace query at year-old shard | MEDIUM | Spatial index + LOD culling + decay pruning | Designed; needs prototype with synthetic data |
| Storage growth on long-lived shards | LOW | Decay-pruning to 200 GB steady state | Designed; per-shard VPS sizing handles |
| Bandwidth at 100+ CCU per shard | LOW | Protocol optimization; voice is biggest factor | Designed |

---

## Cut hierarchy (per 2026-05-10 design discussion)

When budget overrun is identified, cut order:

1. **Cut uncommitted §13 brainstorm items first** — nothing in §13 is load-bearing
2. **Defer v1.x candidates** — Wild-Tender sub-node (Run 16), managed-wild patch-state notebook, multiplayer settlement systems (Run 17)
3. **Scope-cut** — fewer launch biomes (3→2), fewer archetypes (8→5), smaller shard radius (10→7 km), fewer apex individuals tracked (10→6)
4. **Fidelity downgrade** — weather grid 100→64 cells, scent grid coarsened, saliency 5 levels→3 levels (Sharp/Narrowed/Tunneled only)
5. **NEVER cut** — §1, §2, §2.5, §14, §15, §17 commitments are constitutional

---

## Prototype priorities

| Priority | What to prototype | Why | Estimated effort |
|---|---|---|---|
| **P0** | Saliency shader cost at 20, 50, 100 visible entities | Biggest variance in per-frame budget | 1-2 weeks |
| **P0** | Apex AI utility-eval at 10 individuals, full input/output | Validates periodic tick estimate; affects shard pop cap | 1-2 weeks |
| **P1** | Trace SQL query at simulated 1-year shard data | Validates async tier; could trigger redesign | 1 week |
| **P1** | Multiplayer bandwidth at 100 CCU realistic action mix | Validates server spec | 2 weeks |
| **P2** | Saliency-degradation composition with 8 modulators × 100 players | Validates state-composition caching | 1 week |

P0 items gate v1 vertical slice. Without them, scope decisions are guesses.

---

## Decision log

| Date | Decision | Reason |
|---|---|---|
| 2026-05-10 | Seeded perf-budget.md with ~30 currently-committed systems classified by tier | Established operational tracking per design discussion. All entries `proposed`; none `verified` until prototyping. |
