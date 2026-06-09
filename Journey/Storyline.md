# Storyline

*A running narrative of building verbAfrica. Newest entries at the top.*

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
