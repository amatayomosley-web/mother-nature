# Playthroughs

Simulated runs that exercise the design's systems through specific characters in specific scenarios. Each playthrough is a written narrative annotated with which systems fired, what gaps surfaced, and how the architecture held up under play.

## Why playthroughs matter

The design's principle (§2 Mechanical Behavior, §1 No Dominant Strategy, §14 Skill System, etc.) only proves its value under actual play. Writing playthroughs forces the design's edges to surface: what's not yet specified, what conflicts unexpectedly, what's redundant.

Each playthrough generates:
- **Gap list** — system spec gaps surfaced (G-numbered, severity-tagged)
- **Need list** — player needs surfaced (N-numbered)
- **Architecture stress-test** — which committed systems held up; which need refinement
- **Death analysis** — when the run ends in death, the mechanical conditions that aligned

Playthroughs are referenced extensively across the open issues. When an issue says "Run 01 Beat 14 surfaced this," the corresponding section of the run file is linkable.

## Format

Per-run file structure:

- **Setup**: character, kit, scenario, starting compendium grants, skill node, goal
- **Beats**: numbered narrative segments, typically ~30-60 in-game minutes each, with system tags and gap callouts inline
- **Architecture stress-test results**: which systems fired and how
- **Gap / need lists**: enumerated and severity-ranked
- **Big takeaways**: design implications

## Currently in this folder

**Death-signature runs (01-04):**

- [`run-01-hunter-spring-forest.md`](run-01-hunter-spring-forest.md) — Hunter Ren, forest spring, **cougar predator-pounce death Day 19**
- [`run-02-survivalist-tundra-storm.md`](run-02-survivalist-tundra-storm.md) — Survivalist Ari, tundra Cat-3 storm, **cold-cascade arc**
- [`run-03-builder-tundra-autumn.md`](run-03-builder-tundra-autumn.md) — Builder Tova, tundra autumn, **sepsis injury-cascade death Day 21**
- [`run-04-hunter-fall-forest.md`](run-04-hunter-fall-forest.md) — Hunter Sam, fall forest, **moose rut-defense trampling death Day 10**

**Builder set (05-08):**

- [`run-05-builder-forest-spring.md`](run-05-builder-forest-spring.md) — Builder Maren, forest, 60-day permanent cabin; survives. Surfaces solo-Builder caloric coupling to MP economy.
- [`run-06-builder-tundra-summer.md`](run-06-builder-tundra-summer.md) — Builder Joren, tundra summer, sparse-timber adaptation; survives. Mason vs Carpenter sub-node distinction surfaced.
- [`run-07-builder-desert-fall.md`](run-07-builder-desert-fall.md) — Builder Selene, desert dry-wash, hybrid stone-adobe; survives. Water-as-construction-input identified as load-bearing desert constraint.
- [`run-08-builder-forest-autumn-rush.md`](run-08-builder-forest-autumn-rush.md) — Builder Kai, forest autumn cold-onset rush, triage shelter; survives Day 23 path-forked. Stress-modulated saliency need surfaced.

**Crafting set (09-12):**

- [`run-09-forester-crafting-bow.md`](run-09-forester-crafting-bow.md) — Forester Eira, forest summer, 30-day bow-making chain; 1-of-3 staves becomes deliverable. §14 multi-axis grading proof.
- [`run-10-trapper-crafting-trapline.md`](run-10-trapper-crafting-trapline.md) — Trapper Heath, forest autumn, trapline; **bear escalation death Day 27**. Scent-persistence as Carrion Chain generalization.
- [`run-11-forager-crafting-medicines.md`](run-11-forager-crafting-medicines.md) — Forager Bo, forest summer, 30-day medicine pantry; survives. Folk-medicine doctrine: delay magnitude, not cure probability.
- [`run-12-survivalist-crafting-fieldcraft.md`](run-12-survivalist-crafting-fieldcraft.md) — Survivalist Pell, forest fall, mobile knife-only fieldcraft; survives. Cross-skill repair tier confirmed.

**Farming exploratory (13-15, 17) — farming NOT currently in MN scope:**

- [`run-13-forager-farming-forest-garden.md`](run-13-forager-farming-forest-garden.md) — Forager Ren, forest, 90-day kitchen garden; survives. ~13% caloric intake from garden. Cultivator skill-node required.
- [`run-14-builder-farming-desert-irrigation.md`](run-14-builder-farming-desert-irrigation.md) — Builder Vasco, desert dry-wash, 60-day irrigation; infrastructure built, no harvest. Multi-season cycle implied.
- [`run-15-builder-farming-tundra-greenhouse.md`](run-15-builder-farming-tundra-greenhouse.md) — Builder Karuk, tundra summer, greenhouse-as-microclimate; survives. ~25% caloric intake (highest viable). Glass-as-salvage-only is the §1 dominance-preventer.
- [`run-17-multiplayer-settlement-farming.md`](run-17-multiplayer-settlement-farming.md) — Coop settlement Anika/Beren/Cale, forest 60d. Coop changes viability not yield; farming sensibly lives at coop scale but not gated.

**Alternative paths (16, 18):**

- [`run-16-forager-managed-wild.md`](run-16-forager-managed-wild.md) — Forager Iri, forest 90d, managed-wild stewardship of 18 known patches; survives. **Outperformed Run 13 farming on same biome.** Wild-Tender sub-node proposal.
- [`run-18-survivalist-wrong-skill-farming.md`](run-18-survivalist-wrong-skill-farming.md) — Survivalist Reni, forest 60d wrong-skill farming attempt; survives by Day 53 pivot. §14.9 confirmed by demonstration (backstory ≠ mechanical capability).

## Adding new playthroughs

When you do a playthrough — solo design exercise, group brainstorming, or eventually playtest of an actual build — add a file here following the same template. Reference run-01 or run-02 as structural examples.

Naming: `run-<NN>-<character-archetype>-<biome-or-scenario>.md`
