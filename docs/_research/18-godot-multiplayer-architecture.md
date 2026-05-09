# Godot 4 Multiplayer Architecture — Research Report

## Domain summary

Godot 4's high-level multiplayer API — `MultiplayerPeer` + `MultiplayerSpawner` + `MultiplayerSynchronizer` + `@rpc` annotations — is genuinely capable for survival-sim workloads up to ~16 players, was reworked in 4.0 with proper authority semantics, and is actively shipping commercial titles (Dome Keeper added co-op April 2026, Pratfall on 4.6.1 + C#). It is **not** an MMO engine: independent stress tests put per-server connection stability around ~40 CCU, the engine ships **no built-in client-side prediction, no rollback, no interest-management API beyond `set_visibility_for()` callbacks, no NAT traversal, no matchmaking, and no anti-cheat** ([Ziva 2026](https://ziva.sh/blogs/godot-multiplayer)). For Mother Nature's three-tier model, this is fine for solo and private (2–10), but the public-world tier is an infrastructure problem (regional sharding, persistent DB, custom orchestration) that Godot helps with only at the per-shard level — the engine builds the server process; you build the fleet.

The single most important architectural rule is the meta-rule from the system-architect pack: **the orchestrator (the dedicated server) owns invariants; clients are computation that produces candidate inputs, never authoritative state**. Survival sims have PvP, durable inventory, and persistent worlds — exactly the trust surface that punishes client-authoritative shortcuts.

## Topic catalog

### Transport: ENet vs WebSocket vs WebRTC vs SteamNetworkingSockets
- **Brief**: `ENetMultiplayerPeer` wraps ENet (UDP with reliable + unreliable + unreliable_ordered channels, sequencing, bandwidth shaping); `WebSocketMultiplayerPeer` is TCP-over-WS for HTML5 exports; `WebRTCMultiplayerPeer` is P2P with native HTML5 support but requires a GDExtension on desktop and your own signaling server; GodotSteam wraps Steam Networking Sockets and is Mother Nature's most likely production transport on Steam.
- **Why-drilled**: L1: pick the right pipe. L2: TCP head-of-line blocking ruins position updates; UDP with selective reliability matches the physics of "drop a stale packet, send a fresh one." First principle: **latency floors are physics; you cannot send below RTT/2**, so you choose the protocol that lets you drop bytes that are already obsolete.
- **Mother Nature application**: Solo — no peer at all (or a `MultiplayerPeer` of `null`/offline). Private — `ENetMultiplayerPeer.create_server(port, max_clients=10)` on the host; clients use `create_client(ip, port)`. Public-world — `ENetMultiplayerPeer` on a headless Linux VPS per shard, fronted by a region router; or GodotSteam if going Steam-native for the social graph.
- **Novice failure mode**: Shipping `WebSocketMultiplayerPeer` for desktop because the tutorial used it. TCP retransmits stale position packets, position visibly stutters under any loss.
- **Priority**: core
- **Sources**: [ENetMultiplayerPeer docs](https://docs.godotengine.org/en/stable/classes/class_enetmultiplayerpeer.html), [Godot 4.0 multiplayer report](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-3/)

### Peer ID semantics and `is_multiplayer_authority()`
- **Brief**: Server is always peer ID `1`. Each client gets a unique non-1 ID on connect. `multiplayer.get_unique_id()` returns self; inside an RPC, `multiplayer.get_remote_sender_id()` returns the caller. `Node.is_multiplayer_authority()` returns true if the local peer is the node's authority. **Without authority, RPCs and replication silently fail** — frequently without errors ([Meshiest gist](https://gist.github.com/Meshiest/1274c6e2e68960a409698cf75326d4f6)).
- **Why-drilled**: First principle: trust. Every networked node needs exactly one authority; ambiguity = race.
- **Mother Nature application**: Server (peer 1) owns wildlife, weather, world-state, durable inventory. Each player's input node has authority transferred to that peer. This split is the load-bearing pattern — bodies on server, input on client.
- **Novice failure mode**: Forgetting `set_multiplayer_authority(peer_id)` after spawn → client moves locally but server ignores; or leaving authority on server but expecting client RPC to mutate state.
- **Priority**: core
- **Sources**: [Node.is_multiplayer_authority](https://docs.godotengine.org/en/stable/classes/class_node.html), [Meshiest overview](https://gist.github.com/Meshiest/1274c6e2e68960a409698cf75326d4f6)

### MultiplayerSpawner
- **Brief**: A node that auto-replicates instantiation of children under a `spawn_path` to all peers. You add scenes to its spawn list (or use a `spawn_function` callback for runtime control). Only the spawner's authority can spawn.
- **Why-drilled**: L1: avoid manual `add_child` + RPC dance. L2: scene replication needs deterministic identity. First principle: **spawned node names must be unique and deterministic** (e.g. `str(peer_id)`); random names break replication ([StraySpark](https://www.strayspark.studio/blog/godot-4-multiplayer-networking-authoritative-server)).
- **Mother Nature application**: Server-spawned wildlife, dropped items, projectiles. Use `spawn_function` so the server can stamp authority correctly per-instance (player body → that peer; wolf → server).
- **Novice failure mode**: Setting random names. Spawning from a client without authority. Calling `change_scene_to_packed` mid-session — late joiners desync; use a `MultiplayerSpawner` to spawn the level itself.
- **Priority**: core
- **Sources**: [MultiplayerSpawner docs](https://docs.godotengine.org/en/stable/classes/class_multiplayerspawner.html), [Scene replication article](https://godotengine.org/article/multiplayer-in-godot-4-0-scene-replication/)

### MultiplayerSynchronizer
- **Brief**: Replicates configured properties from authority to non-authorities. Two modes per property: `REPLICATION_MODE_ALWAYS` (every interval, controlled by `replication_interval`) and `REPLICATION_MODE_ON_CHANGE` (delta sync, controlled by `delta_interval`). Both default to 0.0 = every network frame.
- **Why-drilled**: L1: don't write per-property RPC sync. L2: bandwidth ∝ properties × peers × rate. First principle: **bandwidth is money**; "always" replicate only data that genuinely changes every tick (position of moving entities); "on_change" everything else (health, hunger, ammo, equipped item).
- **Mother Nature application**: Player position → ALWAYS at ~10–20 Hz with `replication_interval ≈ 0.05`. Player hunger/thirst/temp → ON_CHANGE only, and only sync to that player (visibility filtered). Wildlife position → ALWAYS but interest-managed. Weather grid cell → ON_CHANGE per cell.
- **Novice failure mode**: Putting full inventory dictionary in a synchronizer with always-mode → bandwidth collapse. Multiple synchronizers with mismatched visibility (one with host auth, one with peer auth) → "moves but invisible" or "visible but frozen" bugs.
- **Priority**: core
- **Sources**: [MultiplayerSynchronizer docs](https://docs.godotengine.org/en/stable/classes/class_multiplayersynchronizer.html)

### `@rpc` modes
- **Brief**: `@rpc` annotation has four orthogonal axes — **mode** (`authority` default, `any_peer`), **sync** (`call_remote` default, `call_local`), **transfer** (`reliable` default, `unreliable`, `unreliable_ordered`), **channel** (int, default 0). Order doesn't matter; `@rpc("any_peer", "call_local", "unreliable_ordered", 1)` is valid.
- **Why-drilled**: First principle: **reliability has cost** (head-of-line blocking on lossy links). Use `unreliable` for stuff that has a fresher version coming (positions). Use `unreliable_ordered` for stream-like effects (footstep sounds at sequence X). Use `reliable` for state-machine transitions (door open, item pickup). Channels prevent independent streams from blocking each other.
- **Mother Nature application**:
  - `@rpc("any_peer", "reliable")` — client requests "drop item" on server (server validates).
  - `@rpc("authority", "reliable", "call_local")` — server announces "wolf killed deer at (x,y)"; runs locally on server too so its own state mutates.
  - `@rpc("any_peer", "unreliable_ordered", 2)` — client streams its movement intent inputs.
  - `@rpc("authority", "unreliable", 3)` — server broadcasts wolf positions in interest set.
- **Novice failure mode**: Forgetting `any_peer` on client→server inputs (silent failure). Validating no sender — `multiplayer.get_remote_sender_id()` MUST be checked on every `any_peer` RPC, or any client can mutate any other player's state.
- **Priority**: core
- **Sources**: [Godot 4.0 RPC syntax article](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-2/)

### Authoritative server vs P2P
- **Brief**: Three trust models. **Server-authoritative** (Rust): server owns state, clients send inputs. **P2P-with-host** (Project Zomboid, Valheim): one client elected host, other clients trust host. **Pure P2P/lockstep** (RTS): all clients run identical sim deterministically. Godot supports all three but `set_multiplayer_authority` and the high-level API lean server-auth.
- **Why-drilled**: First principle: **clients lie**. Any client-authored gameplay state will be cheated within hours of public release if the game is even mildly popular. P2P-with-host is "the host is the server" — which means the host can cheat too, but in friend-group survival that's tolerable.
- **Mother Nature application**: Solo — no model needed. Private (2–10) — host-authoritative is fine; the host can technically cheat but the social contract of a friend lobby contains it. Public (thousands) — must be dedicated-server authoritative; PvP makes any client trust suicidal.
- **Novice failure mode**: Letting clients write to inventory/health/position directly via synchronizer. Even in private, this lets a malicious friend grief.
- **Priority**: core

### Server tier semantics (solo / private / public)
- **Brief**: Three deployment shapes — single-process offline, integrated host (one client also runs server), dedicated server (headless export, no rendering, runs on Linux VPS).
- **Mother Nature application**: Solo — single process; multiplayer code paths run with peer = `null` or peer ID 1 alone. Private — integrated host pattern: detect "Host Game" → `create_server`, "Join" → `create_client`. Pause-on-empty: server checks `multiplayer.get_peers().is_empty()` and either pauses world tick or shuts down. Public — headless export with `--headless` flag; `OS.has_feature("dedicated_server")` to gate visual code; persistent DB; one Godot server process per shard/region; orchestrator (Kubernetes / Pterodactyl / custom) manages fleet.
- **Novice failure mode**: Not using dedicated-server export → ships textures and shaders to the VPS, wastes RAM and CPU. Not gating visual code with `OS.has_feature("dedicated_server")` → server tries to instantiate ParticleSystem, crashes on headless.
- **Priority**: core
- **Sources**: [Exporting for dedicated servers](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_dedicated_servers.html)

### Persistence and reconnect
- **Brief**: Godot ships **nothing** for this. You write file I/O (solo, private) or DB writes (public) yourself. The pattern: on `peer_disconnected` save the player's record; on `peer_connected` after auth, load and respawn.
- **Why-drilled**: First principle: server crashes are when the database earns its keep. Save on every meaningful state transition (item pickup, death, log-out), not only on shutdown.
- **Mother Nature application**: Solo — JSON or `ResourceSaver` to user://. Private — host saves a single world file periodically + on shutdown. Public — Postgres or SQLite-per-shard with periodic snapshots; player record (inventory, position, body sim numbers) keyed by Steam ID or account ID.
- **Novice failure mode**: Saving only on clean shutdown → power loss loses an hour of progress. No save schema versioning → new patch can't load old worlds.
- **Priority**: core

### Replication strategy and interest management
- **Brief**: Out of the box, every replicated node is visible to every peer. `MultiplayerSynchronizer.set_visibility_for(peer_id, bool)` and `public_visibility = false` give per-client filtering. Trigger this from an `Area3D`/`Area2D` AoI around each player. The engine has **no automatic AoI**; you build it with collision bodies or grid lookups.
- **Why-drilled**: First principle: **bandwidth and CPU are bimodal under load**. 10 players each seeing 50 wildlife = 500 sync targets; same map at 100 players = 5000. Naive sync is O(N²). AoI culls to roughly constant per-player.
- **Mother Nature application**: Wildlife — visible only to players within ~80m AoI. Weather grid — each player gets only their cell + neighbors. Body sim (hunger/thirst/temp) — `set_visibility_for(self_id, true)` and false everywhere else; nobody else needs your hunger value. World items on ground — AoI-based. Other player positions — AoI-based + view-range.
- **Novice failure mode**: Forgetting that **visibility is ignored if the local peer doesn't own the synchronizer's authority** — you must split into two synchronizers (server-auth visibility, peer-auth input).
- **Priority**: core for public, important for private, deferred for solo

### Anti-cheat baseline
- **Brief**: For indie Godot survival without paying for BattlEye/EAC, the baseline is: (1) server-authoritative state — clients send inputs only; (2) sender validation on every `any_peer` RPC; (3) sanity checks on input — position delta ≤ max_speed × dt, resource consumption matches inventory; (4) periodic state hash — server hashes inventory and world state and compares between snapshots to catch memory editing; (5) rate limiting on RPC channels.
- **Why-drilled**: First principle: **cheating is an economic decision**. Make it cost more time to write the cheat than the player saves vs honest play. Indie scope can't afford kernel-level anti-cheat; baseline server validation kills 95% of script kiddies and all naive memory editors. Keep the remaining 5% out of public PvP servers via report+ban.
- **Mother Nature application**: All inventory mutations go through `@rpc("any_peer", "reliable")` to server which validates "did this peer have this item?" before granting effect. Movement validated via max-speed clamp on server. Speedhacks fail because server clamps. Wallhacks fail because AoI never sends data outside view range.
- **Novice failure mode**: Trusting client-reported damage. Trusting client-reported position. Storing health on a peer-authority synchronizer.
- **Priority**: core for public PvP, important for private, n/a for solo

### Latency / lag compensation
- **Brief**: Godot ships **no built-in client-side prediction or rollback**. Third-party addons fill this: **Netfox** (rollback netcode + interpolation + lag compensation) and **Godot-monke-net** (server-auth + CSP/SR). For survival pace (vs FPS), simple linear interpolation of remote entities + immediate local feedback on inputs is usually enough.
- **Why-drilled**: First principle: **latency physics**. Round-trip lower bound = 2× speed-of-light delay between peers. ~40ms regional, ~150ms transcontinental. Predator attack windows must exceed RTT or the defender literally cannot react.
- **Mother Nature application**: Local player movement — predict immediately, reconcile from server. Other players — interpolate between last two server snapshots (~100ms behind). Predator attacks — server-authoritative hit resolution, ≥250ms wind-up animation so even 200ms-RTT players can dodge. Don't over-engineer for survival pace.
- **Novice failure mode**: No interpolation → other players teleport between updates. Full lockstep → all clients wait on slowest, everyone stutters.
- **Priority**: important

### Sharding / regional fleet for public-world
- **Brief**: Godot has no built-in sharding. Pattern from MMO architecture: auth server + region router (e.g. us-west-1, eu-central-1) + many headless Godot processes each owning one shard (one geographic region of the world map). Player-position-keyed DB lookup decides which shard to route into; seamless transition is engineered by the orchestrator handing off DB ownership.
- **Why-drilled**: First principle: **Godot is single-threaded for game logic in practice**, so one process scales vertically only to one core's work. Per-shard player ceiling realistically 50–200 depending on simulation cost.
- **Mother Nature application**: Public-world is a fleet of shards, each running one biome region. Cross-shard travel = save+load+reconnect to neighbor shard. ~40 CCU per shard is the conservative published number; some setups go higher with care.
- **Novice failure mode**: Trying to fit thousands on one process. Treating "MMO" as an engine feature instead of an infrastructure project.
- **Priority**: core for public-world tier; deferred otherwise

### Hosting cost models
- **Brief**: Solo — zero. Private — peer-hosted, zero infra cost. Public — VPS rental. Rough numbers: a small dedicated VPS adequate for 1 shard of ~50 CCU runs ~$20–60/month (Hetzner, OVH, DigitalOcean). Scaling to 5,000 CCU across regions ~$1,500/month is the published estimate for a comparable MMO.
- **Mother Nature application**: Plan for community-hosted private servers (downloadable headless build) as the primary model + 1–3 official region shards. This is the Project Zomboid / Valheim model and economically sustainable for an indie.
- **Priority**: important

### Real-world Godot 4 multiplayer ships
- **Brief**: **Pratfall** (Godot 4.6.1, C#, Facepunch.Steamworks → SteamNetworkingSockets), **Dome Keeper** (added co-op April 2026), **Cassette Beasts** (local co-op), and the **godot-tiny-mmo** open-source MMORPG demo all confirm Godot 4 multiplayer ships at small-and-mid-session scope. Independent stress shows ~40 CCU stability ceiling per server before tuning.
- **Mother Nature application**: Confirms private 2–10 is squarely in the proven envelope. Confirms public is engineering risk.
- **Priority**: nice-to-have

## Tier-by-tier architecture recommendation

**Solo**: Run the multiplayer code paths with no peer set, or `OfflineMultiplayerPeer`. Treat self as authority for everything. Same scenes as multiplayer so the code paths converge. Save to `user://` JSON.

**Private (2–10)**: Integrated host. Player who clicks "Host Game" runs `ENetMultiplayerPeer.create_server(port, 9)`. Host is peer 1 and authoritative. Pause world tick when `get_peers().is_empty()`. Optional: GodotSteam transport for friend-list joining without IP/port + Steam relay for NAT bypass. Save world file on disconnect/shutdown. No anti-cheat needed beyond sender validation; trust is social.

**Public (thousands)**: Headless dedicated-server export per shard, one shard per biome region. Postgres for accounts + per-shard SQLite or shared DB for world state. Auth server in front (HTTPS REST or gRPC, not Godot). Region router decides shard based on player position. AoI-culled replication mandatory. State-hash anti-cheat baseline. Plan for ~40–80 CCU per shard initially, scale horizontally. Budget $200–800/month for a 3-region launch fleet of small shards.

## Replication strategy verdict

- **Wildlife**: Server-authoritative spawn via `MultiplayerSpawner` with `spawn_function`. Position via `MultiplayerSynchronizer` (`REPLICATION_MODE_ALWAYS`, ~0.1s interval), AoI-culled with `set_visibility_for` driven by per-player Area2D AoI nodes. Behavior state (idle/hunting/fleeing) `REPLICATION_MODE_ON_CHANGE`. Health server-only, never replicated to clients (clients see hit reactions via authority RPC).
- **Weather grid**: Single source of truth on server. Each player synchronizer carries only their cell + 8 neighbors as ON_CHANGE properties. Updates pushed at ~1 Hz, not per-frame.
- **Body sim (hunger/thirst/temp/SAN)**: Per-player synchronizer with `public_visibility = false` and `set_visibility_for(self, true)` only. Nobody else needs your numbers and broadcasting them is a bandwidth-and-leak liability.
- **World state (placed structures, durable items, terrain edits)**: Server-authoritative spawn, ON_CHANGE replication for property mutations (door open, sign text), AoI-culled. Persisted to DB on every meaningful change.
- **Player input**: Client-authoritative `@rpc("any_peer", "unreliable_ordered", channel=2)` carrying intent (movement vector, action verb), validated and applied server-side. Never trust outcome reports.

## Must-include shortlist (8–10 things to internalize)

1. **Server is peer 1.** Always. `is_multiplayer_authority()` decides who can mutate.
2. **Clients send inputs, never outcomes.** Outcomes are server's job.
3. **Validate `get_remote_sender_id()` on every `any_peer` RPC.**
4. **Spawned node names must be unique and deterministic** (use peer ID or world UUID).
5. **Two synchronizers per player**: one server-auth (visibility, health, inventory), one peer-auth (input vector, look angle).
6. **`set_visibility_for` is your interest management.** Build AoI with Area2D + `body_entered`/`body_exited`.
7. **Use `unreliable_ordered` for streams** (movement, footsteps); `reliable` for state transitions (door, pickup).
8. **Headless export for dedicated server**, gate visual code with `OS.has_feature("dedicated_server")`.
9. **Save on every meaningful state change**, not just shutdown.
10. **No built-in CSP** — use Netfox if you need it, otherwise interpolate remotes by ~100ms and predict locals.

## Commonly oversold

- **Rollback netcode**: Survival pace doesn't need it. Netfox is impressive but adds simulation-determinism constraints (no `randf()` outside seeded RNG, no float drift, fixed timestep) that bleed into every system. For Mother Nature, server-auth + interpolation is enough.
- **Pure P2P / mesh**: Sounds free but NAT traversal, host migration, and the fact that one peer always becomes a de facto server make it strictly worse than picking a host explicitly.
- **Custom replication addons** before you've measured the built-in ones. `MultiplayerSynchronizer` with sensible intervals and AoI handles most workloads. Optimize when you measure pain.
- **Building your own MMO framework**. Use the "many small shards" model — that's how Valheim, Project Zomboid, Rust shipped. Seamless 10,000-player worlds in Godot are an engineering crusade.
- **Client-side anti-cheat**. Indie scope can't afford it and won't catch determined attackers anyway. Server-side validation and reporting catches the relevant 95%.
- **Building lobbies/matchmaking from scratch**. Use GodotSteam lobbies for Steam release; use Nakama or PlayFab for non-Steam. Lobby code is a bog.

## Cross-references

- Aligns with **system-architect-core Rule 2** (orchestrator owns invariants, generators are computation): server is the deterministic orchestrator; clients produce candidate inputs.
- Aligns with **Rule 6** (bimodal SLOs): per-player private session and public-world shard are categorically different operating regimes; one set of metrics will not serve both.
- Aligns with **security-architecture / lethal-trifecta** logic: AoI is identity-contraction at the network boundary — never send a peer state they shouldn't see.
- See `multi-agent-orchestration` skill for the "harness-as-aggregate" framing applied to dedicated server as game-state aggregate root.
- See `data-patterns` for outbox-pattern thinking on persistence (state mutation + DB write must be atomic; agent event log as outbox applies directly).

## Sources

- [Ziva — Godot Multiplayer in 2026: What Actually Works](https://ziva.sh/blogs/godot-multiplayer)
- [StraySpark — Multiplayer Networking in Godot 4: Building an Authoritative Server from Scratch](https://www.strayspark.studio/blog/godot-4-multiplayer-networking-authoritative-server)
- [Godot Engine — Multiplayer in Godot 4.0: ENet wrappers, WebRTC](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-3/)
- [Godot Engine — Multiplayer in Godot 4.0: RPC syntax, channels, ordering](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-2/)
- [Godot Engine — Multiplayer in Godot 4.0: Scene Replication](https://godotengine.org/article/multiplayer-in-godot-4-0-scene-replication/)
- [Godot docs — High-level multiplayer (stable)](https://docs.godotengine.org/en/stable/tutorials/networking/high_level_multiplayer.html)
- [Godot docs — ENetMultiplayerPeer](https://docs.godotengine.org/en/stable/classes/class_enetmultiplayerpeer.html)
- [Godot docs — WebSocketMultiplayerPeer](https://docs.godotengine.org/en/stable/classes/class_websocketmultiplayerpeer.html)
- [Godot docs — WebRTCMultiplayerPeer](https://docs.godotengine.org/en/stable/classes/class_webrtcmultiplayerpeer.html)
- [Godot docs — MultiplayerSpawner](https://docs.godotengine.org/en/stable/classes/class_multiplayerspawner.html)
- [Godot docs — MultiplayerSynchronizer](https://docs.godotengine.org/en/stable/classes/class_multiplayersynchronizer.html)
- [Godot docs — Exporting for dedicated servers](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_dedicated_servers.html)
- [Meshiest — Godot 4 Multiplayer Overview gist](https://gist.github.com/Meshiest/1274c6e2e68960a409698cf75326d4f6)
- [SomethingLikeGames — Interest Management with MultiplayerSynchronizer](https://www.somethinglikegames.de/en/blog/2023/interest-management-with-multiplayersynchronizer/)
- [SomethingLikeGames — Scene Replication with Godot 4(.1)](https://www.somethinglikegames.de/en/blog/2023/scene-replication/)
- [Blue Robot Guru — How to use RPCs in Godot](https://bluerobotguru.com/how-to-use-rpcs-in-godot/)
- [Godot Forum — Godot multiplayer anti-cheat](https://forum.godotengine.org/t/godot-multiplayer-anti-cheat/51607)
- [Godot Forum — Netfox addons for online multiplayer games](https://forum.godotengine.org/t/netfox-addons-for-online-multiplayer-games/36066)
- [Godot Proposals #3904 — Add interest management support to the high-level multiplayer API](https://github.com/godotengine/godot-proposals/issues/3904)
- [GitHub — godot-tiny-mmo (SlayHorizon)](https://github.com/SlayHorizon/godot-tiny-mmo)
- [GitHub — godot-monke-net (grazianobolla)](https://github.com/grazianobolla/godot-monke-net)
- [80.lv — Shipping Multiplayer In Godot 4: Pratfall Postmortem](https://80.lv/articles/indie-team-breaks-down-shipping-multiplayer-game-with-godot-4-6-using-c)
- [dev.to — Godot 3D MMO: Server, Auth, Sharding](https://dev.to/petervanderhook/godot-3d-mmo-server-and-network-infrastructure-authentication-and-security-gameserver-sharding-48kh)
- [Gabriel Gambetta — Client-Side Prediction and Server Reconciliation](https://www.gabrielgambetta.com/client-side-prediction-server-reconciliation.html)
- [easypc.io — Game server cost in 2024](https://www.easypc.io/game-hosts/cost/)
