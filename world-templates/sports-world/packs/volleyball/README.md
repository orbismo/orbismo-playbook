# Sport pack: volleyball

A sport pack for the Sports World template. It teaches the connected AI the vocabulary of volleyball, indoor (six a side) and beach (pairs), at every level: international tournaments, national professional leagues, collegiate, club and age-group club, school, recreational gym leagues (including co-ed) and beach leagues and tournaments. Sport code: `volleyball`. Rule slug: `rule/sport_volleyball`.

## What it covers

- **The score model volleyball needs.** `home_score` / `away_score` are **sets won** (3-1, 2-0), never rally points. The set scores go in `score_display` in one strict, parseable format (`25-21, 23-25, 25-19, 25-27, 15-12`, home side first in every pair) and in `periods` as structured `{number, home, away}` objects, which the standings read. Set scores never go in a stat line.
- **Results that fit the sport:** best-of matches can only be won or lost; `draw` is legal only in fixed-set rec formats (1-1); `decided_by` adds `golden_set`, `retired`, `time_limit`, `forfeit`, `walkover`, `aggregate`.
- **Positions and the libero:** `S OH OPP MB L DS UT`, the libero rules the AI checks before logging a lineup, starting rotation on `PLAYED_IN.lineup_order` 1-6 (1 serves), libero as `position L` with a Libero note, rotations per set in lore. Beach has no positions.
- **Stat keys** for attacking, setting, serving, receiving, defence, blocking and scoring, with `sets_played` as the denominator for every per-set rate. `hitting_pct`, `kills_per_set`, `points`, `blocks_total` and the rest are derived, with formulas, and may be stored only when the owner types a published figure.
- **Standings** with the international 3-2-1-0 points (3 for 3-0/3-1, 2 for 3-2, 1 for 2-3, 0 otherwise), set ratio and point ratio as tiebreakers, the result-family columns, and the exact rebuild algorithm; alternatives `win_loss`, `2-1-0` and `sets_as_points` for rec leagues.
- **Tournaments:** pools then a bracket (`group_knockout`), a parent tournament event inside a season with pool, seed and placing per team, consolation brackets, two-leg ties with golden sets, best-of playoff series (series score counts matches, match score counts sets).
- **Beach pairs as teams** (`team_type pair`, named "Surname / Surname"), a new partner is a new pair, a player's season summed across pairs on request.
- **Officials:** first referee, second referee, line judges, scorer, assistant scorer, challenge referee, and the work team.
- **Recreational and Professional chunks**, a Glossary, a Season calendar, Forfeits and abandonments, and a Match Night workflow (`rule/volleyball_match_night`) that logs a match or a tournament day in the right order and quotes averages with their coverage.

**No Fantasy chunk:** there is no widely played fantasy volleyball format, so the pack ships none. The base's `rule/app_fantasy` plus the pack's Extended keys cover a home-made league.

## What it assumes

- The base Sports World (its Building Blocks, `instructions.md` and the core rules) is installed first. This pack only adds vocabulary and one workflow.
- Scores live on the match event; everything else numeric lives on stat lines; the AI computes hitting percentages, per-set rates and tables from stored lines and says how many sets and matches they cover.
- Your league publishes its own format (best of three or five, set lengths, caps, timed sets), substitution rule, forfeit scoring, block-point convention and tiebreakers. The pack ships the international defaults and expects you to correct them in a **House rules** chunk on the competition.
- Everyone in a club, school, rec or local beach world is a private person; the AI records what you say and never looks them up. Players in public professional and international competitions may be looked up on request, for sporting facts only.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World template: Building Blocks, world instructions and its `world.json`.
2. Give this pack's `world.json` to your connected AI and ask it to add everything in it to the world. It creates two rules: `rule/sport_volleyball` (the pack) and `rule/volleyball_match_night` (the workflow). Re-running is safe; existing entries are skipped.
3. Optionally add a row to the world instructions' rule index so the workflow is routed explicitly (the base already routes any volleyball competition to `rule/sport_volleyball`):
   `| A volleyball match or tournament day just happened; set scores, rotations, box scores, hitting percentages, pools | rule/volleyball_match_night |`
4. Start a conversation. Onboarding asks what you want to track; say volleyball (indoor or beach), name your league, tour or team, and the AI creates the competition (`sport: volleyball`, the level, `format` league / league_playoffs / group_knockout / knockout / series, `scoring_system: 3-2-1-0` by default) and links the pack to it with APPLIES_TO. There is no seeded competition and no league shape ships for volleyball: national leagues and international fields change every year, and the league shapes in `league-shapes/` are built for other sports (the group-then-knockout cup carries the football pack's APPLIES_TO edge). Onboarding builds the competition from your list of teams or pairs.
5. Tell it your house rules once (below).

## Stat keys at a glance

| Group | Stored | Derived (formula) |
|---|---|---|
| Always | `sets_played` | per-set rates divide by it; `games_played` on the line is the match count |
| Attacking | `kills` `attack_errors` `attack_attempts` | `hitting_pct` = (K-E)/Att; `kill_pct` = K/Att; `kills_per_set` |
| Setting | `assists` `setting_errors` `set_attempts` | `assists_per_set` |
| Serving | `aces` `service_errors` `serve_attempts` | `aces_per_set`; `ace_pct`; `service_error_pct` |
| Receiving | `reception_attempts` `reception_errors` `reception_perfect` `reception_positive` | `passing_rating` (when per-pass grades are kept) |
| Defence | `digs` `dig_errors` | `digs_per_set` |
| Blocking | `blocks_solo` `blocks_assist` `block_errors` | `blocks_total` = solo + 0.5×assist (or solo + assist, per House rules); `blocks_per_set` |
| Scoring | | `points` = kills + aces + blocks (convention per House rules); `points_per_set` |
| Discipline and misc | `net_violations` `rotation_faults` `yellow_cards` `red_cards` `expulsions` `disqualifications` | `total_errors` |
| Team, per match | the same counting keys as totals, plus `team_errors` `opponent_errors` `substitutions_used` `timeouts_used` `challenges_won` `challenges_lost` `longest_run` (sets and set scores stay on the event) | `hitting_pct`, `blocks_total`, `points` |
| Team, season (standings) | `matches_played` `wins` `losses` `draws` `wins_3_0_3_1` `wins_3_2` `losses_2_3` `losses_0_3_1_3` (or `wins_straight` `wins_decider` `losses_decider` `losses_straight` for best-of-three) `points` `sets_won` `sets_lost` `rally_points_for` `rally_points_against` `forfeit_wins` `forfeit_losses` `no_results` `matches_booked` | `set_ratio`; `point_ratio`; `win_pct`; `set_differential` |

Published rates (a .312 hitting percentage from a league site) may be stored on a season line with `source official` and components null. Every derived rate is computed only over lines whose components were recorded, and the AI says how many sets that is. Keys you add later go in the pack's **Extended keys** chunk, dated, before they are used.

## What to customize

Say these to the AI once; they go into a **House rules** chunk on your competition and override the pack's defaults for that league:

- Match format: best of five to 25 (decider to 15), best of three, sets to 21, fixed two or three sets, timed sets, caps.
- Scoring system: `3-2-1-0` (default), `2-1-0`, `win_loss`, `sets_as_points`; whether beach pools use 2/1 or sets.
- Tiebreaker order (default: points, matches won, set ratio, point ratio, head to head).
- Forfeit scoring (default 3-0 at 25-0 per set) and whether forfeit sets count in ratios.
- Substitution limit (6, 12, 15, rolling), number of liberos, whether the libero may serve.
- Block-point convention: half an assist (default) or a full point per blocker.
- Co-ed rules in your own words, so the AI can quote them; it will not enforce them (see Privacy).
- Minimum players to start, time-limit handling, what an unfinished set counts for.
- Tournament scoring across a season or tour (how placings turn into tour points).
- Qualifying minimums for leaderboards (default 2 attempts per team set for hitting).
- Whether lineups and rotations are tracked at all. Scores and your own box score are a complete setup.

Several competitions under one pack are fine (a club league, a gym league and a beach tour); each carries its own House rules.

## Privacy, deliberately

- **No injuries.** Ankles, knees, shoulders and heads are not on file. Someone who cannot play is `availability: unavailable` with no reason; a side that cannot continue is `decided_by: retired` and nothing more. The AI does not ask why.
- **No gender data.** Co-ed composition rules (three hits must include a female touch, caps on men on court, reverse co-ed) are yours to check. The pack refuses to add a gender key, tag or position to do it.
- **Children.** Club volleyball is mostly age-group. A minor is first name and last initial, position, number and team; the age group is a tag and a word on the team, never a birth year on a child; no profile links, videos or photos; no height, reach, approach or block touch for anyone under 18.
- **Body measurements** stay out beyond the base's height for adult professionals.
- **Money and contact details** are a lore chunk in your words on the team, on request only.
- **Nobody private is looked up.** Only players in public professional and international competitions, on request, for sporting facts.

## Known limits

- Point ratio needs the set scores, which live in `periods` on the event; a match logged only as "3-1" has empty `periods` and is left out of point ratio, and the table says so.
- Partial box scores carry `completeness: partial`; derived rates skip nulls and say how many sets they cover.
- Rotations per set, reception grades per pass, attack zones and serve speeds are lore, not stat keys.
- Co-ed composition cannot be validated (by design, above).
- No fantasy chunk, no seeded competitions, teams or venues.
