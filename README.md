# Mother Nature

A 2D 3/4-perspective multiplayer survival simulation where nature is the antagonist and the only goal is to live. No bosses, no quests, no win condition. Heat, cold, hunger, thirst, exhaustion, injury, and weather behave like a real body answering a real environment.

The entertainment isn't progression — it's the player's own learning curve. Every run is a different survival problem, and the meta-game is everything you carry from one life to the next.

> Comparable titles: Project Zomboid without the zombies, The Long Dark with other people, Rust without the raid meta. A survival simulation where the entertainment is the player's own learning curve, and where the only thing they keep when they die is what they've come to understand.

## Status

**Design phase.** No playable build yet. The repository currently holds:

- Comprehensive design document
- Foundational research reports (genre analysis, atmospheric simulation, wildlife AI, persistent server architecture, compendium systems, Godot multiplayer)
- System specifications (per-system AI behavior, mechanics)
- Causal chain notes (multi-system cause-and-effect tracking)
- Curriculum table of contents (a parallel learning project on game development)

Engine: Godot 4 (chosen for AI-assisted development workflow). Target platform: Steam (PC).

## Navigate

| Where | What you'll find |
|---|---|
| [`docs/design/survival-design.md`](docs/design/survival-design.md) | The design document. **Start here.** ~1100 lines covering pitch, principles, characters, mechanics, architecture commitments, skill system, combat, system interaction graph. |
| [`docs/systems/`](docs/systems/) | Per-system specifications. Each note describes one game system (e.g., bear AI, scent system) with explicit inputs, outputs, and player-facing surfacing. |
| [`docs/chains/`](docs/chains/) | Multi-system causal chain notes. Cause-and-effect patterns spanning multiple systems. |
| [`docs/_research/`](docs/_research/) | Research reports informing the design. 23 reports covering genre, simulation, wildlife behavior, Godot architecture, persistent servers, and more. |
| [`docs/originals/`](docs/originals/) | Original design document (.docx) preserved as a frozen reference. |
| [`docs/curriculum-toc.md`](docs/curriculum-toc.md) | Parallel learning project — a complete novice-to-shipping game-dev curriculum framed around Mother Nature. |

## Foundational principles

These principles govern every design decision. They are non-negotiable; pull requests that violate them will be redirected.

- **Nature is the antagonist** — no human enemies as a primary loop. The world itself pushes back.
- **No dominant strategy** — every powerful tool, class, or playstyle has a specific cost preventing it from becoming the optimal answer.
- **Variance, not randomness** — outcomes vary, but in ways the player can read.
- **Mechanical behavior, not scripted narrative** — agents (wildlife, weather) act from their own state, never from a designer's story arc.
- **Diegetic UI, no overlays** — no floating world labels, no minimap, no popup alerts. Information lives in the world or in the compendium.
- **Skill is debuff-removal, not bonus-stacking** — novice characters operate at a deliberate impairment; mastery is the absence of friction, not superhuman ability.
- **Permadeath teaches** — character knowledge persists per-account through field notes; characters reset.

The full set of design principles is at [`docs/design/survival-design.md`](docs/design/survival-design.md) §1 and §2.

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for full details.

Short version: this repo is in a design phase, and contributions of design feedback, content (additional system notes, causal chain analyses), typo fixes, and structural improvements are welcome via issues and pull requests. Code contributions will open later when implementation begins.

The design DNA stays under William's authority. Major design changes go through issue discussion before PR.

## License

All rights reserved. See [LICENSE](LICENSE) for full terms, including contributor terms.

The repository is publicly visible to invite design feedback and discussion. It is not open-source software in the traditional sense — Mother Nature is being designed in the open as a learning collaboration, but the resulting commercial game belongs to William.
