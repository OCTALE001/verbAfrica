# Cohort

*The 10 creatives in the verbAfrica beta. The cohort **is** the product.*

---

## Two kinds of data per creative

| | What it is | Where it lives | Who sees it |
|---|---|---|---|
| **Public profile** | What shows on the verbAfrica website | `data/creatives.json` — one entry per creative | Anyone on the site |
| **Internal notes** | Relationship, ops, history, contact prefs | `Cohort/01-name.md` ... `Cohort/10-name.md` — one file per cohort member | Only us |

**Test:** Would I be comfortable showing this to a client? If yes → public. If no → internal.

---

## Files

| File | What |
|------|------|
| `_TEMPLATE.md` | The intake form. Fill one per cohort member during onboarding. |
| `01-...md` through `10-...md` | Per-cohort-member internal notes (created as we onboard each). |

---

## Workflow

### Intake (once, at the start)

1. Alexander fills `_TEMPLATE.md` for each of the 10 cohort members (10 filled-in copies).
2. Claude takes the templates, populates `data/creatives.json` with the public sections, and creates `Cohort/01-...md` through `Cohort/10-...md` with the internal sections.

### Ongoing

- **After every conversation with a cohort member** — add a dated note to their internal file. Even one line. Future-you will thank you.
- **When public info changes** (new rate, new bio, new highlight) — edit `data/creatives.json` directly OR tell Claude.
- **When the tech intervention for a creative gets defined** — add it to their internal file under "Tech intervention".

---

## Related

- `data/creatives.json` — the public profile data (what the site reads)
- `Strategy/02_Roadmap.md` — Phase 1 is cohort onboarding; Phase 4 is tech interventions
- `index.html` — the website that reads `data/creatives.json`
