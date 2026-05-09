# Art Pipeline (Solo, Non-Artist) — Research Report

## Domain summary

The honest take: a non-artist solo dev shipping a 2D Godot game in 2026 needs roughly two skills, not twenty. They need (1) the discipline to commit to a low resolution and a fixed palette, and (2) the patience to learn one tool well enough to edit asset-pack sprites. Everything else — anatomy, color theory, animation principles — is optional polish that returns less per hour than gameplay work. Constraint discipline is the load-bearing competency. A 32x32 sprite with 16 colors looks "intentional" even when it is technically crude, because the human visual system reads cohesion before it reads skill. The tooling cost of admission is one paid app (Aseprite, ~$20) plus a CC0 starter pack from Kenney, and the non-artist's true competitive surface is taste in selecting and recombining existing assets, not raw drawing ability.

## Topic catalog

### Canvas size choice (16x16, 32x32, 64x64)
- **Brief**: Pick a base sprite size and stick with it. 16x16 = classic retro/roguelikes (max sprite variety per hour). 32x32 = modern indie sweet spot (Stardew, Dead Cells-adjacent). 64x64+ = punishing for solos.
- **Why-drilled**: L1: fewer pixels = fewer decisions per sprite. L2: 32x32 has 4x the pixels of 16x16 (1024 vs 256), quadrupling the time per sprite and quadrupling animation frame cost. First principle: cognitive load is quadratic in canvas area, but information legibility plateaus quickly — past ~32x32 you are paying linearly for diminishing readability gains.
- **Tool / source**: Aseprite "New Sprite" dialog. Sprixen and Pixel Parmesan size guides.
- **Novice failure mode**: Starting at 128x128 to "get more detail," then never finishing a single character because every sprite takes 6 hours.
- **Priority**: Critical — pick before you draw a pixel.

### Color palette discipline (DB16, DB32, Lospec)
- **Brief**: Lock to a 16- or 32-color palette before drawing. DawnBringer's DB16/DB32 are the de facto starting points; Lospec.com hosts thousands more. Aseprite ships DB16 and DB32 as default presets.
- **Why-drilled**: L1: limited palettes look intentional — the brain reads "designed system" instead of "amateur color picking." L2: shared hue/saturation/value relationships across all sprites create automatic cohesion the artist did not consciously craft. First principle: human visual perception groups by color similarity (Gestalt principle of similarity), so any unified palette wins over a sophisticated drawing made with arbitrary colors.
- **Tool / source**: Lospec.com palette database; Aseprite > Palette > Presets > DB16/DB32.
- **Novice failure mode**: Picking colors with the eyedropper from reference photos and ending up with 80 near-duplicates that read as muddy.
- **Priority**: Critical — second-most-important after canvas size.

### Sprite size conventions and powers of two
- **Brief**: 16, 32, 64, 128 are the standard sizes. Tilesets are usually 16x16 or 32x32. Backgrounds are multiples thereof.
- **Why-drilled**: L1: power-of-two textures pack cleanly into atlases. L2: GPU mipmap chains historically required POT dimensions; modern hardware no longer requires it but tile math is still cleaner. First principle: hardware texture units descend from a POT-aligned cache architecture (1990s GPUs); divisible grids align to that history and to integer screen scaling.
- **Tool / source**: Aseprite canvas presets; Godot TileSet tile-size field.
- **Novice failure mode**: Picking 17x17 sprites because "it felt right," then fighting tile alignment forever.
- **Priority**: High.

### Animation principles (Disney 12, applied to sprites)
- **Brief**: For 2D game sprites the load-bearing principles are squash & stretch, anticipation, follow-through, and ease in/out. The other 8 (staging, secondary action, etc.) matter less for game feel.
- **Why-drilled**: L1: squash & stretch communicates weight; anticipation telegraphs intent (jump windup tells the player a jump is coming). L2: game animations need to read in 4-8 frames at 12fps — they cannot rely on subtle naturalistic motion. First principle: human peripheral and rapid-glance perception locks onto silhouette change and shape distortion before it parses detail; squash/stretch and anticipation are silhouette-driven.
- **Tool / source**: GameAnim.com "12 Principles in Video Games"; AdamCYounis YouTube animation playlists.
- **Novice failure mode**: Animating realistic motion (24+ frames, subtle) instead of game-readable motion (4-8 frames, exaggerated).
- **Priority**: High.

### Frame counts (4 walk, 8 run, 2 idle)
- **Brief**: Walk cycles work at 4 frames (contact-passing-contact-passing) for retro feel, 6-8 for smoother. Idle: 2-4 frames is plenty. Attacks: 3-5 frames (windup, hit, recover). 8-frame run cycles for higher-fidelity work.
- **Why-drilled**: L1: each frame is 30-60 minutes of work; cost is linear. L2: a 4-frame walk read at 8-12fps is the SNES-era convention the brain has been trained to read since 1990. First principle: pattern-matching against established game-feel conventions buys "looks like a game" cheaply; deviating from convention requires more frames to reach the same legibility.
- **Tool / source**: Aseprite Tag system for frame ranges per animation.
- **Novice failure mode**: Drawing 24-frame walk cycles, never finishing the 6 NPC types they planned.
- **Priority**: High.

### Tilesets and autotiling (47-tile blob, 16-tile, terrain)
- **Brief**: Godot 4 uses TileMap + TileSet with terrain sets. The "Corners and Sides" mode is the 47-tile blob layout. Plugin "Better Terrain" by Portponky is a community favorite for nicer auto-rules.
- **Why-drilled**: L1: autotiling lets you paint with one terrain type and the engine picks the right edge tile. L2: an 8-bit bitmask of the 8 neighbors yields 256 combinations that collapse to 47 visually distinct tiles. First principle: the visual continuity problem is inherently combinatorial — manual tile placement scales O(map_size), autotile scales O(1) per paint stroke.
- **Tool / source**: Godot docs "Using TileSets"; itsjavi/autotiler generator; Portponky/better-terrain plugin.
- **Novice failure mode**: Drawing 47 unique tiles by hand before learning the autotile editor takes 80% of that work away.
- **Priority**: Critical for any tile-based game.

### 9-slice scaling for UI panels (NinePatchRect)
- **Brief**: Godot's NinePatchRect node splits a texture into 3x3 — corners stay fixed, edges tile, center stretches. One small panel sprite scales to any UI size.
- **Why-drilled**: L1: lets one 32x32 panel sprite become any size without distortion. L2: corners carry visual identity (rounded edges, decoration); they cannot stretch without breaking. First principle: humans recognize border/edge as semantic; stretched corners look broken because they violate learned visual grammar of bordered rectangles.
- **Tool / source**: Godot NinePatchRect docs (4.4 / 4.5); Kenney UI packs include 9-slice-ready panels.
- **Novice failure mode**: Drawing one panel per UI size, ending up with 12 button textures.
- **Priority**: High once UI is needed.

### Pixel-perfect rendering setup in Godot 4
- **Brief**: Project Settings: viewport size = base resolution (e.g. 320x180 or 480x270); Window > Stretch Mode = `viewport` or `canvas_items`; Stretch Aspect = `keep`. Enable integer scaling (Godot 4.3+). Set Rendering > 2D > Snap 2D Transforms to Pixel = ON. On every imported pixel-art asset: Filter = Off (Nearest), Mipmaps = Off, Compression = Lossless.
- **Why-drilled**: L1: linear filtering blurs pixels; non-integer scale produces 1.5-pixel-wide borders that shimmer. L2: integer scale (2x/3x/4x) preserves the 1:1 pixel mapping. First principle: a pixel art pixel is a quantized design unit; subpixel sampling violates the design contract by introducing colors not in the palette.
- **Tool / source**: GDQuest "Setting up pixel art graphics in Godot 4"; Godot docs "Multiple resolutions."
- **Novice failure mode**: Forgetting to set Filter Off, then wondering why their crisp 32x32 sprite looks blurry in-game.
- **Priority**: Critical — blocks "looks like a game."

### Asset organization (by-type vs by-feature)
- **Brief**: Godot's official guidance is to organize by feature/scene, not by asset type. Each scene gets a folder containing its scene file, scripts, and exclusive textures. Globally shared assets go in `/shaders`, `/fonts`, `/ui` siblings. Third-party in `/addons`.
- **Why-drilled**: L1: when a scene is deleted, its folder is deleted — no orphan textures. L2: scenes change as cohesive units; assets that change together should live together. First principle: locality of reference applies to filesystems too — coupling-by-folder reduces cross-cutting search cost.
- **Tool / source**: Godot docs "Project organization"; abmarnie/godot-architecture-organization-advice GitHub.
- **Novice failure mode**: Creating `/sprites`, `/scripts`, `/scenes` mega-folders, then needing to touch 4 directories per feature change.
- **Priority**: Medium — easy to refactor later.

### File naming conventions in Godot
- **Brief**: snake_case for files and folders. PascalCase for class names inside scripts and node names in the scene tree. Avoid spaces, kebab-case (translation issues with class auto-generation), and uppercase (case-sensitivity bugs cross-platform).
- **Why-drilled**: L1: Godot's GDScript style guide mandates snake_case. L2: Windows is case-insensitive but Linux export targets (and Steam Deck) are case-sensitive — `Player.gd` vs `player.gd` will silently break exports. First principle: a single naming scheme eliminates an entire class of cross-platform bugs.
- **Tool / source**: Godot GDScript style guide; GDQuest naming conventions.
- **Novice failure mode**: Mixing `Player.png` and `player_idle.png`, then losing 2 hours to a Linux export bug.
- **Priority**: Medium.

### Godot import settings for pixel art
- **Brief**: Per-image: Filter Off (nearest neighbor), Mipmaps Off, Compression = Lossless. Set once per project as default in Editor Settings, then verify per-asset.
- **Why-drilled**: L1: Filter Off keeps pixels sharp; Mipmaps Off avoids blurring at small scales; Lossless preserves exact palette colors (lossy compression introduces off-palette colors). L2: VRAM compression is designed for high-res photographic textures and ruins low-res indexed art. First principle: pixel art is information-dense per pixel; any sampling or compression that averages neighbors destroys the design.
- **Tool / source**: Godot docs "Importing images"; Shaggy Dev "Configuring your Godot project for pixel art."
- **Novice failure mode**: Importing one asset with default settings, getting mystery blur, blaming Aseprite.
- **Priority**: Critical.

### Bitmap fonts vs SDF fonts in Godot 4
- **Brief**: Pixel-art games: bitmap font (BMFont) at native resolution, no scaling. Higher-res / mixed-resolution UI: SDF or MSDF for crisp text at any size. Godot 4 supports MSDF natively via the font import setting "Multi-channel Signed Distance Field."
- **Why-drilled**: L1: bitmap fonts blur when scaled; SDF stores distance-to-glyph-edge so any scale stays sharp. L2: pixel art games use integer scaling (2x/3x/4x), at which bitmap fonts already work; non-pixel games scale fonts to match window size, breaking bitmap fonts. First principle: font legibility = contrast at edges; SDF preserves edge contrast under arbitrary affine transforms because distance fields are mathematically continuous.
- **Tool / source**: Godot docs "Using Fonts"; Lvl1Danko itch.io pixel font collection.
- **Novice failure mode**: Using a TTF at 8pt in a pixel-art game and watching it antialias against a crisp pixel background.
- **Priority**: Medium.

### 2D normal maps and lighting (Godot 4)
- **Brief**: Godot 4 supports 2D normal maps via CanvasTexture (combines diffuse + normal). PointLight2D casts dynamic light onto sprites with normal maps. Rarely worth the effort for a first game.
- **Why-drilled**: L1: a normal map is a per-pixel surface direction; light reflects realistically. L2: generating decent 2D normal maps requires tools like Sprite Lamp or hand-painting in Aseprite — both are a separate art skill. First principle: lighting cost is super-linear in art-pipeline complexity (every sprite needs a normal-map pass); skip unless lighting is core gameplay.
- **Tool / source**: GDQuest "Lighting with 2D normal maps"; Connor Wolf isometric-tile lighting tutorial (Godot 4.4).
- **Novice failure mode**: Six weeks lost on a lighting system the game does not need.
- **Priority**: Low — defer to game 2.

### Sprite atlases and texture packing
- **Brief**: Godot 4 supports AtlasTexture for grouping sprites in one image. TexturePacker (CodeAndWeb plugin) automates packing and exports a `.tres` resource. Mostly a performance optimization.
- **Why-drilled**: L1: one large texture = fewer GPU draw calls = better mobile performance. L2: each unique texture binding is a state change; modern GPUs amortize 1000s of sprites in one draw if they share an atlas. First principle: GPU bottleneck is state changes, not raw fill — pack textures to reduce state changes.
- **Tool / source**: TexturePacker Importer plugin; Godot AtlasTexture docs.
- **Novice failure mode**: Premature atlas optimization before the game has 50 sprites; or never atlasing on mobile and dropping to 20fps.
- **Priority**: Low for desktop, medium for mobile.

### "I'm not an artist" survival strategies
- **Brief**: Three live strategies: (1) buy/grab CC0 packs (Kenney, OpenGameArt) and use as-is; (2) recolor a single pack to one palette to unify visually; (3) commit to a geometric/minimal style (Thomas Was Alone — colored rectangles; Downwell — 3-color; Vlambeer-style flat shapes).
- **Why-drilled**: L1: assets exist for free; reinventing the wheel costs months. L2: a unified palette applied to mismatched packs makes them feel cohesive (palette swap > asset uniformity for cohesion). First principle: the audience reads emotional intent and consistency, not draftsmanship. A game that commits to "rectangles with personality" reads as designed; a game that wobbles between styles reads as abandoned.
- **Tool / source**: Kenney.nl (CC0); OpenGameArt.org (mixed licenses, check); itch.io game-assets (mixed licenses); Aseprite palette-swap via Sprite > Color Mode > Indexed.
- **Novice failure mode**: Mixing Kenney top-down RPG, a free Pirate Stuff icon pack, and a custom UI font from another era — visually fragmented.
- **Priority**: Critical for non-artists.

### AI-generated art (concepts, generation, status 2026)
- **Brief**: Mid-2026 status: AI-generated content can be in shipped games on Steam (policy clarified Jan 2026). Pure AI output is not copyrightable in the US — the Supreme Court declined Thaler v. Perlmutter in early 2026, leaving the human-authorship requirement intact. Meaningful editing/compositing/painting-over by a human grants protection to the human-authored portions. Steam now distinguishes dev-tool use (no disclosure) from player-facing AI content (must disclose, follow content policy).
- **Why-drilled**: See "AI art — verdict" section.
- **Tool / source**: Steam AI policy (Jan 17, 2026 rewrite); US Copyright Office reports; Morgan Lewis legal commentary.
- **Novice failure mode**: Generating 200 sprites in MJ, shipping with no human re-paint, discovering competing devs can use them too (no copyright protection).
- **Priority**: High — legal and consistency landmines.

### Working with art-pack assets (mix-and-match rules)
- **Brief**: Rules of mix-and-match: pick ONE pack as the base, treat others as "guests" you must conform to the host's palette and pixel size. Always reduce all imported assets to your project's palette via Aseprite's "Convert to Palette" before using.
- **Why-drilled**: L1: cohesion matters more than provenance. L2: a 16x16 sprite next to a 24x24 sprite at the same screen scale visibly disagrees in pixel density. First principle: the eye reads pixel density as material — different densities = different worlds.
- **Tool / source**: Aseprite "Convert to Palette" (Sprite > Color Mode > Indexed > pick palette); Lospec batch palette converter.
- **Novice failure mode**: Native-pasting Kenney 16x16 next to a 32x32 itch.io pack, looking like two games glued together.
- **Priority**: Critical when using multiple packs.

### Vector and hand-drawn alternatives
- **Brief**: Vector (Thomas Was Alone, Geometry Wars-style) is approachable for non-artists who can use shapes. Hand-drawn / illustrated styles (Hollow Knight, Cuphead, Gris) require trained art talent and are NOT a non-artist option despite looking "loose."
- **Why-drilled**: L1: vector hides drawing weakness in geometric primitives; hand-drawn exposes every line. First principle: stylistic "looseness" in great hand-drawn games is the result of confidence on top of years of fundamentals — you cannot fake it from zero.
- **Tool / source**: Inkscape (free vector); Godot supports SVG import natively in 4.x.
- **Novice failure mode**: Trying to copy Hollow Knight's visual identity from no art training.
- **Priority**: Medium — informs scope decision, not daily work.

## Recommended novice art pipeline

For a non-artist solo dev shipping their first 2D Godot game in 2026:

1. **Tools**: Aseprite ($20, one-time) for sprite work; Pixelorama (free) if budget-constrained; Godot 4.4+ for the engine. Skip Krita for pixel work — it is general-purpose paint, not pixel-tuned.
2. **Resolution decision**: Pick 32x32 for character sprites and 16x16 tiles, or 16x16 character + 16x16 tile. Lock this on day one. Internal viewport = 320x180 or 384x216 (16:9).
3. **Palette decision**: Start with DB32 (DawnBringer 32) loaded as your Aseprite default palette. All imported pack assets get reduced to this palette via Sprite > Color Mode > Indexed.
4. **Asset source order**: (a) Kenney.nl CC0 packs as base — characters and tiles in one matched pack. (b) Edit Kenney sprites in Aseprite to your DB32 palette and your scene's mood. (c) Custom-draw only the things Kenney doesn't have: protagonist hat, key items, the big boss. (d) AI generation only for one-off illustrations (title screen, ending cutscene), with paint-over.
5. **Godot project setup**: Project Settings — Display > Window > Viewport Width/Height = 320/180; Stretch Mode = `viewport`; Stretch Aspect = `keep`; enable integer scaling. Rendering > 2D > Snap 2D Transforms to Pixel = ON. Editor Settings > Import Defaults > Texture > Filter = Off.
6. **Folder structure**: `/scenes/<feature>/` per feature, `/ui/`, `/fonts/`, `/addons/`. snake_case all the way.
7. **Animation discipline**: 4-frame walk, 2-frame idle, 3-frame attack. Aseprite Tags name each animation. Export as a single sprite sheet (horizontal strip) per character, import to Godot as AnimatedSprite2D with frames cut by SpriteFrames editor.
8. **UI**: Kenney UI Pack RPG (CC0); use NinePatchRect for any panel that resizes. One bitmap font (e.g. m5x7 or Lvl1Danko itch packs) at native size — no scaling.

## Must-include shortlist

1. **Aseprite + DB16/DB32 palette** — set canvas size and palette before drawing.
2. **Godot pixel-perfect setup** — Filter Off, Snap 2D Transforms, integer scaling, viewport stretch.
3. **Kenney.nl CC0 starter pack** — your first game's base art, free, commercial-OK.
4. **9-slice (NinePatchRect) for all UI panels** — one panel scales to all sizes.
5. **Autotile via TileSet terrain sets (or Better Terrain plugin)** — never hand-place tiles.
6. **snake_case file naming, by-feature folder structure** — prevents cross-platform export bugs.
7. **One palette unification pass on every imported asset** — cohesion via Convert to Palette.
8. **4/2/3 frame counts (walk/idle/attack)** — ship animations, do not perfect them.

## Commonly oversold

- **The 12 Disney principles in full**: Most game animations only need squash/stretch, anticipation, and ease. Staging, secondary action, exaggeration, appeal — defer.
- **Anatomy and figure drawing**: Game sprites are stylized icons. A 16x16 character has 256 pixels — no anatomy fits.
- **Color theory courses (color wheels, complementary colors)**: Pick a Lospec palette someone else made and trust it. The course is a 6-month detour.
- **Skeletal 2D animation (Spine 2D, Godot Skeleton2D)**: Frame-by-frame is faster for novices; skeletal pays off only past ~50 unique animations per character.
- **Normal maps and 2D dynamic lighting**: Beautiful, but a separate art pipeline doubling work. Skip for first game.
- **Sprite atlases / texture packing**: Premature optimization on desktop. Ship first, profile second.
- **Vector art tools (Illustrator, Inkscape) for non-vector games**: If your game is pixel-art, you do not need vector. Pick one paradigm.
- **Multiple resolutions / DPI scaling drama**: Set integer scale, viewport stretch, ship. Modern Godot handles it.
- **Custom shaders for "stylized look"**: A palette-correct sprite already looks stylized. Shaders are post-shipping.

## AI art — verdict

**Net-positive uses in 2026:** concept exploration (look-tests for a game's mood before committing); single-instance illustrations (title screen, ending splash, key-art for the Steam page) where consistency is not required; mood boards; reference for hand-paint-over; texture noise/dirt overlays. AI is a brainstorming and one-off tool.

**Net-negative uses in 2026:** generating dozens of in-game sprites, NPCs, or tiles. The two killers are (1) **consistency** — diffusion models have weak per-asset coherence; a 30-NPC town generated by Midjourney does not share a palette, line weight, or pixel density even with identical prompts; (2) **legal** — pure AI output is uncopyrightable in the US (Thaler v. Perlmutter 2026), so a competitor can lift your spritesheet and you have no claim. Heavy hand-paint-over is the legal fix, but at that point you saved less time than expected.

**Steam policy (Jan 17, 2026 rewrite):** AI used in development = no disclosure required. AI-generated content shipping to players = required disclosure on the store page and content must comply with Steam content rules. Game-jam and indie titles using AI face no Steam-side ban; the constraint is consistency and copyright, not platform.

**Verdict for a non-artist solo dev:** use AI for the title splash, the Steam capsule, and concept iteration. Do NOT use it for in-game sprites unless you have the Aseprite skill to repaint each one to your palette and pixel grid — at which point the AI saved you the blank-canvas anxiety, not the work.

## Cross-references

- Engine choice / Godot 4 setup → cross-pollinates with the engine-onboarding research scope (viewport, project settings).
- Scope and shipping → "ship a small game" research; canvas/palette/frame-count discipline IS scope discipline applied to art.
- Audio pipeline → similar "use CC0 packs first" pattern (Kenney also publishes audio).
- Marketing / store page → AI art for capsules and key-art is the legitimate use, ties to Steam policy.

## Sources

- [Pedro Medeiros / Saint11 — Pixel Art Articles](https://saint11.art/pixel_articles/)
- [Pedro Medeiros — How to Start Making Pixel Art (Medium)](https://medium.com/pixel-grimoire/how-to-start-making-pixel-art-2d1e31a5ceab)
- [Lospec Pixel Art Tutorials by Pedro Medeiros](https://lospec.com/pixel-art-tutorials/author/pedro-medeiros)
- [Lospec Pixel Art Tutorials by MortMort](https://lospec.com/pixel-art-tutorials/author/mortmort)
- [AdamCYounis — YouTube channel](https://www.youtube.com/adamcyounis)
- [MortMort — YouTube channel](https://www.youtube.com/channel/UCsn9MzwyPKeCE6MEGtMU4gg)
- [GDQuest — Setting up pixel art graphics in Godot 4](https://www.gdquest.com/library/pixel_art_setup_godot4/)
- [Godot — Multiple resolutions docs](https://docs.godotengine.org/en/stable/tutorials/rendering/multiple_resolutions.html)
- [Godot — Importing images docs](https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_images.html)
- [Shaggy Dev — Configuring your Godot project for pixel art](https://shaggydev.com/2021/09/21/project-setup-for-pixel-art/)
- [Kenney.nl — Free CC0 game assets](https://kenney.nl/)
- [Kenney Starter Kits](https://kenney.nl/starter-kits)
- [DawnBringer 16 Palette (Lospec)](https://lospec.com/palette-list/dawnbringer-16)
- [DawnBringer 32 Palette (Lospec)](https://lospec.com/palette-list/dawnbringer-32)
- [Lospec Palette List](https://lospec.com/palette-list)
- [Godot — Using TileSets docs](https://docs.godotengine.org/en/stable/tutorials/2d/using_tilesets.html)
- [Better Terrain plugin (Portponky)](https://github.com/Portponky/better-terrain)
- [Bitmask Autotiling: 47-Tile Reference](https://jaconir.online/blogs/bitmask-autotile-guide)
- [itsjavi — Autotiler 47-tile generator](https://github.com/itsjavi/autotiler)
- [Godot — NinePatchRect class reference](https://docs.godotengine.org/en/stable/classes/class_ninepatchrect.html)
- [Godot — Using Fonts docs](https://docs.godotengine.org/en/stable/tutorials/ui/gui_using_fonts.html)
- [Godot — 2D lights and shadows docs](https://docs.godotengine.org/en/stable/tutorials/2d/2d_lights_and_shadows.html)
- [GDQuest — Lighting with 2D normal maps](https://www.gdquest.com/tutorial/godot/2d/lighting-with-normal-maps/)
- [Connor Wolf — Realtime 2D Lighting in Godot 4.4](https://www.connorwolf.com/post/realtime-2d-lighting-with-shadows-on-isometric-tiles-in-godot-4-4)
- [Godot — 2D skeletons docs](https://docs.godotengine.org/en/stable/tutorials/animation/2d_skeletons.html)
- [Godot — Project organization docs](https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html)
- [abmarnie — Godot architecture & organization advice](https://github.com/abmarnie/godot-architecture-organization-advice)
- [Godot — GDScript style guide](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html)
- [TexturePacker Importer for Godot](https://godotengine.org/asset-library/asset/1503)
- [Pixel Art Sprite Sizes: 8x8 vs 16x16 vs 32x32 vs 64x64 (Sprixen)](https://sprixen.com/blog/pixel-art-sizes-guide)
- [Pixel Parmesan — Choosing the Right Resolution](https://pixelparmesan.com/blog/choosing-the-right-resolution-for-your-pixel-art)
- [GameAnim — 12 Principles of Animation in Video Games](https://www.gameanim.com/2019/05/15/the-12-principles-of-animation-in-video-games/)
- [itch.io free pixel art game assets](https://itch.io/game-assets/free/tag-pixel-art)
- [OpenGameArt.org](https://opengameart.org/)
- [Pixelorama (free Godot-built pixel editor)](https://orama-interactive.itch.io/pixelorama)
- [Krita](https://krita.org/)
- [Aseprite](https://www.aseprite.org/)
- [Indie Game Art Styles guide (Pixune)](https://pixune.com/blog/indie-game-art-styles/)
- [Steam AI Policy 2026 — Legal Moves Law Firm](https://legalmoveslawfirm.com/steam-ai-policy/)
- [US Supreme Court declines AI authorship case (Morgan Lewis 2026)](https://www.morganlewis.com/pubs/2026/03/us-supreme-court-declines-to-consider-whether-ai-alone-can-create-copyrighted-works)
- [AI-Generated Content in Gaming — National Law Review](https://natlawreview.com/article/leveling-or-losing-rights-copyright-challenges-ai-generated-content-gaming)
