# Shipping + Distribution + Marketing — Research Report

## Domain summary

Shipping a 2D Godot game in 2026 is technically simple and commercially brutal. Steam took 20,282 releases in 2025; only 608 (2.99%) cleared 1,000 reviews, the proxy for any meaningful commercial success ([GamesRadar / Zukowski](https://www.gamesradar.com/games/a-terrifying-20-282-games-were-released-on-steam-in-2025-and-just-608-managed-to-get-1-000-reviews-expert-finds-we-might-be-in-a-bit-of-an-indie-golden-age/)). The median indie earns $249 gross — $174 after Valve's cut ([SteamData Research](https://medium.com/@research_86150/steam-in-2025-the-year-indie-games-ate-the-industry-alive-666abf04551f)). Mike Rose (No More Robots) calls 2026 "2025 all over again" — continued bad times for publishers and devs ([gamedeveloper.com](https://www.gamedeveloper.com/business/-this-is-just-a-death-cycle-no-more-robots-gets-candid-about-the-state-of-indie-publishing)). For a novice's first game, the honest objective is "ship a real game on a real store" — not financial success. Two stores matter: Steam (130M MAU, paid funnel, algorithm-driven) and itch.io (30M MAU, friendly, low-stakes, the indie default for first releases).

## Topic catalog

### Steam Direct (the gateway)
- **Brief**: $100 USD per app, non-refundable but recouped after $1,000 adjusted gross revenue. 30-day waiting period between fee payment and release; minimum 2 weeks of public "coming soon" page required.
- **Why-drilled**: L1 fee gates spam → L2 fee gates store quality (Valve's Greenlight replacement) → first principle: platforms with infinite supply must price-floor submissions to keep the algorithm-feed signal-to-noise survivable.
- **Source**: [Steamworks docs](https://partner.steamgames.com/doc/gettingstarted/appfee).
- **Novice failure mode**: paying the fee before having a finished game, burning the 30-day clock, or skipping the 2-week coming-soon window and missing wishlists.
- **Priority**: Required (P0).

### itch.io (the default first-launch venue)
- **Brief**: 90/10 default revenue split (developer keeps 90%; you can also choose your own split, including 100/0). Pay-what-you-want supported. No gatekeeping, instant publishing.
- **Why-drilled**: L1 friction-free publishing → L2 testing ground for first games → first principle: novice work needs a low-stakes venue where shipping itself is the win.
- **Source**: [itch.io docs](https://itch.io/docs/butler/), [Fungies comparison](https://fungies.io/steam-vs-itch-io-indie-developers/).
- **Novice failure mode**: treating itch as a revenue platform (it isn't — 30M visits, but most traffic is jam-driven and free-content-driven).
- **Priority**: Required (P0).

### butler CLI (itch.io deploy pipeline)
- **Brief**: Command-line tool for pushing builds; supports channels (windows/mac/linux/web), patches (delta uploads), `--hidden` for review-before-publish, and GitHub Actions integration via `setup-butler`.
- **Why-drilled**: L1 automate uploads → L2 enforce reproducible builds → first principle: every manual deploy step is a future regression vector.
- **Source**: [butler manual](https://itch.io/docs/butler/pushing.html).
- **Novice failure mode**: uploading via the web form, forgetting to mark a build as the active one, leaving a broken build live.
- **Priority**: Strong (P1).

### Godot 4 export presets (Windows/Mac/Linux/Web)
- **Brief**: Export Templates installed via Editor menu; Windows export is straightforward (.exe), Linux is straightforward (.x86_64), macOS requires either ad-hoc signing (no Gatekeeper trust) or a $99/yr Apple Developer ID + notarization, Web exports to WebAssembly + multiple files (must be zipped with `index.html`).
- **Why-drilled**: L1 cross-platform reach → L2 reach measured by user-discoverable platforms → first principle: the cost of "support all platforms" is signing, notarization, and per-platform QA — not the export click.
- **Source**: [Godot docs — exporting for macOS](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_macos.html), [Web export](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_web.html).
- **Novice failure mode**: not zipping web exports; shipping unsigned Mac builds and getting Gatekeeper-blocked; forgetting to install export templates that match Godot version.
- **Priority**: Required (P0 for Windows; P2 for Mac signing).

### Code signing (Windows/macOS)
- **Brief**: macOS notarization requires Apple Developer ID Certificate ($99/yr) + app-specific password; without it, downloaded apps are blocked by Gatekeeper. Windows EV cert (~$300/yr) avoids SmartScreen warnings but is non-essential for a first indie release; ad-hoc unsigned exes work but show "publisher unknown."
- **Why-drilled**: L1 OS trust gates → L2 user friction at first launch → first principle: OSes treat unsigned binaries as adversarial; the cost is conversion at install.
- **Source**: [Godot macOS export docs](https://github.com/godotengine/godot-docs/blob/master/tutorials/export/exporting_for_macos.rst).
- **Novice failure mode**: paying for Apple Developer Program before validating Mac demand; assuming Windows EV is mandatory (it isn't for indie scale).
- **Priority**: P3 for first game — ship Windows-only or use ad-hoc Mac signing.

### Steam capsule art (the highest-leverage visual)
- **Brief**: The single biggest pre-launch lever inside the dev's control. Must be readable at 120x45px. Bold typography, clear genre cues, not detailed artwork. Drives CTR through wishlist-to-buy stack. 68-88% of wishlists come from people who never play the demo — they saw the capsule, read the description, hit wishlist ([Zukowski Next Fest survey](https://presskit.gg/field-guides/steam-capsule-art-guide)).
- **Why-drilled**: L1 capsule = ad → L2 the algorithm shows the capsule, not the trailer → first principle: in an attention-economy with thousands of competitors, the thumbnail is the product.
- **Source**: [presskit.gg capsule guide](https://presskit.gg/field-guides/steam-capsule-art-guide).
- **Novice failure mode**: putting the entire game's identity into one detailed illustration that disappears at small sizes; not A/B testing.
- **Priority**: Required (P0).

### Steam wishlists (the leading indicator)
- **Brief**: Wishlist conversion rates by volume: <5K = ~15%, 5-40K = 20%, 40-100K = 23%, 100K+ = 25%. Day-1 conversion ~5%, Week-1 ~20%, Month-1 ~27% ([game-oracle.com](https://www.game-oracle.com/blog/wishlist-to-sales-2025), [game discoverco](https://newsletter.gamediscover.co/p/the-state-of-steam-wishlist-conversions)). 7K wishlists in a short window threshold for "Popular Upcoming"; 30-50K for Gold-tier ($100K+) revenue.
- **Why-drilled**: L1 wishlists predict sales → L2 Valve sends launch + sale emails to wishlisters, which drives the algo "spike" → first principle: Steam's algorithm rewards revenue spikes; wishlists are the queued ammunition for the launch-day spike.
- **Source**: [Zukowski "Killing the myths"](https://howtomarketagame.com/2023/09/04/killing-the-myths-behind-steams-visibility/).
- **Novice failure mode**: treating wishlist count as a vanity metric, or believing there's a "magic threshold" for guaranteed featuring (there isn't).
- **Priority**: Required (P0).

### Steam Next Fest (the demo lever)
- **Brief**: Quarterly week-long demo festival. Median wishlist gain: ~800 over the week (February 2026) — not a breakout number. The algorithm treats pre-fest wishlists and recent velocity as a quality signal; it amplifies existing momentum, doesn't create it ([Ziva](https://ziva.sh/blogs/steam-next-fest-2026)). Use it once per game (re-entry rules tightened).
- **Why-drilled**: L1 free visibility burst → L2 Valve uses fest engagement to predict launch revenue → first principle: festivals are launch-day dry runs, not magic.
- **Source**: [presskit.gg Next Fest guide](https://presskit.gg/field-guides/steam-next-fest-guide).
- **Novice failure mode**: entering Next Fest with no demo polish, no marketing runway, expecting it to "discover" the game.
- **Priority**: Strong (P1).

### Trailer (Derek Lieu principles)
- **Brief**: Establish genre in first few frames. First 5 seconds must stop the scroll. The "Ice Cream Shop" principle: once viewers know your genre, they need to know why your game over the alternatives — your hook is the differentiator. Genre-familiar games need more proof of differentiation; novel-concept games can do more with less footage ([Derek Lieu](https://www.derek-lieu.com/start-here)).
- **Why-drilled**: L1 trailers convert → L2 platforms autoplay them muted → first principle: the trailer is judged on the first 5 silent seconds because that's the slot the platform gives it.
- **Source**: [derek-lieu.com](https://www.derek-lieu.com/start-here).
- **Novice failure mode**: a 90-second logo intro; saving the hook for the climax; relying on audio.
- **Priority**: Required (P0).

### Pricing tiers
- **Brief**: Indie defaults: $9.99, $14.99, $19.99, $29.99. The $10 tier dominates indie hits (R.E.P.O., Buckshot Roulette at $3, Rusty's Retirement at $7). Games over $10 see ~10% wishlist-to-sale conversion vs ~15% under $10 ([game-oracle](https://www.game-oracle.com/blog/wishlist-to-sales-2025), [TechSpot](https://www.techspot.com/news/110391-indie-games-steam-getting-cheaper-10-hit-taking.html)). Add 20-30% to your target price because games sell mostly at discount. Launch discounts have weak correlation with conversion — committed buyers buy regardless.
- **Why-drilled**: L1 price = expectation bracket → L2 low price recovers what marketing can't (impulse) → first principle: in a discovery-broken market, friction at checkout is the dominant variable.
- **Source**: [HowToMarketAGame pricing](https://howtomarketagame.com/2022/08/23/4-tips-to-help-you-price-your-indie-game/).
- **Novice failure mode**: pricing first 2D game at $20 because "that's what indies cost"; refusing to discount.
- **Priority**: Required (P0).

### Steam reviews (the score gate)
- **Brief**: 10 reviews unlocks the visible label. Tier thresholds: Mostly Positive (70-79% + 10), Very Positive (80%+ with 50+), Overwhelmingly Positive (95%+ with 500+) ([SteamDB rating](https://steamdb.info/blog/steamdb-rating/)). The 10-review unlock is the urgent first goal — until then, the page is unscored and converts worse.
- **Why-drilled**: L1 social proof → L2 algorithm uses score as quality signal → first principle: pre-10 the page lacks the label that converts the marginal undecided viewer.
- **Source**: [Steamworks review docs](https://partner.steamgames.com/doc/store/reviews).
- **Novice failure mode**: not asking buyers to review (a non-obnoxious in-game post-credits prompt is fine); responding to negative reviews (don't, mostly — it never reads well).
- **Priority**: Required (P0).

### Steam refund policy (the short-game tax)
- **Brief**: 14 days + under 2 hours playtime = automatic refund. For short games (<2 hr completion) this is a structural wound — players can finish then refund. Examples: Summer of '58, Before Your Eyes ([XDA](https://www.xda-developers.com/steams-two-hour-refund-window-killing-niche-indie-games/)).
- **Why-drilled**: L1 consumer protection → L2 refund cost externalized to dev → first principle: short experiences are misaligned with Steam's refund model; design for >2 hours of meaningful play, or accept the loss.
- **Source**: [Steam Refunds](https://store.steampowered.com/steam_refunds).
- **Novice failure mode**: shipping a 90-minute experience on Steam at $5 and watching ~30%+ refund.
- **Priority**: P1 — design constraint, not just marketing.

### Press kit (presskit.gg / dopresskit)
- **Brief**: presskit() by Rami Ismail (2014, mostly stale but standard) and presskit.gg (modern WordPress plugin successor). Must include: logo, 5+ screenshots (1080p+), 2-3 GIFs, fact sheet (genre, price, platforms, release date, dev contact), trailer link, capsule art ([presskit.gg](https://presskit.gg/blog/indie-game-press-kit-guide), [dopresskit.com](https://dopresskit.com/)).
- **Why-drilled**: L1 give press what they need → L2 reduce friction-to-coverage → first principle: press writes about whoever is easiest to write about.
- **Source**: [Rami Ismail dopresskit](https://github.com/ramiismail/dopresskit).
- **Novice failure mode**: making press dig for screenshots; no high-res logo; no contact email.
- **Priority**: Strong (P1).

### Streamer/Curator outreach
- **Brief**: Cold email response rate is 3-5% generally; for game-streamer outreach, single-digit reply with 1-2 actual plays per 100 emails ([cloutboost](https://www.cloutboost.com/blog/top-10-influencer-outreach-tools-for-gaming-publishers-2026)). Personalization (genre fit, custom clip) outperforms volume. Keymailer/Woovit/Terminals are the platforms; free tiers are throttled.
- **Why-drilled**: L1 streamers drive sales → L2 streamer attention is supply-constrained → first principle: the 95% no-response rate is normal and the strategy must absorb it.
- **Source**: [Mailforge cold email](https://www.mailforge.ai/blog/average-cold-email-response-rates).
- **Novice failure mode**: taking silence personally; spamming; one long email instead of: "hi [name], here's a 30-sec clip of my game in your genre, key on request."
- **Priority**: Strong (P1).

### Music licensing
- **Brief**: Kevin MacLeod (Incompetech) is CC-BY — free use commercial, must credit "Kevin MacLeod (incompetech.com) Licensed under Creative Commons: By Attribution 4.0." CC0 = public-domain-like, no credit required. CC-BY-SA = ShareAlike, viral — your derivative work must also be CC-BY-SA, generally avoid for commercial games. Kenney is CC0 (safe). itch.io has CC0 music asset packs.
- **Why-drilled**: L1 use free music → L2 license type controls downstream rights → first principle: licenses define what you can monetize and how — read them, don't trust filename.
- **Source**: [Incompetech licenses](https://incompetech.com/music/royalty-free/licenses/), [Creative Commons Music Guide](https://www.silvermansound.com/creative-commons-music-licensing-guide).
- **Novice failure mode**: using "free music" off YouTube without checking; using CC-BY-SA tracks in commercial work; not crediting in-game/store-page.
- **Priority**: Required (P0).

### Trademark name search
- **Brief**: Before locking a game/studio name: USPTO TESS search (US), Steam search, social handles, domain. Even unregistered names with prior reputation can block your registration on bad-faith grounds.
- **Why-drilled**: L1 avoid lawsuits → L2 brand is identity infrastructure → first principle: rebranding mid-launch destroys your wishlist URL, social handles, and press accumulation.
- **Source**: [Game World Observer trademarks](https://gameworldobserver.com/2022/01/19/what-developers-should-know-about-trade-marks-to-protect-their-games).
- **Novice failure mode**: discovering a name conflict 2 weeks before launch.
- **Priority**: Required (P0).

### LLC vs sole prop
- **Brief**: Single-member LLC ($50-300 to file, ~$0-800/yr depending on state) gives personal-asset protection. Sole prop = personal liability for any lawsuit. For a novice's first game with no investment and pre-revenue, sole prop is fine; form the LLC when revenue justifies the filing fee or when you sign a publisher/store contract that exposes personal assets.
- **Why-drilled**: L1 limit liability → L2 separate business legal entity → first principle: liability protection is insurance, not free.
- **Source**: [ZenBusiness LLC for game devs](https://www.zenbusiness.com/starting-an-llc-for-video-game-designers/).
- **Priority**: P3 for first game.

### Reddit (r/IndieDev, genre subs)
- **Brief**: r/IndieDev (263K) accepts work-in-progress GIFs, devlogs, feedback requests. Self-promotion tolerated when framed as feedback. Genre subs (r/roguelikes, r/strategy, r/godot) are strict — read rules, don't drop links cold.
- **Why-drilled**: L1 free reach → L2 community-moderated quality gate → first principle: paid attention is rare; earned attention requires community participation, not extraction.
- **Source**: [Filament Games Reddit guide](https://www.filamentgames.com/blog/grassroots-reddit-marketing-a-how-to-guide-for-indie-devs/).
- **Novice failure mode**: posting a Steam link with "wishlist plz" — gets nuked instantly.
- **Priority**: Moderate (P2).

### Steam algorithm signals (what actually moves it)
- **Brief**: Per Zukowski: revenue (primary), wishlists (secondary, via launch emails), playtime, follower count, tag alignment. Page visits don't matter. Review percentage above "Mixed" doesn't matter much. Poor launches don't permanently brand the game ([Zukowski](https://howtomarketagame.com/2023/09/04/killing-the-myths-behind-steams-visibility/)).
- **Why-drilled**: L1 algo rewards revenue → L2 Valve maximizes platform GMV → first principle: every "growth hack" must trace back to a revenue spike or it's noise.
- **Source**: [howtomarketagame.com](https://howtomarketagame.com/).
- **Priority**: Required reading (P0).

### Sales / discount strategy
- **Brief**: Major Steam sales (Summer, Autumn, Winter, Spring) are the long-tail revenue. Discount cycle: 10-15% at 1 month, 25% at first major sale, 33-50% deeper sales. Steam restricts deep discounts within X days of launch and between sale events. Never discount above launch-week price (anti-gouge).
- **Source**: [HowToMarketAGame benchmarks](https://howtomarketagame.com/benchmarks/).
- **Priority**: P2 (post-launch).

## Recommended novice shipping path

For a first 2D Godot game by a developer who is new to game dev:

**1. Ship on itch.io first** — pay-what-you-want or low fixed price ($3-5). No $100 fee, no 30-day window, no Gatekeeper, instant publish via butler CLI. Use itch's devlog system as you build. If 50 people play it, that's a real game-dev portfolio asset.

**2. If the itch release shows life (any traction — 100+ plays, positive comments, friends asking when it's on Steam), commit to a Steam release.** If itch traction is zero, that's diagnostic, not failure: it tells you the next game needs different scope, hook, or genre.

**3. Steam shipping pipeline (8-12 weeks before launch):**
- Pay $100 Steam Direct fee.
- Post Steam page (capsule + screenshots + 30-60 second trailer + description). 2 weeks public minimum required.
- Get capsule art done by someone competent ($200-800 on Fiverr or genre-specific freelancer is correct spending; budget here is the highest-ROI marketing dollar).
- 30-60 second trailer: cold open with gameplay in first 2 seconds, hook by 5 seconds, no logo intro.
- Press kit on presskit.gg or a simple static page: logo, 5 screenshots, 3 GIFs, fact sheet, contact.
- Demo for Steam Next Fest. Enter once with the demo polished and a small wishlist baseline (Next Fest amplifies; it doesn't create).
- Run a 1-month pre-launch wishlist push: r/IndieDev devlog GIFs (1-2/week), genre Reddit posts (rules-aware), 50-100 personalized streamer/YouTuber emails to channels in your genre with <10K subs (they reply more).

**4. Launch:** target a low-traffic month (February or August), price at $9.99 or $14.99, ad-hoc Mac sign or skip Mac entirely, Windows-only is fine for a first release.

**5. Post-launch:** push for 10 reviews ASAP (a polite end-of-game prompt is acceptable). Don't respond to negative reviews. Patch obvious bugs in week 1. First major Steam sale = first 25% discount.

**Realistic expectation: most first 2D Godot indie games will sell <100 copies on Steam.** The win condition for a first game is "shipped, learned the pipeline, didn't take on debt." Plan accordingly.

## Must-include shortlist

1. **Capsule art** — highest-leverage single asset.
2. **Wishlists** — leading indicator; build for 3+ months pre-launch.
3. **30-60 second trailer with gameplay in first 2 seconds.**
4. **Steam Next Fest demo** (one entry, polished).
5. **Press kit** (presskit.gg style — logo, 5 screenshots, 3 GIFs, fact sheet, contact).
6. **Pricing at $9.99 or below** for a first 2D game.
7. **butler CLI for itch deploys** + Godot export presets locked in.
8. **Music license proof** (CC-BY attribution string in credits + store page).

## Commonly oversold

- **TikTok/Twitter/X marketing for non-visually-striking games**: Zukowski: "nothing really works on Twitter and TikTok" for most indies. Outliers (1-2K wishlists from a viral video) exist but require a visually exceptional game and luck.
- **Paid ads (Meta, Google, Reddit ads)**: negative ROI for indies <$15. CPM math doesn't close at indie price points.
- **Discord community building pre-launch for a first game**: empty servers cost more attention than they generate. Skip until you have 1K+ wishlists.
- **Influencer agencies/Keymailer paid tiers**: $25-100/month for outreach platforms is a tax most first-game indies should not pay.
- **Press releases to gaming sites**: PR Newswire / paid distribution = ~0 coverage. Direct personalized email beats it.
- **Launch discounts**: weak correlation with conversion; committed wishlisters buy at full price.
- **Building a YouTube devlog channel as marketing**: devlog viewers are mostly other devs, not buyers. Marcus Persson / Sebastian Lague are exceptions, not the rule.
- **Localizing a $10 first game into 12 languages**: do English + 1-2 high-conversion languages (Simplified Chinese, Russian) only if your genre supports it.
- **Console ports for a first game**: cert costs, dev kit gating, and platform fees rarely close the math at first-game scale.

## Realistic expectations

- **Median 2025 indie Steam game**: $249 gross, $174 to dev ([SteamData Research](https://medium.com/@research_86150/steam-in-2025-the-year-indie-games-ate-the-industry-alive-666abf04551f)).
- **2.99% of 20,282 games released in 2025 cleared 1,000 reviews** — and 1,000 reviews ≈ ~30K-50K wishlists ≈ ~$100K gross at $10. That is the bar for "actual modest income" ([GamesRadar / Zukowski](https://www.gamesradar.com/games/a-terrifying-20-282-games-were-released-on-steam-in-2025-and-just-608-managed-to-get-1-000-reviews-expert-finds-we-might-be-in-a-bit-of-an-indie-golden-age/)).
- **Average debut team year-1 gross was ~$120K — but that's outlier-skewed**; the median is much lower.
- **Mike Rose**: average year-1 Steam game revenue is under $25K ([ResetEra](https://www.resetera.com/threads/mike-rose-founder-of-indie-pub-no-more-robots-roughly-estimates-that-average-year-1-sales-revenue-for-steam-games-is-under-25k.137682/)).
- **First 2D Godot game from a novice**: realistically <100 copies on Steam, <$1K gross. Plan for the learning, not the income.
- **First-month sales = 25-40% of lifetime revenue** for typical indie ([game-oracle](https://www.game-oracle.com/blog/wishlist-to-sales-2025)). Steam favors front-loaded, not long-tail.

## Cross-references

- Build pipeline (gates 12-13: Godot architecture, asset pipeline) — exports must work before any of this matters.
- Playtesting (gate 11) — the demo for Next Fest is the playtested vertical slice, not raw build.
- Scope (gate 5) — refund policy structurally penalizes <2hr games; design 2-4 hour minimum or accept losses.
- Legal (gate 9 if separate) — music CC-BY attribution, trademark search, LLC timing all gate the legal-safe-to-ship checklist.

## Sources

- [Chris Zukowski — Killing the myths behind Steam's visibility](https://howtomarketagame.com/2023/09/04/killing-the-myths-behind-steams-visibility/)
- [Chris Zukowski — Benchmarks](https://howtomarketagame.com/benchmarks/)
- [HowToMarketAGame — pricing tips](https://howtomarketagame.com/2022/08/23/4-tips-to-help-you-price-your-indie-game/)
- [HowToMarketAGame — 60 mistakes ebook](https://howtomarketagame.com/wp-content/uploads/2023/05/Zukowski_60MistakesEbookV1.pdf)
- [Steamworks — Steam Direct fee](https://partner.steamgames.com/doc/gettingstarted/appfee)
- [Steamworks — User Reviews](https://partner.steamgames.com/doc/store/reviews)
- [Steam Refunds policy](https://store.steampowered.com/steam_refunds)
- [SteamDB — rating algorithm](https://steamdb.info/blog/steamdb-rating/)
- [Godot — exporting for macOS](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_macos.html)
- [Godot — exporting for Web](https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_web.html)
- [butler manual](https://itch.io/docs/butler/)
- [presskit.gg field guides](https://presskit.gg/field-guides/steam-capsule-art-guide)
- [presskit.gg Next Fest guide](https://presskit.gg/field-guides/steam-next-fest-guide)
- [presskit.gg press kit guide](https://presskit.gg/blog/indie-game-press-kit-guide)
- [Rami Ismail — dopresskit](https://github.com/ramiismail/dopresskit) / [dopresskit.com](https://dopresskit.com/)
- [Derek Lieu — start here](https://www.derek-lieu.com/start-here)
- [Mike Rose / No More Robots — death cycle interview](https://www.gamedeveloper.com/business/-this-is-just-a-death-cycle-no-more-robots-gets-candid-about-the-state-of-indie-publishing)
- [Mike Rose — average year-1 indie revenue under $25K](https://www.resetera.com/threads/mike-rose-founder-of-indie-pub-no-more-robots-roughly-estimates-that-average-year-1-sales-revenue-for-steam-games-is-under-25k.137682/)
- [GamesRadar — 20,282 games / 608 over 1K reviews 2025](https://www.gamesradar.com/games/a-terrifying-20-282-games-were-released-on-steam-in-2025-and-just-608-managed-to-get-1-000-reviews-expert-finds-we-might-be-in-a-bit-of-an-indie-golden-age/)
- [SteamData Research — Steam 2025 indie analysis](https://medium.com/@research_86150/steam-in-2025-the-year-indie-games-ate-the-industry-alive-666abf04551f)
- [GameDiscoverCo — wishlist conversions 2024-2025](https://newsletter.gamediscover.co/p/the-state-of-steam-wishlist-conversions)
- [Game-Oracle — wishlist-to-sales 2025](https://www.game-oracle.com/blog/wishlist-to-sales-2025)
- [Ziva — Steam Next Fest 2026 data](https://ziva.sh/blogs/steam-next-fest-2026)
- [TechSpot — indie games getting cheaper, $10 hit](https://www.techspot.com/news/110391-indie-games-steam-getting-cheaper-10-hit-taking.html)
- [Fungies — Steam vs itch.io comparison](https://fungies.io/steam-vs-itch-io-indie-developers/)
- [Incompetech — Kevin MacLeod licenses](https://incompetech.com/music/royalty-free/licenses/)
- [Silvermansound — CC music licensing guide 2025](https://www.silvermansound.com/creative-commons-music-licensing-guide)
- [Game World Observer — trademarks for indie devs](https://gameworldobserver.com/2022/01/19/what-developers-should-know-about-trade-marks-to-protect-their-games)
- [ZenBusiness — LLC for video game designers](https://www.zenbusiness.com/starting-an-llc-for-video-game-designers/)
- [XDA — Steam 2-hour refund window killing niche indie games](https://www.xda-developers.com/steams-two-hour-refund-window-killing-niche-indie-games/)
- [Filament Games — Reddit grassroots marketing](https://www.filamentgames.com/blog/grassroots-reddit-marketing-a-how-to-guide-for-indie-devs/)
- [CloutBoost — influencer outreach tools 2026](https://www.cloutboost.com/blog/top-10-influencer-outreach-tools-for-gaming-publishers-2026)
- [Mailforge — cold email response rates 2025](https://www.mailforge.ai/blog/average-cold-email-response-rates)
