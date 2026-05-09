# Atmospheric Simulation for Survival Games — Research Report

## Domain summary

A coarse-grid weather model is a thin physics simulation layered behind a thick visual presentation. The simulation owns six floats per cell and runs at human-perceptible time scales (minutes, not frames); the renderer interpolates everything visible from those cells. The literature splits sharply: classical CFD (Stam, shallow-water solvers) gives realism that no survival player will perceive, while purely scripted weather (RimWorld, Don't Starve) is cheap but hits a ceiling on emergent surprise. The middle path — what Dwarf Fortress and Project Atmosphere converge on — is **separated scalar fields plus terrain feedback plus front-based advection**, with empirical rules instead of conservation equations. The whole point is that *the player reads the sky to predict gameplay*, which collapses into a much smaller correctness target than meteorology.

## Concept catalog

### Cell-based atmospheric state
- **Brief**: World partitioned into 50-200 polygons, each holding scalars (temperature, pressure, humidity, cloud density, wind vector, precipitation rate).
- **Why-drilled**: L1: "discrete cells let you update independently." L2: "you need a grid because partial differential equations get cell-wise discretized." First principle: *all field-based simulation reduces to local-rule updates on neighborhoods* — the cell *is* the simulation unit because radiation, advection, and diffusion are all neighborhood operations.
- **Mother Nature application**: 100-150 cells of ~150m each over a ~2km × 2km play area, lookup-by-position O(1).
- **Implementation depth for indie scope**: Square or hex grid; hex is nicer for diagonal advection but square is faster to debug. Store as flat array, index by `y*W+x`.
- **Novice failure mode**: Per-cell-per-frame update at 60 Hz, 10,000 cells. Dies.
- **Priority**: P0
- **Sources**: Forsyth (cellular automata), Project Atmosphere

### Six-float-per-cell discipline
- **Brief**: temperature, pressure, humidity, cloud density, wind_x, wind_y. Optional: precipitation_rate (derived).
- **Why-drilled**: L1: "fewer floats, less work." L2: "the rendering layer reads only what it draws." First principle: *every additional state variable doubles the test surface*; Tarn Adams's principle of "limiting complexity to essential variables for practical tuning."
- **Mother Nature application**: 6 × 4 bytes × 150 cells = 3.6 KB. Fits in L1 cache. Trivial to serialize, sync, debug.
- **Novice failure mode**: Adding pollutants, electromagnetism, fronts-as-objects, snow-depth — drowns the model.
- **Priority**: P0

### Pressure → wind via gradient
- **Brief**: Wind in cell C points down-pressure-gradient: `wind = -k * grad(P)` plus damping. Skip Coriolis at this scale.
- **Why-drilled**: L1: "high pressure pushes air to low pressure." L2: "force balance between pressure-gradient force and friction sets velocity." First principle: *air mass flows along the gradient because pressure is potential energy density* — work per unit volume is exactly `∫P dV`, so a gradient is literally a force per volume.
- **Mother Nature application**: Each tick, compute `(P[x+1] - P[x-1])/2dx` along each axis, accumulate into wind vector with friction term `wind *= 0.95`. Wind then advects humidity and temperature to neighbors.
- **Implementation depth**: Geostrophic balance is for synoptic scales (~1000 km, 24 h). For 2 km / hours, use straight gradient flow + friction. Flag explicitly: "Coriolis omitted, scale below regime where it matters."
- **Novice failure mode**: Trying to make winds curve around pressure highs — too small a domain for the effect to dominate.
- **Priority**: P0

### Adiabatic lift → cloud → rain
- **Brief**: When humid air is forced up (terrain or front collision), it cools at ~9.8 °C/km dry / ~5 °C/km wet. Below dewpoint, water condenses → cloud density up. Above a threshold → precipitation.
- **Why-drilled**: L1: "rising air cools and condenses." L2: "expansion against lower pressure does work, cooling the parcel." First principle: *first law of thermodynamics for adiabatic processes*: `dU = -P dV` — no heat exchange, so all expansion work comes from internal energy, i.e. temperature.
- **Mother Nature application**: When wind hits a cell with elevation > upwind cell, increment `cloud_density += k * humidity * elevation_gain`. When `cloud_density > C_threshold`, emit precipitation, decrement humidity.
- **Implementation depth**: One-pass per tick. `k` is a tuning knob, not a physical constant.
- **Novice failure mode**: Tracking parcel buoyancy through CAPE, LCL, LFC. Six weeks of work, players see "it rained more in the mountains."
- **Priority**: P0

### Radiative balance → temperature
- **Brief**: Each cell gains heat from sun (depends on time-of-day, cloud_density, slope aspect) and loses heat to IR (depends on cloud_density, ground emissivity). Net flux drives temperature.
- **Why-drilled**: L1: "sun heats the ground, ground radiates heat away." L2: "Stefan-Boltzmann: radiative loss ∝ T⁴." First principle: *net energy in equals net energy out at equilibrium*; temperature is the balance variable.
- **Mother Nature application**: `T[c] += (solar_in(c, hour) * (1 - cloud[c]*0.7) - ir_out(T[c], cloud[c])) * dt`. Curve-fit constants empirically.
- **Novice failure mode**: Linearizing around 273 K and getting -50 °C swings overnight.
- **Priority**: P0

### Diffusion to neighbors
- **Brief**: Each tick, each scalar bleeds toward neighbor average: `X_new[c] = X[c] + α * (mean(neighbors) - X[c])`.
- **Why-drilled**: L1: "smoothing." L2: "discrete heat equation." First principle: *gradient flow minimizes the L2 norm of differences* — diffusion is Laplacian of the field, smoothing instabilities the explicit advection introduces.
- **Mother Nature application**: α = 0.05–0.15. Stops sharp gradients from blowing up the wind step. **Order matters**: advect first, then diffuse, then radiate, then condense.
- **Novice failure mode**: Diffusing from neighbors that have already been updated this tick — direction-of-update bias (Forsyth's "turn counter" problem).
- **Priority**: P0

### Advection by wind
- **Brief**: Quantities (humidity, temperature, cloud) move with wind velocity. Semi-Lagrangian: trace back from cell center along `-wind*dt`, sample bilinearly.
- **Why-drilled**: L1: "wind carries moisture downwind." L2: "advection term ∂φ/∂t = -v·∇φ." First principle: *material derivative — quantity follows the parcel, not the cell*.
- **Mother Nature application**: Stam's semi-Lagrangian backtrace is unconditionally stable, doesn't need CFL constraint. For 150 cells at 0.1 Hz, ~150 sample operations per scalar per tick.
- **Novice failure mode**: Forward advection (push to neighbor) — leaks mass, oscillates, requires tiny timesteps.
- **Priority**: P0

### Front-based weather (Project Zomboid model)
- **Brief**: Spawn cold/warm fronts at the map edge with bearing, speed, intensity. Front sweeps through cells, modifying their state. Decoupled from per-cell physics.
- **Why-drilled**: L1: "weather feels like passing weather, not random per-cell flicker." L2: "real synoptic-scale weather IS front-driven; per-cell state alone gives noise." First principle: *atmospheric instability self-organizes into coherent structures (fronts) at scales much larger than grid cell* — emulate the structure directly rather than waiting for emergence.
- **Mother Nature application**: 0–2 fronts active. When a front center is in cell `c`, push `c.pressure` toward front's pressure for several ticks. Front trajectory is straight-line + light noise.
- **Implementation depth**: Front is a struct {pos, velocity, type, intensity, lifespan}. ~30 lines of code. Massive narrative payoff per line.
- **Novice failure mode**: Trying to derive fronts emergently from pressure patterns — they will not form at this grid resolution.
- **Priority**: P0

### Elevation lapse
- **Brief**: `T_cell -= 6.5 °C/km × elevation_above_sea`. Apply as a static modifier each radiation step.
- **Priority**: P1

### Rain shadow
- **Brief**: Windward slope of mountain gets extra precipitation; leeward stays dry. Already an emergent consequence of adiabatic lift + advection if both are wired correctly.
- **Mother Nature application**: Free if lift + advection are correct. Test by placing a mountain ridge across prevailing wind and asserting leeward humidity drops by >40%.
- **Priority**: P1 (test, don't code)

### Valley cold inversion
- **Priority**: P2

### Coastal heat capacity
- **Brief**: Cells with water mass have a thermal inertia ~5× land. Damp temperature swings.
- **Priority**: P2

### South-facing slope insolation
- **Brief**: South-facing slopes (in northern hemisphere) receive more solar flux per area; warm faster.
- **Priority**: P2

### Update-frequency tiering
- **Brief**: Simulation tick at 0.1–1 Hz; visual interpolation at 60 Hz. Don't conflate.
- **Why-drilled**: First principle: *Nyquist — sample the slowest interesting variable at 2× its rate*. Synoptic weather changes over 30+ minutes; 0.5 Hz oversamples by 900×.
- **Mother Nature application**: Tick the grid every 2 seconds (0.5 Hz) of game time. Visuals lerp between previous and next tick. **Bimodal SLO**: tick latency budget = 30 ms (rare); render budget = 0.5 ms (every frame).
- **Priority**: P0

### Visualization layer (read-only from cells)
- **Brief**: Sky color, particle density, wind animation parameters all derived from the cell containing the camera. No back-feedback into simulation.
- **Mother Nature application**: Each frame, sample player's cell, lerp sky/rain/wind shaders. Audio mixer also reads cell state.
- **Priority**: P0

### Storm telegraphing
- **Brief**: Approaching front is *visible to the player* before it arrives via cloud bank, wind shift, pressure drop on barometer item, animal behavior change.
- **Mother Nature application**: Front lifecycle = APPROACH (10 min cell time) → ARRIVE (5 min) → PEAK (10 min) → DEPART (10 min). Visible cues each phase.
- **Priority**: P0

### Multiplayer authoritative grid
- **Brief**: Server runs the only simulation. Clients receive cell-deltas at low rate (~1 Hz).
- **Mother Nature application**: Server simulates 150 cells. Each client gets sent only nearby cells (e.g., 9-cell neighborhood) at 1 Hz, ~216 bytes/sec.
- **Priority**: P0

### Save/load and schema versioning
- **Brief**: Serialize grid as `{version, W, H, cells[], fronts[], time}`. Bump version on every cell-field change, write migration.
- **Priority**: P1

### Determinism for sync
- **Brief**: All sim math uses fixed-point or carefully-ordered float. Random uses seedable PRNG fed only from server tick number.
- **Priority**: P1

## Recommended Mother Nature implementation

**Architecture**: Single autoload Godot node `WeatherGrid` owning a flat array of `WeatherCell` structs. Hex grid, 100 cells (10 across × 10 down approximate), each ~150 m per side. Update tick = 2 s game time (0.5 Hz). Visual layer is a separate `WeatherView` node reading the grid each frame.

**Per-tick pipeline** (executed on server only):
1. **Solar input**: per cell, accumulate `solar_flux(hour, slope) * (1 - cloud*0.7)`.
2. **IR loss**: subtract `σ*T⁴`-curve-fit term.
3. **Front evolution**: advance each active front by `front.velocity * dt`. Apply pressure pull on cells within radius.
4. **Wind**: compute pressure gradient, wind = `-k*∇P + 0.95*prev_wind`.
5. **Advection (semi-Lagrangian)**: backtrace each cell's humidity/temperature from `pos - wind*dt`.
6. **Lift / condensation**: where wind crosses elevation gradient, dump humidity into cloud.
7. **Precipitation**: cells with `cloud > C_rain` emit rain, decrement cloud and humidity.
8. **Diffusion**: 3-point Laplacian smoothing, α = 0.1.
9. **Tick counter ++**, broadcast deltas to clients.

**Front spawning**: Director script every 30–90 minutes spawns one front from a random map edge, type weighted by season (winter→cold-front-heavy, summer→thunderstorm).

**Visual layer** reads `cell_under_player`:
- Sky shader gradient: `lerp(clear_sky, overcast, cell.cloud)`.
- Rain particle emission: `cell.precipitation * 200 particles/sec`.
- Wind direction → vegetation sway shader, dust particle bias.
- HUD temperature: `cell.T`.
- Audio buses: `wind_howl_volume = |wind|`, `rain_intensity = precipitation`, distant thunder probability scales with neighbor cells' precipitation > heavy.

**Multiplayer**: Server runs all 9 steps. Client receives compact 9-cell window around player at 1 Hz (~120 bytes). During server downtime, client freezes weather visually but doesn't extrapolate.

## Predictability mechanic — design recommendation

The barometer is the single most important predictability item. Craftable mid-game, reads `cell.pressure` directly. Players learn that "drops below 1000 hPa within 1 hour → storm." Layer with **environmental tells**: distant cloud bank shader on incoming front edge, wind direction shift visible in flag/grass shaders, animal behavior change (birds settle, deer flee toward shelter) triggered when front is in `APPROACH` phase. Survival skill *Read the Sky* — taught quest — surfaces a HUD overlay that interprets cell state in plain English ("cumulus building, rain in ~10 min"). This makes weather a game of information accumulation, not a slot machine — Don't Starve's seasons feel emergent precisely because their transitions are telegraphed for days.

## Must-include shortlist

1. Coarse hex grid, 6 floats per cell, 100 cells, 0.5 Hz tick.
2. Pressure → wind via gradient (no Coriolis).
3. Semi-Lagrangian advection of humidity and temperature.
4. Adiabatic lift on terrain → cloud → precipitation (free rain shadow).
5. Front director: spawn 0–2 named fronts/hour with telegraphed lifecycle.
6. Solar + IR radiative balance for temperature.
7. Server-authoritative grid; clients receive cell-window deltas.
8. Visual layer one-way reads cell under camera.
9. Barometer item exposing `cell.pressure` directly.
10. Save/load with schema versioning and migration script.

## Commonly oversold

- **Coriolis force** at 2 km scale: zero gameplay payoff.
- **Full Navier-Stokes / projection step** (Stam): visual quality invisible behind weather particles.
- **Multi-layer cloud altitudes** (DF cumulus/cirrus/stratus): DF wiki notes these have no fortress-mode gameplay impact.
- **Weather machine learning / LLM forecasting**: vastly overscoped; rule-based front directors give better-feeling output.
- **Per-tile microclimate**: the *grid* is already the microclimate; per-tile interpolation is a render concern.
- **Lightning strike physics**: scripted thunder-distance audio is 95% of the player perception.
- **Hailstone size hierarchy, snowflake albedo, fog dewpoint elevation**: time sinks with sub-1% perceptual return.

## Cross-references

- *system-architect-core* Rule 6 (bimodal SLOs): tick latency vs frame latency must be separately budgeted.
- *system-architect-core* Rule 2 (orchestrator owns invariants): server is the only writer to the grid.
- *concurrency*: avoid per-cell threading at 150 cells — sequential is faster than thread overhead.
- *data-patterns*: grid serialization is a data contract; version it from day 1.
- *performance-engineering*: classify "active cells in storm" vs "idle cells" as separate SLO populations.

## Sources

- [Simulation Principles from Dwarf Fortress (Tarn Adams, Game AI Pro 2 ch. 41)](http://www.gameaipro.com/GameAIPro2/GameAIPro2_Chapter41_Simulation_Principles_from_Dwarf_Fortress.pdf)
- [DF2014:Weather (Dwarf Fortress Wiki)](https://dwarffortresswiki.org/index.php/DF2014:Weather)
- [DF2014:Climate (Dwarf Fortress Wiki)](https://dwarffortresswiki.org/index.php/DF2014:Climate)
- [Cellular Automata for Physical Modelling (Tom Forsyth)](https://tomforsyth1000.github.io/papers/cellular_automata_for_physical_modelling.html)
- [Weather (PZwiki, Project Zomboid)](https://pzwiki.net/wiki/Weather)
- [Climate Change devblog (Project Zomboid 2018)](https://projectzomboid.com/blog/news/2018/03/climate-change/)
- [Heat (Frostpunk Wiki)](https://frostpunk.fandom.com/wiki/Heat)
- [Why Frostpunk Game Design Is So Good (RetroStyleGames)](https://retrostylegames.com/blog/frostpunk-game-design/)
- [Seasons (Don't Starve Wiki)](https://dontstarve.wiki.gg/wiki/Seasons)
- [Events (RimWorld Wiki)](https://rimworldwiki.com/wiki/Events)
- [Project Atmosphere (Modrinth)](https://modrinth.com/mod/project-atmosphere)
- [Geostrophic wind (Wikipedia)](https://en.wikipedia.org/wiki/Geostrophic_wind)
- [Practical Meteorology ch. 10 (UBC EOAS)](https://www.eoas.ubc.ca/books/Practical_Meteorology/mse3/Ch10-Dyn.pdf)
- [Lapse rate (Wikipedia)](https://en.wikipedia.org/wiki/Lapse_rate)
- [Atmospheric Stability (Hawaii OER)](https://pressbooks-dev.oer.hawaii.edu/atmo/chapter/chapter-5-atmospheric-stability/)
- [Orographic Precipitation (Minder/Roe, U. Washington)](https://earthweb.ess.washington.edu/roe/GerardWeb/Publications_files/MinderRoe_OrogPrecEncyc.pdf)
- [Practical Fluid Dynamics: Part 1 (Game Developer)](https://www.gamedeveloper.com/programming/practical-fluid-dynamics-part-1)
- [Real-Time Fluid Dynamics for Games (Stam)](http://graphics.cs.cmu.edu/nsp/course/15-464/Fall09/papers/StamFluidforGames.pdf)
- [Fast Fluid Dynamics Simulation on the GPU (NVIDIA GPU Gems)](https://developer.nvidia.com/gpugems/gpugems/part-vi-beyond-triangles/chapter-38-fast-fluid-dynamics-simulation-gpu)
- [Predicting Weather Using a Barometer (Old Farmer's Almanac)](https://www.almanac.com/predicting-weather-using-barometer-and-wind-direction)
- [Dynamic Weather Systems in Godot (Wayline)](https://www.wayline.io/blog/dynamic-weather-systems-godot)
- [godot-weather-2D (pirachute, GitHub)](https://github.com/pirachute/godot-weather-2D)
- [Game Networking: State vs. Input (Ruoyu Sun)](https://ruoyusun.com/2019/03/28/game-networking-1.html)
- [How's the Weather: Simulating Weather in Virtual Environments (Game Studies)](https://gamestudies.org/0801/articles/barton)
- [The Dynamic Impact of Weather Systems on Gameplay (DesignTheGame)](https://www.designthegame.com/learning/tutorial/the-dynamic-impact-weather-systems-gameplay)
