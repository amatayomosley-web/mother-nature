# Contributing to Mother Nature

Thanks for your interest. Mother Nature is being designed in the open during its design phase. This document describes what kinds of contributions are welcome, how to make them, and what the conventions are.

## Project status

The project is in **active design phase**. There is no playable build yet. Most of the work right now is articulating, refining, and stress-testing design ideas — primarily through written documentation and through "playthrough scripting" (simulated runs that surface system gaps and design needs).

When implementation begins, the contribution scope will expand to include code. For now, contributions are documentation, design discussion, system specs, and causal chain analyses.

## Foundational principles

Before contributing, please read:

- [`README.md`](README.md) — project overview
- [`docs/design/survival-design.md`](docs/design/survival-design.md) §1 ("The Pitch", "No Dominant Strategy") and §2 ("Variance vs Randomness", "Mechanical Behavior, Not Scripted Narrative")

These principles govern every design decision and are non-negotiable. Contributions that violate them will be redirected. Specifically:

1. **Nature is the antagonist** — no design suggestions where human enemies replace this loop
2. **No dominant strategy** — every tool, class, or playstyle must have a specific cost
3. **Variance, not randomness** — outcomes vary; signals are readable
4. **Mechanical behavior, not scripted narrative** — agents act from their own state; no Storyteller-style narrative pacing
5. **Diegetic UI** — no popups, no floating labels, no overlays
6. **Skill is debuff-removal** — mastery is absence of friction, not superhuman ability

If your contribution touches one of these principles, expect discussion before merge.

## What contributions are welcome

### In scope (now)

- **Issues for design discussion** — questions, "have you considered X," critiques, alternate-universe what-ifs
- **Issues reporting documentation problems** — typos, broken links, factual errors, unclear sections
- **Pull requests for documentation fixes** — typos, formatting, broken cross-references, clearer phrasing of existing content
- **Pull requests adding system notes** to `docs/systems/` (following the convention in `docs/systems/README.md`)
- **Pull requests adding causal chain notes** to `docs/chains/` (following the convention in `docs/chains/README.md`)
- **Pull requests adding research summaries** to `docs/_research/` (with sourcing)
- **Issue-then-PR for major design refinements** (see "How to contribute" below)

### Not currently in scope

- **Code** — there is no codebase yet. When the implementation phase begins, this will change. Watch the repo for that announcement.
- **Art assets** — visual style hasn't been locked. Asset contributions deferred to later phase.
- **Audio assets** — same, deferred.
- **Marketing / community management work** — not yet, this is a single-developer commercial project.

### Always out of scope

- Pull requests that violate the foundational principles
- Pull requests that introduce in-game UI elements that float on the world (per the diegetic-UI commitment)
- Pull requests that introduce dominant strategies (per §1 No Dominant Strategy)
- Pull requests that add scripted-narrative meta-systems (per §2 Mechanical Behavior)

## How to contribute

### For small fixes (typos, formatting, broken links)

Submit a PR directly. Brief PR description. Likely fast merge.

### For documentation expansions (adding system notes, chain notes, research summaries)

Submit a PR. Follow the convention files (`docs/systems/README.md`, `docs/chains/README.md`). Reference any §-numbers in the design doc the contribution touches. Likely review-and-discussion before merge.

### For design discussions or major refinements

**Open an issue first.** Describe the proposal. Explain how it relates to the foundational principles. Wait for discussion before submitting a PR. Major design changes will only be accepted after issue-level alignment.

### For questions

Open an issue tagged "question." Don't open a PR with speculative changes — ask first.

## Conventions

### File structure

- Design philosophy: `docs/design/survival-design.md`
- Detailed system specs: `docs/systems/<system-name>.md`
- Causal chain notes: `docs/chains/<chain-name>.md`
- Research: `docs/_research/<NN>-<topic>.md`
- Originals (immutable): `docs/originals/`

### Cross-references

- **Wikilinks `[[note-name]]`** for cross-references between system notes and chain notes (Obsidian-style; works in any markdown editor; renders as graph in Obsidian)
- **§-numbers** for cross-references to the design doc (e.g., "per §13.2 Carrion Chains")
- **Markdown links** for cross-references to research files

### File naming

- System notes: lowercase-with-hyphens.md (e.g., `bear-ai.md`)
- Chain notes: lowercase-with-hyphens, descriptive of the chain (e.g., `overhunting-predator-aggression.md`)
- Research files: numbered prefix matching `docs/_research/_index.md` if maintained

### Writing style

- **Describe MECHANISMS, not strategies** — design notes describe what a system DOES; they do not prescribe how the player should play
- **Ethnographic / field-naturalist tone** — when describing wildlife or environmental behavior
- **Engineering / spec tone** — when describing system inputs/outputs
- **No second-person prescription** in design notes — avoid "the player should" or "you should" phrasings; prefer "a player can..." or "in this scenario..."
- **No emoji unless explicitly relevant**

### What goes in a system note vs a chain note

- **System notes** describe ONE system (bear AI, scent system, weather grid). Inputs, outputs, internal state, player-facing surfacing.
- **Chain notes** describe interactions between TWO OR MORE systems across time (over-hunting → predator hunger; camp scent → Camp Stalker pathway). Sequence, time scale, player-readable signals, counter-actions.

If unsure, the convention READMEs in `docs/systems/` and `docs/chains/` have detailed examples.

## Authority

Mother Nature is being designed and developed by a single developer (William). All design decisions are ultimately William's call. This isn't a democratic project — it's an open design phase for a commercial indie game.

If a contributor's preferred design direction doesn't align with William's vision, the contributor's vision won't override. Discussion is welcome; final calls aren't votes.

## Response cadence

This is a single-developer project alongside other commitments. Realistic expectations:

- Issues: typically reviewed within 1-2 weeks
- PRs (small fixes): typically reviewed within 1-2 weeks
- PRs (substantive content): typically reviewed within 2-4 weeks
- Discussion threads: variable; long threads may pause and resume

If your contribution sits without response for >4 weeks, ping the issue/PR. Apologies in advance.

## Code of conduct

Contributors agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md). It boils down to: be civil, focus on the work, don't make it personal.

## Contributor terms (legal)

By submitting a contribution, you agree to the terms in [LICENSE](LICENSE) under "CONTRIBUTOR TERMS." In summary: you grant William a perpetual, irrevocable license to use your contribution in the project, including the commercial release. You retain copyright in your contribution; you don't gain rights to the broader project.

If those terms aren't acceptable to you, please don't submit a contribution. There is no ill will here — different projects have different shapes; this one is open-design + commercial-ownership-retained.

---

Thanks for reading. The fastest way to learn what kinds of contributions land well is to read a few existing system notes and chain notes, and look at recent merged PRs once there are some. The repo's current state is its own best documentation.
