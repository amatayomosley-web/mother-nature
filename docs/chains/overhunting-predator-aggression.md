# Chain: Over-hunting → Predator Aggression

## Time scale
~6-14 in-game days

## Sequence

1. Player kills deer at unsustainable rate in a [[drainage]]
2. [[population-model]] reduces deer density below local carrying capacity
3. Resident wolves and bears in [[wildlife-ai]] enter elevated [[hunger-state]]
4. Hunger state shifts utility weights in [[bear-ai]] and [[wolf-pack-ai]] toward predatory and territorial outputs
5. Encounter probability with player increases (predators range farther seeking food)
6. Predatory-attack weight rises specifically against isolated, vulnerable humans

## Player-readable signals (diegetic only)

The player learns this chain through observable in-world consequences. There is **no UI alert.** Signals include:

- Fewer or no deer tracks in habitual locations
- Snare lines empty for consecutive days
- Predator vocalizations heard closer to camp than before
- Predator tracks appear in places they don't normally range
- A bear that previously avoided the player approaches more closely
- Encounter behavior shifts — predators less avoidant, more investigatory

The [[compendium-observations]] log records what the player has personally seen, day by day. Reviewing those entries shows the time-series pattern — deer-sightings/day declining, predator-sightings/day rising. The player draws the inference themselves; the system does not announce or summarize.

## Counter-actions

- **Migrate to a fresh drainage** — reduces local pressure; non-trivial effort, see Run 01 Beat 17 for paradigmatic example
- **Switch effort to small game / fishing / foraging** — relieves deer pressure
- **Wait for population recovery** — seasons-scale, not days
- **Move away from the area entirely** — long-term solution if the player has burned through multiple drainages

## Why this chain matters

Per §1 No Dominant Strategy, hunting cannot be free of consequence. If a Hunter could repeatedly kill deer in one drainage forever, that becomes the dominant strategy. This chain ensures over-hunting has cascading mechanical consequences — not via abstract "scarcity scoring" displayed in UI, but via real predator behavior shifts that the player will feel.

This is a paradigmatic example of the design's commitment to "every system that produces variance must also produce signals the player can learn to read" (§2). The signals here are diegetic, not abstracted.

## Multiplayer dimension

On shared servers, one player's over-hunting can destabilize ecology for everyone in the region. This is a feature, not a bug — it creates negotiation pressure and shared consequence. Per the No Dominant Strategy pillar, this means a player who bow-hunts deer aggressively imposes externalities on cohabitants of their drainage. Those cohabitants experience the predator-aggression rise and may need to respond.

The compendium does not announce "another player overhunted this drainage." Players infer the cause from observation, just as they would infer from their own behavior.

## Cross-references

- §1 — No Dominant Strategy
- §2 — Variance vs Randomness
- §12.8 — Lotka-Volterra-with-K population dynamics
- §13.2 — Camp Stalkers (related but distinct chain — that one is about repeated successful camp scavenging, not population-pressure)

## Tags

#causal-chain #ecology #player-pressure #predation
