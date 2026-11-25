# CLAUDE.md — Road to Sub-19

Context for AI assistants (and humans) working on this repo.

## Project
- Single-page portfolio site (`index.html`) analysing my Strava running data and
  presenting a 12-week plan toward a sub-19 minute 5k (goal pace 3:47/km = 18:55).
- Hosted on GitHub Pages from the repo root (main branch). Netlify is the fallback.
- One self-contained file: plain HTML, CSS and JavaScript, hand-written SVG charts,
  no libraries, no external requests. Keep it that way.

## Privacy rules (the repo is public)
- Never commit raw exports: `*.csv`, `*.gpx`, `*.fit`, `*.fit.gz`, `activities/`.
  These are in `.gitignore`.
- The site may contain only dates, distances, durations, grade-adjusted distance
  and heart rate.
- Public race results (e.g. parkrun times and dates) may be shown. They are already
  public on the results sites, and can point to which event I run.
- No GPS tracks, maps, start times, start locations, activity names, descriptions,
  photos, or other people's names.

## Data notes
- Source: Strava "Download your account" export, `activities.csv`, runs from 2025–2026,
  embedded as the `RAW` array in `index.html`.
- Row format: `[date, km, moving_s, grade_adjusted_km, avg_hr, max_hr, tag]`.
  Tag `'R'` marks a race-type effort: currently 2026-08-28 (parkrun) and 2026-10-02
  (hard 7.7 km). Race efforts are always classed as hard.
- Strava export timestamps are UTC. Local time (NZ) is ahead of UTC, so an
  early-morning run (e.g. Saturday parkrun) can appear on the previous date. Dates
  are kept as UTC dates.
- Data fix: 2026-03-04 had a broken moving time (GPS glitch), so elapsed time
  (1965 s) is used instead.
- The CSV has whole-activity totals only, with no splits. Best efforts are
  therefore whole activities. The one exception is the 2026-10-22 track effort.
  Its splits are self-reported and checked by lap count (see Baseline).
- Heart rate comes from a wrist (optical) sensor. Wrist HR can lag at the start of
  hard reps and spike briefly, so max HR and the hard-session rule below are
  approximate. Say so on the page wherever HR-based classification is shown.
- Heuristic classification (set in `CONFIG`):
  - hard: avg HR >= 158 or max HR >= 195
  - long: 15 km or more
  - otherwise easy
- Runs slower than 7:00/km grade-adjusted pace are excluded from the efficiency
  chart (hike/run outings).
- Efficiency = grade-adjusted speed / avg HR, shown as metres per heartbeat.

## Baseline (as of 25 Nov 2026)
- 2026 to date: ~779 km over 89 runs.
- Last 12 complete weeks: ~28 km/week, about 3 runs/week.
- Longest single run in the last 30 days: ~15 km (self-reported).
- About 4 in 10 runs were hard sessions, which is a lot relative to easy volume.
- Monthly volume: July ~59 km, then Aug ~137, Sep ~129, Oct ~124 km.
- Easy-run efficiency improved ~7% (6.6%) from July to Oct–Nov 2026. Noisy, but the
  direction is positive.
- Best verified 5k: 19:51 (parkrun; UTC date 2026-08-28 = Sat 29 Aug local).
- Hard 7.7 km effort: 30:55 at 4:01/km (2026-10-02).
  - Riegel projection (exponent 1.06, as published): 19:34.
  - With a steeper exponent of 1.10, shown for illustration only (no published
    source): 19:14.
  - Show this on the page as "19:14–19:34", with both exponents labelled.
- Best training effort: track session, 2026-10-22 (same date in UTC and local).
  - Whole run: 14.38 km at 4:34/km average (about 65:40). Warm-up, then 10 km
    continuous, then cool-down. Warm-up and cool-down together were ~4.4 km at
    ~6:10/km.
  - 10 km continuous, no stops: 38:39 (3:52/km average). Distance checked by
    counting 25 laps in lane 1.
  - km splits: 3:46, 3:42, 3:50, 3:50, 3:53, 3:54, 3:50, 3:53, 4:02, 3:59.
  - Fastest 5 km from the whole-km splits: 19:01 (km 1–5). Strava's best effort
    for 5 km is 18:59, measured over any 5 km stretch rather than from km markers.
  - Riegel projection from the 10 km (exponent 1.06): 5k ≈ 18:32. That's a
    projection from a training effort, not a race result.
  - On the page: show this as a training effort, not a race. Use 19:01 (5 km,
    whole-km splits) and 38:39 (10 km), say the distance was checked by laps, and
    keep the 19:51 parkrun as the best official 5k. Don't tag it `'R'`.
  - The CSV has no splits, so store these splits in a small, commented
    `TRACK_EFFORT` constant next to `RAW`. Don't type the numbers into the page text.
- Max HR observed: 205 (wrist sensor, so treat as approximate).

## Training plan (30 Nov 2026 – race Sat 20 Feb 2027)
- Structure:
  - Tue: intervals
  - Thu: threshold
  - Sun: long run
  - Sat and Wed: easy runs
  - Mon and Fri: rest or swim
- Intensity: mostly easy running with two quality sessions a week. Informed by
  Stöggl & Sperlich 2014, but not strictly polarized: the weekly threshold session
  makes it closer to a pyramidal distribution. (Their study found polarized
  training beat threshold-focused training, so don't claim it's polarized.)
- Paces:
  - easy 5:30–6:15/km, HR mostly under 150
  - threshold 4:05–4:15/km
  - intervals 3:40–3:52/km
- Weekly km and suggested long runs:

  | Week | Starts  | km | Long run (suggested) | Note |
  |------|---------|----|----------------------|------|
  | 1    | 30 Nov  | 33 | 12 | |
  | 2    | 7 Dec   | 36 | 13 | |
  | 3    | 14 Dec  | 41 | 14 | |
  | 4    | 21 Dec  | 30 | 11 | recovery |
  | 5    | 28 Dec  | 37 | 14 | ~90% of week 3 |
  | 6    | 4 Jan   | 43 | 15 | |
  | 7    | 11 Jan  | 49 | 16 | peak |
  | 8    | 18 Jan  | 33 | 11 | recovery + parkrun time trial (Sat 23 Jan) |
  | 9    | 25 Jan  | 44 | 15 | ~90% of week 7 |
  | 10   | 1 Feb   | 48 | 16 | last full week before taper |
  | 11   | 8 Feb   | 28 | 10 | taper (−42% vs week 10) |
  | 12   | 15 Feb  | 25 | —  | race week, race Sat 20 Feb |

- Volume rules:
  - The first week after a recovery week returns to about 90% of the last full
    training week, not straight back to the previous peak.
  - Single-session cap (Frandsen et al. 2025): no run, long run included, should be
    more than 10% longer than the longest run in the previous 30 days. The long-run
    column stays under this cap. It's a ceiling, not a target.
  - Taper (Bosquet et al. 2007): the two taper weeks average 26.5 km, about 45%
    below week 10, which is inside the 41–60% range. Keep the intensity.
- Week 8 checkpoint (Sat 23 Jan 2027):
  - 19:15 or faster: on track
  - 19:15–19:40: continue and reassess after week 10
  - slower than 19:40: treat February as a stepping stone
  - Result: [ADD]
  - The 22 Oct track effort (38:39 for 10 km, projecting ~18:32 for 5k) suggests
    sub-19 may already be within reach. The goal stays sub-19 for now. Decide after
    this checkpoint whether to raise the race target and paces.
- Race target: 18:50–18:55, even splits at 3:46–3:47/km.

## Evidence the plan is based on (cite these, don't invent sources)
- Nielsen et al. 2014, J Orthop Sports Phys Ther 44:739–47 — weekly distance
  progression (studied novice runners).
- Frandsen JSB, Nielsen RO et al. 2025, Br J Sports Med 59(17):1203–10,
  PMID 40623829 — single-session distance spikes above 10% of the longest run in
  the past 30 days linked to higher overuse-injury rates.
  (BJSM blog: https://blogs.bmj.com/bjsm/?p=11751).
- Stöggl & Sperlich 2014, Front Physiol 5:33 — polarized intensity distribution
  (the plan is informed by it, not a strict copy; see Training plan).
- Bosquet et al. 2007, Med Sci Sports Exerc 39:1358–65 — 2-week taper, cut volume
  41–60%, keep intensity.
- Riegel 1981, American Scientist 69:285–90 — T2 = T1 × (D2/D1)^1.06.

## Working preferences
- Explain clearly. Source claims from reputable sources and never fabricate citations.
- Keep code clean and well-commented, since this is a CV portfolio piece.
- When new data arrives, update `RAW` and this file's Baseline section rather than
  hard-coding numbers in the page text. The page computes its numbers from `RAW`.

## Open items
- [x] Confirm the 18:59 track split (date + laps). 2026-10-22, 25 laps in lane 1
      for the 10 km. 18:59 is Strava's best effort; whole-km splits give 19:01.
- [ ] After the week 8 time trial, decide whether to raise the target beyond
      sub-19, given the 22 Oct effort.
- [x] Confirm the heart-rate sensor type. Wrist (optical).
- [ ] Add the week 8 time-trial result and adjust the paces if needed.
- [x] Build `index.html` to match this file (2026-11-25).

## Changelog
- 2026-11-25 (index.html built):
  - Built `index.html` from the export. 107 runs from 2025–2026 are in `RAW`, and the
    Baseline figures check out against them.
  - Tagged the 28 Aug parkrun and the 2 Oct 7.7 km as `'R'`. Race efforts now count as
    hard, so the parkrun no longer shows up as an easy run in the efficiency chart.
  - Efficiency change updated from ~6% to ~7%; the computed value is 6.6%.
- 2026-11-25 (second update):
  - Added the 22 Oct track effort: 10 km continuous in 38:39, checked by laps.
    Fastest 5 km from splits is 19:01; Strava's best effort is 18:59.
  - Added a Riegel projection from the 10 km (5k ≈ 18:32), labelled as a projection.
  - Goal unchanged. Raising the target is now an open item after week 8.
  - Splits go in a `TRACK_EFFORT` constant, because the CSV has no splits.
- 2026-11-25:
  - Goal pace changed from 3:48 to 3:47/km. 3:48 × 5 km = 19:00, which isn't sub-19.
  - Race target changed to 18:50–18:55 at 3:46–3:47/km.
  - Riegel range changed from 19:11–19:34 to 19:14–19:34. The 1.10 exponent gives
    19:14 and is labelled as illustrative only.
  - Week 5 changed from 44 to 37 km and week 9 from 48 to 44 km (90% rule).
    Week 6 changed from 47 to 43 km to keep the build to the week 7 peak.
  - Taper week 11 changed from 38 to 28 km to fit Bosquet's 41–60% range.
  - Added the 10% single-session cap and a suggested long-run column.
  - Reworded the intensity model as pyramidal, informed by Stöggl & Sperlich.
  - Added the full Frandsen et al. citation and the Nielsen novice caveat.
  - Recorded the HR sensor as wrist (optical) and added an accuracy caveat.
  - Privacy rules now allow public race results.
