# verbAfrica — Project Context

## What It Is

Curated booking platform for South African creatives. Connects clients (brands, SMBs, event companies, individuals) with a vetted cohort of 11 creatives in Johannesburg. Not an open marketplace — the cohort is the product.

verbAfrica is product #1 of a broader creative-industry tech-enablement studio founded by Alexander October.

## Status (as of June 2026)

- Single-page HTML app (`index.html`) — no framework, no build step
- Creative data lives in `data/creatives.json` (fetched at runtime; not hardcoded in JS)
- 11 cohort members loaded onto the platform
- Enquiry/brief forms wired (Phase 2 complete) — submit to booking tracker + email notifications
- `creatives.json` fetched with cache-busting (`?v=Date.now()`, `no-store`) so the live link always reflects latest
- Live at https://octale001.github.io/verbAfrica/ (push to `main` → GitHub Pages rebuilds in ~30–60s)

## The Cohort (11 creatives)

| # | Name | Category |
|---|------|----------|
| 1 | Shivolski | Dancer / MC / Content Creator |
| 2 | Chef Nqobi | Culinary Artist / Food Filmmaker |
| 3 | Kgale | Rapper / Singer / Guitarist |
| 4 | Zolelo Magwaza | Videographer / Photographer / Content Creator (iiam3siixtii) |
| 5 | Fifi Frequency | MC / Broadcaster / Voice-over |
| 6 | Seru The Ellipsis | Artist / MC / Producer |
| 7 | OhTeeh | Artist / Producer / Musical Director |
| 8 | Hāzy | Audio Engineer / Music Producer |
| 9 | LuMai | Singer / Songwriter / Model / Actress |
| 10 | DG-TAL Format | Post Production Company |
| 11 | Just Jabba (Njabulo Mpanza) | Rapper / Recording Artist / Wordsmith |

## Folder Structure

| Folder | What It Is |
|--------|------------|
| `Cohort/` | Internal notes per creative (01–10 files + template) |
| `Journey/` | Running narrative of building verbAfrica (`Storyline.md`) |
| `Operations/` | Day-to-day booking ops playbook — empty, Phase 2 work |
| `Strategy/` | Vision + Roadmap |
| `data/` | `creatives.json` — the public profile data the site reads |
| `verbAfrica Research/` | Market research, competitive landscape, insights |

## Key Assets

| Asset | Location |
|-------|----------|
| Live site | https://octale001.github.io/verbAfrica/ |
| GitHub repo | https://github.com/octale001/verbAfrica (local: `Claude/verbAfrica/`) |
| Strategy | `Strategy/01_Vision.md`, `Strategy/02_Roadmap.md` |
| Cohort profiles | `Cohort/01-...md` through `Cohort/10-...md` |
| Public creative data | `data/creatives.json` |
| EPKs & media kits | `Datalake/` |

## Current Phase

**Phase 1 — Cohort Onboarding (in progress)**

All 10 cohort members are loaded into `data/creatives.json` and visible on the platform locally. Internal notes (`Cohort/*.md`) are created; the B-sections (why in cohort, contact prefs, tech interventions) are still blank — to be filled through ongoing conversations.

Next: Phase 2 — wire the enquiry/brief forms, write the Operations playbook, set up booking tracking.
