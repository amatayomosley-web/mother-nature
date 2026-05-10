# Systems notes

This folder holds detailed specifications for individual game systems and mechanics. Each note covers ONE system in depth, with explicit inputs (what triggers it) and outputs (what it triggers). Cross-references use Obsidian-style wikilinks `[[note-name]]` so the graph view shows system interaction at a glance.

## Conventions

- **One file per system or mechanic.** If a concept is large enough to need its own behavioral spec, it gets its own file.
- **Wikilink to other systems** via `[[system-name]]`. The filename without `.md` extension is the link target.
- **Cross-reference §-numbers** to the design doc when relevant.
- **Tags** classify by type: `#wildlife`, `#weather`, `#player`, `#economy`, `#ai-agent`, etc.
- **Internal vs player-facing**: each note distinguishes what the SYSTEM tracks (engineering reference) from what the PLAYER experiences (in-game observable). The two are deliberately different per the diegetic-UI commitment in §12.8.

## See also

- [_index.md](_index.md) — list of all system notes
- [../chains/](../chains/) — causal chains linking multiple systems
- [../design/survival-design.md](../design/survival-design.md) — high-level architecture
