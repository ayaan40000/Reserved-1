# Reserved-1 — Reliable Car Under $12K Dashboard

Ayaan Pathan's used-car search dashboard: a filterable, single-file board of Houston-area sedan listings, with an expert pursue/avoid workup, an Android Auto by year/trim matrix, and a VIN / inspection / scam guide.

**Live (placeholder until Pages is enabled):** https://ayaan40000.github.io/Reserved-1/

**Data stamp:** Listings from Facebook Marketplace sweep, Oct 26, 2026 — listings sell fast; a car shown here may already be gone.

## Owner's criteria

- Under $12,000 cash, no financing
- Sedan only — no trucks, no compact hatchbacks
- At most 75,000 miles
- Must have: factory Android Auto, reverse camera
- Preferred: hybrid, lane assist
- Reliability first; every model reliability cross-checked

Honest headline for the Oct 26 sweep: **only 1 of 32 listings meets all must-haves** — the 2019 Kia Forte FE, Spring TX, $7,000, 44,500 mi (manual transmission, flagged on its card). The Sep 28, 2026 listings were not re-verified and are presumed sold, so they are not included.

## What's inside

| File | What it is |
|---|---|
| `index.html` | The dashboard — Listings tab (filters: max price, max mileage, model, hybrid only, Android Auto confirmed-by-matrix, meets-criteria only, underpriced only; sort by price / mileage / discount / pref score) and FAQ & Help tab. Self-contained, no frameworks, works on mobile; listing text is in the page even with JavaScript off. |
| `data/listings.json` | The 32 fresh Oct 26 records, fixed schema, unchanged from the sweep. |
| `docs/FAQ.md` | The same FAQ & Help content as the page, in Markdown. |
| `docs/RESEARCH.md` | Expert dossiers condensed: pursue/avoid per model-year-engine with failure modes, hybrid battery notes, Houston flood/salvage notes, recall-confirm list, AA matrix with UNVERIFIED flags. |
| `docs/METHOD.md` | How the sweep and refresh work: Pi-browser-only Facebook access + session rule, sweep method, fixed schema, matrix-over-seller-claim rule, data stamp rule, publish flow. |

Key dashboard rules: Android Auto on each card is the expert matrix verdict for that year/trim (not the seller's claim — mismatches are flagged), and the underpriced flag is measured against comparable Marketplace asking prices seen in the same sweep, named in each record's notes.

## How it updates

A refresh re-runs the Facebook Marketplace sweep via the Pi browser only (see `docs/METHOD.md`), replaces `data/listings.json` with the new sweep only, regenerates the dashboard, updates the data stamp date, and republishes the files to this repo. Research only — no sellers are contacted and nothing is purchased.
