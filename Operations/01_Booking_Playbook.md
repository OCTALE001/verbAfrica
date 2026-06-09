# Booking Playbook

*v1 — 2026-06-09 | Living doc — update after every booking*

---

## Operating Principle

**verbAfrica is the curator, the deal-maker, and the operational point-of-contact for every booking — through to delivery.**

We take booking admin off the cohort so they don't have to hunt leads. We stay involved at every stage during the beta because we are deliberately in **learning mode** — we observe operations end-to-end so we can later find the right places to productise, automate, or differentiate.

Differentiation comes *after* we know what to differentiate. Not before.

---

## Two Sources of Supply

| Source | Description | When We Use |
|---|---|---|
| **The Cohort (10)** | Publicly displayed on the platform. Curated. The face of verbAfrica. | Default. Every enquiry is fulfilled from the cohort first. |
| **Broader Talent Network** | Off-platform, behind the scenes. Alexander's wider network. | When the cohort can't fulfill a role on a brief, or as a substitute when a cohort member declines. |

The cohort *is the product*. The broader pool is *operational reserve* — invisible to clients, used to keep promises.

*(Open question: when does someone in the broader pool become part of the cohort? Captured in `Strategy/02_Roadmap.md` Open Questions.)*

---

## Source of Truth

**The booking tracking sheet** — *verbAfrica Bookings* (Google Sheets, owned by `alextober5@gmail.com`). **Live as of 2026-06-09.**

Every enquiry, brief, and booking lives in the sheet from first contact to completion. **Notification channels** (currently email; WhatsApp / future inbox optional later) are *alarm bells* — swappable, additive, removable. The sheet is the ledger.

The form → sheet plumbing runs on a Google Apps Script web app attached to the sheet. Setup documented in [`03_Forms_Setup.md`](03_Forms_Setup.md). The Apps Script URL is embedded in `index.html` (`VA_CONFIG.FORM_ENDPOINT`).

**Sheet tabs:**
- `Bookings` — every enquiry + brief submission lands here as a row.
- `Form Responses 1` (or similar) — cohort feedback form responses (auto-created by the form when first response arrives).

---

## The Booking Lifecycle

### Stage 1 — Receive & Log
*Trigger: enquiry or brief submitted on the platform.*

- Form submission → new row in the sheet → notification to Alexander.
- Row captures: timestamp, contact info, client message, type (enquiry vs. brief), creative(s) requested, event date.

### Stage 2 — Classify
- **Single-creative enquiry** (clicked from a profile) → narrow workflow.
- **Multi-creative brief** (used the brief builder) → assembly workflow.
- **Sanity check:** is the request fulfillable? Date not in the past, scope reasonable, no red flags.
- **Bad fit:** decline politely. Close row as `declined-unfit`. Note the reason.

### Stage 3 — Assemble
- **Single-creative enquiry:** the requested cohort member is provisional until Stage 4 confirms them.
- **Multi-creative brief:** select a roster of cohort members who fit. If a role can't be filled from the cohort, pull from the broader pool.
- The goal: come back to the client with a **completed package**, not a list of options for them to chase.

### Stage 4 — Quote the Cohort
- Reach out to each chosen cohort member with the gig description, date, scope.
- Ask: *Are you in? What's your price?*
- Channel: whatever's fastest with each cohort member (WhatsApp by default).
- Log responses to the sheet as they come in.

### Stage 5 — Handle Declines
*If a cohort member declines:*
- Pull from the broader pool to fill the role, OR
- Substitute another suitable cohort member.

**Default behaviour during beta:** solve behind the scenes. Present the client a completed package with the swap baked in. We tell the client *what happened*, not *what to choose*.

**This is provisional.** We're collecting client + cohort feedback during the beta on whether they prefer (a) we solve invisibly, or (b) we offer them a choice between alternatives. Captured in the feedback forms (see Stage 9).

### Stage 6 — Package & Propose
- Send the client **one** proposal: confirmed roster, total price (pass-through), suggested next steps.
- Pricing during beta is **pass-through** — what the cohort quotes is what the client pays. verbAfrica takes no cut. *(Margin and brokered models remain options, revisited in Phase 5 review.)*

### Stage 7 — Confirm
- **A booking is "confirmed" when:** client gives a verbal yes AND every member of the roster has confirmed.
- *Provisional definition for the beta — will tighten when payment flows through the platform.*
- Lock the date in the sheet. Send confirmation to client and roster.

### Stage 8 — Handoff (we stay)
- We collect logistics from the client (date, time, location, deliverables, day-of contact) and pass to the roster.
- **We remain the gig-day point of contact for both sides during the beta** — so we learn how execution actually works.
- This is on purpose. It's expensive. The payoff is the learning. We will differentiate *after* the beta, not during.

### Stage 9 — Deliver & Learn
- Track the gig through to completion in the sheet.
- **After the gig:** send feedback forms to both the client and the cohort member(s). *(Forms TBD — see Open Items.)*
- If something significant came out of the booking — a learning, a surprise, a pattern — log it to `Journey/Storyline.md`.

---

## Decisions Captured

These are the operating decisions from the founder interview (2026-06-09). All are **provisional during beta** — they get re-examined in Phase 5 review.

| Decision | Value | Status |
|---|---|---|
| Source of truth | Tracking sheet (Google Sheets). Notifications are channels. | Locked |
| Public response promise | None (no SLA on the site) | Locked |
| Pricing model | Pass-through during beta | Provisional — margin / brokered remain options |
| Cohort declines | Solve behind the scenes, present completed package | Provisional — gathering feedback during beta |
| Direct cohort ↔ client comms | We stay involved through gig-day | Provisional — to maximise learning |
| Broader pool of talent | Operational reserve, not displayed | Locked for beta |

---

## Open Items

Things we haven't decided yet. Will get answers from the beta.

- **"Confirmed" tightening** — when payment flows through verbAfrica, "confirmed" should require a deposit. What threshold? Provisional verbal-yes-only for now.
- **Decline handling** — invisible swap vs. offer-choice. Gather client + cohort preference via feedback forms.
- **Cohort promotion criterion** — what makes someone in the broader pool ready to join the cohort?
- **Feedback form schema** — what do we ask both sides after a booking? Tied to Phase 3 work.
- **Day-of escalation** — what's the protocol if something goes wrong during the gig? (Not relevant yet; will become relevant during the trial pilot.)

---

## The Tracking Sheet — v1 Columns

To be created when we wire the forms.

| Column | Notes |
|---|---|
| Timestamp | When the form was submitted |
| Status | `new` → `assembling` → `quoted` → `confirmed` → `delivered` → `declined-unfit` / `declined-cohort` |
| Type | `enquiry` (single creative) / `brief` (multi-creative) |
| Client name | |
| Client contact | Phone / email — from form |
| Event date | |
| Event location | |
| Scope / description | Free text from the form |
| Creative(s) requested | Cohort member name(s) — for enquiry, the one clicked; for brief, the matched list |
| Roster (after assembly) | Final cohort roster + any wider-pool fills |
| Price (pass-through) | Sum of cohort quotes |
| Confirmation date | When all parties said yes |
| Delay reason | Filled only when we miss an internal target |
| Notes / learnings | Free text — what was unusual about this booking |

---

## How This Doc Evolves

**Update after every booking** in the early beta. If something doesn't match what's written here, decide: was reality wrong, or was the playbook wrong? Update accordingly. Add to the Change Log below.

---

## Change Log

- **2026-06-09** — v1 draft from founder interview. Pass-through pricing, deep involvement through gig-day, broader pool acknowledged.
