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

- [`run-01-hunter-spring-forest.md`](run-01-hunter-spring-forest.md) — Hunter Ren, hiked-in, drainage 1 (forest), 19 days, ending in cougar attack day 19 in drainage 3
- [`run-02-survivalist-tundra-storm.md`](run-02-survivalist-tundra-storm.md) — Survivalist Ari, plane crash, tundra Cat-3 storm onset, 4 days, alive, survival arc continuing

## Adding new playthroughs

When you do a playthrough — solo design exercise, group brainstorming, or eventually playtest of an actual build — add a file here following the same template. Reference run-01 or run-02 as structural examples.

Naming: `run-<NN>-<character-archetype>-<biome-or-scenario>.md`
