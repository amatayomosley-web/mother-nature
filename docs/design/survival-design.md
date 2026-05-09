# Survival Game Design Document

> Working version for design iteration. Original at `docs/originals/survival-design.docx`.
> Edit this file directly. Re-extract from the .docx if William updates the original.

---

Working title TBD — Design Synthesis v1

## 1. The Pitch

A 2D top-down multiplayer survival simulation where nature is the antagonist and the only goal is to live. No bosses, no quests, no win condition. Heat, cold, hunger, thirst, exhaustion, injury, and weather behave like a real body answering a real environment. The entertainment isn't progression — it's the player's own learning curve. Every run is a different survival problem, and the meta-game is everything you carry from one life to the next.

Comparable titles: Project Zomboid without the zombies, The Long Dark with other people, Rust without the raid meta. A survival simulation where the entertainment is the player's own learning curve, and where the only thing they keep when they die is what they've come to understand.

### Design North Star

Every run is a different survival problem, and the meta-game is the player's own learning curve.

This principle is the test for every design decision: does it create variance between runs, and does it reward learning? If yes, build it. If no, cut it.

### Platform & Engine

- Target platform: Steam (PC)
- Engine: Godot — chosen for AI-assisted development workflow (text-based scenes, GDScript, full text-editable wiring of nodes, signals, and resources)
- Art style: 2D top-down or isometric

## 2. The Learning Curve as Game

Players survive longer than their last run. Each run is meaningfully different from the last. The compendium grows. The player's own knowledge grows. The game has no progression bars to fill — it has a world to learn, and learning it is the reward.

### Variance vs. Randomness

There is a critical distinction between random and variable. Randomness — "the bear attacks 20% of the time" — feels unfair and teaches nothing because there is no signal to read. Variability — "this bear is in a defensive state because there are cubs nearby, and the player can see them if they look" — produces different outcomes across runs and rewards the player who learns to read the signs.

Every system that produces variance must also produce signals the player can learn to read.

### Sources of Per-Run Variance

- Spawn variance: biome, season, time of day, starting weather. A spring forest spawn is a fundamentally different game than a late-autumn tundra spawn.
- World seed: hand-authored map geography with procedural overlays — animal populations, plant distributions, water sources, seasonal effects shift per seed. The land is learnable; the ecology is per-run.
- Anomaly events: rare per-run conditions that change the survival problem — a drought year, a harsh winter, a wildfire that already burned a region before the player spawned. Roughly one notable anomaly per run.
- Animal individuality: each meaningful animal has its own state, needs, and territory; encounters emerge from individual circumstance, not spawn rolls.
- Weather emergence: weather is simulated, not scripted; no two runs have the same storm sequence even on the same map and season.

## 3. The World

### 3.1 Launch Biomes

Three launch biomes, each its own survival problem. Same systems, but the dominant threats, survival priorities, and daily rhythm differ enough that mastery in one biome does not equal competence in another.

#### Deep Forest

The forgiving one — relatively. The recommended starting biome for new players. Where the compendium fills fastest. The survival problem is abundance with hidden danger.

- Climate: Temperate, four real seasons. Moderate rainfall. Long growing season. Liquid water everywhere.
- Resources: Plentiful. Wood for everything. The widest plant diversity in the game — including the highest density of toxic lookalikes. Game is plentiful and varied.
- Threats: What you don't see. Dense canopy hides predators, weather, and routes. Visibility is limited; sound carries strangely; scent moves unpredictably under canopy. The forest punishes inattention more than incompetence.
- Signature experience: Getting lost. The forest is dense and repetitive, and a player who hasn't built a way to mark their path or read the sun can lose track of where they are.

#### Desert

The brutal one for new players, the puzzle one for veterans. The survival problem is scarcity managed against time.

- Climate: Hot days, cold nights. Two effective seasons: a long hot/dry season and a brief wet/cool season with flash floods and rare rainfall.
- Resources: Sparse. Wood is rare and scrubby. Water is the constant problem — sources are few, far apart, often shared with predators. Specialized plant life. Game is scarce but high-value when found.
- Threats: Heat exhaustion, dehydration, hypothermia at night, sandstorms, flash floods in canyon terrain, venomous animals. The visibility paradox — you can see for kilometers, but so can everything else.
- Signature experience: The water calculation. Every day is a math problem: how much water, where is the next source, can I get there before dehydration becomes critical, and what's between me and it.

#### Tundra

The lethal one. Where veteran players go to test themselves and where most first-time visitors die quickly.

- Climate: Cold most of the year, brutal in winter. Brief intense summer of perpetual daylight. Long winter of perpetual or near-perpetual night. Treacherous shoulder seasons.
- Resources: Concentrated and seasonal. Driftwood and stunted trees only. Short summer plant abundance. Game follows migrations strictly — missing the migration window means missing the year's main calorie source.
- Threats: Cold above all. Wet-cold from falling through ice has minutes to kill, not hours. Frostbite as permanent injury. Whiteouts. Apex predators desperate in winter. The dark itself in deep winter — six or more hours of barely-twilight, eighteen of full night.
- Signature experience: The first winter night the fire goes out. Everything else in the game leads up to "can I keep this fire alive."

### 3.2 Map Sizes

Map size scales with the play style. A small map for seven friends on a private server. A continent for thousands on a public world server. Same systems, same stakes — only scope changes.

### 3.3 Weather Simulation

Weather is simulated, not scripted. A coarse atmospheric model produces emergent weather rather than rolling from a table or running on a schedule.

#### Approach: Coarse-Grid Simulation

A grid of cells over the map (roughly 50–200 cells depending on map scale) maintains atmospheric state — temperature, pressure, humidity, wind vector, cloud density, precipitation. The simulation runs at low frequency. Visible weather effects (rain, wind gusts, cloud cover, fog) are interpolated and stylized on top.

This produces most of the emergent behavior of true atmospheric simulation at a fraction of the cost. Pressure systems drift across the map; fronts arrive over hours; warm moist air condenses into clouds and precipitation; mountains create rain shadows; valleys trap cold air. None of this is special-cased — it falls out of the simulation.

#### Player-Visible Effects

- Sky shader, rain particles, wind animation, fog density, temperature on HUD all read from the simulation cell the player occupies.
- Audio: thunder distance, wind howl, rain on canopy.
- Predictability for skilled players: the player who learns to read the sky, the wind, and the pressure trend (eventually with a craftable barometer) can forecast the weather. This is a survival skill that mirrors a real one.
- Storms have approach, peak, and aftermath. The player sees the front coming for an hour. Sky darkens, animals go quiet, wind shifts.

#### Microclimates

Terrain feeds back into the atmospheric grid. Valley floors are reliably foggier in the morning. South-facing slopes melt snow first in spring. Coasts are milder than the interior. Players learn the map in a way no scripted system can teach.

## 4. The Day-Night & Seasonal Cycle

### 4.1 The Two Clocks

- Day length: how long a single in-game day takes in real time. Sets the texture of moment-to-moment play.
- Season length: how many in-game days in a season. Sets the arc of a run.

### 4.2 Day Length

Default: 60 minutes of daylight, 20–30 minutes of night. Total cycle 80–90 minutes.

Day is the game; night is recovery. Real survivalists don't operate at night — they wake at dawn, work through the day, are at shelter before dusk. The game reflects this. Day is the survival problem; night exists for sleep and is punishment for poor planning when the player is caught out.

A 60-minute day has shape — morning, midday, afternoon. There is time within a day to do real work (a hunt, a build, travel) and time to be in the world between urgent activities. A 30-minute day delivers a survival game; a 60-minute day delivers a survival simulation.

The 20–30 minute night is short enough that a player caught out is not sentenced to an hour of hiding, but long enough to have real teeth when conditions are bad.

### 4.3 Sleep & the Time-Skip

Sleep is a conditional time-skip. The player initiates sleep at a prepared sleeping spot (bedroll, shelter, fire). The game compresses 20–30 minutes of in-game night into roughly 30 seconds of real time. The player wakes at dawn rested.

The skip is conditional on: adequate warmth (fire, shelter, clothing), sufficient food and water reserves, reasonable safety (no predator within detection range, no immediate environmental hazard), and hunger/thirst not in critical range.

If conditions go bad during the skip — fire goes out, predator approaches, weather turns — the game interrupts the skip and drops the player back into real-time at the moment of the disturbance. They wake up to a wolf at the camp edge, or to rain on their face, or to embers and creeping cold.

Prepared players effectively get a "skip night" privilege earned by their day's work. Unprepared players lose that privilege and live through what they brought on themselves. Same mechanic, different experience based on preparation.

#### Resting (When Sleep Isn't Possible)

If the player can't sleep (inadequate conditions, dangerous area, no shelter), they can rest in real-time. Rest passes time slightly faster (~1.5x) but doesn't time-skip. The player stays in control, can react to threats, and survives the night with their thumbs on the wheel — at the cost of poor recovery for the next day.

### 4.4 Seasonal Day-Length Scaling

Day length varies dramatically with biome and season. The same atmospheric simulation that drives temperature drives day length, modulated by latitude (biome) and time of year. This is one of the highest-leverage atmospheric levers — it makes the seasonal experience feel different in a way no other mechanic can replicate.

#### Approximate Day-Length Distribution

- Forest, summer: 70 min day, 15 min night. Long days, brief twilight nights.
- Forest, winter: 35 min day, 25 min night. Days are tight; nights have weight from cold.
- Desert, summer: 75 min day, 15 min night. Brief precious cool window. Inverted strategy possible (travel by night).
- Desert, winter: 50 min day, 25 min night. Cold nights with frost.
- Tundra, summer: 80 min day, 10 min night. Near-perpetual day; the midnight-sun effect.
- Tundra, winter: 20 min day, 30 min night. The signature horror — only place in the design where night outweighs day.

#### Floor & Ceiling

- Minimum night length: ~10 minutes — even at its shortest, night is still a thing.
- Maximum night length: ~30–35 minutes — past this, players will not roleplay through it.
- Twilight: 3–5 minutes of dawn/dusk. Crepuscular predators are most active here. Don't compress out.

### 4.5 Season Length

Default: 8 in-game days per season, 32-day year.

Each season is a distinct survival problem with its own threats, opportunities, and correct strategies. The eight-day length is long enough that each season has internal arc and feeling, short enough that a dedicated player sees a full year in 2–4 sessions.

#### Server-Configurable Presets

- Standard: 8 days/season, 32-day year. Default.
- Extended: 14 days/season, 56-day year. For dedicated groups who want each season to be a chapter.
- Compressed: 4 days/season, 16-day year. For learning runs, single-session play, or hardcore players.

### 4.6 The Four Seasons (Forest Template)

#### Spring — Establishment

The world recovers. Snow melts; ground becomes mud; rivers swell with snowmelt; plants come back; bears emerge desperate and irritable. Migrations arrive — birds return, fish run, ungulates move out of winter range.

- Threats: mud, wet-cold (hypothermia in spring kills players who think the cold is over), violent storms with sudden temperature drops, aggressive emerging bears, the "hungry gap" where winter stores are gone but new growth isn't ready.
- Opportunity: fish runs (huge food windfall), nesting birds, medicinals at peak harvest, rapidly lengthening days.
- Strategy: establish position, build the shelter you'll keep all year, learn local geography, stockpile preservation materials.

#### Summer — Harvest

The world gives. Long days, short nights, warm temperatures. Plant life at maximum. Animals at healthiest body weights. Predator activity peaks because prey is abundant.

- Threats: heat exhaustion and dehydration, insect-borne illness, parasites, fevers, harder-to-keep-clean wounds, thunderstorms, defensive predators raising young — and the psychological trap of summer's apparent ease.
- Opportunity: the harvest. Calories should come in faster than they're going out. Smoking, drying, salting, fermenting. Tanning hides. Running traps. Tool upgrades.
- Strategy: convert abundance into stored reserves. A summer that didn't prepare for winter is a summer wasted, and the next two seasons will tell you so.

#### Autumn — Preparation Deadline

The most strategic season. Days shorten visibly. Temperature drops. First frost arrives partway through. Leaves fall — visibility in the forest dramatically increases. Animals at peak body weight. Bears pre-hibernation hyperphagic and aggressive about food. Migrations begin in reverse.

- Threats: the hungry shoulder in reverse, dramatic temperature swings (warm afternoon to freezing night), the most dangerous bear encounters of the year, predators bulking up.
- Opportunity: the big hunt window. Largest single-kill calorie windfalls. Last harvests. Last good window for traveling — winter locks the map down.
- Strategy: top off everything. Verify stores. Reinforce shelter. End autumn with full provisioning, ample fuel, processed food, layered cold-weather clothing, and a sense of where migrating animals went.

#### Winter — The Test

The world takes back. Short days, long nights. Sub-freezing temperatures. Storms hit harder and last longer. Snow accumulates and changes the world — tracks become readable like nowhere else, but movement slows, fires need more fuel, water is locked up. Many animals migrate out, den, or die. The animals that remain are the ones built for the cold: wolves at peak hunting form, scavengers, overwintering birds.

- Threats: cold as dominant threat. Wet-cold is fast-acting and lethal. Frostbite as permanent injury. Snow-blindness. Whiteouts. A fire going out at 3 AM in a winter storm is a death sentence the player saw coming for an hour. Wolves at most dangerous because prey is lean.
- Opportunity: tracking at its best. Some big game more vulnerable in deep snow. Cold preserves meat indefinitely. Surviving a winter yields disproportionate compendium and challenge progress.
- Strategy: conserve. Stay close to base. Burn fuel efficiently. Hunt selectively. Trust the storehouse. Sleep through the worst nights. Use long dark hours for repair, processing, and long-form crafts. Wait for spring.

### 4.7 Biome Distortions of the Seasonal Template

All biomes share the same calendar synchronization, but each season's content is biome-specific.

- Desert: Two effective seasons matter — a long hot/dry stretch and a brief wet/cool window where everything that grows in the desert grows, where flash floods are real, and where conditions are forgiving enough to seek out. Migration is toward water sources, not toward warmer ground.
- Tundra: A brief intense summer of perpetual daylight, plant abundance, melted ground, calving herds — followed by a long brutal winter of darkness, cold, and scarcity. Spring and autumn are short treacherous transitions with breakup ice, freeze-up ice, and sudden storms. Players who don't harvest the boom intensely will not survive the bust.

### 4.8 Spawn Season

New characters spawn into a safe season window. For the forest: spring or summer. For desert: the wet/cool season. For tundra: summer. Veterans who have earned harder spawn windows through challenges ("survive a winter") may opt into them. Custom characters may be configured to spawn into any season the player has earned.

This applies across all server tiers. On public world servers with continuous clocks, new spawns are gated to whatever the easiest currently-available season window is across the map — even if that means spawning in a different biome than intended. Players walk to the biome they want once they're alive.

### 4.9 The Year as Meta-Arc

Surviving a full year on a single character — all four seasons, full calendar, returns to spring — is a major meta-progression milestone. By that point all season-locked compendium entries (migratory animals, season-specific plants, seasonal weather phenomena) have been unlocked. The player has, in a real sense, learned the world. "Survive a full year" is a candidate marquee challenge feeding into the custom-character unlock.

## 5. Wildlife

### 5.1 The Principle: Animals Are Agents, Not Encounters

In most survival games, animals are encounters — walk into a zone, animal spawns, fight or flee, encounter ends. In this game, animals are agents living in the world whether or not the player is there. They have hunger, thirst, fatigue, territory, social bonds. Most of the time they are doing things that have nothing to do with the player. When the player intersects with one, they are walking into the middle of its day, not triggering its appearance.

This is the source of every emergent story the game produces. A scripted bear attacks when you enter its zone — predictable, gameable, eventually boring. A simulated bear is hungry, hasn't eaten in three days, smells the deer carcass on the player's back, and is now stalking them across two kilometers of forest. The first is a mechanic. The second is a story.

Not every animal is deeply simulated — squirrels don't need a hunger meter. But every animal that matters to survival (predators, large prey, anything dangerous, anything edible) runs the same kinds of needs the player runs.

### 5.2 Tiered Danger

The respect-without-paralysis balance comes from this distribution. Players need to feel that most animals are not a threat, some require caution, and a few are genuinely dangerous — and they need to be able to tell which is which on sight.

- Ambient fauna: birds, squirrels, fish, insects. React to player but pose no threat. Functionally signal — birds going silent means something larger moved through. Fish surfacing means the water is healthy. Insects out in force means the air is warm and still.
- Small prey: rabbits, hares, pheasants, ducks, small fish. Edible, catchable with patience and the right tools. Where basic hunting skills are learned.
- Large prey: deer, elk, moose-as-prey (when not in rut), wild boar. Calorie windfalls. Hard to take down without proper tools or traps. Bringing one home is a multi-hour project that feeds the player for days.
- Mid-tier predators: wolves (especially in packs), coyotes, lynx, wolverines. Reactive threats — generally avoid healthy adult humans but pursue weakness, scavenge corpses, harass camps. Most player-predator encounters are this tier.
- Apex predators: bears (brown and black behave differently), big cats (cougar in appropriate biome), and contextually dangerous large herbivores (moose in rut, wounded boar). Uncommon encounters that genuinely change the player's plans for the day. Fighting is almost never the right answer.

Proportional rule: an average player on an average run might have one genuine apex encounter per in-game week, several mid-tier predator encounters, and constant background contact with prey and ambient fauna. Apex encounters are rare; their presence in the world — tracks, scat, distant sounds, scavenged kills — is constant. The player should always know there are bears in this forest and rarely see one.

### 5.3 Behavioral States

Every meaningful animal has a current state and transitions between states based on its needs and the world. The state determines reaction to the player. The player can't directly see the state, but the signs of the state should be readable — body posture, pace, what the animal is doing, time of day, time of year, environmental context.

- Resting / sleeping: low alertness; won't attack unless directly threatened.
- Feeding: focused on food, defensive of it. A bear at a carcass is the most dangerous bear.
- Hunting / foraging: active, alert, looking for food. Predators in this state may track players carrying food or smelling of blood.
- Traveling: moving between locations. Lower threat than hunting; higher awareness.
- Defensive: triggered by threat. A bear with cubs nearby; a wounded animal; a cornered animal. The most dangerous state for the player to encounter.
- Fleeing: running from something. Possibly the player; possibly something bigger.
- Mating / rutting: seasonal; erratic and aggressive. Where seasons interlock with wildlife to produce per-run variance.

Critical example: a bear standing on its hind legs is not in attack posture — it's trying to see and smell better. A player who learns this from a high-tier compendium entry has an advantage; a player who panics and runs has triggered a chase response.

### 5.4 Senses & Detection

Animals have sight, hearing, and smell as separate detection systems with different ranges, reliabilities, and environmental modifiers.

- Sight: line of sight, limited by terrain, vegetation, and light. Movement is more visible than stillness. A crouched player behind a bush can be invisible; the same player walking past at distance is spotted.
- Hearing: omnidirectional but attenuated by distance, vegetation, and ambient noise (wind, rain, river). Footstep noise depends on substrate and pace. Crafting sounds, voices, and fire are detectable.
- Smell: the most underused sense in survival games. Carried by wind. A bear three kilometers downwind of a fresh kill knows about it. A wolf upwind doesn't. This single mechanic produces enormous variance per encounter and rewards players who consider wind direction. It also makes carrying and storage of food strategic.

These three senses also give the player levers. Cover scent (some plants, mud, smoke). Crouch and move slowly to defeat sight. Wait for wind, rain, or river noise to mask sound. The player isn't fighting predators — they're working around them.

### 5.5 Population, Territory & Ecology

Animals do not respawn from spawn points. They exist, in finite numbers, with territories and home ranges, and reproduce slowly over seasons.

- Over-hunting a region depletes it. The player who stays in one valley shooting deer eventually has no more deer.
- Predator-prey populations are linked. Removing predators can cause prey booms then crashes.
- On multiplayer servers, this creates real territory dynamics — popular hunting grounds get hunted out; players who range further or hunt sustainably have an edge.

#### Implementation Approach

Population dynamics run on a per-region, per-species basis as a coarse simulation. Individual animals are spawned only as players approach. The abstraction matters: when an animal is killed, the regional population genuinely decreases by one and recovers slowly.

#### Apex Predator Identity

Apex predators near the player are tracked individually with persistent identity ("this bear has a damaged ear and avoids the river"). Ambient population members aren't. This middle path gives memorable apex encounters without paying the cost of individual tracking for every animal in the world.

### 5.6 Migration & Seasonality

Migrations are the strongest per-run variability lever for wildlife. The same map in spring vs. autumn has fundamentally different animals available in different places.

- Deer move down from the highlands in autumn.
- Fish run upriver in spring.
- Bears den in winter (and become threats again in spring when they're starving).
- Caribou-equivalents in tundra move from summer range to winter range.
- Birds migrate seasonally.

The player who learns the calendar gets food the player who doesn't will starve looking for. This is survival-as-curriculum baked directly into the world.

### 5.7 Tracks, Signs & Reading the World

Tracks are physical objects in the world — left by every animal as it moves, with characteristics that depend on species, size, substrate (mud, snow, dry ground, sand), and time elapsed (fresh tracks are sharp; old tracks degrade with weather).

Other readable signs: scat, scratch marks on trees, hair caught on bark, partially eaten kills (different predators leave different remains), broken vegetation along game trails, beds where deer have lain. The forest should be full of information for the player who learns to see it.

This works in two directions. The player tracks animals to hunt or avoid. Predators track players the same way — a wounded player leaves a blood trail and a wolf will follow it for kilometers. Smell-as-tracking complements vision-tracking. The wounded player who thinks they've escaped because they're out of sight is in a worse position than they realize.

### 5.8 Domesticated Animals — Deferred

Dogs, horses, and other tameable animals are deliberately deferred from launch. They are huge design levers (a dog dramatically changes threat profile via early predator detection; a horse changes traversal economics) and warrant being built as a deep system in a major content update rather than rushed for launch. AI architecture should be built with their eventual addition in mind.

## 6. The Body

Vital systems are systems, not meters. Each is deep enough that managing it is a real decision rather than a slider to top off.

#### Cold

Not a single slider. Core temperature affected by ambient temperature, wet vs. dry state, wind chill, what the player is wearing, and activity level. Wet-cold is faster than dry-cold. A player who falls through ice has minutes, not hours.

#### Heat

Heat exhaustion and dehydration linked but distinct. Time-of-day matters in desert and summer. Activity level interacts with ambient heat. Shade, water, clothing choice all factor.

#### Hunger

Calories, but also calorie type (fat vs. protein vs. carbs), digestion time, food poisoning risk from spoiled food, nutritional deficiency over weeks.

#### Thirst

Water consumption tracked. Source quality matters — purified vs. running vs. stagnant. Waterborne illness is real and slow to develop.

#### Exhaustion

Stamina for physical action; fatigue accumulating over the day; sleep debt accumulating across days. Hands shake when tired or cold (relevant for fine tasks like fire-starting). Recovery requires real sleep, which requires the conditions described in §4.3.

#### Injury

Wounds bleed, infect, scar. Bone fractures impair movement long-term. Frostbite leaves permanent reduced dexterity in affected fingers and toes. Snow-blindness as temporary visual impairment. Bites and stings carry venom and disease.

### Death-as-Lesson

Permadeath is locked. When the player dies, that life is over. Their corpse stays where it fell and so does everything they carried. There is no expected lifespan — a seasoned player making a routine water run can cross a hostile bear and be gone in thirty seconds.

But every death is meant to be a lesson, not a punishment. Predators give warning. Environmental dangers have onset. The death screen reconstructs what killed the player and why — core temperature graph, last meal timestamp, wound timeline. This converts every loss into a learning artifact rather than a frustration. "That was bullshit" becomes "oh, I see what I did."

## 7. Characters

### 7.1 The Roster

Twelve characters at launch. Each starts with different equipment, different physical traits, and a different specialization in the crafting tree.

Examples (full roster TBD):

- Hunter — masters traps, butchery, animal behavior. Strong in tundra and forest.
- Medic — wound care, infection treatment, herbal remedies. Centrally valuable in tundra (frostbite) and desert (heatstroke, venom).
- Forager — plants that heal and plants that kill. Strongest in forest, weakest in tundra.

### 7.2 Soft-Locked Crafting

Specializations are soft-locked, not hard-locked. Craft enough within your tree and you'll begin to glimpse the next, slowly broadening what you can make. Mastery in everything is a long road; other players will almost always be a faster one.

The soft-lock mechanic: making enough items in one tree gains insight into the next level — randomized skill upgrades that incrementally open adjacent crafting nodes. This preserves character identity while preventing solo isolation from feeling impossibly punishing.

### 7.3 Character–Biome Balance

No single character is optimal in all three biomes. The forager is strongest in forest, weakest in tundra. The hunter has roughly even utility but excels in tundra. The medic's value is most visible in tundra and desert. This is a feature — players naturally rotate characters when rotating maps, and the compendium fills out across both axes.

### 7.4 Custom Character Unlock

After completing enough developer-designed challenges across runs, the player unlocks the ability to build a custom character of their own. The challenges are universal (not per-character) and exist to ensure the player has genuinely engaged with the game's systems before configuring their own.

Example challenges: survive 15 days with a single character; walk X amount of steps; treat a wound in the field; weather a blizzard without shelter; survive a full year. The point is that before a player can make a custom character they have learned the different game systems through experience.

## 8. The Compendium

### 8.1 Purpose

A persistent personal knowledge base that fills only through play. The compendium is the player's meta-progression. Empty at start. Every plant, animal, weather pattern, and crafting recipe the player encounters generates an entry. Across runs, the compendium is the only thing the player keeps.

### 8.2 The Critical Design Insight

The compendium is for when the player is operating outside their current character's specialty. The expert character does not need the compendium for things in their domain — they have the knowledge, and the recipes are unlocked in their crafting tree directly. The compendium exists for the gap between what the current character knows and what past characters have learned.

#### Player Loop

Play a forager. The forager learns plants deeply. Those plant recipes are in the forager's crafting tree, available in real-time without ever opening a book.

The forager dies. The plant knowledge gets recorded into the compendium at expert tier.

Now play a hunter. The hunter's crafting tree doesn't include plant recipes. But when the hunter encounters a plant, they can consult the compendium and read the expert-tier entry the forager wrote. The hunter knows what's safe, what's dangerous, what's nutritious — but cannot craft with the plant the way a forager could (their hands don't know how). Some plant uses are accessible to anyone (don't eat the wolfsbane); some require the specialist (brewing it into a sedative).

This means specialists become genuinely valuable to other players in multiplayer. The hunter knows wolfsbane is poisonous because the compendium says so. The hunter cannot turn it into a sedative — only a forager can. Specialists trade not just goods but capabilities.

### 8.3 Skill Tiers (Static)

Entries do not have character voice. They are static skill-level recordings, written in neutral reference tone. The player can trust the compendium as a reference.

- Unknown: no entry. The plant or animal is just a colored shape with no name.
- Glimpsed: "A red berry. Looks similar to ones I've seen before, but I'm not sure." Generated when a non-specialist character encounters something briefly. Useless except as a placeholder.
- Novice: "Wolfsbane berry. Poisonous." Bare minimum — name and headline danger. Generated by a non-specialist who has actually learned the consequence.
- Practiced: "Wolfsbane berry. Poisonous if eaten. Causes nausea within an hour, can be fatal in quantity. Often grows near streams in shaded areas." Generated by a non-specialist with sustained experience, or a specialist on first encounter.
- Expert: "Wolfsbane (Aconitum). Highly toxic — alkaloid poisoning. Symptoms within 30 minutes: numbness, nausea, irregular heart rate. Lethal dose is small. Roots and leaves equally toxic. The dried root, in carefully measured quantities, can be used as a sedative — but the margin between sedation and death is narrow." Generated only by the relevant specialist with experience.

The expert entry is not just more text — it contains actionable information the novice entry doesn't. The expert knows wolfsbane has a use case as a sedative or weapon coating; the novice just knows not to eat it. Specialist runs are genuinely valuable to a long-term player because they're the only way to unlock certain uses of certain things.

### 8.4 Entries Can Be Wrong

If a novice writes an entry based on partial information, the entry can contain mistakes. "Red berries: poisonous" — but actually only some red berries are poisonous, and the novice didn't know to distinguish. A future character checking the compendium gets bad information from a past one. Only when a more expert character encounters the same thing does the entry get corrected or expanded.

This is a load-bearing mechanic. The compendium itself gets better over time as more specialist runs happen, and it teaches the player that their own past knowledge is provisional. It also gives a real reason to play characters the player doesn't normally play — to upgrade entries stuck at novice quality.

Low-confidence entries are visually flagged. Entries are upgradeable, not just overwritten — the expert entry may preserve the novice's original observation as a footnote, because the novice may have noticed something the expert dismisses.

### 8.5 Cost of Consultation

The compendium is a real object, not a wiki. Pulling it out costs:

- Time: the world keeps running. Predators don't wait, rain keeps falling, cold keeps biting. No consulting during a fight or while sprinting.
- Hands: reading occupies the player's hands. Can't be holding a torch, carrying water, or nocking an arrow.
- Light: can't be read in the dark without a light source.

Optional refinements: physical wear (the book degrades, pages get water-damaged); no symptom-based search (must flip to plant section trying to remember which one looked like the one eaten). The compendium is a book, not a database.

### 8.6 First-Run Behavior

The compendium is empty at start. No seeded "common knowledge" entries — every entry must represent something the player has personally lived through. Seeding entries the player didn't earn would erode the system's premise.

To compensate: the world is visually readable enough that a new player can survive without a populated compendium. Ripe berries look ripe. Predators look dangerous. Clear running water is probably drinkable; stagnant pool water is probably not. The compendium is the deep-knowledge layer; visual cues handle the surface layer.

First-run learning is forgiving in terms of how entries are earned (low interaction cost to generate first novice entries) but uncompromising in terms of what they represent. The first death is the first real lesson, and the resulting compendium entries — the things that just killed or saved the player — mean something.

### 8.7 Cross-Tier Persistence

Compendium content carries across all server tiers. Knowledge built in solo carries to private and public servers. By the time a player walks onto a world server, their compendium represents real hours of learning.

Open question for later resolution: whether the compendium is per-account or per-server on public world servers. Per-account is more rewarding but creates a soft pay-to-win dynamic for veterans on public servers. A possible middle path: per-account, but with read-only restrictions on entries until the player has survived some baseline threshold on that server.

## 9. Multiplayer Architecture

### 9.1 Three Tiers

Players progress outward from solo through private to public world servers. Each tier serves one need cleanly rather than every server trying to be everything.

#### Solo / Tutorial Tier

- Where new players learn. Where everyone builds compendium content.
- Single-player or fully instanced. Clock pauses when not playing.
- Complete game in itself — all systems, all biomes, full character roster (with early-unlocked subset visible to first-time players), full permadeath.
- Compendium and challenge progress transfers to all higher tiers.
- Unlocked from the start.

#### Private Server Tier

- Friend groups of 2–10 (configurable to small tight maps).
- World pauses when no one is online (default on, host-toggleable).
- Same systems and content as solo. Same character roster. Same compendium.
- Unlocked after baseline competence (e.g., complete one survived run, or survive 24 hours on a single character — exact threshold tunable in playtest).

#### Public World Server Tier

- Hundreds to thousands of players on persistent shared worlds.
- Continuous global clock — runs whether or not players are online.
- Regional timezone alignment — server clock aligns to primary regional player base. Servers tagged by region (e.g., "World Server EU", "World Server NA-East").
- Real seasons, persistent player camps, multi-day events that affect everyone simultaneously.
- Unlocked after demonstrated competence — a serious threshold (e.g., survive 7 days on a single character; reach a compendium completeness mark; weather a winter).

### 9.2 Why Tiered Access

New players don't enter world servers. This is a positive design decision, not a workaround. It respects the world server's integrity (no compromised difficulty, no special spawn accommodations needed) and respects new players (who deserve to fail in dignity, not in front of a thousand-player audience).

It also creates aspirational progression. Earning world server access becomes a meaningful milestone. By the time a player walks into the persistent shared world, they have a compendium, instincts, and the right to the experience.

Gates are one-time per account, not per character. A veteran with thirty deaths doesn't re-prove themselves on world server access just because their last character died.

### 9.3 Player Interaction — Jungle Rules

Jungle rules — the same as meeting a stranger in the wild in real life. The player reads them, they read the player, and what happens next is on both of you. No mechanical alignment system, no faction structure. Trade, cooperation, conflict, and avoidance all emerge from the systems and the situation.

The character-locked crafting trees give specialists genuine value to other players. The compendium-versus-tree distinction makes that value durable: knowing a thing and being able to use a thing are different, and only specialists can use the things their tree includes.

## 10. Meta-Progression

### 10.1 What Persists

- The compendium (with skill-tier entries, possibly with low-confidence flags).
- Challenge completion progress.
- Server tier unlocks (private access, public access).
- Custom character unlock and configuration.
- Spawn-window unlocks (e.g., earned ability to spawn into a winter season).

### 10.2 What Doesn't

- The character. Permadeath is total.
- Inventory, gear, shelter, stored food. All on the corpse, where the corpse fell.
- In-character knowledge of the current run's specifics — locations of resources, tracks, weather patterns.

### 10.3 The Challenge System

Universal developer-designed challenges that ensure the player has engaged with the game's systems. Examples: survive 15 days, walk X steps, treat a wound, weather a blizzard, survive a winter, survive a full year. Completing enough unlocks the custom-character builder. Specific challenge thresholds and content are TBD and will be tuned in playtest.

## 11. Open Design Items

Areas that need further design work before this becomes an implementation document:

- Character roster: twelve names, twelve specializations, twelve starting kits. Full roster definition.
- Crafting trees: specific node structures for each specialization. The mechanics of "insight" — how randomized skill upgrades unlock adjacent nodes.
- Combat & interaction feel: moment-to-moment combat mechanics for player-vs-predator and player-vs-player. Twitchy vs. deliberate. Wound persistence after fights.
- Building & shelter: modular vs. freeform placement. Degradation. Raidability in multiplayer.
- Death corpse rules: how long a corpse persists, looting rules, decomposition, predator attraction.
- Specific challenge thresholds: exact requirements for private server unlock, public server unlock, custom character unlock.
- Compendium scope question: per-account vs. per-server on public worlds.
- Sound, light, and tracking depth: how deeply player noise, light sources, and player-left tracks are simulated.
- Steam-specific scope: Steamworks integration plan (achievements, cloud saves, workshop), Steam page timing for wishlist marketing (target 6–12 months pre-launch).
