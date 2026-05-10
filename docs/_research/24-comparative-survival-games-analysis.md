# 24 — Comparative Survival Games Analysis

Date of analysis: 2026-05-10
Methodology: 38 research agents covering 15 reference games across 4 sequential batches, viewed through 4 playthrough cascade-points (Run 01 cougar predation, Run 02 cold-cascade, Run 03 sepsis, Run 04 rut-bull trampling).

## Methodology

Each game was analyzed by 5 agents (3 mechanics + 2 principles), with each agent covering 2 games for cross-game pattern recognition. Agents read all four playthrough files and the design document's principle sections (§1, §2, §14, §14.13, §15) before researching their assigned games via web sources (developer interviews, design talks, dev blogs, wikis).

**Lens distinction:**
- **Mechanics agents** identified specific implementation patterns from each game's design and mapped them to specific run-cascade-points where they could have helped.
- **Principles agents** identified design philosophies from developer articulations and evaluated them against MN's foundational pillars.

**Output discipline:** every finding tagged ADOPT / HOLD / REJECT with reasoning grounded in MN principles (§1 No Dominant Strategy, §2 Mechanical Behavior, §14 Skill is debuff-removal, §14.13 Skill-Modulated Saliency, Diegetic UI, Permadeath teaches).

## The 15 reference games

| Tier | Games | Coverage |
|---|---|---|
| **Tier 1 — Wilderness/survival analogs** | The Long Dark, Project Zomboid, Don't Starve, Green Hell | Closest design DNA to MN |
| **Tier 2 — Building/cooperative** | The Forest, Valheim, Vintage Story, RimWorld | Builder + group dynamics |
| **Tier 3 — Multiplayer dynamics** | 7 Days to Die, Rust, DayZ, Wurm Online | First-contact, jungle-rules, settlement |
| **Tier 4 — Narrative/edge** | This War of Mine, Frostpunk, Subnautica | Parable + diegetic UI gold standard |

---

## EXECUTIVE SUMMARY — KEY FINDINGS

The corpus produced 5 cross-cutting patterns, ~120 ADOPT-recommended mechanics, ~25 HOLD-with-translation, and ~30 REJECT-with-reasoning. The headline result: **MN's architecture is in the right design lineage** — TLD + PZ + DayZ + Subnautica validate the principles MN has committed to. **MN's distinctive bet** is generalizing §14.13 saliency across every domain (where TLD removes labels and PZ tiles forage-skill, MN extends shader-level saliency to weather, predators, plants, tracks, terrain — every domain).

**One genuine architectural threat survived adversarial review**: Tynan Sylvester's "Simulation Dream" critique (RimWorld) — that pure simulation produces dead time inaccessible to player understanding. MN's response (§14.13 saliency + §13 perception ladder + §14.12 Field Notes prospectively doing what the Storyteller does probabilistically) is **plausible but not yet empirically validated by player-pace sessions**. This is a playtest gate, not a design gate.

---

## CROSS-CUTTING PATTERNS

Patterns that appeared in 3+ agents independently. These are the most load-bearing imports.

### Pattern 1: State-ladders with observable thresholds

Found in: TLD afflictions, PZ moodles, Valheim wet→cold→freeze chain, DayZ exposure stages, Frostpunk Hope/Discontent ladders, Subnautica depth-pressure, Green Hell wound-state progression.

**Synthesis**: every survival game with successful cascade dynamics uses **discrete observable stages** rather than continuous opaque sliders. Stages give the player legible warning before terminal threshold; continuous sliders create death-from-nowhere.

**MN integration**: extends §14.13 saliency directly. Each body-meter and environmental hazard should have 3-4 readable stages with diegetic signals at each transition. Aligns with #34 hypothermia cascade chain note and #63 saliency degradation under conditions.

### Pattern 2: Recognition is a verb (active discovery, not passive identification)

Found in: Subnautica scanner, PZ examine-action, TLD plant-identification-is-a-skill, DST examine voicelines, Vintage Story knapping reveals tool affordances.

**Synthesis**: knowledge entries are produced by player action, not passively populated. The compendium fills because the player *did something* (scanned, examined, harvested, killed). This commits to the principle that the player learns the world through interaction with it.

**MN integration**: the §14 compendium spec should commit to recognition-as-verb. Walking past an entity logs nothing; scanning/examining/interacting logs an observation. Aligns with #27 compendium system note and #44 recognition channels.

### Pattern 3: Diegetic UI lives in held objects

Found in: Subnautica PDA, DayZ in-character map, Valheim in-game compass-from-rune, Green Hell smartwatch, TLD journal.

**Synthesis**: information UIs in successful diegetic games are **objects the character holds** — a notebook, a PDA, a compass. The interface is *worn*, not painted on the screen.

**MN integration**: aligns with §14 hybrid-C debuff visibility (back-room compendium) and #35 self-state visibility. The player's compendium should be in-fiction a journal/notebook the character carries.

### Pattern 4: Cascades have stages; each stage has counterplay

Found in: TLD hypothermia stages, PZ infection→disease→death, RW infection-vs-immunity dual-curve race, Frostpunk discontent escalation, Green Hell parasite progression.

**Synthesis**: well-designed cascades are *winnable* at every stage with the right action — but each stage costs more than the previous. The player isn't doomed at stage 1; they have escalating recovery requirements through stage 3.

**MN integration**: the Run 02 hypothermia cascade and Run 03 sepsis cascade should both have stage-counterplay specs. Aligns with #34 hypothermia chain, #64 medical treatment quality, #6 injury system.

### Pattern 5: Strategic silence as content (audio negative-space)

Found in: TLD strategic silence design, Subnautica silence between leviathan roars, DayZ environmental audio occlusion.

**Synthesis**: the absence of sound is itself a signal. Predators announce by silencing their region; players read tension by hearing what's *missing* from the soundscape. This is content carried through restraint.

**MN integration**: should be added to the audio design discipline for predator AI. Aligns with #8 wolf-pack-AI, #9 cougar-AI, #59 smoke as long-distance signal (audio analog).

---

## TOP ADOPT-RECOMMENDED MECHANICS

Ranked by frequency of independent agent recommendation. These are the most load-bearing imports across all 38 reports.

### 1. Body-part hit-zones with separate care states (Green Hell + RimWorld + DayZ)

Three agents independently recommended this for Run 03 sepsis cascade. Tova's leg wound progressed independently of her overall body state; current MN spec aggregates injuries into a single meter. Per-bodypart tracking lets the player see "left thigh: infected, fever rising" while right leg remains fine.

**Maps to**: #6 Injury system, #64 Medical treatment quality.

### 2. Stamina + carry-weight as continuous mobility tax (Valheim + DayZ + 7DtD)

Multiple agents flagged this for Run 02 (Ari's 24kg cracked-rib hike) and Run 03 (Tova's Day-20 evacuation). Heavy load + injury produces continuous slowdown + faster fatigue + accelerated caloric burn. Stamina ceiling tied to nutrition.

**Maps to**: #45 Long-distance travel + carry, #60 Mid-hike caloric debt.

### 3. Audio-cue predator identity (TLD + Subnautica + DayZ)

Each apex predator should have distinctive audio that announces presence at distance. Cougars purr/hiss before pounce. Bears huff. Wolves howl in coordinated patterns. The audio is the player's primary detection channel; vision is secondary.

**Maps to**: #8 Wolf-pack AI, #9 Cougar AI, #11 Vision cone (audio supplements visual).

### 4. Multi-stage infection with treatment-quality grading (PZ + RimWorld + Green Hell)

Run 03's sepsis should be a 3-4 stage progression: contamination → local infection → systemic spread → septic shock. Each stage has different treatment requirements. Treatment quality grade (Practiced first-aid = grade B) modulates cascade speed.

**Maps to**: #6 Injury system, #64 Medical treatment quality + antibiotics decision.

### 5. Wind direction visible via diegetic cues (Valheim particles + TLD smoke + PZ smoke)

Wind direction shouldn't require a HUD compass; it should be visible in drifting particles, smoke, leaf-rustle, breath-cloud direction. Hunter saliency makes it readable; novice saliency requires more deliberate observation.

**Maps to**: #25 Scent system (wind reads), §14.13 saliency rendering.

### 6. Voice proximity / in-fiction communication (DayZ + Rust)

For Run 02 first-contact and beyond, MP communication is in-fiction proximity. No global chat; players hear each other only when close enough. Body language + proximity audio + Forest Signs are the communication channels.

**Maps to**: #41 MP cooperation incentive, #58 First-contact encounter design.

### 7. Wet/cold separable sliders (Valheim + Don't Starve + The Forest)

Cold and wet are separate; wet amplifies cold dramatically. Wet clothing + ambient -10°C produces cascade much faster than dry clothing + -10°C. Player can manage by drying clothing before exposure to cold.

**Maps to**: #7 Clothing-state system, #34 Hypothermia cascade.

### 8. Reputation/familiarity weighting in apex AI (The Forest + RimWorld + DayZ)

The Forest's cannibal AI accumulates "this human has hurt us before" over encounters. Path C predatory pathway should similarly accumulate "this drainage = sometimes food" weights from player behavior. Per-individual identity (§12.8) is the substrate.

**Maps to**: #20 Camp Stalker evolution chain, #40 Apex predator economic intelligence.

### 9. Decay timer with upkeep tax (Rust + Wurm + Vintage Story)

Buildings + caches decay if not maintained. Forces ongoing presence vs casual abandonment. Aligns with §13.2 Camp Stalker pressure and #52 cached storage.

**Maps to**: #2 Shelter system, #51 Equipment durability + repair, #52 Multi-day cached storage.

### 10. Scout-team-as-discovery / Trip Reports (Frostpunk + DayZ + RimWorld)

In MP, sending scouts to assess distant areas produces a typed observation packet (per §14.3 quality grade). The Tracker who scouts brings back better info than the Forager.

**Maps to**: #41 MP cooperation incentive (group dynamics), #44 Recognition channels.

---

## TOP ADOPT-RECOMMENDED PRINCIPLES

### A. "The world doesn't care about the player" (Subnautica, TLD validate)

Subnautica's Cleveland: the wilderness is indifferent. TLD's "the cold doesn't care." Both validate MN's §1 + §2 commitments — the antagonist is environmental and mechanical, not narrative-arranged.

### B. "This is how you died" mortality framing (PZ)

Death's primary function is teaching. PZ's tagline embodies what §14.12 Field Notes is doing. Adopt the framing explicitly in onboarding.

### C. Strangers default to distrust (DayZ + Rust)

First-contact mechanics should NOT default to friendly. Per N32 #58, the fundamental design commitment is that strangers aren't safe. Cooperation must be earned through specific signals (compliance with disarm-walk, slow movement, hands-visible). MN's jungle-rules aligns.

### D. Permadeath as encounter-quality multiplier (DayZ)

Every encounter in DayZ matters because death is real. PZ + DayZ + RimWorld all use this. MN's permadeath + Field Notes inheritance is structurally similar.

### E. Diegetic-as-friction (Green Hell, Subnautica validate)

Information shouldn't be free; the player engages with the in-fiction world to acquire it. This validates §14.13 saliency commitment.

### F. Authored geography + procedural detail (Valheim + TLD)

MN's three-biome commitment with procedural variation within biomes is the right shape. Pure procgen (DS) loses learnability; pure handcraft (TLD's regions) loses replayability. Both games validate the hybrid.

---

## TOP REJECT-RECOMMENDED MECHANICS / PRINCIPLES

Each rejected by 2+ agents independently with cited reasoning.

### R1. Storyteller meta-agent (RimWorld) — CONFIRMED REJECT

Already §12.6 LEAN-REJECT. Sylvester's defense is articulate but the principle-collision with §2 is direct. **However**, the Sylvester "Simulation Dream" critique survives — MN owes empirical validation that pure mechanical-behavior produces enough player-readable variance. **This is a playtest gate, not a design gate.**

### R2. Sanity meter (Don't Starve) — CONFIRMED REJECT

Already §12.7 LEAN-skip. DS sanity spawns shadow-creatures from internal state — direct §2 violation. The mechanic is structurally incompatible with MN's realist lane. **Sanity meter rejected; saliency-degradation under fatigue/illness/cold (#63) covers the legitimate concern.**

### R3. Boss-tier biome gating (Valheim) — REJECT

Valheim's biome-progression-via-boss-kills creates a single canonical progression path. Violates §1 No Dominant Strategy. **Adopt the biome-as-tutorial-module insight; reject the gating mechanism.**

### R4. Scheduled wipe / blood-moon / horde-night (Rust + 7DtD) — REJECT

All three are Storyteller-shaped pacing dressed in different costumes. Violates §2 Mechanical Behavior. Reaffirms §12.6.

### R5. Probabilistic catastrophic 5xx-style failures — PARTIALLY REJECT

PZ's Knox 100%-fatal-from-bite (terminal countdown), TF's polaroid-camera-as-waypoint (externalized recognition), DS-style dice-roll events. All collapse §1 No Dominant Strategy by removing real counterplay.

### R6. HP-bar combat resolution (most genre titles) — REJECT

§15.3 hit-zone primacy is the structural alternative. Multiple agents validated this — Run 01 cougar and Run 04 moose proved the wound-mode model is doing real work an HP loop would erase.

### R7. End-screen / score-based mortality reflection (Frostpunk) — REJECT

Frostpunk's "but was it worth it?" Storyteller-shaped retrospection. Violates §2. Adopt the sober ethology of TWoM's chapter close instead.

### R8. Authored-character roster (TWoM) — HOLD WITH FRICTION

TWoM commits to specific named survivors with biographical depth. MN's silent-blank-character has a real cost (less apophenia hook). The compromise candidate is "starting-stamp" — minimal biography seed that informs starting-skill defaults but doesn't lock in personality. Worth playtest.

---

## PER-RUN CASCADE-POINT COVERAGE

Synthesizing across the 38 reports: which mechanics+principles address each run's failure mode.

### Run 01 — cougar predation (Day 19)

**Strongest fixes:**
- Audio-cue predator identity (TLD + Subnautica) — cougar should announce via sound at distance even when invisible
- Reputation/familiarity weighting (TF cannibal AI) — cougar's multi-day stalk should be readable as accumulated familiarity
- Saliency degradation under conditions (#63) — Ren's saliency was degraded by cough+isolation; she missed cues a healthy character would catch
- Audio occlusion + footstep substrate (DayZ) — Ren's reduced-perception action state would have benefited from audio cues
- Effigy/deterrent objects (TF) — non-combat predator deterrent options

### Run 02 — cold cascade (Day 20)

**Strongest fixes:**
- Wet/cold separable sliders (Valheim + DS + TF) — Ari's wet snow-soaked clothing accelerated her cascade
- Multi-stage hypothermia (TLD + Frostpunk) — explicit stages with stage-specific counterplay
- Voice proximity / first-contact protocol (DayZ + Rust) — Day 8 encounter mechanics
- Trip Report pattern (Frostpunk scout-team) — Ari's solo scout would have been a "trip report" with grade and cost
- Stamina ceiling × nutrition (Valheim) — caloric debt → stamina ceiling drop

### Run 03 — sepsis from injury (Day 21)

**Strongest fixes:**
- Body-part hit-zones (RW + GH + DayZ) — Tova's thigh injury localized
- Multi-stage infection with treatment grades (PZ + RW + GH) — wound → local infection → systemic → sepsis stages
- Wound-by-bodypart with separate care states (GH) — independent wound timers
- Decay timer + upkeep (Rust + Wurm + VS) — Tova's incomplete cabin
- Macronutrient hunger split (GH) — kills "meat-only winter" exploit; Forager paths viable

### Run 04 — rut bull trampling (Day 10)

**Strongest fixes:**
- Per-pawn personality + state utility AI (RW) — rut bull behavior emerges from state weights
- Spear-brace defensive verb (Valheim) — close-range alternative to bow against charge
- Climb-tree as escape (TF) — N44 mechanic
- Cover-physics-vs-charge (multiple) — what cover survives mass × velocity
- Yearly behavioral grid (VS) — rut as seasonal mode in animal AI

---

## NEW DESIGN QUESTIONS SURFACED

The corpus surfaced 5 architectural questions that didn't have explicit issues yet.

### Q1. Should §14.13 saliency degrade under temporary conditions?

(Issue #63 captures this; corpus strongly validates the extension. ~6 agents flagged it independently.)

### Q2. Should MN have a starting-character "stamp" instead of fully-blank or fully-authored?

TWoM's authored characters have apophenia value MN's blank characters lack. RimWorld's procedural backstory + traits is between the two. Worth a needs-decision issue.

### Q3. Should there be a Group Charter for cooperative MP camps?

Frostpunk's New Law system + Wurm's settlement deeds combined: when 4 players form a stable camp, they gain a typed "charter" — minimal social-contract artifact (resource-sharing rules, defensive obligations) without the Storyteller's narrative weight. Frostpunk + Subnautica agents (#35, #37) flagged.

### Q4. Should there be a Group Morale meter at cooperative-only level?

Distinct from §12.7 individual sanity (rejected). Group dynamics produce emergent mood-collapse pressures (TWoM characters going silent, refusing to scavenge). Aggregating at group level only — solo characters stay clean — preserves §12.7 LEAN-skip while adding the genuine MP dimension.

### Q5. Should there be year-end / season-end closing screens (TWoM chapter-close pattern)?

Not a Storyteller-arranged narrative — a sober tally: who survived, what was learned, what's coming. Honors §2 by reporting state, not arranging it. Worth considering as a permadeath ritual.

---

## SUBSTANTIVE REFINEMENT TO EXISTING DESIGN

### §14.13 Saliency — proposed extension

Add per-domain audio saliency parallel to visual. Tracker/Hunter Expert hears predator vocalizations more vividly (slight EQ boost on predator-class audio); novice hears generic forest-noise. Same world, same audio data, different mix.

### §15 Combat — proposed extension

Add **Spear-Brace defensive verb** for charge defense. Adapts Valheim's parry/brace stagger system. Spear-tip-grounded against charging large animal = stops the charge if mass < threshold; deflects if larger. Closes Run 04's bow-vs-rut-bull failure.

### §14.12 Field Notes — proposed writing discipline

Per Frostpunk's Fijak: "Cruelty undercuts weight." Field notes should describe mechanism without dwelling on suffering. Per TWoM's Miechowski: "ordinary people, extraordinary circumstances" — Field Notes are about the world, not just the death.

### §6 Body Meters — proposed structure

Per multiple agents: each meter should have 3-4 readable stages with diegetic transition signals. Hypothermia stages: shivering → confusion → lethargy → critical. Hunger stages: peckish → hungry → ravenous → starving. Each transition gets a felt cue.

---

## EMPIRICAL-VALIDATION GATES

Three claims in MN's design require playtest validation; the corpus could not confirm them theoretically.

### G1. Saliency-only resolves Sylvester's "Simulation Dream" critique

MN's bet is that §14.13 saliency + §13 perception ladder + §14.12 Field Notes do prospectively what RW's Storyteller does probabilistically. The corpus could not confirm this from theory alone. **Test in playtest with player-pace sessions** — does the player find enough variance + signal in a quiet hour, or does it feel dead?

### G2. Cooperative survival math is balanced (not crushingly hard)

Run 02's 4-person-camp resource arithmetic is grim. Several principles agents (#36 TWoM/Frostpunk, #41 MP cooperation) raised whether MN's cooperation incentive is strong enough vs the resource burn. **Validate in MP playtest.**

### G3. Apex individual identity produces enough player attachment

The bear-of-the-meadow effect requires players to mentally track individuals across encounters. Corpus says yes (DayZ + RW + The Forest validate); but the in-game experience is the test.

---

## MN'S DISTINCTIVE CONTRIBUTIONS

The corpus surfaced what MN does that NO reference game does:

### C1. Skill-Modulated Saliency at full-domain scope

PZ tiles forage-skill rendering. TLD removes labels. Subnautica scans. **None generalizes a shader-level saliency to every domain (predator, plant, weather, track, terrain, scat, sign).** This is MN's most distinctive bet.

### C2. Compendium per-account + Field Notes from death

MN's commitment that **knowledge persists across characters** is unique. PZ traits don't. DayZ characters don't. RW colonist memories don't. Subnautica's PDA persists per-save but not per-account. Mother Nature is the only game treating *learning the wilderness* as the meta-progression vector.

### C3. Multiple death signatures honored equally

Run 01 (predator-pounce), Run 02 (cold-cascade), Run 03 (injury-infection), Run 04 (prey-defensive-aggression) — four mechanically distinct deaths from one architecture. No reference game produces this signature variance from one ruleset; most have one canonical death (hypothermia, hunger, zombie bite).

### C4. 2D top-down + diegetic UI commitment

Subnautica has the diegetic gold standard but in 3D. PZ is 2D top-down but with HUD elements. **MN combines 2D top-down + diegetic UI + saliency rendering** as a unique formal package.

### C5. Multiplayer survival as cooperative + competitive + avoidant

Most MP survival sims default to one mode (Rust = PvP, Wurm = co-op). MN explicitly supports all three through jungle-rules + cooperation incentives + avoidance options.

---

## REFERENCES

All 38 agent reports stored at `staging/comparative-analysis/agent-NN-{lens}-{games}.md` for future reference. Sources consulted by agents include:

**Developer / design talks**: Tynan Sylvester ("Designing Games", "Simulation Dream"), Dean Hall (DayZ GDC 2013), Marta Fijak + Jakub Stokalski (Frostpunk postmortem), Pawel Miechowski (TWoM GDC), Raphael van Lierop (TLD Game Developer interviews), Charlie Cleveland (Subnautica), Tomislav Sebic (Vintage Story), various Facepunch (Rust) and The Indie Stone (PZ) blog posts.

**Wikis and references**: Project Zomboid Wiki, RimWorld Wiki, Subnautica Wiki, Don't Starve Wiki, The Long Dark, Valheim, Vintage Story documentation, Wurm Online Wurmpedia, Green Hell Wiki, This War of Mine, Frostpunk Wiki, 7 Days to Die official wiki.

**Academic / critical**: Melbourne University paper on DayZ permadeath; ACM article on meaningful distrust patterns; Habrador analysis of RimWorld commercial design; multiple critical reviews and design analyses.

---

## CONCLUSION

**MN's principles validate.** Across 38 agents' independent analysis of 15 reference games' mechanics and articulated design philosophies, the foundational pillars (§1 No Dominant Strategy, §2 Mechanical Behavior, §14 Skill is debuff-removal, §14.13 Skill-Modulated Saliency, Diegetic UI, Permadeath teaches) hold up.

**The architecture is distinctive but not novel-as-research.** MN is in the lineage of TLD + PZ + DayZ + Subnautica. Its bet is a new combination, not new mechanics — applying saliency systematically across every domain, treating learning-the-wilderness as the meta-progression spine, and unifying solo + cooperative + competitive multiplayer under one rule set.

**One real risk survives**: Sylvester's Simulation Dream critique. Pure mechanical-behavior may produce dead time inaccessible to player understanding. MN's response (saliency + perception ladder + Field Notes prospective discovery) is theoretically plausible but empirically untested. **This is the central playtest validation gate.**

**Most-recommended next moves from this corpus:**

1. Spec audio-saliency parallel to visual saliency (extends §14.13)
2. Spec body-part hit-zones with per-zone wound-state (extends §6 + §15)
3. Spec multi-stage hypothermia + sepsis cascades with stage-specific counterplay (closes #34 + #64)
4. Spec spear-brace defensive verb for close-range charge (extends §15 + #66)
5. Spec voice-proximity + Forest Signs as MP communication channels (extends §13.1 + #58)
6. Open new issue: "Group Charter for cooperative MP camps?" (Q3 above)
7. Open new issue: "Year-end closing screen (TWoM chapter pattern)?" (Q5 above)
8. Plan playtest gate for Sylvester's Simulation Dream critique (G1 above)

**The corpus is the design's stress-test result. The architecture survived. The remaining work is mostly content + spec, not architectural reversal.**
