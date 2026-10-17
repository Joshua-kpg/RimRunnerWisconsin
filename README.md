# RimRunnerWisconsin

Unofficial Wisconsin HS boys basketball ratings. Not affiliated with the WIAA or MaxPreps.
Live site: https://rimrunnerwisconsin.pages.dev/ (backup copy: GitHub Pages).

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project `rimrunnerwisconsin` (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 05:30 and 13:00 UTC
(11:30 pm / 7:00 am Central standard time) and on manual dispatch. Before the Wisconsin boys season opens
(`SEASON_START` in index.html = first game on the MaxPreps schedules = the WIAA earliest game date, Tue Nov 24, 2026) the script makes no requests.
WIAA schools' games before Nov 24 are scrimmages/jamborees or bad dates and are ignored.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Wisconsin scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
