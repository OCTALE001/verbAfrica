# Storyline

*A running narrative of building verbAfrica. Newest entries at the top.*

---

## 2026-06-13 — The cohort becomes 11: Jabba in, Zolelo found

The working number has always been 10. That changed.

Just Jabba (Njabulo Mpanza) is a genre-fluid rapper at the intersection of hip-hop, Kwaito and jazz — BA Honours in Political Studies (Wits), BSocSci in International Relations (UCT), six languages, Apple Music editorial placements, brand partnerships with Red Bull and Markham. He's been written up in Mail & Guardian and represented South Africa at governance forums on four continents. 150K+ streams between his solo work and Bougie Pantsula (the satirical duo with Matt Ryan). Tagline: "Township-raised. World-trained. Untranslatable." There was no clean argument for leaving him out.

On the same day, the Zolelo gap finally closed. Since June 9 he'd been the "Profile coming soon" placeholder — no EPK submitted, nothing findable at the time. His intake was filled from his portfolio site: Zolelo Magwaza, brand iiam3siixtii ("I am 360"), a visual storyteller trained in engineering who built his career across video production, photography and VFX. 750+ projects, 5,000+ digital assets, Netflix (Soweto Blaze), NBA Life Season 2, Brand SA's 30 Years of Democracy campaign. His original blankness wasn't absence — just timing.

Both now have profile photos on the platform.

One thing worth sitting with: "10 creatives" was always a working assumption, not a hard rule. The cohort is whoever the cohort actually is. Growing it should be intentional rather than incremental — worth naming explicitly so future additions are decisions, not drift.

---

## 2026-06-11 — Figma 1:1 rebuild: two screens done, rate limit again

The Canva prototype looked nothing like the site — generative tools improvise; they don't copy. So back to Figma (file `rpD0GbZn1P9meCWlecWeTX`) for a true 1:1 rebuild from `index.html` itself: real CSS tokens, real copy, real cohort data.

**Done before the wall:**
- **01 · Landing** — pixel-faithful: gold full-bleed, "Connect. Create. Collaborate.", both Client/Creative cards with the ✓ lists and CTAs.
- **02 · Browse** — the whole page at full height (2007px): nav with active-state Browse pill, gold search hero, 5 category cards, 7 filter chips, all **10 listing cards with the real cohort photos**, ratings/locations/POA badges computed with the site's own formulas, footer with the feedback link.

**Two lessons:**
1. `figma.createImageAsync` isn't supported in the MCP environment — photos go in via the `upload_assets` tool + a curl POST per image. The local `images/` folder made this painless.
2. The Starter plan rate limit struck mid-build again (~12 calls in). It reset between June 9 and 11, so it behaves like a daily quota despite docs saying 6/month. **Remaining:** Profile Detail modal (Fifi), Post a Brief form, Creative Portal selector — the build scripts are written and the photo workflow proven, so next session is execution only.

Handed the remainder to a scheduled cloud agent (`verbafrica-figma-build-resume`, one-time run 2026-06-12 09:00 SAST) carrying the full build spec, the rate-limit budget, and the photo-upload workflow. First experiment with an overnight hand-off — if it works, "leave it with Claude" becomes a real pattern.

---

## 2026-06-11 — Canva prototype + a gap audit of the repo

Two threads today.

**Canva prototype.** After the Figma attempt hit a rate limit on June 9, we tried the other route: Canva generated an 8-page presentation walking through the web app's key screens — landing ("Connect. Create. Collaborate."), browse directory, profile detail, post-a-brief, how-it-works, creative portal — using the live site's actual copy and the gold #C8860A branding. Saved as "Presentation - verbAfrica" in the verbAfrica Canva folder, shareable as a design option without sending anyone to the live site. Three alternative generated versions are parked as candidates if the first read isn't right.

**Gap audit.** Asked "what's missing from this folder?" and the honest answer is: the repo documents *product and operations* well, but three things live nowhere — brand assets (the logo/colours exist only inside `index.html` and in Canva), cohort consent records (their photos and rates are public; nothing on paper says they agreed), and a place to collect beta feedback as it arrives (the form feeds a sheet, but synthesis has no home). None block beta; consent is the one with real-world weight.

First real enquiry sent through the live site — and it vanished. No email, no row in the sheet. The browser had shown "Enquiry sent! 🎉".

Root cause, found by testing the endpoint from the terminal (where you can actually read the server's response, unlike the browser's `no-cors` blindness): the live deployment was **still running Version 1 of the Apps Script** — the one with the placeholder `PASTE_YOUR_SHEET_ID_HERE`. The redeploy during the June 9 debugging never actually took. Every submission since had been executing, erroring on the placeholder, and silently returning a failure nobody could see.

Two lessons, both earned twice now:

1. **Apps Script: edit → save → Deploy → Manage deployments → New version.** Without the "New version" step, the Deploy button re-saves the old snapshot and the live URL keeps running stale code. This is the second time the same gotcha got us. Now it's a rule, written in `Operations/03_Forms_Setup.md` and in memory.
2. **"Verified" means reading the server's answer, not trusting the client's toast.** The browser shows success because `no-cors` hides everything. The terminal test (`curl` the POST, follow the redirect, read the JSON) is the only honest check. From now on, every endpoint change gets verified that way before we call it done — `{"ok":true,"row":N}` or it didn't happen.

Also worth noting: the failure mode was *graceful from the visitor's side* — they saw success, no broken page. Bad for us (lost enquiry), invisible to them. That trade-off is worth rethinking post-beta: maybe the form should only claim success when it can prove delivery.

Fixed, redeployed, verified with a real server response. Also added a second notification email to the alert chain.

---

## 2026-06-09 — Beta testing kicks off: cohort first

Pushed Phase 2 to the public site and sent it to the cohort to test as themselves. Two firsts in one go:

**First public release** of the redesigned platform — cohort + images + working enquiry forms behind the scenes. Before pushing we caught a privacy thing I'd missed: phone numbers and emails of the cohort were rendering openly on the detail view. Anyone who opened the page could copy them. Hidden them on the public profile and replaced with a "Contact via verbAfrica" button that routes through the enquiry form (and therefore through me). Keeps my "I'm the gatekeeper during beta" decision honest, and protects cohort contact info from random visitors.

**First feedback collection.** Auto-created a Google Form via the Apps Script (same script that handles bookings — keeps everything in one place). Nine questions, designed to be fillable in 5 min, structured around what we actually need to learn: does the profile feel like you, what's missing, would you share this with a client, what's clunky. Linked as a "💬 Send feedback →" button in the site footer, also shareable as a direct link via WhatsApp.

This is the **first time something we built is sitting in front of users who aren't me**. The next 2–3 days are going to be the most honest signal we've had about whether the platform reads as serious, useful, and embarrassing-free. Whatever comes back goes straight into the Storyline.

---

## 2026-06-09 — Forms are alive

End-to-end verified: a real submission on the platform → row in the `Bookings` sheet → email notification to me. Took longer than expected to debug — the wrinkle was a quiet one. When you Deploy in Google Apps Script, it **snapshots** the script as a versioned copy. Every edit after that lives in the editor but doesn't run when the web app URL is called. The deployment was running an early version with the placeholder `SHEET_ID`, silently failing in the catch block, and returning "Completed" with no logs. We chased our tail for a while before the redeploy fixed it.

Lesson worth keeping: **with Apps Script web apps, "deployed" ≠ "current."** Every meaningful script change needs Deploy → Manage deployments → New version. The URL stays the same; only the version behind it changes.

Phase 2's hardest dependency is now cleared — clients can actually reach me. The remaining Phase 2 work (direct WhatsApp/email shortcuts on creative cards) is cosmetic in comparison.

---

## 2026-06-09 — Figma prototype: 5 frames created, blocked by rate limit

Started building a clickable Figma prototype in the verbAfrica Figma file (`rpD0GbZn1P9meCWlecWeTX`). Using the Figma MCP plugin and `use_figma` tool.

**Completed:**
- 5 screen frames created at 1440×900, laid out left-to-right: 01 · Landing, 02 · Browse, 03 · Profile Detail, 04 · Post a Brief, 05 · Creative Portal
- Each frame has a full nav bar: amber verbAfrica logo, Browse / Post a Brief / I'm a Creative links, CTA button

**Blocked:** Figma MCP Starter plan rate limit hit on the call to build the Landing page content. Requires Figma plan upgrade to continue.

**Remaining work:** Landing hero + search + category chips + featured cards, Browse grid, Profile Detail modal, Post a Brief modal, Creative Portal selector screen, and prototype interaction wiring between all screens.

---

## 2026-06-09 — Repo sync: docs brought in line with recent work

Ran a pass across the whole repo to align documentation with where the project actually is. A few things were out of date from the June 5 foundation session and had never been updated to reflect the June 9 cohort work.

**What changed:**
- `memory/context/verbafrica.md` — full rewrite. The old version predated the entire folder structure and referenced files that no longer exist. Now reflects current state: 10-creative cohort, `data/creatives.json` as source of truth, correct folder map, Phase 1 in progress.
- `Strategy/02_Roadmap.md` — Phase 0 marked complete (Operations playbook deferred to Phase 2); Phase 1 marked in progress with completed items ticked; Current Phase header updated; June 9 entry added to Decisions Log.
- `data/creatives.json` — entries sorted alphabetically by name (Chef Nqobi → DG-TAL Format → Fifi Frequency → Hāzy → Kgale → LuMai → OhTeeh → Seru → Shivolski → Zolelo); IDs reassigned 0–9 to match.
- `verbAfrica Research/04_Key_Insights_and_Open_Questions.md` — "About the Business" section updated: replaced the old "50+ listed creatives" success metric with the December 2026 testimonial from Vision §4; corrected team description.
- Datalake folder deleted (EPKs removed); references stripped from documentation.

---

## 2026-06-09 — Phase 2 opens: deciding *how* a booking actually works

With the cohort live, the next move was Phase 2 — booking infrastructure. The instinct was to dive into wiring forms first. We resisted: *design the workflow first, build the plumbing to match*. Same logic as the Vision-before-Roadmap order in June.

I sat through a structured interview about how a booking should actually flow. Three answers shifted my thinking on what verbAfrica is operationally:

**Source of truth = a tracking sheet, not an inbox.** The instinct everywhere is "send the enquiry to email." But the right architecture for a flexible MVP is: the *sheet is the ledger*; email / WhatsApp / future channels are *alarm bells*, swappable without losing data. Clean separation between record and notification.

**No public response promise.** I'd assumed marketplaces need an SLA. But I'm not staffing a 24/7 inbox. Better to commit internally to a target we keep, than publicly to one we might miss on a bad day. We can publicise an SLA once we know our real cadence (Phase 5).

**Stay involved through gig-day on purpose — to learn.** This was the biggest shift. The framing I'd been working with said "hand off after confirmation; less work for you." But the right play in the beta is the opposite: stay involved at every stage, including gig-day, so we observe operations end-to-end. **The expensive operations are the data**. We will differentiate (automate, hand off) *after* we know what to.

A new concept got named explicitly for the first time: the **broader talent network** — a pool of off-platform people I can pull from when the cohort can't fulfill a role. Publicly we still present the 10. Operationally we have reserve. Important to write down so it doesn't become invisible.

Two things flagged for the beta to answer: (1) when a cohort member declines, do clients prefer us to solve invisibly or offer them a choice? — we default to invisible swap and gather preference via feedback forms. (2) When does someone in the broader pool earn cohort-promotion?

The pricing decision is also provisional: **pass-through during beta** (cohort quotes, client pays, verbAfrica takes no cut). The trade: no revenue *now*, in exchange for trust, learning, and the data to decide a real pricing model in Phase 5.

Wrote three docs that came directly out of this thinking:
- `Operations/00_README.md` — folder index
- `Operations/01_Booking_Playbook.md` — the 9-stage lifecycle, decisions captured, open items
- `Operations/02_Response_Standards.md` — internal targets, never-miss thresholds

Also set up `images/` with a naming convention so the cohort can supply profile photos and we wire them when they arrive.

The takeaway, which I want to keep in front of me through the beta: **the role I'm playing now is intentionally expensive**. Curator + deal-maker + gig-day point of contact for every booking. The cost buys learning. The learning buys product.

---

## 2026-06-09 — Cohort live on the platform (locally)

Closed the loop on the cohort enrichment: `data/creatives.json` now holds the 10 cohort members instead of the 15 placeholder profiles. The site renders them via the fetch refactor we set up on June 5 — open the local preview, click Browse, and there they are.

A few interpretive calls when porting from the `Cohort/*.md` files to JSON, worth re-checking:
- **Chef Nqobi's category** — she listed `food`, which isn't in the UI taxonomy. Mapped to `content` only for now. Decide later whether to add `food` as a top-level filter.
- **Zolelo** — profile still sparse. Included as a minimal "Profile coming soon" placeholder so the cohort visibly stays at 10. Easy to swap when info lands.
- **Hāzy's emails** — personal and business. Used the business address (HAZYPTYLTD) for the public profile, kept the personal one out.
- **DG-TAL Format** — no Instagram handle, left blank publicly.
- **OhTeeh, LuMai, Chef Nqobi** — no specific rates, defaulted to "POA" with a generic service line.

The Internal Notes (B) sections of the `Cohort/*.md` files are still blank — the *why in cohort*, *contact prefs*, *tech intervention* fields. Worth filling those one or two at a time as conversations happen, not in a batch sit-down.

Next: confirm all 10 render correctly in a real browser, then Phase 2 (operations playbook + wiring the enquiry forms so submissions actually reach me).

---

## 2026-06-09 — Cohort data enrichment: all 10 members

Ran a full EPK audit across the `/EPK_s/` folder — 20 files total. Read every EPK, matched to cohort members, and enriched 9 of 10 cohort files (Zolelo remains unknown). For the 4 members with no EPK submitted, went online and pulled public data.

**What changed:**
- **01-Shivolski** — Added bio enrichment, Halls OMFCC + Global ComLimited campaigns, phone, audience stats in internal notes
- **02-Chef Nqobi** — Fully populated from web research: Sinqobile Kwame Nkabinde, Culinary Director of TastyTing & Taste The Beat, co-founder of Melodies and Munchies, trained at Hilton Sandton
- **03-Kgale** — Added "Mona Lisa" ft Sni Mhlongo as top notable work (11K+ streams, Trace/MTV Base), streaming stats, @kgaldrogo IG handle, Welkom FM
- **06-Seru** — Corrected 3 mixtapes → 5 mixtapes, added debut album Leeto (100K+ streams), viral RedBull 64 Bars, more events/radio, @serutheellipsis IG + TikTok
- **07-Ohteeh** — Fully populated from web research: Ontlametse "Tlami" Putu, R&B/Soul artist, producer, musical director, DJ, teacher; signed to Velocity Muzik; @ohteeh IG
- **08-Hāzy** — Added Stem Master rate (R900), Podcast Final Mix rate (R1,800), HAZYPTYLTD business email
- **09-LuMai** — Populated from web research: singer/songwriter/model/actress, @lumai.rsa, on Apple Music + Deezer, features in Afro-pop/R&B space
- **10-DG** — Added email, expanded Multichoice/Netflix/Showmax titles, additional clients (Prime Video, Telkom, Gallo, Ukhozi FM), extra services (Data Recovery, Subtitling, Encoding)

**04-Zolelo** — No EPK, no public web presence found. Stays blank.

11 non-cohort EPKs identified in the folder (collaborators/reference artists): Kallo, REBA, K. Fresh, Spontaneous DJ, Thapelo Masebe, Lowfeye, Flagonstudios, Just Jabba, LaCabra, Lula Odiba, OB WAN.

---

## 2026-06-05 — The foundation conversation

Today was about figuring out the *shape* of this thing before building it further.

### What started the session

I wanted to know if Claude could see my live site at `octale001.github.io/verbAfrica/`. Turns out it could — but the way it fetches pages doesn't run JavaScript, so all 15 creatives looked invisible to it even though a real browser sees them fine. Useful lesson: **the tool you use to "see" the site matters**. The site has actually worked publicly for a while; I just didn't realise.

### The decision: thinking before building

The instinct was to dive into adding a backend, wiring forms, etc. Instead I paused. The cohort is *10* creatives, not the 8 the conversation started with, and I haven't really written down what I'm building or why. So I asked for help structuring the planning itself before doing more.

### Documentation system

Claude proposed (and I agreed to) **four kinds of "stuff"**:
- **Research** — what's true about the world (already in `verbAfrica Research/`)
- **Strategy** — what we're doing and why
- **Operations** — how we run it day-to-day
- **Cohort data** — the people

Plus `data/creatives.json` as the single source of truth for the website (instead of editing the JS array directly in `index.html`), and per-creative markdown files in `Cohort/` for internal notes — separate from what's publicly displayed.

### Vision v1

I sat through a 5-question interview that pushed me to articulate things I'd been carrying loosely:

1. **The company** — not a marketplace, a *curated booking platform* with an event-builder vision (book a band + setlist + photographer + shotlist in one flow).
2. **Why booking admin first** — it's the *door*, not the destination. Solves a real problem AND gives us deep visibility into how creatives actually work, which unlocks the rest.
3. **Company role** — verbAfrica is product #1 of a creative-industry tech-enablement studio. My engineering + manufacturing background is the unfair advantage. Cohort members get bookings + a tech edge no peer has (projection mapping, 3D printing, AI/VR/AR). Symbiotic by design.
4. **Success by Dec 2026** — every cohort member able to say: *"I'm getting bookings through verbAfrica. verbAfrica has also helped me build a strong business foundation, and levelled up my game by enabling my art and vision through tech."*
5. **Not doing** — open registration, label/PR/management, international.

The most surprising thing was how the wording forced clarity: changing *"people"* to *"cohort members"* in Q1 wasn't a typo, it was a strategic distinction (curated vs open marketplace). **Worth writing things down even when you "know" them.**

### Decision: don't deploy yet

I want to keep working in local preview until I'm happy with the platform. The live site stays as-is for now.

### Decision: keep this storyline

Started this folder so I can build a how-to someday — for myself, for future cohort members who might one day start similar things, and maybe one day as content. Claude will update it as we work, and a scheduled task will catch up between sessions.

### Roadmap and feedback loops

After Vision we wrote the Roadmap (`Strategy/02_Roadmap.md`). Five phases, each tied explicitly to the Vision strands it serves. Every phase has a "what we're measuring" section — that's how feedback gets baked in rather than tacked on. The doc also has a running Decisions Log and a list of Open Questions, both append-only.

Two things that surprised me here:
- I didn't have an answer for "how many bookings = success in Phase 3." That's now sitting in Open Questions, which is honest. Better to admit the gap than fake a number.
- The Vision and Roadmap turn out to be very few pages each. Strategy doesn't have to be long to be useful. Most of the value was forcing the wording, not the length.

### The Journey folder

Decided to keep this storyline as a separate `Journey/` folder — not part of the working docs, just the story behind them. The plan is to build a how-to someday: for myself, for future cohort members, maybe as content. Set up a memory so Claude updates this automatically during sessions, and a scheduled task that catches up every 3 days when I'm not around.

### End-of-session state

- `Strategy/01_Vision.md` — v1 done
- `Strategy/02_Roadmap.md` — v1 done with 5 phases, 6 open questions, 7 decisions logged
- `Journey/Storyline.md` — this file, with the foundation entry
- `data/`, `Operations/`, `Cohort/` — folders exist, empty
- Scheduled task `verbafrica-state-of-the-union` — runs every 3 days at 9am
- Exporting Vision + Roadmap to Canva (plain PDFs) so I can read them away from the screen

### What's next

Phase 1 — Cohort Onboarding. I send Claude the 10 cohort members' details, the data moves into `data/creatives.json`, and we get all 10 profiles up on the local platform. Then operations.

---

## June 13, 2026 — Photo crops fixed

Noticed the cohort photos were cropping badly on the cards — Chef Nqobi's head cut off, a few others drifting out of frame. The cause: every photo defaulted to `background-position: center`, but most of these are portraits with faces in the upper third, so "center" sliced straight through foreheads.

The site already had a per-creative `imagePos` hook (Zolelo was using `center top`), so the fix was data, not code. Looked at all nine photos and set each one's position by where the face actually sits — `center top` for the high-framed shots (Chef, Zolelo), `center 25%`–`35%` for the rest. DG-TAL and Shivolski have no photo yet, so they stay on initials.

Lesson worth keeping: when a layout knob already exists in the data layer, reach for that before touching CSS.

---

Alexander pushed back on that crop pass: the low `imagePos` values centred faces but cut off bodies and left dead space around the subjects. Reframed three by eye in the live preview — Jabba back to plain `center` (full seated figure + green outfit, "perfect before"), Zolelo to `center 12%` (20% clipped his eyes), Chef to `center 25%`. Left Seru alone.

Lesson: "faces in frame" isn't the goal — *subject* centred is. A face dead-centre with the outfit cropped reads worse than a slightly high head with the whole person showing. Verify framing visually per-photo, not by a rule.

---

All 11 cohort cards now have photos. Added DG-TAL Format (an environmental "at the laptop" shot — wider/looser than the rest) and Shivolski (the dynamic framing-hands portrait). DG's head sat under the badges, so its crop went much lower (`center 8%`) than the portraits. Flagged DG's different look as fuel for the eventual photo-uniformity pass. Committed as 85eab67.

---

## June 16, 2026 — Shivolski profile from media kit + killed the location pins

Updated Shivolski's listing off her refreshed 2026 media kit. The PDF had far more than the old entry: real brand work (Capitec, Super C, 250 Machine, music videos for Speedsta & Zoocci Coke Dope), a sharper positioning bio (hip hop / Amapiano / youth culture, crowd psychology over plain hosting), and named events (Hipnotik, Southpoint, Wav Impact). Folded the strongest of it into bio, highlights, and notable in `data/creatives.json`. Left Cotton Fest / Zee Nation out on purpose — those are "where I'm headed" in the kit, not work done.

Per Alexander, the role line now leads with all four titles: Dancer / Content Creator / Social Media Influencer / MC (was just "Dancer / MC / Content Creator"). The app only filters on three categories (Artists / MC / Content), so under the hood she stays `mc` + `content` — the extra titles are display-only for now.

Two standing things came out of this session:
- New hard rule: never use emojis in output unless explicitly asked. Saved to memory.
- Acting on that, stripped the location-pin emoji from all five places it appeared in `index.html` (profile card, both listing-card variants, enquiry pill). Verified in the live preview — DOM shows zero pins left.

Nothing committed yet; all changes sit in the working tree pending Alexander's go.

---

## 2026-06-23 — WhatsApp feedback round 1 (front-end fixes)

Alexander sent a batch of 12 feedback notes (WhatsApp screenshots + text) from testing the live site on his phone. Agreed to defer real auth/accounts (Supabase) to a later proper-backend phase, and to leave the colour rework for last. Knocked out the rest in `index.html`:

- **Bug — "Chef NqobiNew":** the detail header rendered an unstyled `New` span jammed against the name. Replaced with a proper `.detail-new-tag` green pill.
- **Bug — profile photo "lost" on detail:** the 72px avatar square cropped too tight and read as a blur. Enlarged to 104px so the face reads.
- **Badges over the face:** moved New/Available to bottom-left of the listing photo (`.listing-photo-badges`); save/compare buttons stay top-right. Verified the Chef Nqobi card — face fully clear.
- **Socials hyperlinked:** IG handles in the detail Contact pill and the EPK now link to `instagram.com/<handle>` (new tab). Note: listing cards don't currently show a handle, so nothing to link there yet — flagged to Alexander.
- **Fuzzy search:** added Levenshtein-based typo tolerance + token matching. "photogrepher" and "photo-xxx" now both resolve to the photographer (Zolelo). Tightened to avoid short-word false positives.
- **Compare flow reworked:** removed the hidden "Compare mode" checkbox; the ⇄ button is now always visible on every card. Compare bar appears on first add; clicking Compare from a profile no longer dead-ends — second add auto-opens the comparison.
- **Dashboard back button:** added a ← in the creative dashboard tab bar (`cpBackToSelect`) returning to the profile selector.

Verified all of the above in the local preview (python http.server on :8000). Still open / pending Alexander's input: project-type list for Post Brief ("project types pending"), whether to surface IG handles on listing cards, and the colour direction. Nothing committed — changes sit in the working tree.

**Follow-up same session:** Alexander confirmed IG handles on cards (added, purple, click-through to instagram.com). Project types: no preference → kept the current 9. Colour: no preference → reverted to the purple/blue scheme the code was clearly built for (vars are literally named --purple/--blue; a #7C3AED purple still lived in the "Contact via verbAfrica" button). Swapped every #C8860A/#E8A820/#9A6408 + rgba(200,134,10) to purple, set --blue to a real blue so gradients read purple→blue, and gave the landing + search heroes a purple→blue blend.

While fixing the detail photo, found the actual root cause of "photo gets lost": the avatar rules use a `background:` *shorthand* for their no-photo gradient fallback, which silently resets `background-size` to `auto` — so real photos rendered at natural size (extreme zoom). Same latent bug affected shortlist / compare / EPK avatars. Fixed globally with a source-order override forcing `background-size:cover`. Detail header now shows a proper landscape photo (full-width on mobile). All verified in preview; zero console errors. Still nothing committed.

**Follow-up — green CTA + accessibility batch (committed):** After re-running the ui-ux-pro-max engine (which independently returned the exact purple #7C3AED + green #22C55E "trust purple / transaction green" marketplace palette), introduced a --cta green token and switched the true conversion actions to green — Post Brief, Send Enquiry, Find Matched Creatives, the enquiry form submit, and Contact via verbAfrica — while keeping purple as the brand colour everywhere else.

Then a code-review pass (8 finder angles) surfaced two real one-liners, both fixed: the INIT reorder left the browse grid stale if a user navigated there before creatives.json resolved (added renderGrid() to the success handler), and the detail/EPK contact render used c.contact.instagram without the optional chaining the grid already had (now c.contact?.). Plus the accessibility/touch "Batch A": keyboard focus-visible rings, prefers-reduced-motion, 40px touch targets on the save/compare/close buttons, label for= associations on both forms, aria-labels on icon-only buttons, role=img + aria-label on creative photos, role=status on the toast, a WCAG contrast fix on the listing location text (#9CA3AF→#6B7280), and 16px mobile form fields to stop iOS zoom-on-focus. All verified in preview, zero console errors.
