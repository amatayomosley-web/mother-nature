# Bear AI

## Type
Wildlife agent — apex predator (engineering label; in-game text just says "bear")

## Summary

Bears are individual agents with persistent state, behavioral memory, and per-individual personality variance. Behavior emerges from a utility AI evaluating perception inputs against current internal state. They do not have story arcs; they have hunger, fear, learned associations, and family status.

Per the principle from 2026-05-09 conversation: the bear's behavior is purely mechanical. The bear came because scent reached it and it was hungry. Any "narrative" is the player's interpretation, not the system's state.

## Inputs (from)

External / perception inputs:
- [[scent-system]] — wind-borne meat scent triggers investigation
- [[wind-direction]] — affects scent detection range and direction
- [[ambient-light]] — affects visibility-based detection
- [[player-stance]] — running / walking / still / aggressive
- [[player-perceived-threat]] — unarmed / stick / bow-drawn / firearm-displayed
- [[approach-vector]] — closing toward bear / static / retreating

Internal state inputs:
- [[hunger-state]] — drives utility weights toward predatory + investigative
- [[behavioral-memory]] — past human-cache encounters bias future location-checks
- [[family-status]] — sow+cubs override most de-escalation
- [[territory-status]] — own kill site / established range / transient
- [[recent-encounter-outcomes]] — pain biases behavior toward fear-aggression

## Outputs (8-output state machine)

Bear evaluates inputs and selects ONE of 8 behaviors per AI tick:

| Output | Description |
|---|---|
| **ignore** | Continues current activity, does nothing |
| **avoid** | Repositions to increase distance |
| **investigate** | Approaches at low aggression to assess |
| **display warning** | Stands tall, woofs, jaw-pops — ritualized threat, not attack |
| **bluff charge** | Runs toward player and stops short, testing player response |
| **defensive attack** | Full aggression; bites/claws; retreats once threat ends |
| **predatory attack** | Full aggression with persistence; sees player as prey |
| **flee** | Retreats from perceived threat |

The output drives:
- [[player-body-meters]] — successful attack inflicts injury per §15
- [[carrion-chains]] — bear claiming a kill site activates the mechanic
- [[camp-stalkers]] — repeated successful camp-scavenging shifts behavioral memory
- [[compendium-observations]] — encounters log as plain observation entries
- [[scent-system]] — bear movement updates spatial scent emission

## Internal state per individual

- ID — system reference; never displayed in UI
- Spatial range — drainage(s) it patrols
- Hunger curve — continuous variable; decays/accumulates over time
- Behavioral memory — learned associations from past events
- Personality stats — aggression baseline, curiosity, fear (distributed at spawn)
- Recent encounter outcomes — hurt → fear-bias bumps
- Family/health status — sow with cubs / lone male / aging / wounded

## Five attack pathways

Distinct mechanical alignments that shift the utility calculation toward attack outputs:

### Path A — Sow defending cubs

- `family-status = sow_with_cubs`
- Player within ~30m of cubs (perceived proximity)
- Defensive-attack weight saturates; minimal de-escalation possible

**Player-readable signals:**
- Visible cubs nearby
- Sow standing tall, ears back
- Jaw popping, audible huffs

**Counter-actions:**
- Back away slowly, watching but not staring
- Give cubs maximum distance
- Running triggers pursuit
- Going closer triggers attack with near-certainty

### Path B — Defending kill site / cache

- `territory-status = own_kill_site` (per [[carrion-chains]])
- Player within ~50m of cache
- Defensive-attack weight high; bluff charge typically first

**Player-readable signals:**
- Bear at carcass or cache, possessive posture
- Ears back, advancing posture
- Display threats (woofs, jaw-pops)

**Counter-actions:**
- Yield the cache (this is what Run 01 day 2 demonstrated)
- Reclaiming triggers attack with high probability

### Path C — Predatory pathway (Camp Stalker evolved)

- `behavioral-memory.scavenged_food` accumulated multiple times
- `hunger-state = high`
- Player isolated, sleeping, or injured (`vulnerability` state high)
- Predatory-attack weight rises slowly over weeks if conditions repeat
- This is the [[camp-stalkers]] pathway crossing into lethal territory

**Player-readable signals:**
- Bear approaching at low body posture
- No display warnings (predatory bears don't bluff)
- Persistent, doesn't break off when player moves
- Behavior different from defensive — quiet, focused

**Counter-actions:**
- Aggressive deterrence: fire, loud noise, weapon display
- "Play dead" does NOT work for predatory bears — they want the meat
- Active defense or flight
- This pathway can be prevented earlier by securing food caches per [[camp-stalkers]]

### Path D — Surprise encounter

- Player + bear at <10m, both startled
- `personality` and `environment.cover_density` dominate the roll
- Defensive attack possible regardless of other factors — surprise override

**Counter-actions:**
- Don't run, don't approach
- Talk calmly (real-world advice survives in game — voice signals "human" to bear)
- Slow movement; let bear understand the encounter

### Path E — Wounded bear retaliation

- Player has shot bear; `pain-state` high
- Bear didn't drop (per §15: bow shots usually wound, don't drop)
- Bear's threat-perception of *this specific player* spikes
- Pursuit-attack possible across long distance

**Counter-actions:**
- Finish the kill, or survive the retaliation
- Running rarely works — wounded bears can run faster than humans
- This is the most dangerous bear-human interaction in real life

## Personality variance

Each spawned bear draws personality stats from species-typical distributions. Distribution shape: bell curve over `aggression-baseline`, `curiosity`, `fear-baseline`. Most bears land near typical mean; outliers exist in both directions.

This variance is what makes apex-individual identity (§12.8) load-bearing — encounters with different bears feel meaningfully different despite using the same AI framework.

## Player-facing surfacing

Per the diegetic-UI commitment (§12.8), the no-apex-dossier refinement (2026-05-09), and §14.13 Skill-Modulated Saliency:

The player perceives bears through:
- Visible body — size, markings, behavioral animation. Universal rendering: every player sees the same bear in the same place.
- Audible vocalizations — huff, woof, jaw-pop, growl, distinctive per output
- Behavioral output — the 8 outputs render as distinct animation/sound combinations
- **§14.13 saliency overlay** — for trained predator-readers, the bear's edges and posture cues render with subtle contrast/saturation boost. A novice and an Expert Hunter both see the bear; the Hunter's eye catches the bear and reads its posture more readily because of the saliency shader. No outlines, glows, or labels — just slightly more vivid pixels for the trained eye.
- [[compendium-observations]] — encounters log as plain observation entries; trained characters may note identifying features that suggest individual recognition

The player NEVER sees:
- "APEX INDIVIDUAL #N" or any system label
- A "dossier" framing
- Population stats or scarcity alerts
- Predicted attack probability or AI state visualization
- Floating text labels naming the bear or its state
- Outlines, glows, or highlight markers around the bear

## Behavior change over time (mechanical, not narrative)

A bear's behavioral memory updates after specific events:
- Successfully scavenged human food → `behavioral-memory.human-area-yields-food` weight rises
- Was hurt by human → `recent-encounter-outcomes.hurt-by-this-player-type` weight rises (broad bias against humans, with strongest weight against the specific player who caused the hurt if perception allowed identification)
- Successfully evaded human (no consequence) → no update
- Killed by human → individual removed; new bear may take territory eventually per [[apex-individual-identity]]

These updates are mechanical state changes. The player experiences them as the bear behaving differently next encounter. There is no "the bear is becoming a Camp Stalker" notification.

## Cross-references

- §12.8 — apex-individuals-capped-at-10 architectural commitment
- §13.2 — Carrion Chains, Camp Stalkers
- §15 — lethality model applied to bear strikes against player
- §1 — No Dominant Strategy: bow vs rifle vs flight all valid responses

## Tags

#wildlife #predator #ai-agent #apex
