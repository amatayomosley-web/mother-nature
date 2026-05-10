# Run 13 — Forager, Forest, Spring-into-Summer "Garden" Exploration

Date of run: 2026-05-10
Status: 90-day exploratory simulation; outcome variable (see What ended Ren / What didn't)
Run author: William + Claude (design exercise)

> **EXPLORATORY CAVEAT.** Farming is NOT a committed Mother Nature system. §3 biomes (forest/desert/tundra) list no arable terrain; §13 brainstorming covers hunting/trapping/foraging only. This run does not assume a farming system exists. It simulates what a character with kitchen-garden background would DISCOVER if she tried to cultivate in forest biome, and uses that discovery to surface what farming-in-MN would NEED to commit to — and what it would COST against existing principles (§1, §2, §14). Treat this as a scoping exercise informing a yes/exploring/no decision, not a feature spec.

## Setup

- **Character**: "Ren" (late 30s) — Forager primary, Builder mid-secondary, Survivalist baseline. Her grandmother kept a kitchen garden; Ren absorbed methods as a child.
- **Biome**: Deep forest (§3) — mixed deciduous + conifer. Site at forest-edge clearing: south-facing meadow opening into woodland.
- **Season**: Late spring start (mid-May equivalent), last frost passed. Run spans 90 days — late spring through summer into early autumn.
- **Site**: South-facing meadow, sun exposure ~6-8 hrs midday, drainage moderate, stream ~100m east, treeline ~15m north (windbreak + competition).

### Starting kit

- Forager standard: knife, slingshot
- Hand trowel, hand fork, small hoe (improvised — hatchet-back of her hand axe), hatchet, axe
- Bedroll, oiled tarp 2×3m, fire kit, water filter, 30m cordage
- 10 days preserved food, canteen 1L
- **Seed cache** (salvaged from wrecked supply: mix of bean, squash, corn — Three Sisters profile; lettuce, radish, beet; a few perennial herb seeds — thyme, oregano)

### Compendium grants at start

- Expert: edible spring plants, plant-ID broadly, toxic lookalikes, foraging by season, food preservation (drying, fermenting), wild medicinals
- Practiced: shelter-construction (Builder cross-skill), basic carpentry, knot-craft, fence/wattle weaving
- Novice: hunting (bow handling only via cross-skill diffusion), tracking, predator-reading
- Glimpsed: weather signs at front-scale, animal behavioral state

### Skill node

Forager primary / Builder secondary. **Critical**: no "Cultivator" or "Gardener" node exists in §14. Ren's kitchen-garden background is a backstory note, not a mechanical skill. The simulation must surface whether that gap matters.

### Goal

90 days. Establish a kitchen garden at the forest edge alongside hunting/foraging caloric backbone. Test: can a Forager with kitchen-garden backstory produce meaningful food from cultivation in MN's forest biome?

---

## Days 1-7 — Site, clearing, the labor reveal

### Beat 01 — Day 1, site selection

Ren walks the meadow edge. **§14.13 saliency** fires strongly in her Forager domain: wild strawberry runners, fiddleheads in the wet seam, dandelion mat, jewelweed near the stream. The forest reads as already-productive — which is the first design tension this run surfaces (see EXPLORATORY Q2 below).

For the garden specifically, her childhood-knowledge guides site choice: south-facing for sun (~6-8 hrs), slope for drainage (15° south), distance from treeline for root competition (treeline ~15m north — borderline; she'll lose afternoon sun if she sets too close), stream within haul range. She paces a 4m × 8m plot — 32m². The size is a deliberate small-kitchen-garden, not a homestead farm.

**Systems invoked**: §14.13 saliency · ambient terrain · water proximity reading
**Gaps surfaced**:
- F1 — Soil-state as a world property. Currently MN has no soil model. Sun-exposure, drainage, pH, fertility do not exist as world data.
- F2 — Sun-exposure as time-of-day grid value. Weather grid (§12.8) tracks atmosphere; not direct insolation at ground level.

### Beat 02 — Days 1-3, the clearing reveal

Ren begins clearing the 4m × 8m plot. **This is where her kitchen-garden backstory collides with reality.**

The meadow is not a tilled field. It is a perennial sod mat: deep-rooted grasses, woody seedlings, a young pine sapling, dense fibrous root tangle ~20cm down, two large stones, a partly buried birch root running diagonally across the plot from a tree 8m off. She has a hand trowel, a hand fork, and an improvised hoe (hatchet-back). She has no shovel proper, no mattock, no pickaxe.

Day 1 effort: 6 hours of work, ~1m² cleared to bare soil. The sod comes up in mats but each mat is heavy with root and rhizome. Her back aches by mid-afternoon.

Day 2: another 6 hours, ~1.5m² cleared. Hits the birch root mid-plot. Cannot cut a live root that thick with hatchet without major effort.

Day 3: another 6 hours, ~1.5m² cleared. Removes one large stone. The second stone is too embedded; she works around it.

**End Day 3: ~4m² cleared of ~32m² planned. 12% complete after 18 hours of pure clearing labor.**

She revises the plan. The 4m × 8m is unrealistic on her solo timetable; she scales to 3m × 5m = 15m². Even that will take ~4 more days at current pace, and she's burning 3000+ calories/day doing it while drawing on preserved food and supplementing with wild forage. She has not yet planted anything.

**This is the first principle-collision of the run.** The labor cost of converting unfarmed forest-edge sod to garden bed is *immense* and entirely realistic. A Forager-Builder with hand tools cannot prep 32m² quickly. This either becomes a meaningful gate (good — preserves §1) or designers will be tempted to abstract it away (bad — collapses §2).

**Systems invoked**: §14.3 action-time + grade · §14.2 debuff stack (she has no Cultivator passives to reduce dig-time)
**Gaps surfaced**:
- F3 — Clearing labor as a multi-day Builder-class action with sod/root/stone variability
- F4 — Tool tier matters: hand trowel + hoe << shovel + mattock. MN has no shovel/mattock in starting kits and no progression toward them.
- F5 — "Kitchen-garden backstory" is not a mechanical skill. No Cultivator node = Ren is novice-tier at every garden action that isn't covered by Forager/Builder.

### Beat 03 — Days 4-7, finishing the plot + parallel forage

Days 4-6 finish clearing the reduced 3m × 5m plot. Ren intercuts garden labor with foraging walks (fiddleheads, wild onion, early greens — caloric gain meaningful, ~600-1000 cal/day from forage alone) and one unsuccessful slingshot attempt on a squirrel.

By end of Day 6: plot cleared, surface raked smooth, two large stones piled at corners (foundation stones for a future raised bed if she goes that route), birch root sawn through with hatchet over 2 hours (a real labor cost).

Day 7: she rests, treats blisters, processes forage. Body grade-C from cumulative strain. **No seed has gone in the ground yet. She has used 7 of 90 days on clearing alone.**

**This is meaningfully different from how survival games typically gesture at farming.** Stardew Valley clears a tile per swing. Valheim is two minutes per plot. The realism MN is committed to — §2 mechanical behavior, §14.3 honest action-time — produces a 7-day clearing arc for a 15m² plot by a solo Forager-Builder with hand tools. This is correct against MN's principles. It is also a clear signal that **farming in MN would not be a fast loop**.

---

## Days 8-14 — Soil, beds, planting

### Beat 04 — Days 8-10, soil prep + amendment problem

Ren turns the cleared soil with her hand fork. The earth is dark loam — visually rich, organic-smelling. **But it is forest soil**, which her childhood knowledge tells her is acidic (pH ~5.0-5.5) and adapted to fungal-dominant decomposition. The vegetables in her seed cache — bean, corn, squash, lettuce, beet — want neutral pH (~6.5-7.0) and bacterial-dominant soil.

**The amendment problem surfaces.**

Options Ren considers:
- **Wood ash for pH** — she has a fire pit; ash is available; raises pH. Realistic.
- **Compost** — she has been at site 7 days; compost takes 4-8 weeks at minimum. She has nothing aged.
- **Manure** — no domesticated animals (§5.8 deferred). She could use deer/elk droppings but in tiny quantity and uncomposted (raw manure burns roots). Not practical.
- **Leaf mold** — forest floor has decades of decomposed leaf litter. She harvests two pack-loads from a sheltered hollow ~80m into the trees. Quality grade B-A; rich.
- **Stream-bank silt** — fine alluvial deposit ~100m at the stream curve. She hauls three pack-loads.

Days 8-10: amendment haul + incorporation. She spreads wood ash thin, mixes leaf mold and silt into the top 15cm of bed. **Result**: better than raw forest soil, not as good as a tilled garden. Her childhood knowledge knows this isn't enough for full crops — but it's what she has.

**Gaps surfaced**:
- F6 — Soil-pH as a runtime property of garden tiles. Currently nonexistent in MN.
- F7 — Amendments (wood ash, leaf mold, silt, compost) as gathered/produced resources with effects. None exist.
- F8 — Time-to-compost as multi-week passive process. No timer-on-pile system exists.
- F9 — Manure source: §5.8 defers domesticated animals. Wild scat would need spawn density + harvest mechanic.

### Beat 05 — Days 11-12, bed construction (Builder cross-skill)

Ren has cleared bed and amended soil, but she's at the forest edge with deer-elk-rabbit pressure imminent. Her Forager-Builder cross-skill activates: she builds a low fence + raised bed framing.

**Materials**: she fells 12 saplings (4-6cm diameter, 5 hours), debarks, sets corner posts driven 30cm into ground. Weaves wattle panels (Builder Practiced — quality grade B) around the perimeter, ~1m tall. Constructs two raised-bed frames using sectioned logs ~20cm tall around the planting areas, infilled with the amended soil + more leaf mold for a slight mound.

Day 11: 4 fence posts driven + 2 sides of wattle woven (~7 hours).
Day 12: remaining wattle + raised-bed frames (~7 hours).

**This Builder work is legitimate cross-skill** under §14. Wattle-weaving is in Forager-Builder territory; she has Practiced grade. Her output is grade-B: serviceable, will hold against rabbits and slow most deer but a determined deer or a curious bear could push through.

**Gaps surfaced**:
- F10 — Wattle / improvised-fence construction recipe. Adjacent to §13 brainstorming but not specified.
- F11 — Fence durability vs animal AI. A bear should be able to break a wattle fence; a deer should be deterred but not stopped if hungry. Animal AI does not currently model "test fence" behavior.
- F12 — Raised bed as in-world object with state (soil quality, water content, frost-protection differential).

### Beat 06 — Days 13-14, planting

Day 13 morning: she plants. Her grandmother's methods guide her — Three Sisters in one bed (corn first, beans + squash later as corn establishes), lettuce + radish + beet in the other bed (cool-season, expecting bolting by midsummer), thyme + oregano in pots improvised from sections of hollowed log.

Planting depths and spacings she recalls from childhood. **Reality check**: she has no Cultivator skill. Her best output here is whatever the Forager Expert / Builder Practiced cross-applies as. Optimistically, she's at Practiced grade on planting; realistically her output is Novice-Practiced on a domain MN doesn't model. Her depth and spacing are probably right; her timing on the corn vs the legume is a guess.

She also drives a few stakes for crows: a Forager superstition from her grandmother — bits of cloth on string above the corn rows to deter seed-corn theft.

**Day 14**: hauls water from the stream — 4 trips with the canteen and her cookpot (~6L total), waters in the seedlings + soaks the rows. The stream-haul is ~12 minutes round trip per load. **Six liters is not enough water for a freshly planted 15m² bed**. Her childhood knowledge knows this; she promises herself she'll triple the haul tomorrow.

**Body state end of Day 14**: warmth 80%, hunger 60% (still drawing on preserved food + forage), thirst 75%, tiredness 60% (consistent moderate-to-high physical load).

**Calories: she has consumed half her preserved food and has not yet harvested a single garden calorie.**

---

## Days 15-35 — Tending, watering, wildlife pressure, parallel forage

### Beat 07 — Days 15-21, the watering load

Ren establishes a daily rhythm. Dawn forage walk (1-2 hours, ~400-600 calories return). Morning water haul to garden (4-6 trips, 1-2 hours). Mid-day garden tend — weed pull, pest check, replant where germination failed. Afternoon longer forage or small-game attempt (slingshot, occasional). Evening fire + processing + sleep.

The watering load is the surprise. **The 15m² garden, in late spring transitioning warm, wants ~25mm/week of water, or ~375 liters/week, or ~54 liters/day.** Ren's pack carries 6L per trip; the round trip is ~12 minutes; a full water day is 9 trips totaling ~108 minutes. That's a fixed ~2 hours/day commitment to watering alone, every dry day, for the duration of the growing season.

**This is precisely the cost MN's design philosophy would want.** §2 mechanical behavior — water doesn't appear from nothing; the stream haul is a real labor cost. §1 No Dominant Strategy — the labor cost prevents farming from being "set it and forget it." But it requires that MN model:

- Per-bed water demand by crop, by weather, by stage
- Soil moisture as a runtime state
- Rainfall integration with the §12.8 weather grid (good news: rain via weather is already in scope)
- Haul-tool tier (a clay pot or wooden bucket holds more than a canteen — implicit tool progression that doesn't exist)

**Gaps surfaced**:
- F13 — Per-bed water-state. Soil moisture grid coupled to weather grid.
- F14 — Crop water demand by stage. Lettuce wants 25mm/wk; established corn wants 25-40mm/wk; squash 30-50mm; bean 25-30mm.
- F15 — Water-haul tool progression (canteen → cookpot → improvised bucket → carved bucket).

### Beat 08 — Day 17, the first deer

Pre-dawn Day 17, Ren wakes to a sound. Through tarp opening: a doe and yearling at the edge of her garden. The wattle has held; the doe is testing it with her nose at a low corner. Ren bangs her cookpot and shouts; the deer bolts.

Inspection: a chunk of lettuce row is gone — the doe reached over the wattle (1m fence; deer can browse anything up to ~1.5m from a stretch). Loss: ~40% of the lettuce, planted 4 days ago. The radish is intact (too low to reach over). The corn is intact (in the second bed, further from the edge).

**This is the §1 stress-test in real time.** Ren can build a higher fence — but each height tier costs more sapling-felling, more wattle-weaving, more time NOT spent foraging or hunting. The mechanical question is: at what fence cost does deer pressure stop being economic? At a 2m woven fence she might lose 5% of crop to determined deer; at 1m she's losing 40% of edge rows; at no fence she'd lose 100%.

**Compendium auto-log**: "*doe + yearling, garden edge, pre-dawn Day 17, lettuce 40% loss; fence 1m insufficient.*"

**Gaps surfaced**:
- F16 — Wildlife pressure on garden as recurring nightly probability. Deer AI must include "test fence" + "browse over fence" behaviors when crops emit edibility scent.
- F17 — Crop scent on weather grid. Plants emit growth-state-dependent edibility signal that animals perceive.

### Beat 09 — Days 18-25, fence escalation + first pests

Day 18: Ren raises the wattle by 50cm using saplings she fells over 4 hours. Now ~1.5m. Builder Practiced grade-B work. This will deter most deer; a hungry deer at the hungry-gap (which has now passed, late spring is generous) will still try.

Days 19-25: she also notices aphids on her bean shoots, slugs on her lettuce, a single Colorado-potato-beetle-equivalent on a squash leaf. **Ren's Forager Expert in folk medicine covers some of this** — she crushes garlic-mustard leaves (foraged) into water + a bit of wood ash and sprays the bean rows. She picks beetles by hand. She sets shallow bowl traps with stream water for slugs. Cumulative pest-control time: ~30 min/day.

Crop losses to pests by Day 25, estimated: lettuce -15%, beans -10% (aphids), squash -5%, corn 0%, radish 0%, beet 0%. Tolerable.

**Gaps surfaced**:
- F18 — Pest AI as low-tier agents with per-crop preferences, perception ladder for the player to spot infestation early.
- F19 — Folk-remedy crafting (garlic-spray, wood-ash dust, hand-picking) — could ride on Forager Expert medicinals, but the recipes don't exist.
- F20 — Disease (blight, mildew, rust): not surfaced this run because weather has been favorable, but a wet week would have introduced it.

### Beat 10 — Days 26-35, parallel-life sustainability

Days 26-35 establish Ren's parallel life: garden tend + forage + occasional small-game.

Per-day garden time: ~2.5-3 hours (watering, weeding, pest, observation).
Per-day forage time: ~2-3 hours (broader meadow + forest edge).
Per-day other: ~1-2 hours (firewood, cooking, water for self, preserves processing).

Caloric arithmetic at Day 35 (Day 15 since first planting):
- Forage yield: ~600-1200 cal/day depending on season-state. Strong in late spring; declining as some species pass season.
- Garden yield: zero edible harvest yet. Radish closest (radish matures ~30 days from seed; she's at ~22 days; ~8 days until first pulls).
- Small game: 1 squirrel (slingshot, Day 22, ~150 cal); 1 grouse (slingshot, Day 31, ~400 cal); 1 rabbit attempt failed.
- Total daily caloric balance: net-slightly-positive on forage days, net-slightly-negative on heavy garden-labor days.

**She is not starving. She is also not building a calorie surplus. Her preserved-food is 30% remaining.**

**This is the most important finding of the run so far.** The garden has consumed substantial labor for 35 days and returned zero food. The forage and small-game are carrying her. If she had committed full days to forage + hunt instead of garden, she'd be better-fed on Day 35 by ~3-5 days' worth of preserved food saved.

**This is the §1 question in numerical form.** Garden labor pre-harvest is a debt. Whether it pays back depends on the harvest in Days 60-90.

---

## Days 36-60 — Growth, drought week, first harvests

### Beat 11 — Days 36-42, growth + first radish

Day 36: she pulls her first radish. Small but real. ~30g, ~5 cal. Over the next 8 days she harvests ~25 radishes, ~125 cal total. **The first garden calorie cost her 36 days of labor.**

Lettuce starts cropping Day 42 (she planted relatively late; succession would help). Cumulative lettuce yield Days 42-55: ~800 cal across ~50 leaves. Then it bolts in early heat and goes bitter.

Beet greens start being harvestable as thinnings Day 40. Yield: ~300 cal across the next 3 weeks.

**By Day 50: garden has returned ~1200 calories total.** Forage in same period: ~30,000 calories. Garden is 4% of caloric intake.

### Beat 12 — Days 43-49, the drought week

The §12.8 weather grid produces a dry stretch: 9 days no rain, ambient temperature climbing to 28°C peaks. **Ren's water-haul commitment triples.** She is hauling ~80 liters/day to keep the corn and squash alive. That's ~13 trips, ~2.5 hours, every day, in heat.

She rigs a shading system over the lettuce (a stretched section of her tarp). This costs her tarp's coverage of her own sleeping area; she sleeps under bough-only cover for the week. Sleep grade C.

Mid-week the stream itself drops ~20cm — still flowing, still drawable, but a longer scoop. The water-haul time creeps up.

**This drought week is the run's mechanical demonstration of weather-as-pressure on cultivation.** It is *correct* against §2: the weather grid drove the cost. It is also expensive enough that a single bad weather pattern can compromise weeks of investment.

**Gaps surfaced**:
- F21 — Drought as cumulative non-rainfall state with crop-stress consequences (wilting, reduced yield, death).
- F22 — Shading / micro-climate intervention by player (tarp over crops trades player shelter for crop survival).
- F23 — Stream flow as variable; long droughts reduce haul access.

### Beat 13 — Days 50-60, the first wave

The radish and lettuce cycle is done by Day 55. **The Three Sisters bed is the real test.** Corn is ~1m tall by Day 55. Beans are climbing. Squash vines spread across the bed floor, shading roots and suppressing weeds. The companion design is working visually.

Day 58: she finds her first finger-sized summer squash. Harvests over the next several days. By Day 65, ~5 squash, ~600 cal total. Small but real.

Day 60: she has harvested ~2700 calories total from garden across all crops since Day 36.

**This is the Day-60 inflection.** If the corn + bean main harvest delivers in Days 75-85 as expected (corn ~80 days from planting, dry beans through the season), she may approach 8000-12000 calories of stored garden harvest by Day 90. That would represent roughly 5-7 days' caloric reserve.

**For 60 days of labor.**

In the same 60 days, she has foraged ~36,000 calories. **Forage is 13× more caloric-efficient than her garden, at this scale, in this biome, for this character.**

---

## Days 61-90 — Harvest, wildlife collision, preservation

### Beat 14 — Days 61-70, the bear visits

Pre-dawn Day 64. Ren wakes to a sound she has not heard before at this camp: a heavy crashing through brush at the garden side. She lights a stick from banked embers, holds it high, shouts.

A young black bear, ~150kg, has pushed through her wattle fence at a corner. The corner was the weakest weave (Builder grade B not A). The bear is in the Three Sisters bed, eating squash.

She backs off. Bear vs human at close range with no rifle and no escape route is unwinnable. She watches from 30m.

The bear eats squash for ~15 minutes. Trample damage to the corn rows is severe — multiple stalks bent or broken. The bear, satiated, leaves the way it came.

**Survey at dawn**: ~3 squash plants destroyed (trampled), 4 corn stalks broken, fence corner needs full rebuild (~3 hours work), beans intact. Estimated harvest loss: 30% of corn potential, 40% of squash potential. **Single bear visit removed ~6 weeks of garden output in 15 minutes.**

**Compendium auto-log**: "*young black bear, garden raid Day 64, fence corner failure, squash/corn major loss.*" Apex individual identity (§5.5) flags: this bear now has a behavioral memory of "garden = food." He will return.

**This is the §13.2 Camp Stalker mechanic firing on a garden instead of a food cache.** The garden is, mechanically, a 15m² persistent food cache. Once a bear learns it, the bear comes back.

**Gaps surfaced**:
- F24 — Garden as food-cache-equivalent for Camp Stalker behavioral learning (§13.2).
- F25 — Fence breach at lowest-grade section (Builder grade B = weak corner).
- F26 — Trample damage as multi-plant collateral.

### Beat 15 — Days 65-75, defense escalation + main harvest

Ren faces a choice. She can:
- **A. Rebuild fence stronger** (full 2m, Builder Grade-A re-weave) — ~3 full days labor.
- **B. Set deterrents** — perimeter fire, hung pots that clatter, scent — uncertain effect.
- **C. Try to kill or scare the bear** — she has slingshot, knife; she's Forager not Hunter; not realistic.
- **D. Accept losses and harvest aggressively** — pull everything mature; eat the loss on green corn and squash.
- **E. Move camp + abandon garden** — keep what she has harvested, walk away from remaining 60% potential.

She chooses hybrid B+D: clatter-line (pots strung on cordage as alarm), one perimeter brush-fire she relights nightly, aggressive early harvest where possible. The bear returns twice in the next week (Days 67, 71) — the clatter scares him both times but doesn't deter return. He gets less each visit but he gets some.

**Days 70-80 harvest push:**
- Beans: dried on the vine, threshed Days 72-78. ~2.5 kg dry beans, ~9000 cal.
- Corn: harvested Days 75-82 as the cobs filled. ~12 cobs salvaged from ~25 originally planted. Bear took most of the rest. Drying frame built; corn hung to dry over 2 weeks. ~3000 cal projected.
- Squash: 4 mature winter squash + ~6 smaller summer squash salvaged. ~3500 cal.
- Beet roots: ~12 pulled Days 80-85. ~600 cal.
- Herbs: dried bundles for preservation. Caloric contribution negligible but flavoring value.

**Estimated total garden harvest by Day 85: ~16,000 calories.** Plus the ~3,500 cal in radish/lettuce/early greens earlier. **Total run garden output: ~19,500 calories.**

### Beat 16 — Days 80-90, preservation, the autumn pivot

The last 10 days are preservation work. Bean threshing + drying + storage in cordage-tied cloth bundles. Corn drying on frame. Squash curing in sun (must be cured to store). Herbs bundled hanging.

She also builds a small drying rack and a small underground root storage pit (~30cm deep, lined with leaf litter) for the beet — Builder cross-skill, Grade B-A.

**Day 90 inventory:**
- Stored garden food: ~19,500 cal (~10 days at maintenance burn, ~6 days at heavy-labor burn)
- Foraged stored: drying meadow herbs, dried berries from late summer, a few preserved mushroom strings (~5000 cal in preserves)
- Total preserved: ~24,500 calories — call it 12 days reserve

**She is alive, in reasonable body state (warmth 80%, hunger 70%, tiredness 50%, no injury), with modest stores entering autumn.** This is a real outcome. But the path to get there was 13× less caloric-efficient than pure-forage would have been over the same 90 days, BEFORE counting bear losses + drought stress + 18 days of pre-planting labor.

---

## What ended Ren (didn't, this run)

Ren survives Day 90. The death-cascade analysis from runs 01-04 doesn't fire here — she had time, the season was generous, the forest is the forgiving biome (§3 Deep Forest). The most plausible failure modes she dodged:

1. **Bear escalation** — the young bear had Camp Stalker pathway open; another visit at a more aggressive moment, or a different bear, would have produced injury. Forager has no combat tools; Hunter rescue would not have been available solo.
2. **Drought intensification** — a 3-week drought instead of 9-day would have killed the corn outright, dropping harvest by 60%+.
3. **Caloric debt during clearing** — she came close to net-negative on Days 5-7 and Days 43-49. A bad-weather week in there with no forage backup would have created the cascade conditions that killed Tova (Run 03) and Ari (Run 02).
4. **Injury during construction** — wattle work involves the same axe-slip risk that ended Tova (N40). Ren ran it 14+ days without incident; mechanically plausible but lucky.

**The garden did not kill her. The garden also did not feed her.** Forage and slingshot small-game carried 87% of her caloric intake. The garden delivered 13% — meaningful as supplemental, decisive as nothing.

---

## Architecture stress-test results — principle-collision focus

### §1 No Dominant Strategy — VERDICT: HELD, but only because the simulation produced losses

The run held §1 *only because* the bear took ~50% of the corn and squash, drought stress reduced yields, and 60 days of pre-harvest labor was a deferred caloric debt. Without those costs, a stable 15m² garden producing 20,000 calories with no losses would represent ~16 days of food for a much lower per-day labor than forage. **If MN built farming as a system without modeling the wildlife pressure + weather pressure + labor cost as ALL of these did in this run, farming would dominate.** A "stable garden that just produces" would eliminate the Forager hunting/foraging viability commitment (§15.6).

**§1 holds in this simulation. §1 would be broken by a softer simulation. The wildlife/weather/labor cost is not optional, it is load-bearing.**

### §2 Mechanical Behavior — VERDICT: HELD

Every event in this run emerged from inputs: the deer came because the lettuce was reachable; the bear came because the fence corner was Grade B; the drought was the weather grid's run; the clearing took 18 hours because that's what sod-on-roots costs hand-tools. No scripted moments. **§2 holds and would continue to hold if all the surfaced gaps (F1-F26) were built mechanically.**

The risk to §2 is in fast-loop temptation. A Stardew-style "till plot in 5 seconds, plant seed, water once, harvest in N days" loop would *be* scripted-feeling at MN's resolution. The realism MN commits to means farming would be a slow domain.

### §14 Skill Architecture — VERDICT: UNRESOLVED

This is the biggest principle question this run surfaces. **Ren had no Cultivator skill.** Her kitchen-garden backstory was narrative, not mechanical. Her output quality on planting, soil-amendment-judgment, pest-diagnosis, harvest-timing was effectively Novice-Practiced — meaning her crops grew less than they could have, her yields were sub-optimal, and her diagnostic moments (the aphids, the slug bowls, the wood ash decision) were good guesses, not informed reads. **A Forager Expert is not a Cultivator Expert.**

If MN adds farming, it requires one of:
- **A new Cultivator (or Gardener) node** in the skill graph — adds a 9th node, expands the Compendium domain set, adds passive debuffs (sub-baseline soil-read, sub-baseline planting-time, sub-baseline pest-ID) and active skills (the garden equivalents of fire-starting, splinting). This is significant scope.
- **Cross-domain attachment**: gardening as Forager+Builder applied to a new system, like fishing was speculated. This works less well because the underlying *knowledge* of gardening (when to plant, when something is sick) is not a Forager's wild-plant ID, even though they overlap visually. Risk of feeling either trivialized (any Forager can garden) or trivially-skill-gated (the wrong node can't garden at all).

**§14 commits to all skills being meaningful debuff-removal ladders. A "garden tile that just produces if you water it" with no per-step quality grading would violate §14.3.**

### §14.13 Saliency — VERDICT: NEW DOMAIN REQUIRED

If gardening is in, then per §14.13, every cultivated entity is a saliency target. A Cultivator Expert sees: which seedlings are vigorous, which are leggy, which are pest-stressed, which are ready to harvest, which leaves show early blight, which corn ears are filled vs hollow. A Novice sees: green plants. This is the same shader pattern as plant-ID and predator-reading — but it is a new shader-target domain that does not currently exist.

### Permadeath compatibility — VERDICT: COMPATIBLE, AND MEANINGFUL

If Ren had died on Day 30, the garden would have continued to grow without her — and would have died of drought or been entirely consumed by deer/bear in the following weeks. **This is a meaningful cascade.** A player who plants in spring and dies before autumn loses everything they invested. This is *exactly* the kind of cost-of-death that aligns with MN's permadeath ethic. Field-notes entry (§14.12) "you starved waiting for crops that the deer ate while you tracked a moose" is a compelling possible entry.

A multiplayer wrinkle: if another player finds an unattended garden in a public-world server, it is a free-resource encounter. This is consistent with jungle rules (§9.3) and creates an interesting MP texture — abandoned camps with rotting beds, plundered corn rows, are real diegetic objects.

---

## Design Inventory

### Structures surfaced
- Cleared garden plot (sod-removed, root-cleared, stone-piled)
- Raised bed (log-framed, infilled, mounded)
- Wattle fence (Grade-B 1m, Grade-A 2m tiers)
- Crop shade rig (tarp-over-frame)
- Drying frame (post-and-rail for hung corn/beans)
- Root-storage pit (lined, ~30cm)
- Clatter-line perimeter (cordage + hung pots)
- Perimeter watch-fire (nightly relit)

### Recipes / processes surfaced
- Soil amendment: wood ash + leaf mold + stream silt blend
- Folk pest sprays: garlic-mustard infusion + ash dust
- Slug bowl traps (stream-water-baited)
- Three Sisters companion planting layout
- Succession planting (lettuce, radish, beet)
- Bean threshing + drying
- Corn drying-on-frame
- Squash sun-curing for storage
- Herb bundle-drying

### Materials introduced
- Seeds (mixed cache: bean, corn, squash, lettuce, radish, beet, herbs)
- Wood ash (already a fire byproduct)
- Leaf mold (forest floor gathered)
- Stream silt (alluvial)
- Improvised compost (would require multi-week timer system; not built this run)
- Folk-spray ingredients (garlic-mustard, ash)

### Tool-wear / gaps
- Hand trowel + hand fork + improvised hoe = inadequate for sod-clearing scale.
- Shovel, mattock, spade absent from MN tool roster.
- Watering vessel limited to canteen + cookpot.
- Bucksaw / 2-handed saw would speed Builder timber for posts; only hatchet/axe.
- Sickle / scythe absent (not relevant this run; would matter for grain harvest if grain were grown).

---

## EXPLORATORY DESIGN QUESTIONS

### Q1: Does farming-in-MN need its own skill node?

**Run evidence**: Ren's Forager Expert + Builder Practiced did not cover gardening-specific knowledge: when to plant, how deep, how to read seedling vigor, how to identify pest infestation at early stage, harvest timing, yield-readiness. Her output was clearly sub-optimal at planting-domain decisions because the skill system has no Cultivator coverage. **Answer leaning YES** — if farming is added, a Cultivator/Gardener node fits the §14 architecture cleanly. This is meaningful scope: +1 starting node in the roster of 8-12, new passive/active skill graphs, new Compendium domain. Cost is real.

### Q2: Does farming break §1?

**Run evidence**: §1 held in this run *because* losses were ~50% to bear, ~20% to drought stress, ~10% to deer-before-fence-escalation, and 60 days of pre-harvest labor was a calorie debt. With those pressures, garden output was 13% of intake; forage was 87%. **Answer leaning NO, conditionally** — farming does not break §1 *as long as* wildlife pressure, weather pressure, and honest labor cost are all modeled as load-bearing systems. Without any one of them, farming would dominate. The conditionality is sharp: a "casual gardening" implementation would collapse §1; a "realistic cultivation" implementation preserves it.

### Q3: What's the pressure that prevents farming dominance?

Three load-bearing systems, all required together:
- **Wildlife pressure** — deer, rabbit, bear, raccoon, crow, vole. Realistic-rate visits with realistic damage. Modeled fence-defeat behaviors. Camp Stalker behavioral memory on garden objects.
- **Weather pressure** — drought stress, hail damage, frost damage at edges, blight in wet weeks. Tied to the §12.8 weather grid.
- **Labor cost** — clearing, soil prep, planting, daily watering, weeding, pest control, fencing maintenance, harvest, preservation. All real time. No instant operations.

If MN adds farming, all three must ship together. Cutting any one collapses §1.

### Q4: Does farming compose with the existing world?

**Run evidence**: Farming would require new world-state systems:
- **Soil grid** (per-tile or per-bed): pH, moisture, fertility, frost.
- **Plant growth ticks**: per-crop life-cycle staging tied to weather grid + soil grid + water input.
- **Crop scent** as edibility-signal-to-animals on the existing scent-on-weather-grid system. (Composes with §12.8 — good news.)
- **Pest agents**: low-tier AI for aphids/slugs/beetles with crop-preference and seasonal cycles.
- **Disease propagation**: blight/mildew/rust as wet-weather contagion across crop tiles.

The compositional cost is substantial. The scent + weather + animal AI integrations ride on existing systems (good); the soil grid + plant growth + pest agents + disease are net-new.

### Q5: Multiplayer angle — is farming only sensible at cooperative scale?

**Run evidence**: Ren's solo 15m² garden was barely-economic against the labor cost. A 4-player camp could trivially defend a 60-100m² garden (more eyes, more fence-build labor, more daily watering hands), grow staples for a winter, and run hunting/foraging in parallel. The output-per-player-hour improves dramatically with cooperation. **Answer leaning YES** — farming probably scales much better in multiplayer, where harvest of grain or staple crop becomes a real winter-survival asset. Solo farming is plausible but is a hard mode.

### Q6: Does farming fit permadeath?

**Run evidence**: Yes, and meaningfully. A character who plants in spring and dies in summer loses all garden investment to wildlife/weather/decay. This is a *good* cost-of-death — the deferred-investment-lost-on-death cascade is consistent with MN's permadeath ethic. Field Notes (§14.12) entry for "starved waiting for crops" or "killed by bear at garden raid" would be earned. **Answer YES — farming composes with permadeath cleanly.**

### Q7 (new): Is "kitchen-garden backstory" doing too much work in this run?

This run depended on Ren's narrative backstory to make planting decisions she had no mechanical skill for. In MN's design, backstory cannot substitute for skill (§14.9 onboarding requirement: "starting weak is the design"). Either farming gets a real skill node (Q1 = yes) or the player who tries to garden without that node operates fully at sub-baseline Novice — fumbling depths, wrong timings, mis-IDing pests, picking crops too early or too late. That second world is also coherent with §14, but it makes solo first-time farming brutal.

---

## Scope-creep risk assessment

If MN adds farming as a committed system, the cascade of required systems is substantial. Surfaced in this run:

| System | Status now | Required for farming | Cost tier |
|---|---|---|---|
| Soil state (pH, moisture, fertility) | Absent | Required | HIGH — new per-tile world data |
| Plant growth ticks | Absent | Required | HIGH — per-crop life-cycle simulation |
| Crop scent (on weather grid) | Composable on §12.8 | Required | LOW — extends existing scent |
| Per-crop water demand | Absent | Required | MEDIUM |
| Drought stress on crops | Absent | Required | MEDIUM |
| Pest AI (aphid, slug, beetle, crow) | Absent | Required | MEDIUM — new agent tier |
| Disease propagation | Absent | Required | MEDIUM — wet-weather contagion |
| Wildlife garden-raid behaviors | Partial (Camp Stalker §13.2 logic exists) | Required | LOW-MEDIUM — extends §13.2 |
| Fence durability + breach | Absent | Required | MEDIUM — new structural system |
| Cultivator skill node | Absent (§14 has no farming node) | Required (per Q1) | HIGH — new node, new passive/active trees, Compendium domain |
| Seed item type | Absent | Required | LOW — inventory extension |
| Compost as multi-week passive process | Absent | Required | MEDIUM — needs persistent timer-on-object |
| Watering tool progression | Absent (canteen only) | Required | LOW — extends inventory |
| Saliency shader for crop state | Composable on §14.13 | Required | LOW — extends existing shader |
| Harvest preservation extensions | Partial (Run 02 / 04 surfaced) | Required | LOW — extends existing |
| Multi-player garden management | Absent | Optional but high-value | MEDIUM-HIGH |

**Approximate total scope cost: substantial.** Farming is not a small add. It is a system equivalent to weight to combat (§15) or skill architecture (§14) — months of design + build, and a long playtest tuning loop for the §1 dominance test.

**The key risk**: if farming is half-built (e.g., crops grow but no pest pressure, or pests exist but wildlife doesn't raid, or wildlife raids but no drought), §1 will not hold. The "minimum viable farming" floor is high.

---

## Recommendation (informing the yes/exploring/no decision)

**Recommendation: EXPLORING with a high bar to commit.**

Farming is not categorically incompatible with MN's principles. The run demonstrated it can preserve §1 (with all three pressures), composes with §2 (everything emerged mechanically), and fits permadeath cleanly. But the scope cost is large (probably comparable to §15 combat in design + build effort), the principles only hold if the full pressure stack is built, and the skill-system implications are significant (likely a new node, with attendant compendium + passive/active tree expansion).

**If pursued, sequence the build defensively:**
1. Cultivator skill node design + integration with §14 first. Without this, gardening either trivializes or skill-gates wrongly.
2. Soil state + plant growth ticks as the foundational world-state systems. Without these, the rest is decorative.
3. Wildlife garden-raid behaviors as extension of §13.2 Camp Stalker logic. Cheapest tier; biggest §1-protection payoff.
4. Pest AI + disease last. Tunable. Can ship at MVP smaller than the wildlife pressure layer.
5. Playtest the §1 dominance test hard. If a single playthrough produces "farming = optimal", retune (likely UP wildlife pressure or DOWN crop yield).

**If not pursued**, the design is unaffected. Forage (§14 Forager node) plus hunting (§15) plus preservation (Runs 02/04 surfaced) already covers caloric strategy without farming. Nothing about the existing design assumes cultivation; nothing breaks if cultivation never ships.

**The principal argument FOR pursuing**: farming opens a new player-archetype lane (the Cultivator) that is fully realized at parity with Hunter/Forager/Builder, and creates rich multiplayer dynamics (shared gardens, harvest splits, abandoned-garden encounters). The principal argument AGAINST: the scope cost is real and the dominance risk is sharp.

---

## Cross-references

- §1 No Dominant Strategy — the §1 dominance test is the load-bearing constraint on farming
- §2 Mechanical Behavior — held under simulation; would be at risk in fast-loop temptation
- §3 Forest biome — site selection viable; tundra/desert farming would be different runs
- §5 Wildlife — garden raids extend §5.5 individual identity + §13.2 Camp Stalker
- §6 Body — caloric arithmetic is the same physics; garden labor is a real body cost
- §12.8 Weather grid — drought stress + scent on grid compose cleanly
- §13.2 Carrion Chains / Camp Stalkers — garden = persistent food-cache for behavioral learning
- §14 Skill Architecture — Cultivator node candidate; §14.13 shader extension required
- §14.3 Quality grading — applies to bed construction, fence, treatment of crops, preservation
- §14.9 Onboarding — "kitchen-garden backstory" cannot substitute for skill node
- §14.12 Field Notes — death-by-garden-cascade is a viable entry candidate
- §15 Combat / Weapons — Forager defensive options against garden raiders (§15.6) constrain bear-encounter outcomes
- Runs 01-04 — same architecture, completely different domain; the system held under stress in those, this run is the deferred-system probe

## What Run 13 delivered

- 90-day exploratory simulation of cultivation in a forest biome by a non-Cultivator character
- §1 dominance test surfaced sharply: held only because wildlife + weather + labor all bit
- §2 held cleanly throughout
- §14 surfaced as the key principle-tension: no Cultivator node exists; backstory cannot substitute
- 26 surfaced gaps (F1-F26) covering soil, water, pest, disease, wildlife, fence, tool, skill, saliency
- Recommendation: exploring with a high bar to commit; if pursued, sequence the build defensively
- The garden delivered 13% of caloric intake at 60+ days of labor; the §1 evidence is concrete

The decision on farming is a scoping decision, not a design discovery. The principles tolerate it. The cost is large. The choice is whether the new lane it opens (Cultivator archetype + cooperative-garden multiplayer + permadeath garden-loss cascades) is worth the scope.
