# Compendium / Knowledge-Progression Systems — Research Report

## Domain summary

A "compendium" in game design is an in-game knowledge base whose entries are filled by player action rather than purchased with experience or gold. It is the rare progression system that *survives permadeath* without violating its spirit: you lose the character but keep what *the player* learned, written down. For Mother Nature — a permadeath ecology survival sim where the meta-progression is explicitly *not* levels and skill points — the compendium is the spine of the game's long-term loop. Three properties matter most: tiered knowledge (entries deepen as the player spends time with the subject), append-only event sourcing under the hood (so an expert character's later observation can retroactively upgrade a novice's note), and physical-object UX (consulting the book is a survival decision, not a hotkey). All three are unusual; each has prior art, but no single game combines them.

The pattern that recurs across the eleven titles surveyed: **encounter unlocks the entry, sustained interaction promotes its tier, specialist character or item unlocks the top tier.** Wrong information at low tiers is rare — only Outer Wilds, NetHack, and Caves of Qud meaningfully do it — and where it appears, it works because the path to correction is visible.

Three meta-rules from the system-architect pack apply directly. **The schema is the primary architectural artifact** — the compendium-entry typed schema (entity_id, tier, observations[], confidence_flags) is the contract every save format and UI screen renders. **The orchestrator owns invariants** — character sessions emit observation events; the per-account compendium projector validates and commits. **Bimodal segmentation** — first-run UX (compendium empty) and veteran UX (compendium dense) are categorically different problems; the visual-readability layer exists precisely so first-run play does not depend on the empty book.

## Prior art

### Caves of Qud
- **Knowledge mechanic**: Artifacts spawn unidentified ("strange tubes"). Examination is an Intelligence check vs. the artifact's complexity; success identifies, partial success changes the displayed name to something more specific, failure of zero successes breaks the item.
- **Tier structure**: unidentified → partially identified (name refines) → identified. Tinker skill, psychometry mutation, and merchant inventories provide alternative paths; psychometry is guaranteed-success but consumes a turn and astral state.
- **What works**: Multiple paths to the same knowledge (use, examine, tinker, psychometry, price-id). Partial identification — the *renaming* of the item as you learn more — is a UX pattern Mother Nature should steal directly.
- **Mother Nature port**: Yes — partial-identification renaming is the cleanest signal that an entry is mid-tier without dumping a paragraph on the player.
- **Sources**: [Caves of Qud Wiki: Artifact examination](https://wiki.cavesofqud.com/wiki/Artifact_examination), [Unidentified artifacts](https://wiki.cavesofqud.com/wiki/Unidentified_artifacts), [Psychometry](https://wiki.cavesofqud.com/wiki/Psychometry).

### Subnautica
- **Knowledge mechanic**: PDA scanner. Scan a creature/plant/wreckage fragment; entry appears in databank; for tech fragments, scanning N fragments unlocks crafting recipe. Knowledge directly gates progression (you cannot build the Cyclops without scanning enough of its fragments).
- **Tier structure**: implicit two-tier — unscanned vs. scanned. No "expert" upgrade.
- **What works**: Knowledge-gates-crafting is the lethal version of soft-locked recipes. Mother Nature's specialist-writes-entries dynamic is the same mechanism with the gating moved from "fragment count" to "character class."
- **Mother Nature port**: Modified — adopt the *databank entry per encounter* but reject the binary unscanned/scanned tier in favor of Mother Nature's five-tier ladder.
- **Sources**: [Subnautica Wiki: Scanner (Subnautica)](https://subnautica.fandom.com/wiki/Scanner_(Subnautica)).

### Outer Wilds
- **Knowledge mechanic**: Ship log with two views. Map mode shows entries by location; rumor mode shows the graph of facts and their cross-references, color-coded by mystery. Entries unlock from observed evidence and from *rumors* (heard from other entries) which create dotted-line connections that resolve when the rumored evidence is found.
- **Tier structure**: rumor → confirmed → sub-entries explored. Implicitly four states: unmentioned, rumored, observed, fully explored.
- **What works**: The graph is the progression. Players "feel it's them who is improving, not their avatar" because the upgrade is in the player's head and on the journal page, not in a stat sheet. The journal *must be returned to the ship* to be consulted at depth — the closest prior art for Mother Nature's physical-book UX.
- **Mother Nature port**: Yes — the rumor/cross-reference structure (entries linking to other entries) and the in-world consultation cost are both directly applicable.
- **Sources**: [Outer Wilds Wiki: Computer](https://outerwilds.fandom.com/wiki/Computer), [Outer Wilds Ventures: Interactive Ship Log](https://outerwilds.ventures/).

### NetHack
- **Knowledge mechanic**: Scrolls and potions appear with random appearances per game ("scroll labeled FOOBIE BLETCH"). Identified by use, by scroll-of-identify, by price-id at shopkeepers, by altar/pet behavior, by quaffing fountains.
- **Tier structure**: unidentified → use-identified (auto-on-use), price-narrowed (constrains to a small set), formally identified.
- **What works**: Use-identification with consequences. Drink the unknown potion; learn what it was; sometimes that's lethal. Price-id is a beautiful "narrow but don't confirm" mechanic.
- **Mother Nature port**: Modified — NetHack randomizes appearance per game; Mother Nature's compendium is per-account, so randomization across runs would break it. Keep the *use-with-consequences* identification; drop the per-game appearance shuffle.
- **Sources**: [NetHack Wiki: Identification](https://nethackwiki.com/wiki/Identification).

### Dragon's Dogma (bestiary + pawn knowledge)
- **Knowledge mechanic**: Bestiary fills in three star-tiers as your pawns observe and kill enemies. Pawns with 3-star knowledge use the right elemental enchantments; pawns with 1-star do not.
- **Tier structure**: 0 stars (unknown) → 1 star (one tactic learned) → 2 stars (multiple tactics) → 3 stars (all tactics + kill threshold, can require ~500 kills for some creatures).
- **What works**: Knowledge has *behavioral* consequence — pawns act differently. The expert tier requires sustained, not one-off, interaction.
- **Mother Nature port**: Yes — the multi-tier system with kill/observation thresholds maps directly. The "pawn behavior changes" mechanic translates to: the compendium *suggests* better recipes/identifications when entries are at expert tier, soft-locking depth behind knowledge.
- **Sources**: [Dragon's Dogma Wiki: Bestiary](https://dragonsdogma.fandom.com/wiki/Bestiary).

### The Witcher 3
- **Knowledge mechanic**: Bestiary entries by encounter + reading specific lore books. Five minutes of post-fight body analysis surfaces weaknesses. Once entered, signs/oils/decoctions UI flags the right counter.
- **Tier structure**: unread → encountered (basic stats) → researched (weaknesses listed).
- **What works**: Books-as-knowledge-source. Reading a specific volume populates an entry without needing the encounter — useful when an entry's expert tier should be reachable through study, not only fieldwork.
- **Mother Nature port**: Yes for the books-grant-knowledge angle; Mother Nature can let scrolls/journals from dead foragers populate entries at novice tier instantly.

### Pokémon Pokédex
- **Knowledge mechanic**: Two-tier seen/caught. Seeing a Pokémon (in battle, in trade, in another trainer's roster) records *basic* info. Catching it records *detailed* info.
- **Tier structure**: unseen → seen → caught.
- **What works**: The simplest possible tier ladder, immediately legible to players. Completion has separate rewards per tier (regional Dex on seen, national on caught).
- **Mother Nature port**: Modified — adopt the principle of *cheap entry, expensive completion* but extend to five tiers, not two.

### Animal Crossing: New Horizons (Critterpedia)
- **Knowledge mechanic**: Catch a fish/bug/sea creature once → entry appears with seasonality, active hours, location, donation status.
- **What works**: Showing *post-hoc* metadata that wasn't visible at catch time. The player knew when and where they caught it, but the Critterpedia surfaces "this fish is only catchable June–August between 4pm and 9pm" only after first catch — turning the entry into a planning tool for subsequent runs/days.
- **Mother Nature port**: Yes — entries should surface phenology, biome, and activity windows the player did not directly observe.

### Slay the Spire (compendium)
- **Knowledge mechanic**: Cards/relics unlocked through play; compendium displays them. Cards unlocked-but-never-drawn show with art blurred and "unknown" labels until first encounter.
- **Tier structure**: locked → unlocked-unseen → seen.
- **What works**: The *blurred art* is the visual flag for "you know this exists but haven't experienced it." Mother Nature's low-confidence flag should look exactly like this.
- **Mother Nature port**: Yes — visual blur/opacity treatment for glimpsed-but-unconfirmed entries.

### Don't Starve Together (Compendium + Plant Registry + Cookbook)
- **Knowledge mechanic**: Plant Registry tracks researched farm plants; Cookbook records recipes the player has cooked, capped at the 6 most recent recent — older recipes are forgotten unless re-cooked. Recipes persist across all servers via online profile.
- **Tier structure**: untested → tested with one growth condition → fully researched (all conditions met).
- **What works**: *Per-account, cross-server recipe persistence* is the shipped version of the model Mother Nature is considering. The capped-at-6 recipe memory is a deliberate forgetting mechanic — interesting but probably wrong for Mother Nature, where the design doc rejects loss.
- **Mother Nature port**: Yes for cross-server account-level persistence; reject the cap.

### Return of the Obra Dinn
- **Knowledge mechanic**: Identify 60 crew members + their fates by deduction. The book never confirms a single guess — it locks in three at once when all three are correct.
- **Tier structure**: unfilled → guessed → committed (in trios).
- **What works**: The *anti-brute-force* gate. Mother Nature could use a similar pattern for high-stakes assertions: a novice's "red berries are poisonous" entry stays low-confidence until corroborated by a second observation from a different character/run.
- **Mother Nature port**: Modified — adopt the corroboration-required pattern for high-stakes claims (food safety, predator behavior) but not for low-stakes catalog entries.

### Project Zomboid (skill books + foraging tiers)
- **Knowledge mechanic**: Skill books grant XP multipliers per level band (Vol 1 = levels 0–1, Vol 5 = 8–9). Wilderness Knowledge trait + magazine unlock berry/mushroom poison detection — knowledge as a perceptual filter on the world.
- **What works**: Knowledge-as-filter — the *world looks different* when an entry is at expert tier (poisonous berries become visually flagged). Mother Nature should consider this for the visual-readability layer.
- **Mother Nature port**: Yes — expert tier on a plant entry should change in-world rendering for that character (subtle desaturation on poisonous berries, e.g.).

## Mother Nature compendium architecture recommendation

### Event log structure

Every observation by any character on any server appends a typed event:

```
ObservationEvent {
  event_id: UUID,
  account_id: UUID,
  character_id: UUID,
  server_id: UUID | null,
  entity_kind: "plant" | "animal" | "weather" | "terrain",
  entity_id: string,                 // stable across runs
  observation_type: "encounter" | "interact" | "consume" | "harvest" | "fail" | "specialist_study",
  observation_payload: dict,         // free-form per type
  character_class: string,           // forager | hunter | naturalist | …
  confidence_self: float,            // 0..1 — character's certainty
  timestamp: int,
  game_version: string
}
```

The log is **append-only**. Saves never edit past events; tier upgrades happen via projection rules. This is event sourcing in the canonical sense — see [Microsoft Learn: Event Sourcing Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing).

### Tier projection rules

A pure function `project(entity_id, events) → CompendiumEntry` runs over all events touching that entity:

- **Unknown**: 0 events.
- **Glimpsed**: ≥1 encounter event from any class.
- **Novice**: ≥3 distinct interactions, OR 1 specialist_study at any tier.
- **Practiced**: ≥10 distinct interactions including ≥1 successful interact + ≥1 failed interact (forces variety).
- **Expert**: ≥1 specialist_study from a class where this entity is in-domain (forager studies plants; hunter studies animals).

Class-domain matrix is data, not code. A hunter can promote a plant entry to Practiced via interactions but never to Expert — soft-lock that creates real specialist value across characters.

### Save schema (with versioning)

```
CompendiumSave {
  schema_version: int,               // bump on breaking change
  account_id: UUID,
  events: ObservationEvent[],        // append-only, capped by entity_id
  projections_cache: {                // optional snapshot
    cache_version: int,
    cache_built_from_event_id: UUID,
    entries: {entity_id: CompendiumEntry}
  }
}
```

Use Godot 4 Custom Resources for the typed entry shape, JSON for the event log itself (human-debuggable, easy to migrate). On version bump, run a migrator over events — never over projections; projections are derivable.

To prevent unbounded growth: cap events per entity at N (e.g., 50) by collapsing oldest events of the same `(observation_type, character_class)` pair into a count, preserving distinct observation types.

### Per-account vs per-server gating

Default: **per-account** for solo and friends-only. **Per-server** for public servers, with server admins able to flag the server "compendium-isolated" at creation.

### Wrong-information mechanic implementation

Each `CompendiumEntry` carries a `notes[]` array, where each note is `{author_character_id, author_class, claim, confidence, written_at, superseded_by?}`. Novice-tier writes claims that may be wrong (data-driven: each entity has a `mislearn_table` mapping observation patterns to wrong claims with probability). Expert-tier observations write *correcting* notes that mark earlier notes `superseded_by`. The UI shows the latest non-superseded note prominently and the superseded ones as a footnote: "Earlier, you wrote: Red berries are poisonous. *Corrected by Lyra (Forager, Expert): Some red berries are safe; the lacquered variety reflects light blue under dawn.*"

This avoids the unfair-feeling failure mode — wrong info is never *hidden*, only *initially-only*. The player always sees that someone wrote it down, and the upgrade feels like learning, not gotcha.

## In-world object UX

The book is a real item: occupies an inventory slot, costs ~3 seconds to open, can't be read in darkness without a light source, can't be read in rain. Reading it pauses time only at safe campsites; in the open, time continues — consultation is a survival decision.

The closest prior art is Outer Wilds' "must return to ship" log, which scoped consultation to safe-zones. NetHack's scroll-of-magic-mapping is the inverse — a one-shot consumable knowledge action — and shows the cost-of-information design works.

Cheap consultation (a glance at three lines: name + tier + top note) should be available via a quick-flip animation taking ~0.5 seconds. Deep consultation (full entry, footnotes, cross-references) requires the multi-second open-and-read. This bimodal cost matches the bimodal-segmentation meta-rule: the two consultation modes are categorically different and need different costs.

## Open question: per-account vs per-server

**Per-character** is rejected by the design doc: it would make permadeath erase the meta-progression that the compendium *is*.

**Per-account** is the strongest default. It matches how most modern survival games handle persistence (Don't Starve Together's recipe profile, Slay the Spire's unlocks). It enables the trade-economy fantasy where a veteran's expertise is real. Steam Cloud handles the sync trivially up to ~1 GB per game, and Mother Nature's event log even unbounded would be far below that.

**Per-server** is the right answer *only* on public multiplayer servers, and only when soft-pay-to-win matters. The design doc §10.1 is correct that veterans walking onto a public fresh server with full Expert entries trivializes the survival arc for everyone. Resolution: server-creation flag. Default for friend-and-family servers: account-shared. Default for public servers: server-isolated *and* account-isolated, so a veteran starts the public run with the same blank compendium as everyone else, and discoveries on that server stay on it.

This avoids the hard either/or: account-shared for the personal long-arc, server-isolated for the social arc, and the player chooses by choosing the server.

## Must-include shortlist

- Five-tier ladder (unknown / glimpsed / novice / practiced / expert) per the design doc.
- Append-only event log with pure projection function — non-negotiable; the audit trail is what makes wrong-info-correction feel fair.
- Specialist soft-lock on top tier — class-domain matrix as data.
- Wrong-information mechanic with superseded-note footnotes (per design doc §8.4).
- Visual blur/opacity treatment for low-confidence entries (Slay the Spire pattern).
- Partial-identification renaming (Caves of Qud pattern).
- Cross-reference graph between entries (Outer Wilds rumor mode).
- In-world physical book with rain/darkness/safety preconditions.
- Phenology surfacing post-encounter (Critterpedia pattern).
- Per-server isolation flag for public servers.

## Commonly oversold

- **Random per-game appearance shuffle** (NetHack model). Breaks per-account persistence; rejected.
- **Forgetting/cap mechanics** (Don't Starve cookbook 6-recipe cap). Players hate it; rejected.
- **Knowledge purchasable with currency**. Defeats the discovery-as-progression principle.
- **One-shot full identification on first encounter** (most bestiaries default here). Wastes the tier ladder.
- **Mandatory deduction puzzles** (Obra Dinn). Loved in a 60-entity puzzle game; would be exhausting across a survival game's hundreds of entities. Use only for high-stakes claims.
- **Anti-pay-to-win as a hard prohibition**. The fix is the per-server flag, not refusing to share knowledge across runs.

## Cross-references

- **system-architect-core meta-rule 1** (schema as primary artifact): the `ObservationEvent` and `CompendiumEntry` schemas are the contract. Define them before any UI.
- **meta-rule 2** (orchestrator owns invariants): per-account compendium projector is the orchestrator; characters emit events and never write entries directly.
- **meta-rule 4** (observable outcomes): tier promotion is grep-able from the event log. "Did Practiced fire?" is `count(distinct events) >= 10 AND has_success AND has_failure`.
- **meta-rule 6** (bimodal segmentation): first-run UX (empty book, world legible visually) and veteran UX (dense book, world re-readable through entries) are separate decision branches.
- **data-patterns sub-skill, three-gate ES adoption**: replay scenario (yes — expert character retroactively corrects novice's notes), cost model (event log capped per-entity is bounded), snapshot discipline (projection cache rebuildable from log). All three gates pass; ES is the right call here.

## Sources

- [Caves of Qud Wiki: Artifact examination](https://wiki.cavesofqud.com/wiki/Artifact_examination)
- [Caves of Qud Wiki: Unidentified artifacts](https://wiki.cavesofqud.com/wiki/Unidentified_artifacts)
- [Caves of Qud Wiki: Psychometry](https://wiki.cavesofqud.com/wiki/Psychometry)
- [Subnautica Wiki: Scanner (Subnautica)](https://subnautica.fandom.com/wiki/Scanner_(Subnautica))
- [Subnautica Wiki: Fragments](https://subnautica.fandom.com/wiki/Fragments_(Subnautica))
- [Outer Wilds Wiki: Computer](https://outerwilds.fandom.com/wiki/Computer)
- [Outer Wilds Ventures: Interactive Ship Log](https://outerwilds.ventures/)
- [Game Developer: Outer Wilds critical analysis](https://www.gamedeveloper.com/design/outer-wilds-critical-analysis)
- [NetHack Wiki: Identification](https://nethackwiki.com/wiki/Identification)
- [NetHack Wiki: Price identification](https://nethackwiki.com/wiki/Price_identification)
- [Dragon's Dogma Wiki: Bestiary](https://dragonsdogma.fandom.com/wiki/Bestiary)
- [Witcher Wiki: The Witcher 3 bestiary](https://witcher.fandom.com/wiki/The_Witcher_3_bestiary)
- [Bulbapedia: Pokédex](https://bulbapedia.bulbagarden.net/wiki/Pok%C3%A9dex)
- [Nookipedia: Critterpedia](https://nookipedia.com/wiki/Critterpedia)
- [Don't Starve Wiki: Compendium](https://dontstarve.fandom.com/wiki/Compendium)
- [Don't Starve Wiki: Cookbook](https://dontstarve.fandom.com/wiki/Cookbook)
- [Wikipedia: Return of the Obra Dinn](https://en.wikipedia.org/wiki/Return_of_the_Obra_Dinn)
- [Frostilyte: A Look at Deduction in Return of the Obra Dinn](https://frostilyte.ca/2019/05/16/a-look-at-deduction-in-return-of-the-obra-dinn/)
- [PZwiki: Skill book](https://pzwiki.net/wiki/Skill_book)
- [PZwiki: Foraging](https://pzwiki.net/wiki/Foraging)
- [Microsoft Learn: Event Sourcing Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
- [Kurrent.io: Introduction to Event Sourcing](https://www.kurrent.io/event-sourcing)
- [GDQuest: Saving and Loading Games in Godot 4](https://www.gdquest.com/library/save_game_godot4/)
- [Godot Forum: How to load and save things with Godot](https://forum.godotengine.org/t/how-to-load-and-save-things-with-godot-a-complete-tutorial-about-serialization/44515)
- [neth392/godot-improved-json: JSON Serialization with property injection](https://github.com/neth392/godot-improved-json)
- [Notes Hamatti: Meta progression with gradual tutorial in roguelike games](https://notes.hamatti.org/gaming/video-games/meta-progression-with-gradual-tutorial-in-roguelike-games)
- [Grid Sage Games: Designing for Mastery in Roguelikes](https://www.gridsagegames.com/blog/2025/08/designing-for-mastery-in-roguelikes-w-roguelike-radio/)
