# Persistent Shared World Server Engineering — Research Report

## Domain summary

Public-tier persistent shared worlds are the architecturally hardest tier of any survival multiplayer game. They differ from session-based multiplayer in three load-bearing ways: (1) world time runs whether or not players observe it, so off-screen simulation cost is real; (2) state must survive disconnects, restarts, and crashes with bounded data loss; (3) bad actors persist between sessions, so moderation is a permanent organ, not a feature. The reference designs for indie scale are Rust, Conan Exiles, DayZ, Valheim, and Project Zomboid; the reference design for "single shared continent" is EVE Online, which has spent 20+ years engineering around the single-shard tax. Mother Nature should plant itself between Conan Exiles and DayZ in the design space — persistent world, ecological state, dozens-to-low-hundreds per shard — and resist the "thousands per shard" framing in the design doc as a v1 commitment.

## Topic catalog

### Continuous-clock semantics
- Brief: World time advances on the server tick, independent of player presence. Animals breed, weather moves, plants grow, fires burn out, tracks decay, camps weather.
- Why: Without this, the world feels "instanced" — players notice when something only happens once they arrive.
- Mother Nature application: Three update cadences are needed. Fast tick (5-10 Hz) for online-player AOI (area of interest) — animals players can see, weather effects rendered nearby. Slow tick (1 Hz or less) for regional ecology — population counts, prey/predator balance, plant maturation. Cron tick (per-minute or per-hour) for camp decay, food spoilage, season advancement. The trick is that slow-tick state is what "feels" persistent; fast-tick state is what feels alive. DayZ's Central Loot Economy is the canonical reference: per-class spawn caps, per-class lifetime, init/load/respawn/save flags drive simulation without per-frame cost.
- Novice failure: Running everything at the player tick rate. CPU explodes the first time a server holds 10,000 entities.
- Priority: P0
- Sources: DayZ CLE, Bohemia community wiki

### Regional clock alignment
- Brief: Server in-world time is offset so that "prime time" in the game (active dawn/dusk hunts, daylight crafting) lines up with the regional player base's evening hours.
- Why: If the European server hits in-game winter night at 3 AM Berlin time, EU players play through summer at noon and never see the high-tension content. Player retention craters.
- Mother Nature application: One clock-offset per regional shard (EU = UTC+1 anchor, NA-East = UTC-5, NA-West = UTC-8, etc.). Decouple in-game time-of-day from server-node UTC. Cross-region migration: allow account-level (Compendium, custom characters, challenge progress carries — design doc §10.1) but do *not* allow cross-region character body migration without a long cooldown. Otherwise players follow the easiest server and the regional design fails.
- Novice failure: Tying world time to system clock and never making the offset configurable.
- Priority: P0

### Persistent player camps
- Brief: Camps survive offline. Decay is a function of (last-interaction-time, player-presence-in-region). Storage chests retain inventory. Fires burn out on a real-time clock, regardless of player presence.
- Why: This is the load-bearing economic anchor — investment players make in the world. If camps vanish on disconnect, no one builds. If camps never decay, the world fills with abandoned junk and resource-locks regions forever.
- Mother Nature application: Use the DayZ pattern adapted: every camp object has a `last_interaction_timestamp`; lifetime resets on touch. Default lifetime tiers — bedroll: 7 days, fire: real-time burn until fuel exhausted, smokehouse: 14 days, storage chest: 21 days. Decay accelerates if region has heavy traffic ("hunted out" pressure). This converts the design doc's ecological promise into a concrete state machine — popular regions clean themselves; remote regions hold camps longer.
- Novice failure: Decay timer that pauses while player is online but doesn't account for region traffic.
- Priority: P0
- Sources: DayZ CLE wiki, Conan Exiles decay system

### Sharding vs single-world
- Brief: Single-world (Rust per-server, Conan per-server) caps at the per-process simulation budget — typically 100-300 players. Sharded (EVE) splits the world by solar system across nodes. Layered (WoW) splits the same nominal world by player groups for crowd control.
- Why: Real-time simulation of hundreds of mobile entities + thousands of static entities is a single-process problem. You don't fix it with a bigger box; you fix it by splitting domain.
- Mother Nature application: Reject "thousands per continent" as a v1 commitment. Adopt the Conan/Rust pattern: one continent = one server process = 64-128 player cap. Multiple shard instances (NA-East-1, NA-East-2, …) for overflow. Forking when population caps: spawn shard N+1 at 80% saturation of shard N. Cross-shard discoverability is a separate problem — don't solve it in v1.
- Novice failure: Building toward "single shard" because EVE does it. CCP has 350 engineering-years invested. You don't.
- Priority: P0

### Persistence on disk
- Brief: Save scope = world tile state, all camps, weather grid, regional wildlife populations + apex individuals, persistent tracks/signs, corpses, world-dropped crafted items. Save format = single SQLite database (Conan model) or LevelDB/RocksDB; full snapshots rare, incremental writes constant.
- Why: Survival of crashes with bounded data loss. The save IS the world.
- Mother Nature application: SQLite or RocksDB single-file. Conan Exiles uses one `game.db` per server, autosaves every 5 minutes, rotating `game_backup_##.db`. For Mother Nature, expect a 64-player server to hold maybe 500 MB-2 GB after months of play. Apex animals + named individuals = small row count, high churn. Aggregate populations = small row count, slow churn. Player camp objects = the dominant table by far. Save discipline: WAL-mode SQLite, fsync on transaction commit, hourly backups rotated 24-deep, daily backups rotated 30-deep, separate offsite backup of the daily. Crash recovery picks the newest good backup, not the oldest.
- Novice failure: Stopping the world tick during the save. Use SQLite WAL mode + read snapshot to write while the tick continues.
- Priority: P0

### Save-state-on-disconnect
- Brief: When a player disconnects, where does their character body go? Three patterns: (1) sleep-vulnerable (Rust) — body remains in-world, lootable; (2) sleeping-bag tether (DayZ-adjacent) — body persists but tied to bed/bag; (3) vanish-on-logout (Valheim, Conan PvE) — body removed from world while logged out.
- Why: Determines whether offline grief is a feature or a bug. Rust treats it as a feature; Valheim treats it as a bug. Mother Nature design doc is silent — flag this as an open design question.
- Mother Nature application: Recommendation — sleeping-bag tether with safe/unsafe regions. In safe regions (own camp, claimed territory), logout vanishes the body. In wilderness, body sleeps for 5-10 minutes (during which loggers can be punished) and then despawns to a tracked "sleep state" that the player rejoins on next login. This balances anti-combat-log without permanent vulnerability while AFK.
- Novice failure: Picking Rust's full-vulnerability mode for an audience that includes solo players who can't predict raid windows.
- Priority: P0 (design decision, must be made before coding)

### Cross-restart state
- Brief: What survives a server restart vs. doesn't. Player camps, characters, ecology, season, weather: yes. In-flight projectiles, current animal AI state, partial crafting interactions: typically reset.
- Why: Scoping the save means scoping the recovery. Things you don't save reset to a defined initial state.
- Mother Nature application: Daily server reboot (low traffic hour, 5-minute downtime) for memory hygiene. State-preserving — full save before reboot, animals respawn on initialized AI, weather continues from saved-state. Wipes: never on public Mother Nature servers (unlike Rust monthly wipe, which is a PvP-balance device irrelevant here). Maintenance windows announced 24h ahead.
- Novice failure: No restart cadence; servers die from leaks at hour 200 of uptime.
- Priority: P1

### Hosting economics
- Brief: VPS for survival servers ranges $5-50/month at indie scales (Project Zomboid 32-player) to $250+/month for dedicated hardware (100+ player ARK-class). Below ~10 engineers, official-server hosting is a recurring cost line; above that, community-hosted dominates.
- Why: This is the line item that decides whether public servers are an Iron-Crown-funded feature or a community feature.
- Mother Nature application:
  - 50-player shard: $20-40/month (managed VPS, 6-8 GB RAM)
  - 100-player shard: $40-100/month (8-12 GB RAM)
  - 500-player shard: $200-500/month (likely needs dedicated CPU, not viable in single-process simulation — would need internal sharding)
  - 1000-player shard: $1k-3k/month, requires custom architecture work, unlikely to fit indie scope
  - 10,000+: $10k+/month per cluster, EVE-class problem
- Funding: Recommend Rust/Conan model — official Iron Crown servers (small fleet, marketing flagship, paid out of unit sales) + community-hosted via standard hosting partners (G-Portal, Nitrado, BisectHosting). Project Zomboid Patreon-funded community model is also viable but secondary.
- Novice failure: Promising free official servers for thousands of CCU. Bandwidth alone bankrupts you.
- Priority: P0

### Anti-grief and moderation
- Brief: Without mechanical alignment, all moderation is operational. Tools needed: in-game `/report`, vote-kick (with anti-spam), admin commands (kick/ban/teleport/inspect), account-level ban list synced across official servers, structured server logs.
- Why: Day-1 community cohesion. Bad actors will arrive in week one.
- Mother Nature application: Tier the response — in-world soft-reputation (NPC reactions, region-traders refuse service after enough player reports), vote-kick (per-shard, requires N players), admin tools (Iron Crown moderator team for official; ServerOwner role for community), account ban (rare, severe). Discord integration for ticketing. Project Zomboid's pattern of "admin = trusted player" works at small scale; at hundreds-of-shards scale, you need paid moderators.
- Novice failure: Pretending mechanical-alignment commitment removes the need for moderation. It increases it.
- Priority: P0

### Cross-tier state portability
- Brief: Compendium, custom characters, challenge progress carry across solo → private → public per design doc §10.1. Body state (inventory, camp position) does not.
- Why: This is the account-level data plane — separate database from per-shard world state, owned by Iron Crown backend, queried by all servers including community-hosted with auth.
- Mother Nature application: Two-database architecture. Account DB (Iron Crown cloud, central) holds Compendium / characters / challenges / cosmetics / friends / achievements. World DB (per-shard SQLite, on the host machine) holds camps / wildlife / weather / world objects. Sync model: client signs in, fetches account state, joins shard; on relevant events, server pushes account-state delta to Iron Crown backend. Steam Cloud is for solo only. ARK's CrossARK pattern is the closest reference.
- Novice failure: Putting account state in the same DB as world state. Cross-shard portability becomes impossible.
- Priority: P0

### Real-world reference servers
- Brief: Survey of observed shard sizes across the survival/MMO space.
- Mother Nature application:
  - **Project Zomboid**: 32 players, dedicated server, persistent map, mod-driven moderation, $5-20/month — closest scale match for 50-player Mother Nature shards
  - **Rust**: ~200 players per server, monthly wipes, PvP-driven persistence — wipe model irrelevant to Mother Nature
  - **Conan Exiles**: 40-70 players per server, SQLite save, daily autosave cadence — closest architectural match for save model
  - **DayZ**: ~60 players, Central Loot Economy — closest match for ecological persistence pattern
  - **Valheim**: 10 hard cap, 20-min autosave — too small, but cleanest single-host pattern
  - **EVE Tranquility**: 30,000 CCU single shard, 30+ standard nodes, 6 reinforced nodes — aspirational only
- Priority: P1

### Indie scope honest call
- Brief: Thousands of CCU on a single persistent world is genuinely hard. Indie launches that promised it (Worlds Adrift, Mavericks, Crowfall) failed. Indie launches that delivered scoped persistent worlds (Valheim, Conan, Project Zomboid) succeeded.
- Mother Nature application: **Launch with 64-player shards, official + community-hostable.** Plan for 128-player shards in first major patch. Defer "1000-player single continent" to v2.0 or never. The design doc's "thousands of players" framing is marketing language; deliver "many simultaneous shards full of dozens of players each" instead.
- Priority: P0 — this is the decision that makes the rest of the report load-bearing.

## Recommended Mother Nature public-world architecture

- **Starting concurrent player target**: 64 per shard, with tested headroom to 128
- **Sharding decision**: Per-shard single-world (Conan/Rust pattern), multiple shards per region, fork at 80% saturation
- **Persistence model**: SQLite WAL-mode single-file `world.db` per shard. Hourly rotated backups (24-deep), daily backups (30-deep), weekly offsite archive. Account-level data lives in separate central Iron Crown DB.
- **Hosting model**: Official Iron Crown servers (paid from unit sales) for the flagship 6-12 shards across regions; community hosting as primary scaling lever via G-Portal/Nitrado/BisectHosting partnerships.
- **Tick cadence**: 10 Hz online-AOI tick, 1 Hz regional ecology tick, 1/min camp decay tick, 1/hour weather/season cron
- **Save-state-on-disconnect**: sleeping-bag tether — wilderness body sleeps 5-10 min then despawns, safe-zone body vanishes immediately
- **Restart cadence**: daily 5-min low-traffic-hour restart, state-preserving, no wipes ever
- **Regional clock**: in-world time offset per region so prime-time content aligns with regional evening
- **Moderation**: tiered (in-world soft-reputation, vote-kick, admin tools, account ban), Discord-integrated, paid Iron Crown moderators for official shards

## Cost model for public server (per shard, monthly)

- 50 CCU: $30 (managed VPS, 8 GB RAM, 4 vCPU)
- 100 CCU: $80 (managed VPS, 12 GB RAM, 6 vCPU)
- 500 CCU: $400 (dedicated CPU, requires proven scaling work; not v1)
- 1000 CCU: $1,500-3,000 (custom architecture, beyond indie scope without a partner)
- 5000 CCU: $10,000+ per cluster (sharded internally; EVE-class)

For a launch fleet of 12 official shards at 100-CCU class: ~$1,000/month. A reasonable indie operating budget.

## What's deferrable post-launch

- Cross-region character body migration (account state portability ships at launch; body migration is v1.2)
- 128-player shard support (target for v1.1 patch after stabilizing 64-player)
- In-game soft-reputation system (ship vote-kick + admin tools at launch; soft-reputation is v1.3)
- Mod tooling for community servers (v1.2)
- Cross-shard discovery (browse who's on which shard from main menu) — v1.2
- Sharded internal architecture for 500+ CCU shards — v2.0 or never

## Must-include shortlist (P0 at launch)

1. Per-shard SQLite save with WAL mode and rolling backups
2. Account-level central DB for Compendium / characters / challenges, separate from world DB
3. Tiered tick rates (player AOI / regional ecology / cron)
4. Camp decay with last-interaction-timestamp + region-traffic pressure
5. Sleeping-bag tether logout model (decision committed)
6. Regional in-world clock offset
7. Vote-kick + admin-command + account-ban moderation stack
8. Daily state-preserving restart, no wipes
9. Official + community-hostable parity on day one
10. 64-player starting cap, advertised honestly

## Commonly oversold

- "Thousands of players in a single shard" — promises EVE-class engineering on indie budget; reframe as "thousands of concurrent players across our shards"
- "Truly persistent ecology that remembers everything" — DayZ-CLE style aggregate persistence is achievable; per-individual-animal lifetime memory is not
- "Free official servers for everyone" — bandwidth math kills this; community-hostable is the leverage
- "Player camps that never decay" — fills the world with abandoned junk; some decay is necessary even in PvE design
- "No moderation needed because no alignment system" — the opposite is true

## Cross-references

- Maps to `system-architect-core` Rule 1 (schema as primary artifact) — the SQLite save schema is the architectural commitment
- Maps to Rule 2 (orchestrator owns invariants) — the server tick is the only authority on world state; clients render; mods extend
- Maps to Rule 3 (classify before tactic) — sharding-vs-single-world classified by per-process simulation cap, not by aspirational CCU
- Maps to Rule 6 (bimodal segmentation) — fast-tick AOI and slow-tick ecology have separate budgets and separate SLOs
- Connects to design doc §9.1 (multiplayer tiers), §10.1 (Compendium portability)
- Connects to upstream curriculum nodes on ecological persistence and weather simulation

## Sources

- [Rust Wipe Schedule and Server Architecture - GINX TV](https://www.ginx.tv/en/rust/wipe-schedule-console-pc)
- [Rust Sleeping Bag Wiki - Fandom](https://rust.fandom.com/wiki/Sleeping_Bag)
- [Project Zomboid Server Hosting - Indifferent Broccoli](https://indifferentbroccoli.com/project-zomboid-server-hosting)
- [Conan Exiles Save Location - LOW.MS](https://low.ms/knowledgebase/conan-exiles-save-location-how-to-upload)
- [Decoding the Game.db - Nodecraft](https://nodecraft.com/support/games/conan-exiles/decoding-the-game-db)
- [Valheim Dedicated Servers - Fandom Wiki](https://valheim.fandom.com/wiki/Dedicated_servers)
- [DayZ Central Economy Configuration - Bohemia Community Wiki](https://community.bistudio.com/wiki/DayZ:Central_Economy_Configuration)
- [DayZ Central Loot Economy - Fandom Wiki](https://dayz.fandom.com/wiki/Central_Loot_Economy)
- [EVE Online Tranquility Server - PC Gamer](https://www.pcgamer.com/eve-online-1/)
- [EVE Online Time Dilation Introduction - CCP](https://www.eveonline.com/news/view/introducing-time-dilation-tidi)
- [7 Ways EVE Online Scales - High Scalability](https://highscalability.com/7-sensible-and-1-really-surprising-way-eve-online-scales-to/)
- [Infinite Space Single-Sharded MMO Argument - Game Developer](https://www.gamedeveloper.com/design/infinite-space-an-argument-for-single-sharded-architecture-in-mmos)
- [How Much Does a Game Server Cost in 2025 - EasyPC](https://www.easypc.io/game-hosts/cost/)
- [Photon CCU Pricing Model - Photon Engine Blog](https://blog.photonengine.com/photon-pricing-explained/)
- [MMORPG Data Storage Part One - Plant Based Games](https://plantbasedgames.io/blog/posts/01-mmorpg-data-storage-part-one/)
- [CrossARK Transfers - ARK Wiki](https://ark.fandom.com/wiki/CrossARK_Transfers)
- [Edgegap MMO Fleet Manager Architecture](https://edgegap.com/blog/how-mmo-games-architecture-scales-with-a-smart-fleet-manager)
- [Game Server Tick Rate - Edgegap](https://edgegap.com/blog/game-server-tick-rate-explained-gameplay-precision-vs-infrastructure-cost)
- [Netcode Wikipedia](https://en.wikipedia.org/wiki/Netcode)
- [Importance of Backup Systems for Game Servers - GGServers](https://ggservers.com/blog/the-importance-of-backup-systems-for-game-servers/)
- [Game Server Moderation Best Practices - GPORTAL Wiki](https://www.g-portal.com/wiki/en/game-server-moderation-and-best-practices-for-game-server-admins/)
