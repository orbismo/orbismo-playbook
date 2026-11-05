# Sports World: Cricket pack

The cricket vocabulary for the Sports World template. It covers cricket at every level in one pack: international Tests, ODIs and T20Is, bilateral series and world tournaments, franchise T20 and hundred-ball leagues, first-class and List A domestic cricket, club and league cricket the owner plays in (Saturday league, timed and two-day declaration cricket, midweek T20), youth cricket, and fantasy cricket.

Nothing in the pack names a real board, competition, franchise, trophy or player. Example teams (Riverside, Castleton) and people (Jordan, Priya, Sam K.) are invented.

## What is in `world.json`

| Entity | What it is |
|---|---|
| `rule/sport_cricket` ("Sport: Cricket") | The pack rule: Overview, Positions, Stages and rounds, Stat keys, Standings, Roster rules, Officials, Extended keys, then Innings and results, Series and tours, Recreational, Professional, Fantasy, Forfeits and abandonments, Glossary. |
| `rule/cricket_scorecard_entry` | Workflow: turning a result, scorecard or scorebook photo into the `periods` array, headline score, result and margin, player lines and season lines, with the reconciliation checks a scorer does. |
| `rule/cricket_saturday_xi` | Workflow for the owner's own club side: availability (without reasons), selection, the result, fees and teas as notes, the league table, end-of-season averages. |

No competitions, teams or people are seeded, and no league shape ships for cricket. The league shapes in `league-shapes/` are built for other packs (the group-then-knockout cup carries the football pack's APPLIES_TO edge), so the onboarding rule builds a cricket competition from the owner's list of sides: a bilateral series, a franchise league of eight or ten franchises, a world tournament with groups, or a club league.

## What it assumes

- The base Sports World template is installed (its `building-blocks.json`, `instructions.md` and `world.json`).
- The base's `event.periods` array holds the innings (one element each, keys below), with `margin_type`, `margin`, `toss_won_by`, `toss_decision` and `match_format` on `event`, `match_format` on `competition`, `replaced_by` on `PLAYED_IN`, and `scope: segment` with `segment` on `stat_line` for per-innings cards. Every stat line carries `completeness`.
- Balls, not overs, are the unit everywhere in numbers. "48.3 overs" is stored as `291`. The AI shows overs in the normal way.
- Scores are the owner's statement. The AI never infers a cricket result from the two totals.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World first (its README has the steps: Building Blocks, instructions, `world.json`).
2. Give `packs/cricket/world.json` to your connected AI and ask it to add everything in it to the world. It is idempotent (`skip_existing`, `skip_duplicates`).
3. Add the three rules to the rule index in the world instructions if the AI does not do it (`rule/sport_cricket`, `rule/cricket_scorecard_entry`, `rule/cricket_saturday_xi`).
4. Start a conversation. Say what you follow or play in ("a five-Test series", "a franchise T20 league", "my club's Saturday 2nd XI in a 45-over league"). Onboarding creates the competition with `sport: cricket` and `match_format`, its season, teams and venues, and links the pack to the competition with APPLIES_TO.

## How a match is stored

- The event holds `periods`: one element per innings in the order played, each with `number`, `batting_team`, `runs`, `wickets`, `balls`, `balls_allotted`, `all_out`, `declared`, `follow_on`, `forfeited`, `target`, `kind` (`regular` or `super_over`).
- `home_score` / `away_score` are each side's total runs (display only). `score_display` is the scorer's line. `result` is `home_win`, `away_win`, `draw`, `tie` or `no_result`. `margin_type` is `runs`, `wickets`, `innings_and_runs` or `super_over`, with `margin` the number. `decided_by` is `regulation`, `dls`, `vjd`, `super_over`, `bowl_out`, `boundary_count`, `walkover` or `awarded`. `toss_won_by` and `toss_decision` (`bat`/`bowl`) record the toss.
- A bilateral series is a season of a competition with `format: series`; each Test or ODI is a match with `round` "1st Test", "2nd ODI". A tour with several formats is one season per format, tied together by a shared tag.

## Stat keys at a glance

Batting: `innings_batted`, `not_outs`, `runs`, `balls_faced`, `fours`, `sixes`, `dots_faced`, `ducks`, `fifties`, `hundreds`, `highest_score`, and dismissal counts `out_bowled`, `out_caught`, `out_lbw`, `out_run_out`, `out_stumped`, `out_hit_wicket`, `out_other`, `out_retired`, `retired_not_out`.

Bowling: `innings_bowled`, `balls_bowled`, `maidens`, `runs_conceded`, `wickets`, `wides`, `no_balls`, `dots_bowled`, `four_wicket_hauls`, `five_wicket_hauls`, `ten_wicket_matches`, `best_bowling_wickets`, `best_bowling_runs`.

Fielding and keeping: `catches`, `wk_catches`, `stumpings`, `run_outs`, `byes_conceded`, `drops`.

General: `matches`, `player_of_match`, `fantasy_points`.

Team season (standings): `played`, `won`, `lost`, `tied`, `drawn`, `no_results`, `points`, `bonus_points`, `penalty_points`, `points_available`, `runs_scored`, `balls_faced_total`, `runs_conceded`, `balls_bowled_total`, `net_run_rate`, `pct`.

Derived, never stored on match lines: batting average = runs / (innings_batted − not_outs), undefined with no dismissals; strike rate = runs × 100 / balls_faced; bowling average = runs_conceded / wickets; economy = runs_conceded × 6 / balls_bowled; bowling strike rate = balls_bowled / wickets. Net run rate = runs per over scored − runs per over conceded, with a bowled-out side charged its full quota and a DLS-decided match using the par score.

## What to customize

- **Scoring and bonus points.** The default is 2 for a win, 1 for a tie or no result. A club league's bonus and penalty points, or a tournament's own table, go in a lore chunk titled `House rules` on the competition.
- **Tiebreakers.** Default order: points, wins, net run rate, head-to-head. Override in `House rules`.
- **Format details.** `match_format` and `balls_allotted` per competition; minimum overs for a result; bowler limits; overseas-player or registration limits.
- **Extended keys.** Anything the owner wants to count that the pack does not list (double hundreds, direct-hit run outs, powerplay splits) is added under the pack's `Extended keys` chunk, dated, before it is used.
- **Fantasy scoring.** The fantasy platform's points table goes in `House rules` on the fantasy competition; the workflows live in the base's `rule/app_fantasy`, and the pack supplies only slot names, the default roster size, the real keys that feed scoring and the `fantasy_points` key.

## Known limits

- No ball-by-ball record. The smallest units are the innings elements of `periods` (on the event) and the player's match line. Partnerships, phase splits and wagon wheels are not modelled unless the owner adds split lines.
- Net run rate is computed by the AI from the stored `periods`, so it is only as complete as the innings data. Matches logged as "we won by 40" with no balls faced cannot feed NRR; the AI says so.
- Rankings published by a governing body are a dated note on the competition, not standings.
- Health is out of scope by design. A concussion or injury replacement is recorded only as who replaced whom; "retired hurt" is stored as `retired_not_out` with no reason; a player who cannot play is `unavailable`, nothing more. Youth players are first name and last initial with no profile links.
