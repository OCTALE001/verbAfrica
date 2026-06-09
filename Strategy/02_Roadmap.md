# Roadmap

*Last updated: 2026-06-09 — Phase status updated; cohort loaded locally.*

*Aim: get every cohort member to the **December 2026 testimonial** described in [01_Vision.md §4](01_Vision.md).*

---

## How to Use This Doc

- **Current Phase** — what we're working on RIGHT NOW. Updated whenever we move phases.
- **Phases** — the sequence of work to reach the Vision. Each has: goal, work, what we're measuring, which Vision strand(s) it serves.
- **Feedback Loops** — the standing rhythms that keep this living instead of stale.
- **Open Questions** — uncertainties to resolve. Add when they come up; cross out when answered.
- **Decisions Log** — every meaningful decision, with date and reasoning. Append-only.

Read alongside `01_Vision.md`. **The Vision is the *where*. The Roadmap is the *how*.**

---

## Current Phase

**Phase 1 — Cohort Onboarding** *(started June 2026, in progress)*

All 10 cohort profiles are loaded into `data/creatives.json` and visible locally. Internal Cohort files exist; B-sections (why in cohort, contact prefs, tech interventions) are blank — to fill through ongoing conversations. Brand guides and business docs are the remaining Phase 1 work.

---

## Phases

### Phase 0 — Foundation
**Status:** Complete (Operations playbook deferred — now Phase 2 work).
**Vision strands served:** All — sets the conditions for everything else.

Establish the operational and strategic foundation.

**Work:**
- Documentation system in place (`Strategy/`, `Operations/`, `Cohort/`, `Journey/`, `data/`) ✅
- Vision v1 drafted ✅
- Roadmap v1 drafted ✅
- Confirmation of the 10-creative cohort ✅
- Operations playbook drafted (first cut, will iterate) → deferred to Phase 2

**Done when:** Vision + Roadmap signed off; cohort of 10 confirmed; Operations playbook drafted.

---

### Phase 1 — Cohort Onboarding
**Status:** In progress.
**Vision strands served:** Bookings work · Cohort professionalised.

Get the 10 cohort members loaded onto the platform with the right data, and start the brand/business documentation work for each.

**Work:**
- Move creative data out of `index.html` JS array into `data/creatives.json`. ✅
- Load the 10 cohort members' details into `creatives.json` (you provide details, I edit). ✅
- Create `Cohort/01-...md` through `Cohort/10-...md` — internal notes per creative. ✅
- Begin brand guide + basic business doc for each cohort member.

**What we're measuring:**
- 10 cohort profiles live on the platform (locally first). ✅
- Brand + business doc started for each.

**Done when:** All 10 cohort profiles visible on the platform; an outline of each member's brand/business doc exists.

---

### Phase 2 — Booking Infrastructure
**Status:** Not started.
**Vision strands served:** Bookings work.

Make the platform actually capable of handling bookings, with you as the manual operator behind the scenes.

**Work:**
- Wire enquiry + brief forms to capture submissions (email + spreadsheet).
- Add WhatsApp / email shortcut for direct creative contact.
- Write `Operations/01_Booking_Playbook.md` — exactly how a booking flows from enquiry to confirmed.
- Set up booking tracking spreadsheet.
- Write `Operations/02_Response_Standards.md` — your promises (response times, etc.).

**What we're measuring:**
- All test enquiries reach you within seconds.
- A test booking can be tracked from enquiry → confirmed → completed.

**Done when:** Forms route correctly; tracking works; playbook holds up against a dry-run booking.

---

### Phase 3 — Trial Client Pilot
**Status:** Not started.
**Vision strands served:** Bookings work.

Invite real trial clients. Run real bookings through the platform manually. Learn what's actually happening.

**Work:**
- Identify and invite trial clients (~3–5).
- Deploy current platform publicly (push to GitHub Pages).
- Run real bookings end-to-end.
- Weekly check-ins with cohort members on what they're seeing / getting.
- **Design + ship feedback forms** for cohort members and clients to fill after each booking. Captures: decline-handling preference, communication quality, gig outcomes, what's broken. *(Open question in Roadmap.)*

**What we're measuring:**
- Number of enquiries received.
- Conversion rate (enquiry → confirmed booking).
- Time from enquiry to first response.
- Qualitative feedback from cohort and clients — via the feedback forms.

**Done when:** At least one booking completed end-to-end for a target number of cohort members. *(Specific target to confirm before phase starts — see Open Questions.)*

---

### Phase 4 — Tech Interventions
**Status:** Not started.
**Vision strands served:** Tech edge per member.

Identify the right tech intervention for each cohort member. Execute at least one per member.

**Work:**
- 1:1 with each cohort member: what's your art, where could tech step-change it?
- Map tech tools (projection mapping, 3D printing, AI, VR/AR, etc.) to each member's needs.
- Execute interventions — start with the lowest-effort highest-impact ones.

**What we're measuring:**
- Each cohort member has received at least one tech intervention by Dec 2026.
- Qualitative: does each member feel their game has been "levelled up"?

**Done when:** All 10 cohort members have at least one tech intervention completed and visible in their work.

---

### Phase 5 — Beta Review & Decide-What's-Next
**Status:** Not started *(target: December 2026)*.
**Vision strands served:** All — closes the loop.

Step back. Collect feedback. Decide what's working, what's not, what to build into a real backend, what to scale, what to kill.

**Work:**
- Sit-down feedback session with every cohort member.
- Sit-down feedback session with every client.
- Review the `Journey/Storyline.md` — what does the journey show?
- Decide: next product / next cohort / what to build / what to outsource.

**Done when:** A v2 Vision + v2 Roadmap exist, informed by what we learned.

---

## Feedback Loops

Built into the rhythm of every phase — so feedback never sits in someone's head:

- **Weekly check-in** *(15 min, Alexander + Claude)* — bring observations, conversations, decisions. Update this Roadmap and the Storyline.
- **Per-booking debrief** — after each completed booking: what worked, what broke, who needs to know.
- **Per-cohort-member 1:1** — monthly, with each cohort member. Capture in their `Cohort/XX-name.md` file.
- **Scheduled Storyline catch-up** — automated, every few days, captures activity between sessions.

---

## Open Questions

Things we don't have answers for yet. Add to this list when they come up; cross out when answered.

- **How many bookings is "success" in Phase 3?** Need a concrete target.
- **What does the brand guide / business doc actually look like for a cohort member?** Need a template.
- **How will trial clients find verbAfrica?** Word of mouth? Direct invite? Something else?
- **What is the company's actual name long-term?** Working name is verbAfrica; is that the company name too, or just product #1?
- **What's the right pricing model for tech interventions?** Free during beta — what after?
- **What would it look like to be wrong about booking admin being the right wedge?** Worth articulating so we'd recognise it if we saw it.
- **"Confirmed" booking definition** — provisional verbal-yes-only for the beta. Tighten to deposit-secured once payment flows through verbAfrica.
- **Decline handling preference** — when a cohort member declines a booking, do clients prefer we solve behind the scenes (current default) or offer them a choice between alternatives? Capture via feedback forms during beta.
- **Cohort promotion criterion** — when does someone in the broader talent pool become part of the cohort?
- **Feedback form schema** — what specifically do we ask creatives and clients after each booking? *(Workstream named in Phase 3.)*
- **Final booking pricing model** — pass-through during beta. Margin, brokered, or hybrid for post-beta?

---

## Decisions Log

Append-only. Date format YYYY-MM-DD.

- **2026-06-05** — Cohort size confirmed at **10** (corrected from earlier "8"). *Reason: that's the actual cohort.*
- **2026-06-05** — **No backend yet.** Concierge MVP: manual booking management by Alexander, captured in spreadsheet + email. *Reason: avoid building infrastructure before we know what to automate.*
- **2026-06-05** — **Curated, not open.** Beta cohort is invitation-only. *Reason: "the cohort is the product"; curation is the differentiator.*
- **2026-06-05** — **Johannesburg first**, then SA. Not international. *Reason: focus; depth before breadth.*
- **2026-06-05** — **Live deployment paused**: keep working in local preview until the platform is in a shape Alexander is happy to publish. *Reason: control over what goes public during foundation work.*
- **2026-06-05** — **Data source of truth = `data/creatives.json`**, not the JS array in `index.html`. *Reason: easier to edit, easier to keep website + records in sync.*
- **2026-06-05** — **Journey log committed to** as a separate folder, updated during sessions and via a scheduled task. *Reason: build a how-to over time; story behind the artifacts is as valuable as the artifacts.*
- **2026-06-09** — **Cohort live locally.** All 10 cohort members loaded into `data/creatives.json`; `Cohort/01-...md` through `Cohort/10-...md` created with public data populated and B-sections blank. Phase 1 in progress; brand guides are the remaining work before Phase 2.
- **2026-06-09** — **Tracking sheet = source of truth** for bookings. Notifications (email, WhatsApp) are channels. *Reason: platform flexibility — swap channels without losing the record.*
- **2026-06-09** — **No public response-time promise** during beta. Internal target only (`Operations/02_Response_Standards.md`). *Reason: don't make a public commitment we might miss; revisit in Phase 5 once cadence is known.*
- **2026-06-09** — **Pass-through pricing** during beta. verbAfrica takes no cut on bookings. *Reason: cohort relationship is symbiotic; trade no-cut for trust and learning. Margin / brokered remain on the table for post-beta.*
- **2026-06-09** — **We stay involved through gig-day** during the beta (do not hand off to creative ↔ client). *Reason: deliberate learning mode — see operations end-to-end so we know where to productise later.*
- **2026-06-09** — **Broader talent network** acknowledged as operational reserve (off-platform, used when cohort can't fulfill). The cohort of 10 remains the publicly-displayed product. *Reason: present completed packages without compromising curation.*
- **2026-06-09** — **Decline handling default: solve behind the scenes** (swap from cohort or broader pool, present completed package). *Reason: client-experience-first default; capture preference via feedback forms.*

---

## Change Log

- **2026-06-05** — Initial draft from founder interview, structured around the Vision strands.
