# Audio (Solo, Non-Audio-Engineer) — Research Report

## Domain summary

Audio is the cheapest, highest-leverage upgrade a solo dev can make to a 2D Godot game — Vlambeer's "Art of Screenshake" puts audio side-by-side with screenshake, particle systems, and squash-stretch as a top juice multiplier, but most novices treat it as the "we'll do it last" task. Audio splits into two design problems with almost nothing in common: **SFX is feedback** (the player's brain uses the first 50ms of a sound to confirm cause-and-effect), and **music is mood** (a continuous emotional bed). Conflating them leads to the two most common novice failures: SFX that fatigues from repetition because there's no variation, and music that fights the SFX because there's no bus structure or ducking. The good news: a non-audio-engineer can ship a game that sounds professional in 2026 by combining CC0 packs (Kenney, OpenGameArt), procedural retro generators (Bfxr, ChipTone), browser music tools (Beepbox, Bosca Ceoil), Godot's built-in `AudioStreamRandomizer`, and a four-bus mixer. FMOD/Wwise are almost always overkill at this scale.

## Topic catalog

### Why audio is the cheapest game-feel upgrade
- **Brief**: A competent 30-minute audio pass (footsteps + impact + UI click + music loop) lifts perceived production value more than a week of art polish.
- **Why-drilled**: L1 — players consciously notice graphics; L2 — players unconsciously rate "feel" by audio-visual sync; first principle — human perception fuses audio + visual within ~80ms (the "McGurk window"), and missing audio reads as "broken" pre-cognitively.
- **Tool / source**: Vlambeer "Art of Screenshake" talk (Jan Willem Nijman, INDIGO Classes 2013).
- **Novice failure mode**: Shipping silent prototypes "until art is done." Players never feel the game.
- **Priority**: Critical.

### SFX vs music: two design problems
- **Brief**: SFX = discrete, short, feedback-coupled. Music = continuous, ambient, mood-coupled.
- **Why-drilled**: L1 — they sound different; L2 — they target different perceptual systems; first principle — auditory cortex processes transient onsets (SFX) on a separate pathway from sustained tonal content (music). Designing them with the same tools blurs both.
- **Novice failure mode**: Treating music as "bigger SFX" — using DAW reverb on shooter feedback, pitch-randomizing music tracks.
- **Priority**: Critical (conceptual foundation).

### The 50ms onset rule
- **Brief**: The first ~50ms of any SFX is what the player perceives — the rest is texture.
- **Why-drilled**: L1 — short sounds feel snappier; L2 — the brain locks impression to the attack transient; first principle — auditory adaptation kicks in after the onset, so the tail decays into perceptual irrelevance.
- **Novice failure mode**: Polishing the tail of SFX (fades, reverb tails) instead of fixing a weak onset.
- **Priority**: Important — drives every SFX decision.

### Variation: same-sound-every-time fatigues
- **Brief**: Every repeated SFX needs at least 3–5 variations OR pitch/volume randomization.
- **Why-drilled**: L1 — repetition annoys players; L2 — the brain flags identical repeats as glitches; first principle — the auditory system performs **stimulus-specific adaptation**, suppressing exact-repeat signals to surface novel ones. Identical SFX literally fade from awareness, then snap back as irritation.
- **Tool**: Godot's `AudioStreamRandomizer` (built-in, holds a list of streams plus pitch/volume randomization parameters).
- **Novice failure mode**: One footstep WAV that plays 10,000 times per session.
- **Priority**: Critical.

### Layering: high/mid/low frequency stack
- **Brief**: A satisfying SFX is three sounds stacked: low (body/punch), mid (texture/whoosh), high (sizzle/crack).
- **Why-drilled**: L1 — layered sounds feel "fuller"; L2 — different frequencies don't mask each other; first principle — the cochlea's basilar membrane is a frequency-spatial map, so distinct bands occupy distinct neural real estate. A sword swing layered as low thump + mid woosh + high crackle reads as one unified weapon-event because each layer triggers a different cluster.
- **Tool**: Audacity (free), Reaper ($60 indie), any DAW with EQ.
- **Novice failure mode**: Stacking 8 layers all in the same frequency band — sounds thicker but actually muddy.
- **Priority**: Important — distinguishes "indie sounding" from "pro sounding."

### Bfxr / ChipTone / jfxr / sfxr — procedural retro SFX
- **Brief**: One-click random-generation SFX tools. Lineage: sfxr → as3sfxr → Bfxr → ChipTone (and parallel browser fork jfxr).
- **Why-drilled**: L1 — you can produce a usable laser/jump/coin in seconds; L2 — they expose a small parameter space tuned to retro games; first principle — they're aliasing-friendly waveform synthesizers (square/saw/noise) plus envelope and pitch-sweep, which is exactly the synthesis vocabulary of NES/Atari hardware. The aesthetic isn't a filter — it's the synthesis primitive.
- **Tool**: bfxr.net, sfbgames.com/chiptone, jfxr.frozenfractal.com, sfxr.me.
- **Novice failure mode**: Shipping all eight pre-set categories with default randomization — sounds like every other Bfxr game.
- **Priority**: Important if your aesthetic is retro/chiptune.

### Audacity / Reaper / FL Studio / GarageBand / LMMS
- **Brief**: General-purpose audio editors. Audacity (free, basic edit/trim/normalize). Reaper (cheap full DAW). LMMS (free DAW, FL-style). GarageBand (free on macOS).
- **Why-drilled**: For a non-audio-engineer, you only need: **trim, normalize, fade-in/out, pitch-shift, EQ**. Audacity covers all five. Reaper is worth $60 if you want one tool that grows with you.
- **Novice failure mode**: Buying FL Studio because of YouTube tutorials, never getting past the first Bus.
- **Priority**: Audacity = important. Anything else = optional.

### Beepbox.co — browser chiptune
- **Brief**: Free in-browser tracker for short chiptune loops. No install.
- **Why-drilled**: Lowest possible barrier to a 16-bar loop. JSON-shareable URLs.
- **Novice failure mode**: Defaulting to its starter preset and shipping it.
- **Priority**: Use if you can't compose at all.

### Bosca Ceoil — Terry Cavanagh's tool
- **Brief**: Built by the VVVVVV / Super Hexagon dev specifically so non-musicians could compose. ~5-min learning curve, MIDI + chiptune presets.
- **Why-drilled**: Cavanagh is a non-musician who needed to score his own games — the tool encodes his subset of "what a non-musician actually needs."
- **Tool**: terrycavanagh.itch.io/bosca-ceoil.
- **Priority**: Important if you want your own music and have zero training.

### File formats: WAV vs OGG vs MP3
- **Brief**: Use **WAV for SFX**, **OGG Vorbis for music**.
- **Why-drilled**: L1 — different file sizes; L2 — different compression behavior; first principle — WAV is uncompressed (decoder cost zero, RAM cost high) so it suits dozens of short SFX firing per second; OGG is compressed (decoder cost moderate, file size small) so it suits one long music stream. MP3 had patent-licensing fees historically and even after expiry, OGG hits better quality at the same bitrate (Vorbis's psychoacoustic model is newer than MP3's). Godot supports all three; OGG is the convention.
- **Novice failure mode**: Importing 200 WAV music tracks and shipping a 2GB build.
- **Priority**: Critical — wrong-format mistakes are easy and costly.

### Sample rate and bit depth
- **Brief**: 44.1kHz, 16-bit is the floor. Higher rarely audible in shipped games.
- **Why-drilled**: First principle — Nyquist theorem: 44.1kHz captures everything up to ~22kHz, above human hearing. 16-bit gives ~96dB dynamic range, exceeding any consumer playback. Higher rates buy production headroom (margins for processing) but consumers can't hear the difference at playback.
- **Novice failure mode**: Recording at 96kHz/24-bit and shipping it — bigger files for no perceived benefit.
- **Priority**: Important.

### Bus structure: Master / Music / SFX / UI
- **Brief**: Four buses minimum. Players want separate volume sliders for music vs SFX.
- **Why-drilled**: L1 — players adjust music and SFX independently; L2 — bus-level effects (reverb, EQ, low-pass) are a one-line change vs editing every clip; first principle — audio mixing is a tree-reduction problem. Mixing happens at bus nodes, not leaves. Edit the leaves and you redo every leaf next week.
- **Tool**: Godot's Audio Bus panel (bottom dock). Add buses, route via `bus` property on `AudioStreamPlayer`.
- **Novice failure mode**: Routing everything to Master, then having to set 200 individual volumes when the player wants quiet music.
- **Priority**: Critical.

### Ducking
- **Brief**: Lowering music volume when an important SFX (dialogue, alert) plays.
- **Why-drilled**: L1 — important sounds get heard; L2 — auditory masking — sounds at similar frequencies cancel each other; first principle — the ear does **frequency-specific masking** within ~1/3 octave bands; pulling the music's mid-frequencies down 6dB during dialogue clears the mids without making the music feel absent.
- **Tool**: Godot bus effects + tween to lower a bus's `db` value during dialogue.
- **Novice failure mode**: Cutting music entirely on dialogue (jarring) or not ducking at all (player can't hear narration).
- **Priority**: Important if you have voice/narration.

### Spatial audio in 2D: AudioStreamPlayer2D
- **Brief**: Position-aware audio in 2D — pans left/right, attenuates by distance.
- **Why-drilled**: L1 — sounds come from where things are; L2 — pan + attenuation cue spatial location; first principle — interaural time/level differences and inverse-square loudness are the two cues the brain uses to localize sound. Free fake-3D from `AudioStreamPlayer2D`'s `position`, `max_distance`, `attenuation`.
- **Tool**: Godot `AudioStreamPlayer2D`.
- **Novice failure mode**: Playing a footstep on a non-spatial `AudioStreamPlayer` so it always sounds centered regardless of which side of the screen the player is on.
- **Priority**: Important.

### Adaptive / vertical layering
- **Brief**: One track playing across multiple layers; intensity adds layers, calmness drops them. Lena Raine's Celeste OST is the canonical example.
- **Why-drilled**: L1 — music responds to gameplay; L2 — same melodic spine, different orchestration; first principle — additive synthesis at the song scale. Raine wrote Reflection's third intensity tier first as a fully-rhythmic track, then **subtracted** elements to derive the calm version — guaranteeing seamless cross-fades because all layers share harmony and tempo. Horizontal layering (transitioning between distinct cues) is harder and usually not worth it for solo dev.
- **Novice failure mode**: Trying full FMOD-style horizontal cue branching for a Celeste-style game; should have done vertical layering.
- **Priority**: Optional — useful only if music-game-state coupling is your design goal.

### Loop seam: zero-crossing edits
- **Brief**: Loop points must land on a zero-crossing or you get a click every loop.
- **Why-drilled**: L1 — clicks at loop points sound broken; L2 — sample value jumps from non-zero to non-zero create a step function; first principle — a step function is a Dirac-like impulse, broadband white noise spike — the click. Cutting at a zero-crossing makes the join continuous.
- **Tool**: Audacity's "Find Zero Crossings" command (Z key).
- **Novice failure mode**: Trim-by-eye loops that click every 30 seconds.
- **Priority**: Critical for any music loop.

### Genre conventions
- **Chiptune**: Limitations are the aesthetic. Square/triangle/noise channels, 4-channel ceiling. Don't fight the constraint.
- **Orchestral / cinematic**: Hard to do well, easy to overplay. Solo dev usually can't write orchestral that sounds professional.
- **Ambient / drone**: Lena Raine territory. Cheap to do badly, hard to do brilliantly. Single sustained chords + texture.
- **Priority**: Pick one. Stay there.

### Voice acting vs syllabic mumbles
- **Brief**: Animal Crossing-style mumbles (Animalese) are 1/100th the cost of voice acting.
- **Why-drilled**: L1 — no localization burden; L2 — emotionally communicative without semantics; first principle — prosody (pitch contour, rhythm) carries 70%+ of emotional content of speech. Mumbles strip semantics, keep prosody. Ship globally without re-recording.
- **Novice failure mode**: Hiring voice actors before the game is shippable.
- **Priority**: Avoid VA until post-launch unless the game is dialogue-driven.

### License hierarchy: CC0 > CC-BY > royalty-free > commissioned
- **Brief**: CC0 = public domain, no attribution, free. CC-BY = free with credit. Royalty-free = paid once, no per-copy fee. Commissioned = paid for original.
- **Sources**: Kenney.nl (40,000+ CC0 assets, audio packs included), OpenGameArt.org (mixed CC0/CC-BY/GPL — filter), Freesound.org (filter to CC0; otherwise read each file's license). Incompetech (Kevin MacLeod, royalty-free with attribution). Free Music Archive (mixed). Soundsnap, Pond5 (paid, royalty-free).
- **Novice failure mode**: Grabbing freesound.org files without checking — many are CC-BY-NC (no commercial use) and would block your Steam release.
- **Priority**: Critical — get this wrong and you have a takedown problem post-launch.

### AI audio generation (2026)
- **Brief**: Suno, Udio, ElevenLabs Sound Effects, Stable Audio.
- **Why-drilled**: Speed of iteration is real. Legal status is unsettled — UMG/Concord/ABKCO filed a $3B suit against AI music companies; UK reversed AI-friendly training-data exemptions in 2026. US Copyright Office: fully AI-generated outputs are **not copyrightable** without human creative input.
- **Novice failure mode**: Shipping 100% AI-generated soundtrack expecting full ownership; Spotify/YouTube detection systems flag suspected AI in 2026.
- **Priority**: Use for placeholders + ideation; treat anything generated as "uncopyrightable" until provider gives explicit indemnification.

### Loudness: -14 to -23 LUFS depending on platform
- **Brief**: Streaming reference is -14 LUFS (Spotify/YouTube). PlayStation 2024 spec: -24 LUFS console / -18 LUFS mobile. No formal indie standard.
- **Why-drilled**: First principle — LUFS measures **perceived** loudness over time, not peak. Mastering louder than -14 LUFS just gets normalized down by streaming services and trades dynamic range for nothing.
- **Novice failure mode**: Maxing every clip to -1dBFS peak — flattens dynamic range, fatiguing within 5 minutes.
- **Priority**: Important — set a target and check with a free LUFS meter (Youlean Loudness Meter free version).

### Reverb / EQ — when not to add
- **Brief**: Both are leave-alone defaults for 95% of solo SFX.
- **Why-drilled**: Source assets from Kenney/freesound are pre-mixed. Adding reverb to "make it feel space-y" usually muddies them. EQ to fix masking, reverb to suggest a specific room, otherwise leave alone.
- **Priority**: Default = don't.

## Recommended novice audio pipeline

**Asset acquisition (45 min, once per project):**
1. Download Kenney's UI Audio + Digital Audio packs (CC0, no attribution required) — covers menus, beeps, generic SFX.
2. Pull from OpenGameArt CC0 sound effects collection for game-specific (impacts, footsteps, voice grunts).
3. For retro-aesthetic: open Bfxr or ChipTone in browser, generate 6–10 sounds per category (jump, hit, pickup, death, etc.), export as WAV.
4. For music: Beepbox or Bosca Ceoil for short loops (4–8 bars × your number of game states). Or Incompetech for ready-made attribution-licensed tracks.

**File prep (Audacity, 15 min per asset batch):**
1. Trim silence, normalize to -3dBFS peak.
2. Apply 5–10ms fade-in to avoid clicks.
3. For music loops: find zero-crossing, cut, export OGG.
4. For SFX: export WAV.

**Godot import:**
1. Drop assets in `res://audio/sfx/` and `res://audio/music/`.
2. In the import dock, set music OGG to "Stream" mode (decode-on-the-fly), SFX WAV to "ForceMono" if positional 2D.

**Bus setup (one-time):**
1. Open Audio panel (bottom dock).
2. Add buses: `Music`, `SFX`, `UI`. Route all to `Master`.
3. Save bus layout; load in autoload script on game start.
4. Hook UI sliders to `AudioServer.set_bus_volume_db(bus_idx, linear_to_db(slider_value))`.

**Per-event playback:**
1. Frequently-played SFX (footsteps, hits): wrap in `AudioStreamRandomizer` with 3–5 variations + ±10% pitch randomization.
2. Spatial events (enemy attacks, environmental): `AudioStreamPlayer2D` with `max_distance` ~500px.
3. UI / global events: `AudioStreamPlayer` on UI bus.
4. Music: one long-running `AudioStreamPlayer` on Music bus, swap stream on scene change with crossfade tween.

**Mix pass (last 10% of project):**
1. Play through each level with all sounds firing.
2. Adjust bus volumes (not individual clips) until master peaks at -6dBFS, integrated loudness around -16 to -18 LUFS.
3. Add ducking: tween Music bus down 6dB during dialogue.
4. Optional: low-pass filter on SFX bus when underwater/menu-paused.

## Must-include shortlist

1. **Audacity** — trim/normalize/zero-crossing.
2. **Kenney CC0 audio packs** — UI + digital + impact baseline.
3. **Bfxr or ChipTone** — procedural SFX in seconds.
4. **Beepbox or Bosca Ceoil** — non-musician music tools.
5. **Godot `AudioStreamRandomizer`** — kills repetition fatigue with one node.
6. **Godot 4-bus mix** — Master/Music/SFX/UI.
7. **OGG for music, WAV for SFX** — non-negotiable file format split.
8. **A loudness meter** (Youlean free) — get integrated LUFS in the right ballpark.

## Commonly oversold

- **FMOD / Wwise**: Built for 30-person AAA audio teams. FMOD has a free indie tier (<$200K revenue), but the integration friction with Godot, the build-portability hit, and the conceptual overhead are not worth it unless your design depends on parametric audio (live mix shifts driven by gameplay state with hundreds of parameters). Solo dev with built-in `AudioStreamRandomizer` + buses + ducking covers 95% of needs.
- **Surround / 5.1 / Atmos for 2D games**: Players ship on stereo headphones. Don't bother.
- **Custom voice acting before MVP**: Localization, re-records, casting overhead. Use mumbles.
- **24-bit / 96kHz "for headroom"**: Bigger files, no audible benefit at playback.
- **Analyzing every SFX in a spectrogram**: Noise. Trust your ears, mix in the actual game.
- **Big DAWs (FL Studio, Ableton) for ten 2-second SFX**: Audacity does it in 1/10 the time.
- **Heavy mastering chains**: Light limit + volume normalize is enough for a ship build.

## AI audio — verdict (late 2026)

**Net positive only as a placeholder + ideation accelerator.** Suno/Udio music generation produces credible 30-second sketches in seconds; ElevenLabs Sound Effects produces usable single-shots. **But:** the US Copyright Office holds that fully AI-generated audio is **not copyrightable**, meaning a competitor can lift your AI track and use it royalty-free. The 2026 UMG/Concord/ABKCO $3B suit and UK's reversal of AI training-data exemptions signal escalating legal risk. Streaming detection systems flag AI, which can affect post-launch promotion. **Practical rule:** generate placeholders to playtest with audio in place, then either (a) commission/license CC0/royalty-free for ship if budget exists, or (b) treat AI tracks as "in the public domain by accident" — fine for a free game, risky for a paid Steam release. Track each AI provider's indemnification policy: a few now offer commercial-use guarantees with caveats. Document what you used in case of post-launch challenge.

## Cross-references

- **Gamefeel/juice (research agent on game-feel)**: audio onset and screenshake are paired feedback; covered in Vlambeer's talk.
- **Asset pipeline**: license hierarchy applies to art assets too — same CC0/CC-BY rules.
- **Performance**: streaming OGG vs full-WAV-in-RAM is a memory budget decision; relevant to mobile builds.
- **Localization**: VA/text-to-speech is a localization multiplier. Mumbles dodge it.
- **Accessibility**: separate volume sliders are a baseline accessibility feature, not just polish.

## Sources

- [Godot AudioStreamPlayer2D docs](https://docs.godotengine.org/en/stable/classes/class_audiostreamplayer2d.html)
- [Godot AudioStreamRandomizer docs](https://docs.godotengine.org/en/stable/classes/class_audiostreamrandomizer.html)
- [Godot Audio streams tutorial](https://docs.godotengine.org/en/stable/tutorials/audio/audio_streams.html)
- [Godot Audio buses tutorial](https://docs.godotengine.org/en/stable/tutorials/audio/audio_buses.html)
- [Godot Importing audio samples](https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_audio_samples.html)
- [Godot 4 Recipes — Audio Manager](https://kidscancode.org/godot_recipes/4.x/audio/audio_manager/index.html)
- [Bfxr](https://www.bfxr.net/)
- [ChipTone (SFB Games)](https://sfbgames.com/chiptone/)
- [jfxr](https://jfxr.frozenfractal.com/)
- [jsfxr / sfxr.me](https://sfxr.me/)
- [Bosca Ceoil by Terry Cavanagh](https://terrycavanagh.itch.io/bosca-ceoil)
- [BeepBox](https://www.beepbox.co/)
- [Kenney UI Audio (CC0)](https://kenney.nl/assets/ui-audio)
- [Kenney Digital Audio (CC0)](https://kenney.nl/assets/digital-audio)
- [OpenGameArt CC0 Sound Effects](https://opengameart.org/content/cc0-sound-effects)
- [OpenGameArt CC0 Music](https://opengameart.org/content/cc0-music-0)
- [Free Game Audio Resources — Hackingtons](https://www.hackingtons.com/free-game-audio.html)
- [Where to Find Free Game Assets in 2026 — AssetHoard](https://assethoard.com/blog/where-to-find-free-game-assets-2026)
- [Vlambeer "Art of Screenshake" — INDIGO 2013](https://www.youtube.com/watch?v=AJdEqssNZ-U)
- [Game Audio Theory: Ducking — Game Developer](https://www.gamedeveloper.com/audio/game-audio-theory-ducking)
- [Sound Design for Video Games: A Primer — Game Developer](https://www.gamedeveloper.com/audio/sound-design-for-video-games-a-primer)
- [Layering Sound Effects — Pro Sound Effects](https://blog.prosoundeffects.com/sound-layering)
- [A Guide to Sound Design for Games — SFX Engine](https://sfxengine.com/blog/sound-design-for-games)
- [Best Practices for Game UI Sounds — SFX Engine](https://sfxengine.com/blog/best-practices-for-game-ui-sounds)
- [Using Audio Busses in Godot — Inglo Games](https://inglo-games.github.io/2020/04/22/audio-busses.html)
- [Godot-Mixing-Desk plugin (kyzfrintin)](https://github.com/kyzfrintin/Godot-Mixing-Desk)
- [Mixing Desk — Godot Asset Library](https://godotengine.org/asset-library/asset/299)
- [Lena Raine on Celeste's Synths — Medium](https://medium.com/@kuraine/a-little-bit-about-celestes-synths-and-some-bonus-piano-461f62605ea1)
- [Lena Raine interview — Original Sound Version](https://www.originalsoundversion.com/interview-composer-lena-raine-talks-celeste-soundtrack-working-in-game-audio/)
- [Lena Raine on Celeste & Minecraft — Spitfire Audio](https://composer.spitfireaudio.com/en/articles/lena-raine-on-celeste-the-minecraft-nether-update)
- [GDC Vault — Game Loudness, Industry Standards](https://gdcvault.com/play/1017781/Game-Loudness-Industry)
- [Loudness and metering in game audio — Anso Audio](https://ansoaudio.com/2016/07/27/loudness-and-metering-in-game-audio/)
- [Mastering a Game with Wwise — Audiokinetic](https://www.audiokinetic.com/en/blog/mastering-a-game-with-wwise-part1/)
- [Wwise or FMOD? — Game Audio Co.](https://www.thegameaudioco.com/wwise-or-fmod-a-guide-to-choosing-the-right-audio-tool-for-every-game-developer)
- [FMOD-Godot integration — FMOD forums](https://qa.fmod.com/t/how-does-fmod-integrate-with-godot/16130)
- [Godot audio import options — Blips Blog](https://blog.blips.fm/articles/understanding-the-audio-import-options-in-godot)
- [AI Music Copyright: Legal Risks — Silverman Sound](https://www.silvermansound.com/ai-music-copyright-legal-risks-content-creators)
- [AI copyright and licensing in 2026 — Artlist](https://artlist.io/blog/ai-copyright-licensing/)
- [U.S. Copyright Office on AI](https://www.copyright.gov/ai/)
- [Licensing AI-Generated Sound Effects — LawGratis](https://www.lawgratis.com/blog-detail/licensing-ai-generated-sound-effects)
