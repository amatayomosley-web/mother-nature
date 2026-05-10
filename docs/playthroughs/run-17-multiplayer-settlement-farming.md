# Run 17 — Three-Player Cooperative Settlement, Forest, Late Spring (Farming + Hunting + Construction)

Date of run: 2026-05-10
Status: 60-day exploratory simulation; party alive at Day 60, settlement standing, first preserved-food stockpile in place
Run author: William + Claude (design exercise)

> **EXPLORATORY CAVEAT.** Farming is NOT a committed Mother Nature system. Runs 13 (Forager solo forest garden) and 14 (Builder solo desert irrigation) surfaced caloric, labor, and scope ceilings on solo cultivation. This run probes the **scaling question**: does multiplayer cooperation change the verdict? The simulation also stress-tests §12 multiplayer commitments (sleeping-bag tether logout §12.4, per-account compendium §12.5, no cabin-fever §12.2) under cooperative-settlement conditions that prior runs did not exercise. Treat as a discovery exercise on coop scale, not a feature spec. Magnitudes are tuning proposals.

## Setup

- **POV character (PC)**: **Anika** — late-30s, Builder primary (Mason-mid, Forager-Expert), Survivalist baseline. Acts as settlement coordinator. Her grandmother kept a kitchen garden; Anika brought seed cache including a few perennial cuttings.
- **NPC-proxy 1**: **Beren** — Hunter-Expert + Trapper-Practiced. Treated as deterministic mechanical actor in this simulation. Provides protein and perimeter scout.
- **NPC-proxy 2**: **Cale** — Forester-Expert (Carpenter sub-node) + Survivalist-Practiced. Provides timber, milling, repair, infrastructure carpentry.
- **Biome**: Deep forest (§3.1), mixed conifer-deciduous, mid-elevation.
- **Season**: Late spring start (mid-May equivalent). 60-day arc planned.
- **Site**: South-facing meadow on forest edge with a year-round stream ~60m east. Stone scree fan ~150m upstream (Anika spots it Day 1). 40m × 60m usable clearing.

### Starting kit (collective)

- **Tools**: 2 full felling axes (Cale's primary + Anika's secondary), 1 two-man crosscut saw, 1 buck saw, 3 hatchets, 3 belt knives, mason hammer (Anika's), drawknife, awl, hand drill + bits, mallet, wedges, sand cone, plumb bob, sharpening stone. **One full-size shovel** (load-bearing item; §13 confirmed shovel rarity in starting kits).
- **Camp**: 3 bedrolls, 3 sleeping bags (each tied to its owner per §12.4 tether commitment), 2 large tarps 3×4m, fire kit, water filter (1 cartridge spare).
- **Containers**: 1 iron pot (communal), 4 glass jars (for fermentation), 30m heavy cordage + 30m light cordage, 200m mixed-gauge wire.
- **Wire** (this is the asymmetric input of the run — 200m wire is a *lot* for one party): supports trap lines AND fencing AND lashings. Cale will turn portions of it into hardware.
- **Consumables**: 21 days collective food (~63 person-days at ~2400 cal), salt 500g, knife sharpening stone, notebook + pencil.
- **Seeds** (Anika's full mix): bean (climbing + bush), corn (3 varieties), squash (summer + winter), lettuce, radish, beet, carrot, kale, leek, plus perennial cuttings — thyme, oregano, sorrel, two horseradish crowns.

No firearms. Beren has bow + 12 arrows; Anika has slingshot; Cale has hatchet/spear improvised. All three carry knives.

### Compendium grants at start (per-account, §12.5)

- **Anika**: Expert plant-ID + edible/toxic forage, preservation (fermentation, drying), wattle/fence weaving. Practiced shelter-construction, mason work, knot-craft, weather signs. Novice hunting, tracking. Glimpsed predator-reading at sub-tier behavioral states.
- **Beren**: Expert deer/bear/wolf/fox/grouse/rabbit + tracks + field-dressing + butchery + predator-reading. Practiced bow handling, snare-craft, hide-curing. Novice basic carpentry. Glimpsed mason work.
- **Cale**: Expert timber (felling, hewing, milling, joinery), tool-craft, fire-craft, knot-craft. Practiced shelter, carpentry-as-craft, mason at journey-level (he can lay a foundation course but is not Anika's equal there). Novice tracking, hunting. Glimpsed plant-ID.

**Critical**: no character has a "Cultivator" or "Gardener" node (still absent from §14 — F5 from Run 13 unchanged). Anika is leading garden work on backstory + Forager-Expert plant-ID, not on a Gardener skill she lacks.

### Skill node

Three specialists, no overlap dominant. Cross-skill diffusion is light (everyone is Novice or Glimpsed in the other two domains). Cooperative labor division is the architectural promise this run tests.

### Goal

60 days. Establish a cooperative settlement with: (a) **100m² communal garden** (3.1× Ren's solo plot), (b) **rabbit hutch** (small livestock — bird pen judged scope-creep), (c) **dual-purpose shelter** (sleeping for 3 + dry storage), (d) **smokehouse + drying rack** for preservation, (e) hunting + foraging support cadence, (f) caloric stockpile for autumn pivot. Test: does coop change the verdict from Runs 13/14?

---

## Days 1-3 — Site walkthrough, role declaration, the three-list

### Beat 01 — Day 1 morning, the three readings

All three walk the clearing edge together. **§14.13 saliency** produces three layered reads of the *same world*:

- **Anika (Forager-Expert + Mason-mid)**: south-facing 6-8 hrs sun, wild-onion patch SE corner, fiddlehead seam in the wet ravine, deep loam at the meadow center (probably acidic but rich), drainage runs SE-to-NE, two large stones at the meadow edge usable for foundation. *Garden site* + *foundation stone source* surface together.
- **Beren (Hunter-Expert)**: deer trail NE-SW crossing the meadow at dawn-dusk, rabbit warren under deadfall ~80m S, bear scat ~3 days old at the stream bend (small black bear), grouse drumming log NW. Wind from W (prevailing). *Game patterns* + *bear awareness* surface together.
- **Cale (Forester-Expert)**: ~45 candidate trees in 200m radius (mostly fir, some birch + maple); two leaning-fir widow-makers to avoid; clean stand of straight fir 120m NW; deadfall already cured for fuel; bark suitable for shingle-underlayer on the down-slope birches. *Timber inventory* + *deadfall risk* surface together.

**This is the first finding of the run.** Three Expert-tier readings of the same clearing produce *three non-overlapping information sets*. A single Builder (Run 05 Maren) sees stones, drainage, logs. A single Forager (Run 13 Ren) sees plants, food, water. A single Hunter (Run 04 Sam) sees trails, sign, threat. **Three specialists at once produce the union** — which is the entire mechanical justification for coop scale.

**Systems invoked**: §14.13 saliency layered across three skill domains simultaneously.

**Gap surfaced — MP1**: Saliency rendering is per-client (§14.13 implementation note). When three players stand in the same clearing each sees a different visual prominence map. How do they *share* observations? Currently the design has cursor-hover + Inspect [E] + Character mutter + Action menu. None of those let Anika say "see that wild-onion patch?" and have Beren's client *light up* the patch for him. This is the **shared-attention gap**: when a Forager wants to point out something a Hunter wouldn't see, the design needs either pointing-gestures, vocal callouts (player audio), or a temporary saliency-broadcast. Open.

### Beat 02 — Day 1 afternoon, the three-list

The party sits at the meadow edge with Anika's notebook and writes the three-list. This is **labor-allocation as in-fiction artifact** — they negotiate, not by spark or contract but by capability:

- **Anika**: garden plot prep + planting + tending. Forage walks. Pantry build-out + preservation later. Communal cook on alternate days.
- **Beren**: clear the bear-scat drainage. Hunt + trap line. Daily perimeter scout. Builds rabbit hutch (small carpentry, his Practiced reaches it). Cook on alternate days with Cale.
- **Cale**: timber felling + milling. Shelter raise. Smokehouse + drying rack construction. Tool repair + handle-fitting + sharpening cadence for the party. Helps Anika with fence-build (his Practiced + her Practiced = Grade-A wattle).

This is not a one-time list. **They commit to rewrite it weekly**, dependent on what each surfaces in their domain.

**This is the cooperative texture §1 demands at coop scale.** No one is forced into any role. Each picks tasks where their *debuff stack is smallest* (§14.2). The alternative — Cale gardening, Anika hunting — would produce Grade-D output on both axes.

**Gap surfaced — MP2**: Trade-economics-internal — currently §9.3 jungle rules covers stranger interactions (Run 05 Beat 13 traders). It does *not* cover persistent cooperative-group dynamics: how do three players keep a running mental tab of *who is contributing what*? Is the iron pot Anika's (she carried it) or communal (she put it down)? When Beren brings in a deer, does Anika owe him cooking labor that she'd have done anyway? **Open: the design needs a position on internal-settlement economics that is neither full-spark nor pure-vibes.**

### Beat 03 — Days 2-3, parallel work begins

Day 2 morning: **three parallel work streams light up simultaneously**.

- **Anika**: walks the meadow grid, marks the 10m × 10m garden footprint with stakes + cordage. Begins clearing — the same sod-mat problem from Run 13. *But she has a real shovel.* With shovel + hatchet + Cale's bucksaw for the diagonal birch root she will hit on Day 5, the clearing rate jumps. Her solo Run 13 was ~1.5m²/day with trowel + hand fork; with shovel she gets ~3.5m²/day. **Tool tier is a multiplier; F4 from Run 13 is partly closed by the shared shovel.**
- **Beren**: walks the deer trail, sets 4 snares at the rabbit warren, scouts the bear-scat drainage. Returns by mid-afternoon with one rabbit (snare luck Day 2 was good) + the location of the bear's apparent home range (north slope, ~800m off).
- **Cale**: fells 4 trees in the clean stand NW. Limbs and bucks them on site. Drags two back to camp using a stoneboat improvised with light cordage (a technique Run 05 surfaced as Builder-Practiced — Cale can do it because Forester-Expert overlaps).

**End Day 3**: ~10.5m² of garden cleared (Anika), 1 rabbit + 1 grouse cached (Beren), 4 trees felled + 2 hauled (Cale). Body grade A across the party. Rations: 18 days collective remaining.

**The compound effect is real**. Run 13 Ren cleared ~4m² of garden in her first 3 days while also doing nothing else. The party of three cleared 10.5m² of garden + felled 4 trees + caught 2 small animals + scouted predator pressure in the same 3 days. **Output per player-hour is roughly equal**; coop's gain is *parallelism on different tasks* — which is the actual force multiplier, not "everyone gardens faster."

**Gap surfaced — MP3**: Tool-sharing as a real ergonomic problem. The shovel is one item used by two people (Anika and Cale both need it Day 2). The two-man crosscut saw is most efficient with two people — and it can be Cale-solo (slow) or Cale + Anika (fast, but Anika is off-garden). **Currently undefined**: how does Mother Nature track tool-conflict-of-use? If Cale picks up the shovel for stone-haul, Anika is blocked. Either inventory-share UI is needed or this becomes a real friction layer the design should model. *Productive friction* (good) or *frustrating friction* (bad)?

---

## Days 4-10 — Establishment week

### Beat 04 — Days 4-7, clearing + first timber

Anika finishes the 10m × 10m garden plot Day 7. **Seven days clearing 100m² with a shovel vs Ren's seven days clearing 15m² with a trowel = ~4.7× labor improvement, of which ~2.3× is shovel and ~2× is "no other tasks competing"**. Coop's win is not pure-parallelism; it's also *task purity* — Anika gardens fulltime instead of also foraging-because-she-must.

Cale fells 9 more trees Days 4-7 — total 13 trees stacked. He starts hewing the sill logs (hewn-bottom mating to stone foundation) on Day 6. **Forester-Expert** + **Carpenter-sub-node** = he's Run 05's Maren-equivalent for cabin work; she was Mason + Carpenter, he is Carpenter + light-Mason. **Anika is the Mason.** Run 05 Maren was both nodes in one head; here the cooperation splits them.

Beren snares Days 4-7: 4 rabbits, 1 grouse. He also bow-shoots a yearling deer on Day 6 (Hunter-Expert + 30m shot at a doe-yearling crossing the trail). Field-dressing happens in the field per §13.4 (scent-beacon timing), pack-out is 14kg meat + hide. **Day 6 evening the party eats hot venison stew from Anika's pot.** Caloric inflow: ~12000 cal of fresh meat + hide.

**This is the cooperative-pantry inflection.** A solo hunter in Run 04 packs out what one body can carry. A three-person team can stage relays. Beren field-dresses, walks 800m back to camp, returns with Cale, and they pack the rest in one second trip. **Caloric retrieval per kill goes up 30-40%.** This is the kind of multiplier that's structural, not skill-based.

### Beat 05 — Days 5-7, soil amendment, parallel

Anika does the same soil-amendment work Ren did in Run 13 — wood ash + leaf mold + stream silt. *But she has surplus labor available* — Cale's morning is consumed by timber, but his evenings can help her haul leaf mold. They do two evening hauls together — 4 pack-loads of leaf mold instead of Ren's solo 2. Soil amendment Grade B+ → A on the Three Sisters bed.

The pH problem (F6 from Run 13) is unchanged at the system level. Anika is still working blind on soil chemistry. Coop doesn't solve missing systems; **it solves labor shortage, not knowledge shortage**.

### Beat 06 — Days 8-10, fence pre-emptive

Day 8: Anika and Cale build the perimeter fence *before* planting. Run 13 Ren built fence at Day 11 then escalated after Day 17 deer raid. Here, Beren's Hunter-Expert observed *fresh deer trail through the meadow* on Day 1; the party builds 1.8m wattle fence around the full 10m × 10m footprint on Days 8-10, Anika weaving + Cale milling sapling posts. **Grade A weave** — both Practiced at wattle = combined output is higher than either solo.

**This is information-transfer paying off**. Beren saw the deer pressure on Day 1 because he was looking for it. Anika *trusted his read* and built defensively. The 40% lettuce loss Ren took on Day 17 of Run 13 never happens here.

**Gap surfaced — MP4**: Information-transfer between specialists is currently mediated by *player voice channels* (which the design has chosen not to enforce — jungle rules §9.3 implies emergent communication) or the cumbersome cursor-hover + Inspect pathway. In coop, **the high-value information is the *inference*, not the entity** — "deer pressure on this clearing" is what Beren knows. The design needs to support specialist-to-specialist inference sharing more directly than "you both look at the same dirt". Open.

**End Day 10 inventory**:
- Garden: 100m² cleared, amended Grade A, fenced 1.8m Grade A wattle, planting in progress (corn first per Three Sisters, second beds also started). 12 person-days of garden labor accumulated.
- Shelter: 13 trees felled, 4 hewn sill logs ready, foundation stones being hauled by Cale + Anika in spare hours. No shelter standing yet.
- Hunt/forage: 1 deer (12000 cal), 4 rabbits (~2400 cal), 1 grouse (~400 cal), foraged greens. Net: ~16500 cal in addition to preserved food.
- Rations: 14 days collective remaining (eating less than projected because of fresh meat).
- Body: all three Grade A or A-. No injury.

---

## Days 11-25 — Three trades, garden growth, the smokehouse decision

### Beat 07 — Days 11-15, the dual-purpose shelter

Cale + Anika raise the shelter Days 11-15. **6m × 4m footprint**, single-room, dry-stone foundation (Anika's lead) + log walls (Cale's lead) + bough roof underlayer + tarp-as-temporary-roof-skin (final shingles will come later; both tarps now committed). **Bigger than Run 05's 4×5m Maren cabin** because three sleeping bags + storage need it.

**Run 05 Maren took 11 days to do the foundation alone (Days 1-11)**. Anika + Cale finish foundation + raise to wall-plate height in 5 days because: (a) Anika is Mason-mid not Mason-Expert (slower per course alone), (b) but Cale's two-man crosscut + double-haul speed up everything, (c) and the *pad clearing* never had to happen — Beren cleared brush on Day 2 in parallel.

**Mastery delta still bites at the corners.** Anika is Mason-mid; foundation is Grade B-A (Run 05 Maren was Grade A clean). The corner notches Cale cuts on the wall logs are Carpenter-Expert Grade A. **The foundation will move slightly in its first winter freeze; the corners will hold.** Each component degrades or holds true to its builder's tier — §14.3 quality grade survives coop, it doesn't collapse into "average team output".

By end Day 15: shelter weather-tight under tarp roof, sleeping bags moved inside, dry storage zone separated from sleeping zone by a hewn-plank partition Cale ran up Day 15. Smoke-hood deferred to Days 30+ (no urgency in late spring).

### Beat 08 — Day 12 evening, the §12.4 logout test

Beren has an offline obligation (his player needs to be away ~10 hours real-time). The party tests §12.4 sleeping-bag tether at coop scale.

Beren crawls into his sleeping bag inside the shelter. **The shelter qualifies as a "safe region — own camp" per §12.4** (claimed territory; the three of them built it). Per the spec his body should *vanish* on logout. **But what is "own camp"?** Three players built it together. Is each of their logout-safety-claims valid here?

The design answer (proposed by this run): **the shelter is a shared claimed territory** — any of the three logs out safely. If a fourth player walks up and tries to sleep here, they would have to either trade with the residents (jungle rules) or sleep in *wilderness mode* (5-10 min vulnerable then despawn).

Beren logs off. His character vanishes. Anika and Cale continue: the in-fiction framing is "Beren is sleeping" — and the shelter has 3 bedrolls but Beren is "the one over there, dozing." **Cabin-fever is rejected per §12.2** so the remaining two players don't suffer for the absent third.

Beren's player returns ~9 hours later (real-time). His character reappears in his bag, slightly hungry/thirsty (the body advanced its meters during the offline interval at a reduced rate — §6 body model running on his character). He gets up, eats, resumes work.

**Gap surfaced — MP5**: Per-character offline meter-tick during tether sleep. Currently §12.4 only specifies the *spatial* logout behavior. The temporal — does Beren's hunger advance while logged off? — is unspecified. Proposal: meters advance at ~30% normal rate during tether-logout, so logging out costs *some* food/water but isn't a free time-skip. Otherwise farming-in-coop becomes "log out for 3 days, log back in to harvest". Severity HIGH.

**Gap surfaced — MP6**: Shared-territory definition. Three players built the shelter — does ownership-of-safety transfer to all three? What if Anika dies on Day 30 and Beren + Cale continue using the shelter — is it still her "safe region"? Per-shard DB needs a claim table with multi-owner support. Severity MEDIUM.

### Beat 09 — Days 16-22, garden + hunt + smokehouse start

The middle weeks settle into rhythm. Anika does ~3 hours/day garden tend (water haul, weed, pest check) — and **the watering load is lower per person** than Run 13 because Cale hauls water for camp use anyway and his trips overlap. The 100m² garden wants ~360L/week (Run 13's 15m² wanted ~54L; scaling is ~linear). Anika hauls ~30L/day; Cale's camp-water trips provide ~20L/day collateral; rainfall provides the rest. **No watering crisis like Run 13's drought week emerges yet** — May rainfall is reliable in this scenario.

Beren is bringing in meat on a cadence:
- Day 17: 2 rabbits + 1 grouse
- Day 20: 1 squirrel (slingshot demo to Anika), 4 rabbits across the snare line
- Day 22: a doe (Hunter-Expert big shot at 45m, vital lung, deer ran 80m before drop; tracking job ~30min). ~22000 cal.

**Cumulative meat through Day 22**: ~46000 cal of fresh + the early venison. Beren is pulling more meat than the party can eat fresh. **This is where the smokehouse becomes decisive.**

Cale begins smokehouse construction Day 19. Forester-Expert + Carpenter-sub means he builds a small enclosed shed (2m × 1.5m × 2m), wattle + clay daub walls with a small smoke-funnel and slats for hanging meat strips. **Grade A construction over 4 days, parallel with other work.** Anika cuts the doe into thin strips Days 22-24, smokes them in the new smokehouse with hardwood from Cale's birch waste.

**Days 22-24 yield: ~18000 cal of smoked venison, shelf-stable for ~6 months at temperate forest humidity.**

**This is the cooperative-pantry payoff in numbers.** Solo Run 04 Sam never lived to smoke her moose. Solo Run 05 Maren had no smokehouse and traded her labor for food. **Coop produces enough surplus protein that it has somewhere to go.** The smokehouse exists because Cale could build it without dropping anything else; Beren's hunting cadence exceeds eat-fresh demand; Anika's preservation Expert finishes the loop. **No one alone could make all three of those happen.**

### Beat 10 — Day 24, rabbit hutch + the contribution question

Beren has been trapping consistently — 11 rabbits in 24 days. He pitches the rabbit hutch: instead of all-trapping, *trap-and-hold* a few breeding pairs. Domesticated animals are §5.8 deferred, but rabbits caught and held in a hutch are *not* domesticated, they are *contained wild* — the design has no current spec for this, but the in-fiction shape is clear.

Cale builds the hutch Days 24-26. ~1m × 2m wood-and-wire enclosure with a brush-cover top. Wire from the collective stash + sapling frame. Beren stocks it with 2 does + 1 buck on Day 27 from his next snare round. **They will be eating periodically from the hutch by Day 50 if the does breed.**

**Gap surfaced — F27**: Contained-wild small livestock as a third-position between hunted and domesticated. §5.8 defers domesticated. §13.2 Carrion Chains covers wild kill. *Caught-and-held rabbits* are neither. Coop introduces this because the cost-benefit is only worth it if someone is consistently feeding and watering them, which a solo player rarely can be. **Proposal: a "wild-captive" mechanic where wild animals can be held for limited periods (~weeks), eat-stress reducing health, escape rate scaling with hutch quality.** Severity MEDIUM — directly impacts whether coop-scale farming-adjacent activity has its own lane.

**Gap surfaced — MP7**: Contribution tracking as social texture. By Day 24 Beren has brought in ~46000 cal of meat. Anika has not yet harvested anything from the garden (still ~16 days until first radish). Cale has built shelter + smokehouse + hutch but produces no calories directly. **Is the trade fair?** The three players have informal sense of it. **The design has no explicit balance**, and that is *probably correct* per §9.3 jungle rules — but the question becomes sharp when an NPC-style "player" under-contributes. If one player in MP goes AWOL for 5 days and returns to a smokehouse full of food, the design implies "jungle rules — settle it among yourselves". Severity: this is the social-friction question §12 has been quiet about.

---

## Days 26-40 — Steady state, the bear, the dispute

### Beat 11 — Day 28, the bear scout

Beren's Day 28 perimeter scout: fresh bear track at the stream bend, ~6hrs old, mid-size adult black bear. *He has seen this bear before* — same track signature as the Day 1 scat. The bear is now ranging the drainage actively.

**Beren's call**: he reports back at camp. The smokehouse is now a *food cache* in §13.2 Camp Stalker terms. The bear *will* discover it eventually.

Their response is layered:
- Cale builds a **hanging cache** for the smoked-meat surplus — 5m up an adjacent fir, weighted line per Maren's Run 05 technique. The smokehouse holds current-week supply; the hanging cache holds the bulk.
- Beren strings a **clatter-line perimeter** around camp using cordage + the iron pot's lid (will become its sole purpose; he carves a wooden lid for cooking). Three points of clatter coverage in the bear's likely approach corridors.
- Anika builds a **smoldering-fire-pit** at the meadow edge — kept going low overnight, scent (smoke) reaches the bear's likely approach. Effective deterrent for cautious bears.

**This is the cooperative-defense math from the briefing question.** Run 13 Ren's bear visit Day 64 removed ~50% of her garden in 15 minutes. Here, the bear has not yet visited because (a) three humans + a smokehouse + a perimeter fire produces a *much* louder + larger threat signature than one solo Forager, (b) Beren's daily presence + his bow are themselves a deterrent (apex predators read three-on-one + sharp-pointed-thing-throwers as serious), and (c) the smoked-meat is now hung not stored at ground level.

**Coop changes the bear math.** A bear's utility AI weights threat-of-injury vs food-reward. Three humans is not 3× one human; it is often *prohibitively* high on threat. The bear may still come (food reward is real) but the math favors patience over the lone solo's "the bear comes once and takes 50%".

**This is the §1 stress test at coop scale.** If coop trivialized bear pressure ("we have a Hunter, we just shoot the bear"), §1 would crack. It doesn't trivialize — Beren still wouldn't shoot a bear with a bow except in defense (§15.4 cost matrix: bow usually wounds, doesn't drop a bear; a wounded bear is the worst encounter). The defensive layering (hanging cache + perimeter + fire) is the *correct* response, not "kill the apex". **§1 holds**.

### Beat 12 — Days 30-35, garden first crops + the contribution dispute

Day 32: Anika pulls first radish. Smaller than Ren's Run 13 first radish (Day 36) because she planted earlier (Day 10 vs Day 14) but the soil is colder by 4 days. Day 36 first lettuce. Day 38 first beet greens.

By Day 40: ~2200 cal harvested from garden. **Forage + small game across the party in same period: ~38000 cal**. **Garden is 5% of party caloric inflow.**

**The contribution dispute (Beat 13 trigger)**: Day 36, Cale notices Anika spent the morning sleeping (she'd been up the prior night defending the garden against a raccoon visit — successful, no losses). He pulls his weight on water haul that morning + cooks. **No conflict, just rebalance**. But the *shape* of what just happened is the architectural moment. If Anika had been off-task because of player AWOL (not in-fiction tiredness), the rebalance would have been less smooth.

**This is what the briefing calls "settlement permadeath" preview**: if any one of the three goes AWOL for too long, the other two carry their share. **The settlement is robust to one slow player.** It would crack at two-slow-of-three.

### Beat 13 — Day 38, the §14.12 Field-Notes hypothetical

**Hypothetical inserted by the design exercise** (no death this run, but the question matters):

If Beren died on Day 38 — say, a bear charge during scout, Run 04 Sam's mechanical signature — what happens to the settlement?

- **Compendium**: Beren's per-account compendium retains his earned tier (§12.5 per-account, since this is solo/friends-only-equivalent — coop on a shared shard). His character's death does not erase his bear/wolf/deer Expert tiers from his *account*. His next character inherits.
- **Field Notes**: Beren's death by bear-charge generates the bear-charge Field Notes entry per §14.12 (death-only trigger). The entry lands in *his account's* compendium-attached field-notes section. **Does it also surface to Anika and Cale's clients?**
  - **Proposal**: Field Notes are per-account, not per-event-witnessed. Anika and Cale see Beren collapse, see the bear, but they do not unlock the *bear-charge* entry — they didn't die from it. They would need their own near-death encounter or their own death to unlock the entry. **This preserves §14.12 "death-as-teaching" mechanic**: knowledge transfer between characters happens via the compendium's *static* tiers, not via field-notes inheritance.
- **Settlement**: Anika and Cale are now two of three. Their labor allocation rebalances. Beren brought 70% of the party's protein — without him, the smokehouse pipeline halves. Anika can hunt with Beren's bow (after a slow Novice→Practiced learning curve) but cannot replace him in 60 days.

**Gap surfaced — MP8**: Field Notes attribution under multi-witness death. §14.12 spec is single-character. Coop introduces the question: when one player dies and others observe, who gets which entry? Proposal (above): per-account-of-the-dying, not per-witness. Severity MEDIUM.

**Gap surfaced — MP9**: Settlement inheritance on character death. If Beren dies, the shelter remains. The hutch remains. The smokehouse remains. *Is the settlement still tethered as "safe region" for Anika and Cale?* Per MP6 the multi-owner claim continues. **If Anika also dies**, is Cale a sole owner? **If all three die**, what happens to the structures? Per §13.1 brainstorming, they would persist in the per-shard DB; another player could find them. **The cabin from Run 05 Maren applies — abandoned settlements become diegetic encounters**. This is *good*. Severity LOW (the design already implies this; just needs explicit confirmation).

### Beat 14 — Days 36-42, the rabbit hutch's first generation

Day 36-38: doe 1 builds nest. Day 40: she gives birth (kits visible in nest). Day 42: 5 kits, healthy. **Rabbit gestation is ~30 days**; in 6 weeks Anika and Beren will have ~12-15 rabbits at the hutch.

This is the **§5.8 boundary in motion**. Caught-and-held rabbits are not domesticated — they have not been selected for docility, color, or yield. They are *captive wild*. They eat what Anika brings them (kitchen greens scraps, foraged grass), they breed because rabbits breed, and they are *much* easier to kill than wild rabbits. **The hutch is a calorie multiplier**.

**Gap surfaced — F28**: Hutch productivity scaling. How many rabbits can a 1×2m hutch support before crowding causes disease + cannibalism? Real-world answer is ~5-6 adults sustainably. The design needs a stocking-density spec that produces realistic crash dynamics when over-stocked. Otherwise hutches become magic-calorie-fountains. Severity HIGH (this is the same shape as the §1 dominance test for gardens).

---

## Days 41-55 — Garden maturation, the autumn pivot signals

### Beat 15 — Days 43-50, first wave of garden harvest

Anika's garden hits stride. Lettuce, beet greens, radish, then beans (bush varieties first), then early squash. **By Day 50, garden output: ~6800 cal cumulative.** Three Sisters bed (corn + climbing bean + winter squash) is still developing — corn at ~1.2m, beans climbing, squash spreading. **Main harvest expected Days 65-80.** 

Comparison: Run 13 Ren by Day 50 had ~1200 cal cumulative from 15m². The 100m² coop garden, in the same calendar window, has produced ~5.7× more — *which is exactly the area-scaling factor*, not a coop multiplier. **Garden output per m² is approximately the same whether one Forager or three split-roles work it.** Coop's win isn't yield-per-m²; it's *enabling the larger plot to exist at all*.

**This is the central finding of the run on the farming question**: cooperative scale doesn't make farming *more efficient*; it makes the *larger garden possible without starving during the deferred-calorie period*. Solo farming has a labor + caloric-debt floor that solo characters can't easily exceed. Coop changes the *scale of viable plot* but not the *calories-per-m².*

### Beat 16 — Day 49, rainfall + the cooperative-stress test

A 4-day rain front parks over the area. **Cale's roof is tarp-only** — he'd planned shingles for Days 55-60. The rain forces priority reshuffle: Cale spends Day 49-51 splitting shingles + getting them on the roof while wet. Grade B work (you can't split shingles cleanly in the wet). Anika's garden takes the rain happily; Beren is unable to hunt productively in the rain (deer trail tracks wash out hourly).

**Day 51 evening: shingles on, party is dry inside.** Coop took a hit to Cale's grade (B not A) but absorbed the weather without crisis. **A solo Cale-equivalent would have either (a) finished shingles in better weather earlier and not had a rain crisis, or (b) gotten soaked. Both are real solo outcomes; the coop's is "rushed shingles, OK quality".**

**This is the §2 mechanical-behavior test**: the rain front emerged from the §3.3 weather grid, not from a designer wanting drama. Cale's response was a real labor-priority decision. The grade-B outcome is a real cost. **§2 holds.**

### Beat 17 — Days 52-58, autumn signals + stockpile push

Day 52: night low ~6°C. First hint of autumn. The party reads it together: Beren's tracking shows deer dropping to lower elevations (early migration); Anika's perennial herbs are setting seed (a seasonal cue); Cale's birch is starting yellow.

**They shift to stockpile-mode.** Beren intensifies trapping (he's been at it; now sets 8 snares instead of 4). Anika begins drying + fermenting in earnest (the 4 glass jars start sauerkraut from kale + carrot, plus mushroom strings). Cale builds a **root cellar trench** — 1m deep, lined with leaf litter, capped with logs + sod — for beet + the coming carrot harvest.

Days 52-58 yield:
- Beren: 1 buck (Day 54, big shot, ~28000 cal); 8 rabbits across snare line (~5000 cal); 2 grouse.
- Anika: ongoing forage (~600 cal/day); fermenting setup; aggressive harvest of mature lettuce + radish + early squash + beet roots.
- Cale: root cellar finished Day 56; smokehouse running double-duty for venison + drying mushrooms; firewood stockpile 4× current weekly burn.

**Day 58 inventory**:
- Smoked + dried meat: ~52000 cal (smoked venison from Days 22+, current buck strips, smoked rabbit jerky, mushroom strings)
- Fermenting: ~4000 cal in jars + crocks (sauerkraut, fermented carrot)
- Dried beans + corn (drying on frame): ~8000 cal projected at full cure (Day 70+)
- Root cellar: ~6000 cal in beet + early carrot
- Hutch: 8 rabbits (5 kits maturing) — eat-on-demand, ~3000 cal standing
- Fresh garden continuing: ~600-1000 cal/day through Day 60
- **Total stored ≈ 73000 calories. ~30 person-days at maintenance + some buffer.**

**This is the autumn-prep math the briefing asked for**. Run 13 Ren ended Day 90 with ~24500 cal stored — ~12 days reserve. Run 17 coop ends Day 58 with ~73000 cal stored — ~30 person-days, with 30 days winter not yet hit. Projecting Days 59-60 + a notional 30 days of low-level autumn harvest, the party could plausibly enter winter with **~120-150 person-days of food** — actual winter-survival floor.

**Crude verdict on farming-in-coop caloric viability**: garden contributes ~12000 cal (16% of total stockpile by Day 58). **Hunting + smoking is still the dominant calorie source at ~60%**. Foraging + preservation is ~15%. Rabbit hutch ~5%. **Garden is meaningful but not load-bearing**. This matches Run 13's verdict at solo scale — *farming is supplemental, even at coop*.

### Beat 18 — Days 59-60, the close + the plan-for-next-season

Days 59-60: Party rests, processes, plans.

- Anika logs detailed garden notes: which varieties produced, which failed, what soil amendment direction next year. **Sets aside seed from her best plants** for next year (the bean stock self-saves; the carrot seed comes from one flowering plant she let bolt).
- Beren caches his bow + spare arrows. Plans winter trapline targeting the bear-active drainage (less prey competition with the bear in winter as bear hibernates).
- Cale lists shelter improvements for winter — smoke-hood (deferred from Day 30+), better roof flashing, sealed pantry door. He'll handle these in the autumn-into-winter shoulder.

**Day 60 close**: settlement standing. Party intact. Food stockpile real. Forward plan articulated.

---

## What ended Anika / the settlement

No deaths this run. No injuries beyond Day 47 (Cale a minor axe-skip on his thigh while splitting shingles in the rain; superficial, treated by Anika with foraged yarrow + clean bandage; ~3 days reduced labor; healed). 

Plausible cascades the party dodged:
1. **Bear escalation**: bear visited the drainage 3 times in 60 days (Days 28, 41, 53). All three times either deterred by perimeter (fire + clatter) or simply not committed to attack (smoked meat hung 5m up; smokehouse smoke-signal + three-human-scent on weather grid). A more aggressive bear individual (§5.5 apex identity variation — say, a sow with cubs, defensive-state high) would have been a genuine threat.
2. **Hutch crash**: an over-stocked or disease-stressed hutch (F28) could have produced a population crash. Anika kept stocking density conservative (5-6 adults max) and ventilated the hutch; no crash.
3. **Player AWOL**: tested at one-of-three (Beren Day 12 logout); coop absorbed. A two-of-three AWOL situation would have stressed the remaining player to breaking. **Implication for public-shard coop: settlements only work with reliable participation. Jungle-rules implies they can also fail at this**, which is design-honest.
4. **Cale's axe injury** could have been worse — Run 03 Tova's tundra axe-slip cascaded to sepsis. Anika's Forager-Expert medicinal knowledge closed that loop. Solo Builder Cale would have faced harder healing.

**The settlement is alive. Whether it survives winter, none of these runs reach. Run 17's data point is: coop changes the start-of-winter floor from "12 days food" (Run 13 solo) to "~30 person-days + a built shelter + a maturing hutch" (Run 17 coop). That delta is the answer to the briefing's question.**

---

## Architecture Stress-Test — what held, what cracked

### §1 No Dominant Strategy — VERDICT: HELD, with new texture

The run held §1 strongly. **Coop did not produce a dominant strategy**: hunting + foraging + preservation remained the load-bearing caloric channels (~75% of stockpile). Farming contributed ~16% — meaningful but supplemental. **No single archetype dominated** the cooperative — Anika, Beren, Cale each carried roughly a third by *task category*, and none was reducible to the others. **§1 also holds in the inverse**: a coop of three Foragers would have starved on protein; three Hunters would have starved on greens; three Builders would have starved on calories. The composition matters.

**The new §1 texture coop adds**: *party composition* is now a strategic axis. Solo-mode §1 says no single character dominates. Coop §1 says *no single party composition dominates either*. The 3-of-3-specialist team is one viable composition; a 4-Hunter raid party (a "war band") is a different viable composition with different costs; a 2-Builder + 1-Forager mining-and-stoneworking team is another. **Three players → three roles → still no dominant strategy**.

### §2 Mechanical Behavior — VERDICT: HELD

Every event emerged mechanically: the bear came (or didn't) on its utility AI weights, the rain came on the weather grid, Cale's axe-slip was a Wet-Wood (§13.4) shingle-split fatigue moment, Beren's deer kills came from his Hunter-Expert reading actual sign. **No Storyteller pacing.** Cooperative simulation doesn't make the world chattier — it makes the *party* react to the same emergent inputs in more layered ways. §2 holds.

### §12 Multiplayer Architecture — VERDICT: HOLDS AT BASE, GAPS AT EDGE

- **§12.1 Per-shard counts**: 3 players on one shard, well within the 64-128 per-shard target. Architecture irrelevant at this scale.
- **§12.2 No cabin-fever**: tested at Beren's 9-hour logout. The shelter did not punish the remaining two. **Held**.
- **§12.4 Sleeping-bag tether**: tested. Body vanished on logout from claimed territory. **Held in spirit, but MP5 + MP6 gaps surface** (meter-tick offline rate + multi-owner territory definition).
- **§12.5 Per-account compendium**: tested in the Field-Notes hypothetical (Beat 13). **Held in spirit, MP8 gap on death-witness attribution surfaces**.

### §14 Skill Architecture — VERDICT: HELD, with multiplier observed

**Mastery delta survives coop**: the foundation is Grade B-A because Anika is Mason-mid; the corners are Grade A because Cale is Carpenter-Expert. **No skill-level averaging** happens. Each output reflects its builder's tier. §14.3 quality grading holds.

**Cross-pollination is light**: 60 days of being-near-experts produces some Glimpsed→Novice movement in each character's adjacent domains (Anika watching Beren field-dress; Beren watching Anika weave wattle). Per §14.4 the XP path is XP-by-action, not XP-by-observation, so this is bounded — but the per-account compendium captures the *observations* and may quietly seed cross-pollinated entries. **§14 holds**.

### §14.12 Field Notes — VERDICT: NEEDS COOP CLARIFICATION

**MP8 surfaced**: when one of three dies and the others witness, who gets which entries? The proposed answer (per-account-of-the-dying) preserves the death-as-teaching mechanic but means in coop, *each player still learns from their own deaths*. This is design-coherent. Needs explicit confirmation.

### §14.13 Saliency — VERDICT: HELD, with MP1 gap surfacing

Three specialists each see different prominence overlays of the same world (Beat 01). Per-client rendering per §14.13 implementation note. **MP1 gap surfaces**: how do specialists *share* salience? Currently the design has only personal-attention channels. Coop introduces the question of shared-attention. **Open**.

### Cooperative-group friction (§12 silent) — VERDICT: GAP

§12.2 explicitly rejects cabin-fever as an *individual* pressure. **It does not address cooperative-group friction**. Beat 12's hypothetical contribution dispute didn't fire — but it could have. **The design is currently silent on whether intra-settlement friction needs mechanical support** (resource-tracking, claim-tracking, dispute resolution) or whether jungle-rules-emergent communication carries it. **Open question, MP7 captures it**.

### Permadeath at coop — VERDICT: HOLDS, with settlement-inheritance gap

A character death in coop loses character-level capability (Beren-dies → no Hunter-Expert in party). The settlement persists as structures. **MP9 gap**: how does multi-owner territory transfer when one owner dies? Proposal in Beat 13: surviving owners retain claim; full-party-death produces abandoned-settlement diegetic object. Coherent; needs explicit spec.

---

## Design Inventory

### Communal structures discovered
- **6m × 4m dual-purpose shelter** (sleeping zone + storage zone with hewn-plank partition) — Mason-mid + Carpenter-Expert collaboration; Grade B-A foundation + Grade A corners
- **2m × 1.5m × 2m smokehouse** (wattle + clay daub walls, smoke funnel, slats) — Carpenter-Expert solo
- **Drying rack** (post-and-rail open frame for corn + bean drying) — Carpenter-Practiced ad hoc
- **1m × 2m rabbit hutch** (wood-and-wire enclosure, brush-cover top) — Forester sub-skill + wire kit
- **Root cellar** (1m trench, leaf-litter lined, log + sod cap) — Builder-Practiced
- **1.8m wattle fence around 100m² garden** — dual Practiced+ collaboration, Grade A
- **Smoldering perimeter fire-pit** (bear deterrent, kept low overnight)
- **Hanging cache** (5m up adjacent fir; bulk smoked-meat storage)
- **Clatter-line perimeter** (cordage + pot lid + improvised noise-makers at bear-approach corridors)

### Shared recipes / processes surfaced
- **Cooperative seed-saving**: Forager identifies bolted carrot for seed; Builder cuts seed-head holder
- **Smoked venison strip-jerky** (Anika Practiced+ technique, Cale-built smokehouse)
- **Mushroom string drying** (Anika Forager-Expert)
- **Fermented kale-carrot kraut** (4 glass jars, Anika Expert)
- **Root-cellar packing** for beet + early carrot (cold-storage, post-frost-improving)
- **Cooperative water-haul** (Cale's camp-water trip + Anika's garden-water trip overlapping)
- **Three-trade barter** (skill-for-skill internal to settlement, not enumerated as economy)

### Labor-division pattern (proposed for §1 codification)
| Time period | Anika | Beren | Cale |
|---|---|---|---|
| Days 1-10 (establishment) | Garden clear + plant + amend | Hunt + trap + scout + bear-recon | Timber + shelter raise |
| Days 11-25 (steady-state) | Tend garden + forage + preserve | Hunt + trap on cadence | Shelter finish + smokehouse + hutch |
| Days 26-40 (defense + growth) | Garden tend + bear-deterrent fire | Hunt + perimeter scout | Repair + minor build |
| Days 41-55 (stockpile push) | Aggressive harvest + preserve + ferment | Big-game push + intensive trap | Root cellar + drying rack + winter-prep |
| Days 56-60 (close) | Seed save + final preserve | Cache gear + winter plan | Smoke-hood + roof flashing |

### Gaps surfaced (MP1-MP9, F27-F28)

| # | Gap/Need | Severity |
|---|---|---|
| MP1 | Shared-attention / saliency-broadcast between specialists | MEDIUM |
| MP2 | Internal-settlement economics — neither full-spark nor pure-vibes; the in-between case | HIGH |
| MP3 | Tool-conflict-of-use under shared inventory; productive vs frustrating friction | MEDIUM |
| MP4 | Specialist-to-specialist inference sharing (not just entity sharing) | MEDIUM |
| MP5 | Per-character meter-tick rate during tether-logout | HIGH |
| MP6 | Multi-owner shared-territory claim spec | MEDIUM |
| MP7 | Cooperative-group friction mechanics — §12.2 is silent on coop dynamics | HIGH |
| MP8 | §14.12 Field Notes attribution under multi-witness death | MEDIUM |
| MP9 | Settlement inheritance on character death; abandoned-settlement diegetic state | LOW |
| F27 | Contained-wild small livestock (caught-and-held rabbits) as third position between hunted + domesticated | MEDIUM |
| F28 | Hutch productivity scaling + crowding-crash dynamics | HIGH |

---

## EXPLORATORY DESIGN QUESTIONS

### Q1 — Does cooperative scale unlock farming viability that solo lacks?

**Partially. Coop does NOT make farming more efficient per m². It DOES enable larger plots to exist without starving during the deferred-calorie period.** Run 13 Ren's 15m² returned ~1200 cal by Day 50; Run 17's 100m² returned ~6800 cal by Day 50. The ratio is roughly proportional to area. The **enabling effect** is task-parallelism: garden labor doesn't compete with food acquisition because Beren hunts. Without Beren's hunting, no one could afford the garden's 40-day caloric-debt window. **Coop changes the floor, not the ceiling of cultivation efficiency.**

**Verdict**: Farming-in-coop is more *viable* (it can happen at all), but not more *productive per unit input* than solo. Coop makes farming a sensible *supplemental* layer of a settlement, but not the primary calorie engine.

### Q2 — Does §1 No Dominant Strategy hold at coop scale?

**Yes, strongly.** Run 17 produced a cooperative arc where (a) no single archetype dominated; (b) garden output remained supplemental to hunt + forage + preserve; (c) party composition is itself a meaningful design axis (different compositions → different runs, none dominant). **§1 generalizes cleanly to coop**: just as no single character path dominates solo, no single party composition dominates coop.

### Q3 — Does cooperative work require new MECHANICS?

**Yes — at least the gaps MP1-MP9 surfaced.** Most pressing: MP2 (internal-settlement economics), MP5 (offline meter-tick rate), MP7 (cooperative-group friction). Of these, MP2 + MP7 are *social-mechanics* gaps the design has been silent on. The design's preferred answer ("jungle rules carries it") may be sufficient for *between-stranger* interactions but is plausibly insufficient for *persistent-cohabitating-group* dynamics. **Open scope question**.

### Q4 — Multiplayer interaction with §14.12 Field Notes?

**Per-account-of-the-dying preserves §14.12's death-as-teaching mechanic.** Witnessing another player's death does not unlock entries — the player must still die themselves (or come close to it via dedicated entries TBD) to populate their own Field Notes. **This is design-coherent**: coop doesn't shortcut the lesson; it just means multiple players are slowly building their independent Field Note collections. MP8 captures the explicit-spec gap.

### Q5 — Is farming gated behind multiplayer? Does single-player MN = no farming?

**Not gated, but skewed.** Solo farming in Runs 13/14 is *barely viable*, often producing ~5-15% of caloric intake at high labor cost. Coop farming in Run 17 produced ~16% of caloric intake at much-lower-per-person cost. **Both are supplemental, neither is primary.**

**The scope question this raises**: is the cost of building farming (~16 systems per Run 13 + the F27-F28 coop additions here) justified by *coop-tier supplemental*? Or should farming be *gated* behind coop (single-player MN = no farming, just foraging + hunting)?

**Argument for solo-farming-stays**: §1 No Dominant Strategy demands the Forager + cultivation arc be solo-viable. Gating to coop violates this.

**Argument for coop-gating**: the F27 contained-wild livestock + F28 hutch dynamics + MP1-MP9 cooperative mechanics are *only meaningful at coop scale*. Solo farming becomes "small garden, lots of pests, mostly fails" — which is *honestly what Run 13 demonstrated*. Solo farming is already *barely viable*; making coop the proper venue would just acknowledge what's already true.

**Recommendation (this run)**: **don't gate, but be honest**. Solo farming should remain available and brutal (per Run 13). Coop farming should be supported as a *settlement-tier activity* with the F27 + F28 systems. The marketing + onboarding should say *"farming is for settled groups"* without forbidding solo attempts.

### Q6 — Settlement permadeath: if Anika dies, do Beren + Cale inherit?

**Per MP6 + MP9 proposal: yes.** Surviving owners retain shared claim. The settlement persists as long as one owner is alive + active. **If all three die**, structures persist in per-shard DB as abandoned-settlement diegetic objects — discoverable by future characters or other players. **This composes cleanly with §13.1 Forest Signs brainstorming and §9.3 jungle rules**: an abandoned cabin with rotting beds + raided pantry is a real diegetic encounter. The exact decay rate (how long before structures collapse without maintenance) is an open tuning question.

### Q7 — Does coop farming justify the scope cost?

**Probably not on farming-alone grounds.** But **coop introduces F27 + F28 + MP1-MP9 anyway** if the design wants to support settlement play at all (which §12 implies it does). **Farming is a downstream payoff of building the coop scope, not the justifier for it.** If coop settlement is in scope (it is), then *adding farming on top* is incremental, not foundational.

---

## COMPARATIVE ANALYSIS — vs solo runs

### vs Run 13 (Forager Ren, 15m² forest garden, 90 days solo)

| Axis | Ren solo | Anika + Beren + Cale (this run) |
|---|---|---|
| Garden area | 15m² | 100m² (6.7×) |
| Days to first edible | Day 36 (radish) | Day 32 (radish) |
| Bear pressure | 1 visit Day 64, 50% loss | 3 visits, 0% loss (defended) |
| Total stored cal Day 60 | ~3000 (mostly forage) | ~73000 (all sources) |
| Person-days food at close | ~5 days for 1 person | ~24 days × 3 people |
| Caloric balance during pre-harvest | Net-negative | Net-positive (Beren's hunting) |
| Per-person labor at close | ~6 hrs/day | ~6 hrs/day |

**Key delta**: Coop doesn't make Anika individually more productive than Ren. **Coop lets Anika's productivity be additive instead of conflicting with food acquisition.** Same hours, same skill tier, vastly different outcome — because the labor *doesn't have to substitute* for food.

### vs Run 14 (Builder Vasco, desert irrigation, 60 days solo)

| Axis | Vasco solo | Anika + Beren + Cale |
|---|---|---|
| Infrastructure built | Cistern + 600m channel + 3 plots | Shelter + smokehouse + hutch + 100m² garden |
| Harvest achieved | 0 (cycles exceed window) | Yes (multiple crops; main wave ahead) |
| Cascade events | Flash flood ate 75% of work Day 26 | Rain front Day 49 (absorbed) |
| Salt depletion | Critical Day 50 | N/A (forest biome, not metabolic equivalent) |
| Plausible 60-day completion | Infrastructure yes, crops no | Both infrastructure + crops + stockpile |

**Key delta**: Vasco's desert run was infrastructure-only because crop cycles exceed window. Run 17's forest + coop produces both infrastructure AND first-wave crop harvest in same 60 days. **Biome + party-size + crop-cycle alignment matters; coop alone doesn't change the cycle math.** Vasco-in-coop-desert would still not harvest in 60 days; the run window is the limit.

### vs Run 05 (Builder Maren, solo forest cabin, 54 days)

| Axis | Maren solo | Anika + Beren + Cale |
|---|---|---|
| Shelter built | 4×5m winter-viable cabin | 6×4m settlement shelter (less finished) |
| Food economy | Caloric deficit; survived via trade | Net surplus, smokehouse pipeline |
| Bear pressure | Background sign, no engagement | Active drainage bear, deterred 3× |
| Days to first cooked food after rations | Day 22 (trade) | Day 6 (Beren's first kill) |
| Mastery delta visibility | Strong; Expert vs novice deltas observable | Strong; per-component grades preserved |
| Solo viability if isolated | Required trade | Independent of external trade |

**Key delta**: Maren's run validated that **Builder is fragile in solo + spring forest at current food math**. Run 17 closes that fragility loop *internally*: Cale (the Builder-equivalent role) doesn't need to trade because Beren is in the party. **Coop closes the Builder-food-fragility gap that Run 05 N46 surfaced.**

---

## Multiplayer-specific implications

### Trade
- **Within-settlement trade** is currently undefined (MP2). It's also currently *necessary* for coop to feel coherent. Proposal: explicit "shared inventory" zone (food cache, tool box) + "personal inventory" zone (bedroll, sentimental items, account-bound gear). Internal trade is movement-between-zones, not formal exchange. This sidesteps spark-economy creep.
- **Between-settlement trade** is the §9.3 jungle-rules case Run 05 Beat 13 surfaced.

### Conflict
- §12.2 explicitly rejects cabin-fever. **But cooperative-group friction is not the same as cabin-fever** (MP7). One player hoarding the smokehouse output, another over-hunting the local deer — these are *behavioral* conflicts the systems should *enable* but not *force*. Proposal: provide diegetic mechanisms for asymmetry-tracking (visible cache levels, observable kill counts via tracks-on-the-grid) but no mechanical "conflict meter."

### Inheritance
- MP9 proposal: settlement structures persist after character death; multi-owner claim continues with surviving owners; full-party death → abandoned-settlement diegetic object. Decay rate TBD. **Field Notes** stays per-account.

### Shared defense
- Run 17 demonstrated that **3 humans + perimeter + smokehouse-cache-management = a defensive posture solo cannot match**. The bear's utility AI weights threat-of-injury higher against three armed humans than against one. **This is a meaningful coop multiplier that doesn't require new systems** — the apex utility AI from §5.5 + scent-on-grid §12.8 already produces this if scent + audio + visual signatures aggregate across nearby players. **Confirm the aggregation rule in implementation**.

---

## Cross-references

- §1 No Dominant Strategy — coop generalization tested: no single party-composition dominates
- §2 Mechanical Behavior — every event mechanical; coop adds no scripted hand
- §3.1 Forest biome — generous seasonal window enables both shelter + first-crop in 60 days
- §5.5 Apex individual identity — bear's avoidance of three-human camp tests the utility-AI threat-weighting
- §5.8 Domesticated animals deferred — F27 surfaces "contained wild" as third position
- §6 Body — meter-tick during tether logout (MP5) needs explicit rate
- §9.3 Jungle rules — between-settlement; doesn't cover intra-settlement (MP2)
- §12.1 Per-shard counts — irrelevant at 3 players
- §12.2 No cabin-fever — held individually; silent on group (MP7)
- §12.4 Sleeping-bag tether — MP5 + MP6 gaps on offline meter rate + multi-owner territory
- §12.5 Per-account compendium — confirmed for friends-only-equivalent coop
- §13.2 Camp Stalker — bear's behavioral learning + Run 17's defensive layering
- §13.4 Wet Wood & Fire — Cale's rain-day shingle Grade B
- §14 Skill Architecture — mastery delta per-component preserved at coop; no team-averaging
- §14.3 Quality grading — Grade B-A foundation + Grade A corners + Grade B rain-rushed shingles
- §14.12 Field Notes — MP8 multi-witness attribution gap
- §14.13 Saliency — MP1 shared-attention gap surfaces at coop
- §15.6 Forager game-completion path — Anika's no-firearms approach validated at coop; defensive posture is layered (fire + clatter + hanging cache + three-person presence)
- Run 04 (Sam, moose-rut death) — defensive-attack hypothetical for Beren
- Run 05 (Maren, solo Builder cabin) — N46 Builder-food-fragility *internally closed* by coop
- Run 13 (Ren, solo Forager garden) — F1-F26 gaps unchanged by coop; F27-F28 are coop-specific additions
- Run 14 (Vasco, desert irrigation) — N66 + N79 confirmed biome+system gaps; coop doesn't close them

---

## What Run 17 delivered

1. **Coop changes the floor, not the ceiling**. Garden output per m² is unchanged; what coop enables is *the larger plot existing at all without caloric crisis* during the 40-day deferred-harvest period.
2. **§1 generalizes to party-composition**: just as no character dominates solo, no party composition dominates coop. The 3-specialist team is one viable shape; war-bands and trader-pairs and Builder-collectives are others.
3. **The Builder-food-fragility gap from Run 05 closes internally** at coop. Maren needed trade; Cale doesn't.
4. **The bear math changes at coop**. Three humans + perimeter discipline + hung cache produces a deterrent solo can't match — *without* requiring new systems beyond what §5.5 utility AI + §12.8 scent grid already imply.
5. **§12 multiplayer commitments hold at coop scale** but reveal nine specific gaps (MP1-MP9), most pressing in MP2 (internal economics), MP5 (offline meter-tick rate), MP7 (cooperative-group friction). All three are *silent in the current spec* and need explicit positions.
6. **§14 mastery delta survives coop without averaging**: per-component output reflects per-builder tier. Coop does not become "average team output"; it remains a *layered output of distinct skills*.
7. **§14.12 Field Notes per-account-of-the-dying** preserves the death-as-teaching mechanic at coop. Witnesses don't unlock entries; the dead-player's account does.
8. **F27 (contained-wild livestock) + F28 (hutch productivity scaling)** are coop-specific additions to the farming-system gap list from Run 13. The rabbit hutch is a clean third position between hunted (§13.2) and domesticated (§5.8 deferred); it earns a spec.
9. **Settlement permadeath inheritance** is design-coherent (MP9): surviving owners retain claim; full-party death → diegetic abandoned settlement. Aligns with §13.1 Forest Signs persistent-world commitment.
10. **The honest answer to the briefing's framing question — "does multiplayer change the verdict on farming?"** — is **mostly no on yield, partially yes on viability**. Coop makes farming *possible to attempt* without starving; it does not make farming *productive enough to dominate*. **Farming remains supplemental in coop too, but supplemental-at-coop is a real lane** in a way supplemental-at-solo is barely. The verdict-shift is from *barely-viable* (solo) to *modestly-viable-but-still-supplemental* (coop). **§1 holds**; farming as a cooperative-tier supplemental activity preserves all design principles. The decision to commit it (or not) is still about the scope cost (Run 13's 16 systems + Run 17's 11 additional MP/F gaps), not about whether the design tolerates it.

The run pauses at Day 60 with the settlement standing, food stockpile real, and the autumn-into-winter question deferred to a future Run 18 or Run 19 — *that* run is where the real test arrives, and where coop's true winter-tier value (or its breakdown) will surface.
