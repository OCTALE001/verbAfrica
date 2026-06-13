# verbAfrica

> *South Africa's curated booking platform for top creatives — and the first product of a creative-industry tech-enablement studio. See [Strategy/01_Vision.md](Strategy/01_Vision.md).*

---

## Live

| What | Where |
|---|---|
| **Public site** | https://octale001.github.io/verbAfrica/ |
| **Cohort feedback form** | https://docs.google.com/forms/d/e/1FAIpQLSfZ04vwUHNWAFdf5Z7u46lSbafc1iv-mqJ-BD_pnK7fV7fS2A/viewform |
| **Booking tracker** (private) | Google Sheet: *verbAfrica Bookings* (owned by `alextober5@gmail.com`) |
| **Repo** | https://github.com/octale001/verbAfrica (GitHub Pages serves `main` as the public site) |

---

## Current state

**Phase 2 complete · Beta testing in progress.**

- The 11-creative beta cohort is loaded onto the platform.
- Enquiry + brief forms submit to the booking tracker and send email notifications.
- Phone/email of cohort hidden on public profiles — all contact routes through the enquiry form.
- Cohort members have the site + feedback form; gathering UX feedback before trial clients are invited.

See [Strategy/02_Roadmap.md](Strategy/02_Roadmap.md) for the phased plan and decisions log.

---

## What's in this repo

| Folder / File | What it is |
|---|---|
| [`README.md`](README.md) | This file — central index, live URLs, repo map |
| [`index.html`](index.html) | The platform itself — single-page app, loads cohort from `data/creatives.json` |
| [`data/creatives.json`](data/creatives.json) | Source of truth for what shows on the website. 11 cohort entries. |
| [`Strategy/`](Strategy/) | Vision, Roadmap, decisions. *Where the company is going.* |
| [`Operations/`](Operations/) | Booking Playbook, Response Standards, Forms Setup. *How the company runs day-to-day.* |
| [`Cohort/`](Cohort/) | One markdown file per cohort member — internal notes (separate from public profile data) |
| [`verbAfrica Research/`](verbAfrica%20Research/) | External research — industry overview, competitive landscape, primary research plan, insights/open questions |
| [`Journey/`](Journey/) | Running narrative of building verbAfrica — the *story* behind the artifacts |
| [`images/`](images/) | Cohort profile photos |

Each folder has its own `README.md` (or `00_README.md`) explaining what's in it and how it's maintained.

---

## Architecture (concierge MVP)

The site is a **single static HTML file** served from GitHub Pages — no backend. Booking infrastructure is layered on top of Google services:

```
   ┌───────────────────────────┐
   │   index.html (static)     │   served from GitHub Pages
   │   + data/creatives.json   │
   └───────────┬───────────────┘
               │ (form submit)
               ▼
   ┌───────────────────────────┐
   │  Google Apps Script       │   web app endpoint
   │  (POST → appendRow)       │
   └───────────┬───────────────┘
               │
               ▼
   ┌───────────────────────────┐
   │  verbAfrica Bookings      │   Google Sheet → "Bookings" tab
   │  (source of truth)        │              → "Form Responses" tab (feedback)
   └───────────┬───────────────┘
               │
               ▼
   ┌───────────────────────────┐
   │  alextober5@gmail.com     │   email notification per submission
   │  (alarm bell, swappable)  │
   └───────────────────────────┘
```

The **sheet is the source of truth.** Notification channels are swappable. See [`Operations/01_Booking_Playbook.md`](Operations/01_Booking_Playbook.md) for why.

The Apps Script setup is documented in [`Operations/03_Forms_Setup.md`](Operations/03_Forms_Setup.md) — useful if you ever need to redeploy or replace the endpoint.

---

## The cohort (10)

Loaded as of 2026-06-09. See [`data/creatives.json`](data/creatives.json) for the data the site renders, and [`Cohort/`](Cohort/) for internal notes per member.

1. Chef Nqobi · Culinary Artist
2. DG-TAL Format · Post Production
3. Fifi Frequency · MC / Broadcaster
4. Hāzy · Audio Engineer
5. Kgale · Rapper / Singer / Guitarist
6. LuMai · Singer-Songwriter
7. OhTeeh · Artist / Producer
8. Seru The Ellipsis · Artist / MC / Producer
9. Shivolski · Dancer / MC
10. Zolelo · *(profile pending)*

---

## Working in this repo

**Editing creative data on the site** — `data/creatives.json` is the source. Edit there; the site picks it up on next page load. Internal notes live separately in `Cohort/*.md`.

**Editing the booking workflow** — the workflow itself is documented in `Operations/01_Booking_Playbook.md`. The form-to-sheet plumbing lives in the Google Apps Script attached to the *verbAfrica Bookings* sheet (Extensions → Apps Script).

**Deploying changes** — push to `main`. GitHub Pages rebuilds in ~30–60 seconds.

**Scheduled state-of-the-union** — a Claude scheduled task (`verbafrica-state-of-the-union`) runs every 3 days, catches up the Storyline, and flags stale Open Questions. Configured in the local Claude app.

---

## Working notes

- **No backend yet, by design.** Booking management is concierge (manual by Alexander). The decision to build a real backend gets made in Phase 5 review (December 2026), based on what the beta teaches us.
- **No public response-time promise.** Internal target is in `Operations/02_Response_Standards.md`.
- **Cohort phone/email are kept out of public HTML** for privacy. They live in `data/creatives.json` (which is technically public via the static site) — but the **render code only displays Instagram + the "Contact via verbAfrica" pill**. Real client contact always routes through Alexander during beta.
- **Pricing during beta = pass-through.** verbAfrica takes no cut. Revisited Phase 5.

---

*Maintained by Alexander October + Claude. Last meaningful update: 2026-06-09 — beta testing kickoff.*
