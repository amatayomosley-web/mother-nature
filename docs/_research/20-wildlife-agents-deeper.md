# Wildlife as Agents — Research Report (Deeper)

## Domain summary

Mother Nature's commitment that "animals are agents, not encounters" places it in a small lineage: The Long Dark, Red Dead Redemption 2, S.T.A.L.K.E.R., and (in classical roguelike form) Caves of Qud. The dominant survival/open-world pattern is the opposite — animals are spawn-on-proximity props with sight cones and a flee/attack switch. The deeper requirements (smell with wind direction, persistent apex individuals, regional populations that deplete, substrate-aware tracks, seasonal migrations, and the player as an ecological actor leaving a scent trail) form a coherent simulation stack where each piece reinforces the others. Scent only matters if wind is simulated; tracks only matter if the player and predators read them symmetrically; populations only matter if over-hunting has consequences. Implementation centers on three shared substrates — a weather/wind grid, a region-keyed population store, and a per-cell decay buffer for transient world objects (scent, tracks, scat). The behavior tree is the consequence, not the architecture.

## Topic catalog

### 1. Sight (line-of-sight + light + cover + motion)
- **Why**: Foundational; players intuit it; cheaply implemented as raycast + dot-product cone.
- **MN application**: Each species has `view_distance_day`, `view_distance_dusk`, `view_distance_night`; cone half-angle; motion bonus (still player at >40m → near-undetectable); cover occlusion via the existing terrain LOS check.
- **Failure mode**: Treating fog/blizzard the same as clear daylight (Project Zomboid's earlier builds explicitly didn't model fog; later patches did — note the regression risk).
- **Priority**: P0.
- **Sources**: The Long Dark wolf detection, Project Zomboid sound/vision.

### 2. Hearing (omnidirectional, attenuated)
- **Why**: Cheap, environmentally responsive, makes weather feel load-bearing.
- **MN application**: Sound events have intrinsic dB; attenuation = `db - distance_falloff - vegetation_dampening - ambient_mask`. Ambient mask is the *current* weather grid value (wind, rain, river noise) — animals near a waterfall hear less, animals on a quiet morning hear more.
- **Failure mode**: Modeling sound as a fixed radius. The Long Dark's wolf hearing is a specific 75m for rifles and 20m for projectiles, and players debug against those numbers.
- **Priority**: P0.

### 3. Smell with wind direction
- **Why**: The design doc's "most underused sense" claim is correct in production games. The Long Dark has a 45m smell radius modulated +/-15m by wind strength — the closest mainstream prior art, but it ignores **direction**. Far Cry simulates wind for fire spread but not for AI scent. None of the surveyed games (Far Cry, RDR2, STALKER, Don't Starve, Project Zomboid, Caves of Qud) do directional smell properly. This is genuinely novel territory.
- **MN application**: Treat scent as a scalar field on the existing weather grid. Each scent source emits intensity per tick; diffusion follows wind vector (per GAMA Platform's documented technique: "two diffusion matrices: one for uniform diffusion and one for the wind direction, with the wind matrices summing to 0"). Predators sample the field at their own cell. A bear downwind of the player at 60m detects them; at 60m crosswind, detects them weaker; at 60m upwind, undetectable.
- **Failure mode**: Per-frame raycasts from every animal. Use the grid; sample from agents.
- **Priority**: P0 — this is the differentiator.
- **Sources**: GAMA Platform diffusion docs, The Long Dark wolf scent, Far Cry environmental wind.

### 4. Scent intensity as inventory consequence
- **Why**: Makes carrying meat an ecological choice, not a UI choice.
- **MN application**: Player scent intensity = `base + Σ(item.smell_emission)`. Raw meat > cooked meat; bloodied clothes emit until washed. Cover scent (mud, smoke) **multiplies down**, doesn't add — applies as `intensity *= 0.3` so it's a real strategic tool.
- **Failure mode**: Boolean "smelly: yes/no" state. RDR2 actually does this for player Bill ("smells so bad you can track him"); good for character flavor, weak as system.
- **Priority**: P0 (binds inventory loop to wildlife loop).

### 5. Individual apex identity
- **Why**: The bear with the damaged ear is a story beat. RDR2's legendary animals do this but as one-shot kills (kill it and it's gone forever). MN wants the bear to persist, hunt, age.
- **MN application**: `ApexIndividual { id, species, hunger, territory_polygon, last_meal_at, last_seen_by_player_at, recognizable_features[] }`. Up to 5–10 tracked near player; ambient prey use the population counter only. When player leaves a region, freeze the apex's state vector with a timestamp; on return, advance state by elapsed game-time (`hunger += elapsed * decay`).
- **Failure mode**: Tracking 200 deer individually. Caves of Qud gets away with this because it's turn-based and grid-bounded; an open-world Godot title cannot. Coarse populations + named apexes is the right cleave.
- **Priority**: P0 for apex; P3 for prey.
- **Sources**: RDR2 legendary animals, Caves of Qud creature/faction persistence, STALKER A-Life lairs.

### 6. Regional population dynamics
- **Why**: Without it, "ecosystem" is theater. Players hunt; populations don't move; respawn breaks the fiction.
- **MN application**: Per-region per-species counter. Update rule each in-game day:
  ```
  N_t+1 = N_t + birth(N_t, season) - mortality(N_t, predators_in_region) - hunted_by_player
  N_t+1 = clamp(N_t+1, 0, K_region_species)
  ```
  Lotka-Volterra simplification: prey growth `r * N * (1 - N/K)` (logistic with carrying capacity); predator mortality scales with prey availability. The full LV model has known pathologies (oscillations) — use the **carrying-capacity-limited** variant cited in the literature, which empirically stabilizes coexistence ranges.
- **Failure mode**: Letting populations crash to zero permanently with no spillover from neighboring regions. Add slow inter-regional immigration so a depleted region recovers over seasons rather than becoming dead.
- **Priority**: P0.
- **Sources**: Lotka-Volterra extended model with prey carrying capacity, periodically varying K research.

### 7. Behavioral states with environmental triggers
- **Why**: Reduces "every bear is the same bear" homogeneity.
- **MN application**: State catalog `{resting, feeding, foraging, traveling, defensive, fleeing, mating, hyperphagic}`. Triggers are predicates over `(time_of_day, season, last_meal_age, threat_proximity, cubs_in_territory, individual.hunger)`. Critical: a bear's reaction to player encounter is the *current state's* reaction, not species-level. Defensive bear charges at 40m; feeding bear retreats at 80m; hyperphagic bear (autumn) treats player as competition for berries.
- **Failure mode**: Conflating state with mood. State is a contextual mode; mood is a continuous parameter. Keep separate.
- **Priority**: P0.

### 8. State transition triggers (deep)
- **Why**: This is what the design doc means by "deep triggers" — the predicates, not the states.
- **MN application**:
  - `cubs_nearby AND threat_within(50m)` → defensive
  - `season == autumn AND days_to_winter < 30` → hyperphagic (eats anything, more territorial)
  - `wounded AND threat_visible` → fleeing not fighting
  - `hunger > 0.7 AND prey_scent_detected` → hunting
  - `time_of_day == midday AND temp > comfort_max` → resting in shade
- **Priority**: P0.

### 9. Tracks as physical world objects
- **Why**: Survival fiction lives or dies on tracks. None of the surveyed games (TLD, RDR2, STALKER) do them as first-class objects with substrate awareness; Vintage Story's Footprints mod does substrate but not aging.
- **MN application**: `Track { species, individual_id?, substrate, created_at, intensity, position }`. On every footstep, query terrain substrate at position. Substrate table:
  - `mud` → intensity 0.9, decay 0.05/hour
  - `wet_dirt` → 0.7, 0.10/hour
  - `dry_dirt` → 0.4, 0.20/hour
  - `grass` → 0.2, 0.30/hour (visible but quickly gone)
  - `stone` → 0.0 (no track)
  - `fresh_snow` → 1.0, 0.02/hour but new snowfall covers
  - `packed_snow` → 0.7, 0.05/hour
  - `sand` → 0.6, multiplied by wind speed for decay
  Decay multipliers: rain ×3, wind ×1.5, fresh_snow ×∞ on covered tracks.
- **Failure mode**: Spawning a Sprite per footstep at full density. Use a sparse decay buffer keyed by terrain cell; render only nearby. Cull at intensity < 0.05.
- **Priority**: P0.

### 10. Sign types beyond tracks
- **Why**: Tracks alone are repetitive. Signs cluster in meaningful places.
- **MN application**: `SignType ∈ {track, scat, hair_on_bark, scratch, partial_kill, bed, scent_marker, claw_mark}`. Each species generates different signs at different rates. Bear leaves scat, claws trees at territory edges, beds under dense cover. Tracking becomes literacy: a hair-on-bark at 2m height is a black bear; at 3m, a grizzly.
- **Priority**: P1.

### 11. Symmetric tracking (predator reads player)
- **Why**: Without this, tracks are a one-way affordance. With it, tracks become tension.
- **MN application**: Player footsteps emit Track records identically to animal footsteps. `Predator.scan_for_tracks()` queries the same buffer. Wounded player → blood drops with longer decay and stronger scent. Bear at the edge of player territory may follow blood trail back to camp. This is the key to the "ecological actor" framing — symmetry is what makes the player part of the world rather than its observer.
- **Priority**: P0.

### 12. Migrations as scheduled population transfers
- **Why**: Don't Starve scripts seasonal *bosses* via timers — the wildlife equivalent should be population shifts, not boss spawns.
- **MN application**: Calendar event handler. On `season == autumn, day == 30`: transfer 60% of `highlands.deer.N` to `lowlands.deer.N`. On `season == spring, day == 15`: transfer 80% of `coastal_river.salmon.N` to `inland_river.salmon.N`. On `season == winter, day == 1`: bears enter `denning` state for the entire season; their region counter doesn't decay but their `available_for_encounter` flag is false. On `season == spring, day == 20`: bears emerge with `hunger == 1.0`.
- **Failure mode**: Animating actual migration. You don't need to. The population counter is the simulation; visible animals are sampled from the counter.
- **Priority**: P0.

### 13. Persistent territories per individual
- **Why**: TLD's wolves have pre-set patrol areas; STALKER's lairs work the same way. RDR2's legendary animal territories are static polygons. All three confirm: territory is a polygon attached to an individual, not a global heatmap.
- **MN application**: Apex individual owns a territory polygon (~1–4 km²). Defends it weakly against members of own species, strongly against rival apex. Player crossing territory boundary triggers low-grade alerts.
- **Priority**: P1.

### 14. Detection state machine separate from behavior state
- **Why**: TLD wolves have stalking/charging/fleeing modes that are orthogonal to "what the wolf is doing today." Don't fold them.
- **MN application**: Two parallel state machines. Behavior state (resting/foraging/etc.) drives default actions. Detection state (unaware/alerted/investigating/locked-on/engaged/disengaging) overrides behavior when threats are detected. Disengaging has a memory cooldown — wolf returns to behavior state but remembers the player for ~10 minutes of game time.
- **Priority**: P0.

### 15. Population view as a debug surface
- **Why**: Without observability, populations either feel random or you can't tell when balance breaks.
- **MN application**: Dev overlay showing per-region per-species counters, predator-prey ratios, recent mortality breakdown. Ship it disabled in release; keep it for tuning and post-launch balance patches. (Observability skill rule: alert on user-visible symptoms, but for this we need to *see* the simulation to tune it.)
- **Priority**: P1.

## Recommended Mother Nature implementation

**Sense systems**: Three sense modules per agent, each implementing `detect(world, agent) -> DetectionEvent | None`. Sight does raycast + cone test. Hearing samples the sound-event queue with attenuation. Smell samples the scent grid at agent's cell. The agent reduces multiple events into a single threat-priority queue. Each sense has independent species-scaled parameters: bear smells at 80m base; deer at 30m; wolf at 60m. Day/night and weather modify each parameter via the weather grid that already exists.

**Scent system on the weather grid**: Per-cell `scent_field[species_emitting]` scalar. Each tick: emit (sources add intensity), advect (shift by wind vector × wind speed), diffuse (blur to neighbors), decay (multiply by `e^(-dt/half_life)`). Half-life 5 minutes for footstep scent, 30 minutes for blood, 2 hours for carcass. Wind speed > threshold causes faster advection but also faster dispersal. Cell size: match the existing weather grid; if that's too coarse, downsample on a sub-grid only in the player's region.

**Apex individual scope**: Hard cap of 10 tracked apexes. Selection by proximity to player + narrative tag (the bear with the damaged ear has a `protected: true` flag, never gets evicted from the tracked set). When an apex moves outside the player's region, it freezes; on player re-entry, advance state by elapsed time. Population counter tracks the *species* count; tracked apexes are accounted as part of that count, not double-counted.

**Population dynamics architecture**: One table `region_species_population(region_id, species_id, count, last_updated_day)`. Daily tick advances all rows via the LV-with-K equation. Carrying capacities stored in `region_species_capacity`. Migrations are a separate `migration_event(from_region, to_region, species, fraction, trigger_calendar_day)` table. Hunting writes a `mortality_event(region, species, cause: player|predator|disease, count)` row — the mortality breakdown is needed for the debug overlay and for the symmetrical "the wolves are starving because you killed all the deer" feedback loop.

## Tracks/signs system design

**Track-as-object lifecycle**: On agent footstep tick (every N frames depending on speed), if substrate is track-capable, append `Track(species, individual_id, substrate, t_now, position, intensity_initial)` to the regional track buffer. Each game-time tick, multiply intensity by substrate-specific decay × weather modifier (rain ×3, wind ×1.5). Cull at intensity < 0.05. Reading a track surfaces species, age (from `t_now - now`), and direction; if it belongs to a tracked apex, the player learns "this is the bear with the damaged ear."

**Substrate variants**: substrate is a property of the terrain cell, queried at footstep time. Mud, wet_dirt, dry_dirt, grass, stone, fresh_snow, packed_snow, sand, leaf_litter. Stone produces no track. Snow has the highest fidelity but gets covered by new snowfall — special case: snowfall event sets all snow-tracks to intensity 0 in affected regions.

**Predator-tracks-player symmetry**: Player footsteps go through the same `emit_track()` path as animals. No special-casing. Bears, wolves, and any predator with the `tracks_prey` tag periodically scans the local track buffer for player-tagged tracks within their territory; following a fresh trail transitions them to `investigating` detection state. Wounded player emits both blood-drop tracks (longer decay) and elevated scent (binds to the scent grid). This symmetry is the architectural commitment: there is no "player track" code path — the player is a creature that emits Track records.

## Must-include shortlist

1. Smell on the weather grid with wind advection (P0; the differentiator).
2. Player scent shaped by inventory (P0; binds loops).
3. Apex individual identity capped at 10 with frozen-state-on-region-exit (P0).
4. Per-region per-species population with LV+K dynamics (P0).
5. Tracks as substrate-aware objects with weather-modified decay (P0).
6. Symmetric tracking (predators read player tracks via the same API) (P0).
7. Behavioral state × detection state as orthogonal machines (P0).
8. Seasonal migrations as scheduled population transfers (P0).

## Commonly oversold

- **Per-individual ambient prey**: Caves of Qud-style faction-on-every-creature does not scale to open-world Godot. Track apexes; aggregate the rest.
- **Real-time animated migration**: Visible migrations are theater; population counters are the simulation. Render samples from the counter.
- **Full Lotka-Volterra without K**: Classical LV oscillates pathologically. Always use the carrying-capacity variant.
- **Fixed-radius scent**: TLD's 45m+wind-strength model is the floor, not the ceiling. Direction is the missing piece.
- **Per-frame raycasts for senses**: Use the grid; sample from agents. Far Cry capped at 20 active animals because it raycast from each.
- **Tracks as Sprite per step**: Sparse decay buffer keyed by terrain cell; render only the nearby subset.

## Cross-references

Senses tie to **observability** (the debug overlay for population/scent fields) and **performance-engineering** (bimodal: cheap-ambient vs expensive-apex paths — separate SLOs). Population dynamics ties to **data-patterns** (the population store is the aggregate; rendered animals are the read model). Tracks tie to **api-design** (one `emit_track()` API used by player and animals symmetrically — the symmetry is a contract). The orthogonal-state-machine pattern (behavior × detection) maps onto the system-architect-core meta-rule: classify before tactic.

## Sources

- [The Long Dark Wolf wiki — detection mechanics](https://thelongdark.fandom.com/wiki/Wolf)
- [The Long Dark — wolves smelling cooked meat thread](https://steamcommunity.com/app/305620/discussions/0/2666627242544778689/)
- [Red Dead Wiki — Legendary Animals](https://reddead.fandom.com/wiki/Legendary_Animals)
- [TheGamer — RDR2's Bill scent tracking detail](https://www.thegamer.com/gaming-detail-rdr2-bill-smells-so-bad-track-him/)
- [STALKER Wiki — Wild Territory and lair system](https://stalker.fandom.com/wiki/Wild_Territory)
- [STALKER Anomaly Dynamic Mutants v1.42 (mutant territorial expansion)](https://www.moddb.com/mods/stalker-anomaly/addons/dynamic-mutants-v142-for-anomaly-151)
- [Game Developer — Systemic AI of Far Cry](https://www.gamedeveloper.com/programming/the-definition-of-artificial-insanity-the-systemic-ai-of-far-cry)
- [Steam Far Cry 2 stealth guide — weather affects detection](https://steamcommunity.com/sharedfiles/filedetails/?id=181803593)
- [GAMA Platform — diffusion implementation with wind matrices](https://gama-platform.org/wiki/next/Diffusion)
- [Lotka-Volterra equations (Wikipedia)](https://en.wikipedia.org/wiki/Lotka%E2%80%93Volterra_equations)
- [Lotka-Volterra with periodically varying carrying capacity (Phys Rev E)](https://link.aps.org/doi/10.1103/PhysRevE.107.064144)
- [BB512 — predator-prey dynamics with K](https://jonesor.github.io/BB512_Book/lotka-volterra-predator-prey-dynamics.html)
- [Vintage Story Footprints mod](https://mods.vintagestory.at/footprints)
- [Don't Starve seasonal spawn mechanics](https://playwithtools.com/post/seasonal_giants_driving_you_mad_don_t_starve_spawn_mechanics_simplified)
- [Project Zomboid — sound and zombie awareness](https://dedicatedgamingservers.com/guides/how-sound-affects-zombie-awareness-in-project-zomboid/)
- [Caves of Qud Wiki — Factions (67 persistent + procedural)](https://wiki.cavesofqud.com/wiki/Factions)
- [Caves of Qud Wiki — Creatures (Brain part as faction-eligibility)](https://wiki.cavesofqud.com/wiki/Creatures)
- [Beehave — behavior tree addon for Godot 4](https://github.com/bitbrain/beehave)
- [LimboAI — alternative BT/FSM for Godot 4](https://github.com/limbonaut/limboai)
