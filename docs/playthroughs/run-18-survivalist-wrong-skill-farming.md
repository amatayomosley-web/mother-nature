# Run 18 — Survivalist, Forest, Spring "Wrong-Skill Farming" Exploration

Date of run: 2026-05-10
Status: 60-day exploratory simulation. **Wrong-skill domain probe.** Outcome: see "What ended Reni" below.
Run author: William + Claude (design exercise)

> **EXPLORATORY CAVEAT — WRONG-SKILL PROBE.** Farming is not a committed Mother Nature system. Run 13 (Ren, Forager) and Run 14 (Vasco, Builder) explored farming with *adjacent* skill profiles. This run inverts the question: a character with NO Forager, NO Builder, NO (hypothetical) Cultivator attempts to farm. The point is to surface what the WORLD does to a character whose skill profile is *misaligned* with the attempted activity — and to stress-test §14.9 (backstory cannot substitute for skill) by demonstration rather than assertion. Magnitudes are illustrative. Several systems referenced (soil grid, plant growth, wildlife garden-raid behaviors) are not in any locked design section. Continues gap codes from N80/F26.

## Setup

- **Character**: "Reni" (mid-30s) — Survivalist primary (Fire-craft, Improvised Shelter, Field Repair, Water Treatment), Hunter baseline, **nothing else**. No Forager. No Builder beyond what Survivalist field-improvisation covers. No Cultivator (the node doesn't exist).
- **Backstory**: Reni grew up near a town with kitchen gardens. She remembers her aunt's tomato cages and her grandfather's potato hill. She has "seen a garden." She has never made one.
- **Biome**: Deep forest (§3) — meadow at forest edge, southerly exposure, mixed deciduous-conifer beyond the treeline. A small stream cuts across the meadow ~80m east of the chosen spot.
- **Season**: Early May (last frost just past), spring proper. 60-day run window — Day 1 through early July.
- **Reni's stated reason**: She is tired of moving. She has run mobile fieldcraft for years (Run 12 was Pell's territory; Reni envies the *idea* of having a place). She has decided to settle. She doesn't know what settling actually requires.

### Starting kit (Survivalist standard + what she scavenged on arrival)

- Survivalist standard: knife, ferro rod, paracord 15m, oiled tarp 2×3m, bedroll, multi-tool, fishing kit, small pot 1L, water filter, fire kit (matches + tinder pouch), 7 days food
- Weapons (per §15.2 Survivalist): spear, knife, atlatl + darts (Reni left atlatl darts at her last camp by accident — she has the spear and knife, atlatl shaft only, no darts to throw)
- **Scavenged on arrival at the meadow site (the day before the run begins):**
  - Rusty hand-trowel (one rivet loose; handle wobbles)
  - Small rusty shovel — head fairly intact, original handle rotted off, **improvised replacement handle** lashed from a green sapling with paracord (her field-repair instinct, executed in 20 minutes; grade C — handle will fail under sustained loads)
  - **Seed cache** salvaged from a wrecked supply box ~200m up the treeline: mixed paper packets, several illegible, several with stylized images (a leafy thing; a round thing; a flowering thing; what looks like beans). She cannot identify them with confidence. The packets are damp.
  - 5m of soft wire
  - No other tools. No axe. No hatchet. No saw. No mattock. No bucksaw. No hoe.

### Compendium grants at start

- Expert: **fire-craft, improvised-shelter, cold-management, water-purification, field-repair, basic navigation** (Survivalist domain)
- Practiced: **predator-reading at the survival level** (avoidance not pursuit), **basic weather signs** (front-scale only), **knot-craft**
- Novice: **bow-handling, tracking, edible-identification at the OBVIOUS-ONLY tier** (dandelion, cattail, wild onion if she stumbles on it). For everything else: nothing.
- Glimpsed: **animal behavioral state, plant-ID** — meaning a single sentence in the Compendium and no shader uplift in the field.

### Skill node

Survivalist — fire-craft, improvised shelter, field repair, water treatment. Hunter baseline (can read deer track; cannot read parsnip vs hemlock). **No Forager.** **No Builder beyond bough-shelter, knot, and ferro-and-stone improvisation.** **No Cultivator (does not exist).**

### Goal

- **Reni's stated goal**: settle here. Grow some food. Have a place.
- **Actual (design exercise)**: surface what §14 does to a character attempting work entirely outside her domain. Test §14.9 (backstory cannot substitute) by demonstration. Surface saliency at *sub-baseline* (§14.13). Establish whether wrong-skill attempts are honored by the architecture, refused by the architecture, or quietly papered over.

---

## Phase 1 — Days 1-7, the friction wall

### Beat 01 — Day 1, what Reni sees vs what Bo would see

Reni walks the meadow at first light. **§14.13 saliency** queries her skill profile and returns: almost nothing.

A Forager-Expert (Bo from a hypothetical parallel run) would see, layered: wild onion clumps in the wet seam, three dandelion mats, fiddleheads at the stream edge, jewelweed, a stand of stinging nettle — early-spring caloric value layered visibly across the meadow.

Reni sees: green stuff. Grass. Some other green stuff. A flowering thing. **The shader does not uplift entities in domains where she has no skill.** Per §14.13 NO-SKILL tier, plants render as-is. She walks past the wild onion three times in the first hour and does not notice it as wild onion.

She *does* see, at Survivalist-Expert tier:
- The deadfall pine at the meadow's north edge — lean-to anchor
- The stream's bend where the gravel is right for a sit-and-fish position
- A fire pit site on the lee side of a rock outcrop, wind from the west
- Dry deadfall under the conifer skirt — fuel grade reading clean

**This is the §14.13 sub-baseline surfacing in its starkest form.** Same world, same pixels, different attentional bias. The forest is full of food for someone trained to see it. To Reni, the forest is "trees and grass" with shelter sites and fuel sources standing out.

She picks her camp spot — the deadfall pine lean-to anchor — and the garden spot — "where the sun is most, near the stream." She paces a rough rectangle. About 4m × 6m. She has no plan beyond "dig it up and put seeds in."

**Systems invoked**: §14.13 saliency at NO-SKILL tier (plants) and EXPERT tier (shelter/fire/fuel)
**Gaps surfaced**:
- F27 — Saliency at NO-SKILL tier as the *default visual baseline* for an entire domain. Currently, this is implicit in §14.13 but has not been stress-tested against a character with multiple domains at NO-SKILL. The field reads as "deafened" in those domains; the player will not notice that they're not noticing.
- N81 — Compendium "Glimpsed" tier surfaces in §14.6 back-room as a label but produces no field uplift. Reni would have to *open the back-room* to learn she has Glimpsed plant-ID — and even then, the entry is decorative. Is Glimpsed worth surfacing at all? Or should it route to "absent"?

### Beat 02 — Days 1-2, the shovel handle fails on Day 1

Reni begins digging at the spot she picked. The improvised shovel handle — paracord-lashed green sapling, grade C field-repair — holds for ~90 minutes of moderate sod work, then twists out of the lashing on a buried stone. She is now down to the trowel.

Her **Field Repair Expert** fires. She has lashed a better handle by lunch — chose a drier, harder sapling this time, cross-lashed it with multi-tool aid, grade B. **This works because field repair is her actual skill.** The shovel is functional again.

She works the rest of Day 1 with the repaired shovel: ~3m² of sod broken, none of it cleanly cleared. The sod has not lifted in mats — it has *shredded*. A Builder would have cut clean mats and rolled them aside for compost. Reni doesn't know to cut around the perimeter first. The shovel jabs and chops. The sod mostly stays where it was, churned up.

Day 2: more of the same. ~5m² broken-up sod, none of it cleared to soil. She is roughly where a competent gardener would be after the first hour.

**This is §14.3 quality grading in its honest form.** Reni's *action time* is doubled. Her *output quality grade* is D — not because she lacks effort, but because she lacks technique she's never learned. Her *failure mode* is continuous degradation: the bed will be lumpy, the sod fragments will re-root, the plot will fight her all season.

**Gaps surfaced**:
- F28 — Sod-cutting as a Builder-Earthworks technique under §14. Cutting clean mats vs churning chunks is a Practiced-vs-Novice distinction that needs to render mechanically as soil quality differential.
- N82 — Tool wear under wrong-skill use. The shovel itself takes more abuse from churning than from clean cutting — should grade out faster. Currently no tool-wear-by-quality-grade system.

### Beat 03 — Days 3-5, the bed Reni made

Days 3-5 Reni works the plot. She does not amend the soil — she does not know to. She does not check drainage — she has never thought about drainage as a property of dirt. She does not test for stones below 20cm — she stops at the visible surface and assumes the rest is fine.

The plot she ends with on Day 5:
- ~12m² of "worked" ground, lumpy with un-rotted sod and root fragments
- Soil is forest-edge meadow loam: acidic (~5.0-5.5), not amended
- One large stone half-buried in the middle of the plot. She worked around it.
- Drainage: the plot sits in a slight low spot relative to the meadow surface. **Reni did not notice that water flowing across the meadow during spring rain will pool here.** A Builder-Earthworks-Practiced would have seen the grade at a glance.
- Sun exposure: ~5 hours midday (Reni miscounted the treeline shadow line)

**Quality grade of the prepared bed: D.** Not because Reni is incompetent — she's at Survivalist-Expert in her own domain — but because *bed preparation is not Survivalist-domain work.*

She has used 5 days. She has not planted a seed. She has consumed 2.5 of her 7 days' rations. She has caught one fish on Day 4 (Fishing kit, Survivalist-cross-skill at Practiced — grade B, ~300 cal). She has *not* foraged effectively at all — the wild onions she walked past on Day 1 are still there, and she has not noticed them.

**Body state end of Day 5**: warmth 85%, hunger 65%, tiredness 70% (back hurts), no injury. Functional but burning into reserves with nothing to show.

### Beat 04 — Days 6-7, planting blind

Day 6: Reni opens the seed packets. The damp has been a problem — three packets are stuck together and a fourth shows mold-spotting. She separates what she can and lays them out on a flat rock to dry in the sun.

What she has, by her own naming:
- "Leafy thing" — looks like lettuce or cabbage seed (small dark seeds; she's not sure which)
- "Round thing" — possibly radish, possibly turnip, possibly beet (a Forager would distinguish at a glance; Reni cannot)
- "Flowering thing" — image suggests a sunflower or marigold; the seeds are large, striped
- "Bean things" — these she's confident on; they're clearly beans by shape and size
- Two packets she can't read at all — small grey seeds, no image
- One mystery packet — single large brown seed, looks like a nut or pit

**She has no Compendium entry to compare against.** She has no shader uplift to read seedling-vs-cotyledon when they emerge. She has no idea which to plant deep vs shallow, which need cold stratification, which want full sun vs partial.

She decides: bury beans in two rows because she's confident on the bean. Scatter the "round thing" seed across a third row because that's what her aunt did with radish. Drop the "leafy thing" in a sparse fourth row. Plant the "flowering thing" at the back as a tall thing — even if it's not food, it'll *look right*. The mystery packets she sets aside.

Planting depth: she guesses 2cm for everything. **The beans are right.** The "round thing" (if radish, ~1cm right; if beet, ~2cm right; if turnip, ~1cm right — she's lucky here). The "leafy thing" — if lettuce, she's planted it 3x too deep and most of it won't germinate. The "flowering thing" — if sunflower, ~2-3cm right. **She doesn't know what she has and she's working from a single guessed depth.**

She waters with the cookpot from the stream — ~10L across the plot over Day 6 evening. Not enough but more than nothing.

**Day 7**: she rests. Body grade C. Hunger at 55%. The "garden" is in. She has used 7 of her 60 days. She has consumed 5 of her 7 days' rations and added ~600 calories from one fish and a small handful of dandelion leaves she finally identified on Day 6 when she sat on one.

**Gaps surfaced**:
- F29 — Seed identification as a Forager-Practiced (or Cultivator-Novice) skill. Without it, packets render as "seed packet (unknown contents)" and the planting action runs blind. This is mechanically correct under §14.
- F30 — Planting depth as a per-crop attribute. Wrong-depth seeds either don't germinate or emerge weak. The plant-growth tick system (F-cluster from Run 13) needs depth-as-input.
- F31 — Mold and damp damage to seed cache. Salvage scenarios should have probabilistic seed-viability degradation. Some packets work; some don't.
- N83 — Action menus skill-gating. Per §14.13 the action menu is supposed to widen with skill. What's in the action menu for Reni at the seed packet? "Plant" only? Or also "Identify (Glimpsed: ???)"? Run 18 says the menu should *show the gap* — "Identify (no skill)" surfaces the missing capability honestly.

---

## Phase 2 — Days 8-21, the slow reveal

### Beat 05 — Days 8-14, what germinates and what doesn't

Reni waters daily. She has worked out her rhythm: dawn water-haul (cookpot only, 6 trips, ~1 hour), morning bed-tend (mostly pulling at the lumpy sod re-rooting through her plot, ~1 hour), then a long forage walk where she now starts paying attention to plants — because she is hungry. The afternoon goes to firewood, fishing, and one small-game attempt with the spear at the rabbit warren she found Day 10 (Hunter-baseline at Novice; she misses).

**Days 9-12**: germination.
- The beans come up cleanly. About 80% of them. **This is the only crop she got right.**
- The "round thing" partially comes up — Reni discovers it's beet (later confirmed by the deep red root, when she finally pulls one Day 30). Germination ~50%. Spacing is wrong — Reni broadcast-scattered and they're crowded. They will produce small roots, not good ones.
- The "leafy thing" mostly does not come up. ~20% germination. **This is consistent with seed planted too deep.** What does come up is a leafy small thing that she can't yet read as lettuce, cabbage, or chard. (It is lettuce. It will bolt by Day 35 and be inedibly bitter.)
- The "flowering thing" comes up. Tall stalks emerging by Day 14. Will turn out to be sunflower; will be eaten by birds in Phase 3.

**Reni notices that things are growing.** She is briefly hopeful. She does not notice — because she cannot read at her skill level — that the germination is partial, the spacing is wrong, the lettuce is too deep, the beet is crowded.

**§14.13 saliency check**: when she looks at her plot, she sees green emerging. A Cultivator-Expert (if the node existed) would see the same plot and immediately flag: bean OK, beet too-crowded, lettuce sparse, sunflower tall-but-vulnerable. The *information is in the world*; Reni's eye does not surface it.

### Beat 06 — Days 12-15, the first deer

Day 12 night, Reni wakes to a sound at the bed. No fence. No barrier. **It did not occur to her to build one.**

She has no axe or hatchet to fell saplings. She has knife and multi-tool. She could have built a low brush-pile barrier, even improvised wattle — Builder-Practiced at minimum. **She doesn't, because she doesn't know to.**

She lights her ferro into a quick fire-stick, shouts, the deer (a yearling alone) bolts. Damage assessment at dawn: the deer browsed the lettuce row. **About 60% of the already-sparse lettuce gone.** The beans are intact (deer don't favor bean tops at this stage). The beet greens are nibbled — ~20% loss. Sunflower untouched.

Reni mutters — diegetic character voice, low — *"need to fence this."* She has the realization. She does not have the tools or skill to act on it well.

**Days 13-15**: she attempts a fence. With her knife and multi-tool she cuts springy saplings ~2cm thick — slow work, knife is the wrong tool for limbing — and pushes them into the ground around the plot perimeter. She tries to weave brush between them. The result is a knee-high, sparse, falling-over hedge. **Builder grade F.** It will not stop a deer. It might confuse one.

She also strings the 5m of soft wire she scavenged in a single low line at thigh height around the plot as a deterrent. Knot-craft is her only Practiced cross-domain skill that fires here — the wire is taut and the knots hold. The wire is too low to stop a deer and too low to be felt at dusk; it is functionally decorative.

**This is the diegetic surfacing of §14.9.** Reni's backstory — "I've seen a garden" — does not generate fence-building skill. The world does not pretend her hedge is anything but what it is. Her grade-F fence renders as a grade-F fence: leaning, sparse, ineffective. No floating warning text. No tutorial nudge. The deer return on Day 15 and walk through it without slowing.

**Gaps surfaced**:
- F32 — Fence-building as a Builder-Practiced skill. Without Builder, the action is available but the output renders at Grade F. The action is not *refused*; it is *honored at the quality the skill produces*.
- N84 — Diegetic character mutter ("need to fence this") as a *retrospective* skill-gap surfacing. Real-time mutter (§14.13) currently surfaces what the character notices in the world. This run suggests an *additional* mutter channel: the character's own retrospective realization after a loss. "Should have fenced sooner." "Should have put it on higher ground." The mutter surfaces the lesson the character is *just now learning* — a diegetic teaching channel that respects no-floating-text discipline.
- F33 — Quality grade F as a renderable output. Most of the design language above grade-D is established (B, A, A+, S). Grade F as "barely functional" or "non-functional" needs a render spec — leaning posts, falling-over weave, gaps the eye can read.

### Beat 07 — Days 16-21, the watering grind + the wood ash she didn't think of

Reni continues watering. She does not amend the soil at any point — she has burned wood at her fire pit for 21 days now and the wood ash sits in the firepit unused. **A Forager-Practiced or any Cultivator would have spread it on the plot.** Reni's Compendium does not light up on "wood ash" as garden input. Her Survivalist-Expert reads ash as "fire byproduct, residue for cleaning a pot." She uses some for cleaning her pot. The rest blows away in a Day 19 wind.

The bean plants flower around Day 20 — earlier than expected; spring is generous. She is briefly proud.

Hunger is starting to bite. Days 16-21 she has burned through the last of her 7 days' starting rations + the few foraged supplements + 2 small fish + 1 squirrel taken with the spear at close range on Day 17 (lucky strike, Hunter-baseline at Novice — she's not skilled, the squirrel was distracted). **Net caloric balance Days 8-21: roughly break-even, trending negative.** She is at ~62 kg starting weight; she has lost about 1.5 kg.

**Body state end of Day 21**: warmth 80%, hunger 50%, tiredness 70%, no injury. The slow grind is starting to show. She is functional but not building toward anything except the deferred bet on the garden.

**Gaps surfaced**:
- F34 — Compendium-driven action surfacing. Reni's wood ash sits inert because no Compendium entry connects fire-byproduct → soil amendment in *her* compendium. A Forager-Practiced would have that entry; Reni doesn't. The information is there in the world; her Compendium gates the action menu.

---

## Phase 3 — Days 22-35, the reckoning

### Beat 08 — Days 22-28, the bird, the mildew, the bolting

Day 22: a flock of small birds (sparrows? finches? Reni doesn't know) discovers the sunflower stalks. The sunflower heads are not yet seeded out — the birds are picking at the developing florets. By Day 26 the sunflowers are tattered. Reni shoos them. They return.

Day 24: she sees white powder on some of the bean leaves. **Powdery mildew.** She has no Compendium entry on it. She rubs a leaf and the powder smears. She does not know if this is bad. She does not know what to do. (A Forager-Expert would crush garlic-mustard into a folk spray. A Cultivator would prune affected leaves. Reni does neither because she doesn't know.)

Day 25-27: the mildew spreads. By Day 28 perhaps 30% of the bean plants are visibly affected.

Day 26: the lettuce — what's left of it — bolts. It shoots up a flowering stalk and the leaves go bitter. Reni tries one. Spits it out. **She does not know that this is normal for lettuce in early summer.** She wonders if she did something wrong.

Day 28: she pulls her first beet. Small — about thumb-sized — and tough. It is real food. She eats it that evening. ~30 calories. The first calorie her garden has produced in 28 days.

**Compendium auto-log (Survivalist domain only)**: she has *no* auto-log entry for plant disease, plant bolting, or sub-optimal harvest. Her Compendium quietly fills only the entries her skill profile supports — fire-craft variations, the new fishing spot, the squirrel-from-spear technique. **The garden is happening in her Compendium's blind spot.**

### Beat 09 — Days 28-32, Reni mutters at the garden

Day 30: a clear afternoon. Reni sits at the edge of her plot. She has lost ~3 kg now. Her body is grade C.

**She mutters, alone:** *"I don't know what I'm doing here."*

This is the diegetic surfacing of §14.9 — the character voicing, in her own mouth, the gap between her backstory and her actual capability. No tutorial popup. No "consider switching strategies" advisor. Just the character realizing it.

She looks at her plot. She looks at the forest. She has, over the past week, become more aware of the wild onion patch by the stream — she's been pulling from it for a few days and it's actually feeding her better than the garden. She has identified fiddleheads in their now-past-prime state at the wet edge (Day 28 — too late; they're already unfurled and bitter). She found chickweed on Day 26 because she ate one accidentally with a wild onion bulb and discovered it was tender.

**The forager skill is starting to come.** Per §14.4 XP-by-action, every plant she successfully identifies and eats ticks a small XP gain in plant-ID and edible-recognition. Glimpsed → Novice is the steepest learning slope. By Day 32 she has crossed Novice in edible-plant-ID for the obvious-foreground species — wild onion, dandelion, chickweed, plantain, the fiddleheads she missed.

She has not crossed novice in seedling-vs-cotyledon, garden-pest-ID, soil-reading, or any cultivator-side skill. **The wrong-skill work is not teaching her cultivation; it is teaching her foraging, because foraging is the skill her actions actually fire.**

**This is a §14 finding.** Reni's failed gardening did not raise a Cultivator skill from 0 to Novice (because Cultivator doesn't exist as a node). It raised her Forager skill from 0 toward Novice on *plant-identification-by-eating-it*, because that's the skill her in-the-world actions cross-referenced.

**Gaps surfaced**:
- F35 — XP routing in wrong-skill work. When a character performs an action outside their domain (Reni planting seeds), where does the XP go? Run 18 says: to whatever skill the *successful sub-component* of the action mapped to. Reni's successful sub-component was "noticed a plant and ate it without dying" → Forager-edible-ID XP. Her failed sub-component was "buried a seed at the right depth" → no Cultivator XP (no node). This is a §14.4 mechanic question with real consequences.
- N85 — Skill-tier-crossing audibility. When Reni crossed Novice in plant-ID around Day 32, did she *notice*? Per §14.13, saliency uplift would begin then — entities in the plant-ID domain would shift from NO-SKILL to MEDIUM. Should there be a one-time diegetic acknowledgement ("eyes are starting to catch the green") or pure-shader-only? Run 18 suggests a *very sparse* character mutter at first crossing of a new domain — once per skill — as a teaching moment without becoming a notification.

### Beat 10 — Days 33-35, the pivot

Day 33-35 Reni shifts. She does not abandon the garden — beans are still flowering, beets are still in the ground, and abandoning the work feels worse than continuing — but she stops investing more than the daily watering and a quick pest pull.

Her day reshapes:
- Dawn: water haul (now 30 min — she's better at it, and she's accepted partial-waters)
- Morning: forage walk, expanded radius. She is finding food.
- Mid-day: shelter and fire maintenance (Survivalist-Expert work — quality A, fast)
- Afternoon: fishing or spear-attempt on small game
- Evening: process forage, dry herbs (she's gathered some thyme-equivalent), bank fire

**She is doing the work her skill profile actually supports.** §1 No Dominant Strategy is *holding* at the character-skill-mismatch level — Reni's attempt at the wrong-skill activity was honored by the architecture with sub-baseline output, and the architecture *did not bail her out*. She has had to pivot to her actual strengths.

**Caloric arithmetic Day 35**:
- Garden return Days 1-35: ~30 calories (one small beet)
- Forage return Days 22-35: ~3,500 calories (improving daily)
- Fish + small game Days 1-35: ~1,800 calories
- Ration consumption: 7 days starting (~14,000 cal) gone by Day 20
- Total intake Days 1-35: ~19,300 cal vs target ~75,000 cal at 2,150/day maintenance
- **Net caloric deficit ~55,000 cal across 35 days. She has lost ~6 kg.** Her body is grade C trending D.

She is not dying. She is *thinning*. The garden has nearly killed her by not feeding her; the forage and Survivalist-Expert efficiency are now starting to claw back.

---

## Phase 4 — Days 36-60, survival on the actual skill profile

### Beat 11 — Days 36-45, the forest carries her

Reni's forage knowledge accelerates. **Each successful ID ticks XP in plant-ID + edible-recognition + cross-linked skills (per §14.4).** She crosses Novice fully around Day 38 and the saliency shader begins to uplift more plant entities in her field of view.

She notices, for the first time on Day 39, the small ramp (wild leek) patch at the forest edge she's walked past 38 times. **The shader has finally uplifted it.** She harvests. Body state improves materially over Days 38-45.

The bean harvest comes in Days 40-50 — partial. Mildew took ~40%. Deer (returned three more times across the run; her grade-F fence stops nothing) took ~20%. What she gets is real food: about 800 g of green beans across the picking window, ~700 calories total, plus another ~400 calories of dry beans saved for storage by Day 55.

Sunflower seeds: birds got 80%. She salvages a single seed-head, partial — ~150 calories.

Beets: ~15 small roots across Days 38-55, ~450 calories.

**Total garden return Days 1-60: roughly 1,700 calories.** (Compare Run 13's Forager-Ren at 19,500 cal for 90 days on a similar plot. Reni's garden is *11× less productive* on the calorie axis — and Ren's garden was already 13× less calorie-efficient than her parallel forage.)

### Beat 12 — Days 45-55, the bear stops by

Day 47 pre-dawn: heavy crashing through brush at the garden side. Reni wakes. **A black bear, mid-sized.** He has come for the bean smell. He pushes through Reni's grade-F fence — barely registers as resistance — and walks into the plot.

Reni does the right Survivalist thing: she does not fight. She makes herself visible at 30m, holds her spear, shouts low and steady, builds the fire stick high.

The bear gets the rest of the beans. ~15 minutes of foraging. He trample-flattens the beet row and exits the way he came.

**Survey at dawn**: beans 95% gone (he ate or trampled the rest). Beets ~60% lost to trample. Sunflower (already bird-stripped) untouched. **The garden is functionally over.** Reni stands at the edge of the plot for a long time and does not say anything aloud.

The bear becomes a Camp Stalker (§13.2). He will be back.

### Beat 13 — Days 50-58, the question becomes "stay or leave"

Reni faces the decision. The bear is now on her camp. She has no rifle, no bow, no Hunter-Expert skill, no Builder reinforcement option. She has Survivalist-Expert — which says: *move.* Mobile fieldcraft (Pell's Run 12 territory) is her actual domain. Her stated goal — "settle down" — collides with the world's response to a character whose skill profile cannot defend a settled position.

She mutters again, Day 51: *"I'm not built for this."*

This is the diegetic surfacing of the §1 character-skill-mismatch test in full. The world has not refused Reni's attempt. It has *answered* her attempt with the consequences her skill profile produces. She tried to settle without the skills for settling. The bear came. The garden failed. The forage is enough to feed her if she keeps moving.

She decides, Day 53: she will leave on Day 60. She will pack her preserved beans (~400 cal), her dried herbs, her small kit, and walk south to a different drainage. She will go back to mobile fieldcraft, which is what her hands actually know.

### Beat 14 — Days 55-60, departure preparation

Reni spends Days 55-58 in Survivalist-Expert work: preserving the last forage, drying mushrooms (she identified one species reliably by Day 50 — a chanterelle-equivalent, edible bright orange, distinctive enough that even Novice could clear it), preparing tinder bundles, repairing her gear.

Field Repair Expert fires repeatedly — she resoles her bedroll's outer wrap (it's worn), re-lashes her shovel handle to a higher grade (B now; she'll leave the shovel here when she goes — too heavy for mobile), re-serves the cordage on her fire-stick. All grade A-B work in her actual domain.

Day 59: she stays close to camp. Body grade C, recovering. Hunger 65%, tiredness 50%. **She is no longer net-losing.** The forage finally exceeds her burn rate.

Day 60: Reni walks south. Camp 1 is behind her. The garden plot will return to meadow over the next two years. The deer and bear will harvest what little remains.

**She is alive.**

---

## What ended Reni (didn't, this run)

Reni survives Day 60. The death-cascade did not fire, but it almost did. The plausible failure modes she dodged:

1. **Starvation on Days 18-32.** The window of net-negative caloric balance before forage XP kicked in was the danger zone. A bad-weather week in there (a cold snap forcing more fuel and reducing forage productivity) would have triggered the Tova-cascade (Run 03) at Day 25 or so. She avoided it because spring was generous and her Survivalist-Expert thermal management kept body warmth high enough to allow caloric efficiency.

2. **Misidentified plant.** Reni ate dozens of plants in Days 25-40 in growing desperation. She got lucky: she chose the obvious-foreground species (wild onion, dandelion, chickweed, ramp, fiddlehead) and skipped the ambiguous ones. **The hemlock-vs-parsnip pair was not in her meadow this run.** Had it been — and had hunger crossed the threshold where she'd risk an "I think this is parsnip" pull — Run 18 would have ended differently. Field Notes (§14.12) entry for "death by misidentified plant" was open the whole time.

3. **Bear escalation.** The Day 47 bear visit was the moment a more aggressive bear, or one in a more aggressive state (cubs nearby, defensive), would have produced an injury or death. Reni had no real defense — Survivalist-spear at 30m is not bear-defense. She survived because the bear was hungry but not stressed.

4. **Injury during sapling-cutting.** Days 13-15 she was cutting saplings with her knife — wrong tool, sustained force. The same axe-slip risk that ended Tova was present in inverted form (knife-slip during sustained chopping). She ran it 3 days without incident.

5. **Cold rain wet-cold.** Spring forest has wet-cold risk. Her tarp is small and her shelter is good — Survivalist-Expert held her warm grade. A 3-day cold front with heavy rain in her gear-thin state could have flipped warmth into the red. The weather grid was gentle this run.

**The garden did not kill her. The wrong-skill attempt at settling did not kill her. The forest fed her once her actual skills caught up. §1 holds.**

---

## Architecture Stress-Test — Does §14 honor wrong-skill attempts honestly?

### §14.9 (backstory cannot substitute) — VERDICT: HELD BY DEMONSTRATION

Reni's "I've seen a garden" backstory produced zero mechanical capability. No fence-building skill emerged from "she's seen fences." No soil-reading emerged from "she's heard of wood ash." No seed-ID emerged from "she watched her aunt." **The architecture honored §14.9 brutally and correctly.** Every action she attempted ran at the quality grade her *actual* skill profile produced — Survivalist-Expert on the Survivalist sub-components, Novice-or-NO-SKILL on everything else.

This is the strongest demonstration in the playthrough corpus to date that backstory is decorative, not mechanical. Run 13 (Ren, Forager) surfaced §14.9 as a question; Run 18 (Reni, Survivalist) answers it by demonstration.

### §14.13 (saliency at sub-baseline) — VERDICT: NEW SUB-VERDICT NEEDED

The shader rendered correctly at NO-SKILL tier for Reni's plant-ID domain. **But the playthrough surfaced a finding §14.13 has not yet addressed**: at NO-SKILL the player *cannot detect that they cannot see*. Reni walked past wild onion 38 times before her skill crossed Novice. **There is no signal that signal is being missed.** A Forager-Expert sees vivid foreground; a Novice sees deafened background; a NO-SKILL player sees *what looks like a normal forest, exactly as a sighted person sees what looks like a normal forest*.

This is the right design — perceptual realism — but it has a teaching consequence. Reni only learned what she was missing by *accidentally* identifying things (chickweed by sitting on it; ramp by walking the same path 38 times). The system relies on hunger pressure + accidental encounter + the slow XP tick to teach. **This is consistent with §14's "starting weak is the design" but the architecture should probably acknowledge it in the onboarding contract.**

### §1 No Dominant Strategy at the character-skill-mismatch level — VERDICT: HELD

§1 was tested here in a way prior runs did not test. The wrong-skill character did not get bailed out, did not get tutorialized into competence, did not have the architecture quietly route her actions to "good enough." She got the consequences her skill profile produced. **§1 held. The world did not negotiate with her.**

### §14.3 (quality grading) — VERDICT: GRADE F NEEDS RENDER SPEC

Reni produced multiple grade-F outputs (fence, planting depth, soil prep, pest response). Quality grades D and below are mentioned in §14.3 but not specified in render terms. **Run 18 surfaces F as needing a clear visual language** — fence that leans, plants that come up sparse, bed that holds standing water after rain. Without that render layer, sub-baseline output may degrade visually-identically to baseline output, which would collapse the diegetic teaching channel.

### §14 mastery-floor question (the run's central architecture probe) — VERDICT: GRADIENT-DEGRADED, NOT REFUSED

**Should the game refuse certain actions to severe-mismatch characters? Or allow at gradient-degraded quality?**

Run 18's evidence is unambiguous: **allow at gradient-degraded quality.** Reni was permitted to attempt every garden action. Each one rendered at the quality her skill produced — F on bed prep, F on fence, near-zero on plant-ID, A on field repair (which fired correctly because field repair is *her* skill). The architecture did not refuse the seed-planting action even though she was at NO-SKILL in cultivation. It honored the attempt and produced the output.

The alternative (refusal) would have been wrong: it would have communicated "you can't" via a UI block, which is exactly the floating-text-and-tutorial pattern §14.13 and §14.6 reject. The gradient-degraded approach communicates "you can, but here's what your skill produces" through the world itself.

**The mastery floor in Mother Nature is performance-floor, not action-floor.** Every character can attempt every action. The world tells them how good their attempt was. The teaching is in the consequence.

This is a §14 architecture finding worth promoting to the locked text.

---

## Design Inventory — what Reni's failures teach the architecture

### Structures surfaced (mostly at grade F)
- Grade-F garden bed (lumpy, un-amended, poor drainage)
- Grade-F sapling-and-brush fence (leaning, gapped, ineffective)
- Grade-F wire deterrent line (decorative)
- Grade-B improvised shovel handle (field repair fired correctly)
- Grade-A Survivalist lean-to (her actual skill, working perfectly)

### Processes / recipes surfaced
- Seed-cache damp/mold viability degradation
- Wood ash sitting inert because Compendium doesn't surface its garden use
- Plant-ID by accidental ingestion (chickweed, ramp)
- The slow Novice-tier-crossing arc through forced foraging
- Departure-preparation as a Survivalist-Expert workflow (field repair, drying, tinder cache)

### Materials introduced
- Seed packets with mixed/unknown identity (a new inventory class: "unknown seed packet")
- Wood ash as Compendium-gated input
- The departed-character's relict garden (will fail to reproductive maturity; will return to meadow over years)

### Tool-wear / gaps
- Shovel head + improvised handle (field repair sustained it)
- Knife used as sapling-cutter (wrong tool; would degrade faster under repeated mis-use)
- Cookpot as water-haul vessel (works, slow)
- Multi-tool blade dulling from sustained sapling work

---

## EXPLORATORY DESIGN QUESTIONS

### Q1: Can a Survivalist-only character even attempt farming?

**Run evidence**: Yes, but the output is so poor it is barely worth the labor cost. Reni's 60-day garden returned ~1,700 calories. Her forage in the same period returned ~12,000+. **Farming is a net loss for a wrong-skill character.** The architecture should not refuse the attempt — it should let the world communicate the misalignment. Run 18 says: **allow with sub-baseline output.**

### Q2: How does §14 handle a character attempting work outside their domain?

**Run evidence**: The architecture handles it by *gradient-degraded quality*, not by *refusal*. Every action is in the action menu (or should be). The skill differential expresses in the output. Reni's fence was a fence — just a grade-F fence that deer walked through. Her planted seeds did germinate — just at wrong depths with wrong spacing. **The world is the teacher. The world tells her what her hands can and cannot make.** This is a §14 locking question worth surfacing to the design doc.

### Q3: Does the character learn from doing wrong-skill work?

**Run evidence**: Partial, and not in the way one might expect. Reni did *not* gain Cultivator skill from her garden work (the node doesn't exist, but even if it did, her actions were too unskilled to tick meaningful XP). She *did* gain Forager skill — because the *successful sub-components* of her actions (noticing plants in the world, eating them and surviving) routed XP to Forager. **Wrong-skill work teaches whatever adjacent right-skill the action incidentally fires.** This is the §14.4 XP routing answer at the character-skill-mismatch level. It is correct against §14 — XP follows actual action success, not action intent.

### Q4: Does Reni's run validate §14.9 by demonstration?

**Run evidence**: Yes, definitively. Her backstory generated no mechanical capability. The architecture honored §14.9 with no exceptions. Run 18 is now the corpus's clearest §14.9 evidence.

### Q5: At what point should the game make Reni's failures diegetic?

**Run evidence**: Throughout. No floating text. No tutorial popup. The failures speak through:
- Plants that don't come up (planted too deep)
- Fences that fall over (grade F render)
- Deer walking through the plot (animal behavior unmodified by failed deterrent)
- Mildew on the leaves (plant disease, no Compendium read at her skill)
- The character's own retrospective mutter ("I don't know what I'm doing here", "I'm not built for this")

**The mutter channel is the key finding.** Real-time saliency mutter (§14.13) surfaces what the character notices in the world. Reni's run suggests an *additional* mutter channel: retrospective skill-gap acknowledgement. Used sparsely (3-5 times across 60 days), it preserves diegesis while teaching the player that the character is realizing their own limits. Worth specifying.

### Q6: Does failed farming work as a TEACHING mechanism?

**Run evidence**: Yes, but the teaching is not "now you know how to farm." The teaching is **"now you know what skills cultivation requires, and which ones you don't have."** Reni did not become a gardener. She became a slightly-less-bad gardener and a meaningfully-better forager. **This is the §14.4 emergence pattern firing correctly.** Wrong-skill attempts surface the missing skills to the player (the *player*, not the character) by making the gaps mechanically visible.

For the next run with this character or another Survivalist who wants to settle: the player now knows that fence + soil + plant-ID are the missing skills. The next character can be built differently (Builder primary, Forager secondary) or the next run can be a deliberate cross-skill apprenticeship (a Survivalist who teams up with a Forager-Builder in MP).

### Q7: What ends Reni?

**Run 18: nothing did.** She survived because the architecture's pivot signal — her own mutter, her own caloric arithmetic, the bear arrival — gave her enough time to abandon the wrong-skill bet and return to her actual strengths. **The architecture pivoted her honestly.** Had spring been less generous, or had the bear come earlier, or had her seed cache included a hemlock-lookalike root that she pulled in desperation around Day 28, this run would have ended at Day 25-35 with a Field Notes entry.

The death cascade was open the entire run. It did not close because she *chose to abandon the attempt*. Permadeath compatibility: confirmed. A player who refuses to pivot would die. Reni's run is the version where the pivot worked.

### Q8 (new): Should "wrong-skill attempts" be an explicit teaching design pattern?

Run 18 suggests **yes**. A character who tries something outside their domain produces:
- Sub-baseline outputs the player can see
- Adjacent-skill XP gain (Reni → Forager)
- Diegetic mutter surfacing the gap
- A caloric/labor cost that pressures pivot

This is a coherent teaching loop. The player learns the skill graph *by being on the wrong side of it*. The architecture should not paper over the misalignment; it should let the misalignment *be the lesson*.

---

## COMPARATIVE ANALYSIS — Reni (Run 18) vs Ren (Run 13)

Same activity (forest-edge garden, spring start, ~60-day window). Different skill profiles. What does the architecture do?

| Axis | Ren (Forager primary + Builder secondary, Run 13) | Reni (Survivalist primary only, Run 18) |
|---|---|---|
| Site selection | Read drainage, sun exposure, treeline competition at Forager-Expert + Builder-Practiced | Picked "where the sun is most"; missed drainage (plot pools water) |
| Bed clearing | 7 days, 15m² cleared to soil with mats lifted, root + stone removed | 5 days, 12m² of churned sod; grade-D bed; one large stone she worked around |
| Soil amendment | Wood ash + leaf mold + stream silt; informed by Forager Expert | Nothing. Wood ash sat inert in firepit |
| Seed ID | Read packets, knew planting depths from Forager Expert + childhood | Could not ID 4 of 7 packets; guessed planting depth |
| Fence | Wattle 1m → 1.5m, Builder-Practiced grade B | Grade-F sapling-stick hedge + decorative wire line |
| Pest response | Garlic-mustard spray (Forager folk-remedy), slug bowls, hand-pick | None — no Compendium connection |
| Disease response | Recognized mildew, pruned, ash-dusted | Did not recognize mildew; rubbed it; spread it |
| Bear visit | Day 64 — fence corner failure, Camp Stalker logic fires | Day 47 — fence walked through, plot taken almost completely |
| Garden return | ~19,500 cal over 90 days (~13% of intake) | ~1,700 cal over 60 days (~7% of intake) |
| Adjacent-skill XP gain | Negligible (already at Expert/Practiced in adjacent skills) | Significant Forager XP (Novice tier crossed Day 32-38) |
| Outcome | Alive, modest stores, ready for autumn | Alive, departing back to mobile fieldcraft |
| Strongest finding | §14 needs Cultivator node if farming committed | §14.9 (backstory ≠ skill) holds by demonstration; mastery floor is performance-floor not action-floor |

**Same world. Same activity. Different skill profiles produce 11× difference in caloric output. §14 holds. §1 holds. The architecture honored both characters with the consequences their skills produced.**

---

## §14 mastery-floor question — Run 18's answer

**Question**: Should the game refuse certain actions to severe-mismatch characters? Or allow at gradient-degraded quality?

**Answer (Run 18's evidence)**: **Allow at gradient-degraded quality.** Three reasons:

1. **Diegetic teaching beats UI blocks.** Reni learned what skills she lacked by attempting and failing. A UI block ("you cannot do this — no Cultivator skill") would have taught her nothing — only that the game was preventing the action. The world's response was the teacher.

2. **Cross-skill emergence requires action.** Reni's Forager XP arrived because she was *in the world doing things*. A refused action ticks no XP. A failed action ticks XP for whatever sub-component succeeded. Refusal cuts the learning pathway.

3. **The architecture is honest under §1.** Every action being available preserves §1 No Dominant Strategy at the action level — there is no "Builder-only" action that locks out other archetypes. The Builder is faster, better, more durable. The Survivalist can still try. The world tells the difference.

**Implementation implication**: every action in MN should be in every character's action menu. Skill differential expresses in output quality, action time, failure mode — not in availability. The only exception should be **signature abilities** (§14.5) which are explicitly master-only by design. Everything else is available; the world honors the skill profile in the result.

This is a §14 architecture finding worth promoting. Recommend lifting into §14 as an explicit "action availability vs. action quality" rule alongside the mastery-floor discussion.

---

## Cross-references

- §1 No Dominant Strategy — held at the character-skill-mismatch level; the world did not bail out the wrong-skill attempt
- §2 Mechanical Behavior — every event emerged from skill profile + tool + world inputs; no scripted moments
- §3 Forest biome — same site as Run 13; the biome's generosity in spring is load-bearing for survival of wrong-skill attempts
- §5 Wildlife — deer + bear visits identical mechanics to Run 13; outcome differed because of fence quality differential
- §13.2 Camp Stalker — bear-on-garden behavioral memory fired identically to Run 13
- §14 Skill Architecture — Run 18 is the strongest §14.9 demonstration and the clearest "mastery floor = performance floor, not action floor" finding
- §14.3 Quality grading — grade F needs render spec
- §14.4 XP and emergence — wrong-skill attempts route XP to whatever adjacent right-skill the action's successful sub-components map to
- §14.6 Player perception model — diegetic mutter channel needs an additional retrospective-skill-gap variant
- §14.9 Onboarding — Reni's run is the clearest corpus evidence that backstory cannot substitute; recommend referencing this run in the §14.9 text
- §14.13 Saliency — NO-SKILL tier confirmed correct; surfaces a teaching consequence (the player cannot detect that signal is being missed) worth acknowledging in onboarding
- Run 12 (Pell, Survivalist mobile fieldcraft) — Reni is the alternate path Pell rejected: Pell embraced mobile, Reni tried to settle. Run 12 + Run 18 together define the Survivalist's domain edges.
- Run 13 (Ren, Forager farming) — same activity, adjacent skill profile; comparative analysis above shows the 11× output differential the architecture produces
- Run 14 (Vasco, Builder desert irrigation) — adjacent skill profile in different biome; the cross-skill cases bracket the wrong-skill case from both sides

## What Run 18 delivered

- 60-day exploratory simulation of farming by a wrong-skill character (Survivalist primary, no Forager, no Builder, no Cultivator)
- **§14.9 (backstory cannot substitute) confirmed by demonstration** — Reni's "I've seen a garden" generated zero mechanical capability; every action ran at the quality grade her actual skill profile produced
- **§1 (No Dominant Strategy) held at the character-skill-mismatch level** — wrong-skill attempts produced sub-baseline output; the architecture did not bail her out
- **§14 mastery floor answer**: performance floor, not action floor — every action available; skill differential expresses in output quality, action time, failure mode
- **Saliency at NO-SKILL tier confirmed correct** — Reni walked past wild onion 38 times before crossing Novice; the world is the teacher, the hunger is the pressure, the slow XP tick is the path
- **Wrong-skill XP routing**: Reni did not gain Cultivator XP from her garden work; she gained Forager XP from the successful sub-components (plant-ID by ingestion). XP follows action success, not action intent.
- **Diegetic retrospective mutter channel** surfaced as a §14.6 extension — "I don't know what I'm doing here", "I'm not built for this" — sparsely used, real teaching moments without floating text
- **Caloric arithmetic**: Reni's garden returned ~1,700 cal over 60 days; her forage returned ~12,000+ in the same period. Wrong-skill farming is a net loss. The architecture made this visible without telling her.
- **Outcome**: alive. She pivoted to her actual skill profile and survived. The death cascade was open throughout; it did not close because she chose to abandon the wrong-skill bet.
- **9 new gaps surfaced**: F27-F35 (saliency baseline, sod-cutting, fence-F render, seed-ID, planting depth, seed mold, Compendium-action gating, fence as Builder skill, XP routing) + 5 new general gaps N81-N85 (Glimpsed tier, tool wear by misuse, action menu skill-gating, retrospective mutter, tier-crossing audibility)

The run is the wrong-skill probe the corpus was missing. Run 13 asked "does farming need a Cultivator node?" Run 14 asked "does farming work in desert?" **Run 18 asks the deeper question: when the architecture is presented with a character whose skill profile is misaligned with the attempted activity, does it tell the truth?** Run 18's answer: **yes. Honestly, in the world's voice, without UI rescue. §14 honored the misalignment. The character had to live in it. The architecture passed.**
