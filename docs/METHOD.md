# Method — How the Sweep and Refresh Work

So a future run can repeat this exactly and produce a dashboard in the same format.

## Access rule: Pi browser only for Facebook

- Facebook Marketplace is accessed **only through the Pi browser** (the shared persistent Chromium on Ayaan's Raspberry Pi, via `pib`), never through Muse's own browser. Logged-in / bot-hostile sites route to the Pi per the standing routing rules.
- Session rule: reuse the existing logged-in Pi session; do not log in fresh, do not create accounts, do not contact sellers, send messages, submit forms, or make any purchase. Research only.
- Bot checks on Pi work: the agent tries first under the owner's protocol (one attempt per site entry); only on failure does it escalate to the owner in the live view. Never charge money, never delete anything.

## Sweep method (Oct 26, 2026 run)

1. Search Facebook Marketplace from the Houston / Stafford TX 77477 area for the candidate sedan families in the spec (Fusion, Accord, Civic, Elantra, Forte, Sonata, plus plausible same-class sedans that appeared: Optima, Mazda6, Malibu, Legacy) in the target year range, under the $12,000 / ≤75,000-mile frame — while also recording same-family near-misses that explain the market (over-miles, over-price, rebuilt) so market estimates have a visible basis.
2. Open each listing; record only what the listing states or what factory fit for that exact year/trim establishes, labelled as inference in the notes. Never invent a feature: unstated = `null`.
3. Score each record with the fixed spec weights (see Schema) and estimate market value from comparable asking prices **seen in the same sweep** — the comparables used are named in each record's notes.
4. Flag `deal_flag` when asking is ≥10% below that market estimate. The flag is advisory: a discount explained by a rebuilt title, suspect miles, or a non-clean title is recorded as such in the notes, not as a bargain.
5. Only the fresh sweep ships. Older sweeps are **not** carried forward or re-labelled as current — the Sep 28, 2026 listings were not re-verified on Oct 26 and are presumed sold, so they are excluded from this dashboard entirely.

## Fixed schema (do not change fields between runs)

Each record in `data/listings.json` has exactly these fields, in this order of meaning:

`url, year, make, model, trim, price, mileage, location, seller, hybrid, android_auto, reverse_cam, lane_assist, reliability_score (1–5), market_value_estimate, discount_pct, deal_flag (true/false), pref_score (0–10), meets_criteria (true/false), notes`

(In the Sep 28 spec the list is the same set; the Oct 26 records add the derived `discount_pct` and `meets_criteria` fields that the sweep tooling already emitted — a future run should keep emitting exactly the Oct 26 field set so the dashboard code does not change.)

Rules inside the schema:

- Feature fields are `true / false / null`. `null` = not stated in the listing. Null stays null in the data file.
- `pref_score`: hybrid +3, lane assist +2, reliability (5→+3, 4→+2, 3→+1), year 2019+ +1, under 50,000 mi +1. Max 10.
- `meets_criteria` is true only when **every** must-have is confirmed: price < $12,000, mileage ≤ 75,000, factory Android Auto confirmed, reverse camera confirmed, sedan body. Unconfirmed (`null`) does not count as met.
- `notes` carries the reasoning: why it misses, title statements, duplicate relationships, red flags, and the comparables behind the market estimate. The dashboard's short miss line is a condensation of the notes, never a replacement for them.

## Matrix-over-seller-claim rule (dashboard)

- The Android Auto badge on each card comes from the **expert matrix** (expert guide Part 1) for that year/make/model/trim — not from the seller's claim and not from the record's raw `android_auto` field alone.
- If seller claim and matrix disagree, the card shows the matrix verdict and a visible mismatch flag, e.g.:
  - 2019 Fusion S: matrix = AA only with the 8″ SYNC 3 screen (listing inference said "no AA").
  - 2019 Elantra SEL with a blank listing: matrix = AA standard on SEL (seller said nothing).
  - 2018–19 Kia Optima: matrix = UNVERIFIED by trim — seller AA claims are shown as unverified, check in person.
- Corrections baked into the matrix: Corolla sedan AA 2021+ (not 2020); 2019 Fusion S conditional; Mazda6 2018 Touring+ (upgrade) / Sport excluded; "eligible ≠ installed" for 2016 Chevy 8″ MyLink and 2015–16 Hyundai/Kia update cars.
- Pursue / Caution / Avoid badge per card comes from the expert dossiers (Part 2), with a one-line plain-language reason.

## Data stamp rule

Every published dashboard carries this line, visibly, at the top:

> Listings from Facebook Marketplace sweep, Oct 26, 2026 — listings sell fast; a car shown here may already be gone.

On a refresh, replace the date with the new sweep date. Never publish a dashboard without the stamp, and never mix two sweep dates in one listings file.

## Photos

The Oct 26 records carry no photo URLs, so the dashboard embeds no photos. If a future sweep records photo URLs, link/embed the URL only — never commit image binaries to this repo, and keep every file ≤ ~110 KB (files are uploaded individually).

## Publish flow

1. Build/refresh files locally in this repo: `index.html`, `data/listings.json`, `docs/FAQ.md`, `docs/RESEARCH.md`, `docs/METHOD.md`, `README.md`.
2. Self-check before publishing: HTML parses, JSON parses, listing count in the page matches the JSON count, no file over ~110 KB.
3. Upload the files individually to GitHub repo **ayaan40000/Reserved-1** (parent agent handles the upload and any approvals; builders stage locally and do not push).
4. GitHub Pages serves the repo at **https://ayaan40000.github.io/Reserved-1/** once Pages is enabled on the repo by the owner/parent.
5. A refresh run repeats the sweep, replaces `data/listings.json` wholesale with the new sweep only, regenerates the cards, updates the data stamp date everywhere it appears (page, README, FAQ), and republishes.
