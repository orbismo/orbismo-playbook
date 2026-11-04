# Rugby pack for Sports World

Sport code `rugby`. One pack for **rugby union** (fifteen-a-side and sevens) and **rugby league** (thirteen-a-side), at any level: a national team the owner follows, a professional club or franchise league, a club with a 1st XV, a 2nd XV and age-grade sides, a sevens or touch tournament, or a fantasy rugby league. The code in play is set per competition with `match_format` (`union_15s`, `union_7s`, `union_10s`, `league_13s`, `league_9s`, `touch`, `tag`), overridable per event, with a matching tag (`rugby-union`, `rugby-sevens`, `rugby-league`) so one tag search finds every competition of a code.

## What it covers

- **Scoring by code.** Union: try 5, conversion 2, penalty 3, drop goal 3, penalty try 7. League: try 4, conversion 2, penalty 2, field goal 1 (2 from outside 40 m where adopted), penalty try 4 + 2. The match-day workflow checks that the components add up to the score.
- **Results.** Draws exist in both codes. Golden point, extra time, kicking competitions, aggregate ties and awarded results are `decided_by` values; `score_display` carries "20-18 (GP)" and the like.
- **Positions by shirt number** (union 1-15, league 1-13, sevens 1-7), with the names for conversation and group codes (FR, SR, BR, HB, CE, BT; SPINE, EDGE, MIDDLE) for fantasy slots.
- **Matchday squads and the bench.** Union 23 with the front-row cover rule; league 17 with an interchange cap; sevens 12. Permanent and temporary replacements and interchange as `left_at`, `replaced_by` and `temporary` on the `PLAYED_IN` edge (never a reason), the 20-minute red card, the sin bin. Minutes played are the stat key `minutes`, so `PLAYED_IN.minutes` stays empty.
- **Cards and discipline as sport.** `yellow_cards` (the sin bin in both codes), `red_cards`, `red_cards_20min`, `on_reports`; citings and bans as `availability: suspended` with `availability_until` plus a dated Disciplinary note. Bunker upgrades count once, as the final sanction.
- **Stats** for players (scoring, attack, defence, set piece, errors, appearances and caps) and teams (tries, kicks, possession, territory, scrum/lineout/ruck success, set completion, half scores), with every rate marked stored or derived and its formula.
- **Standings with bonus points.** Scoring systems `union_bonus_4try`, `union_bonus_margin`, `union_bonus_margin_5`, `league_2-1-0`, `win_loss`, `custom`; byes as `bye` events that score only where the system awards bye points; deductions; the standings rebuild reads each side's `tries` from the team match lines. Tiebreakers are presets (`pd_first` the default, `h2h_first`, `wins_first`, `league_pd`, `for_and_against`) that a competition names in its `House rules` chunk or replaces with its own ordered list.
- **Competition shapes.** A six-nation round-robin championship, a southern-hemisphere international championship, a world championship with pools then knockouts, club leagues with playoffs or a barrage, 13-a-side leagues with a finals series, an interstate series as a `series` competition, sevens world series legs as parent events (pool, seed and placing on each side's `COMPETED_IN` edge to the leg), bilateral Test series as seasons.
- **Club and country.** One person, two open PLAYS_FOR edges: `contract` at the club, `representative` for the nation. Club sides (1st XV, 2nd XV, U14s) are teams PART_OF the club, which holds the roster.
- **Optional chunks:** Recreational (club, age-grade, touch and tag, sporadic stat keeping), Professional, Fantasy (slots, default scoring tables for union and league, categories, roster sizes; the workflows are in the base `rule/app_fantasy`), Glossary, Season calendar, Forfeits and abandonments.
- **`rule/rugby_match_day`**, a workflow that logs a match end to end and always writes the two team match lines (points and tries) the bonus-point table needs.

## What it assumes

- The base Sports World (`building-blocks.json`, `instructions.md`, `world.json`) is installed first, and `rule/app_fantasy` from the base handles fantasy rosters and matchups. The pack uses the base as it stands: `match_format` on competition and event, `left_at`, `replaced_by` and `temporary` on `PLAYED_IN`, `availability_until` and `depth_order` on `PLAYS_FOR`, arrangement `representative` for national and select sides, `bye` events, `season.roster_deadline`, and a standings rebuild that reads team match-line `tries`.
- Metric units (metres) and the sport's own words in prose.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World template into an Orbismo world (Building Blocks, instructions, `world.json`).
2. Give `packs/rugby/world.json` to your connected AI and ask it to add everything in it. It creates `rule/sport_rugby` and `rule/rugby_match_day`. Re-running is safe (`skip_existing`, `skip_duplicates`).
3. Add two rows to the world instructions' rule index: the pack (trigger "any competition in rugby") and `rule/rugby_match_day` (trigger "a rugby match just happened").
4. Either create your competition in onboarding (the AI will ask for the code and set `match_format` and `scoring_system`), or seed the generic league shape `league-shapes/international-championship-6-round-robin.json` and rename its six placeholder nations.

## Stat keys at a glance

Player: `tries conversions conversion_attempts penalty_goals penalty_attempts drop_goals drop_goals_two_point points kicking_pct* try_assists carries metres metres_per_carry* defenders_beaten clean_breaks offloads passes kicks_from_hand kick_metres kick_return_metres dummy_half_runs tackles missed_tackles tackle_pct* dominant_tackles turnovers_won intercepts lineouts_won lineouts_stolen lineout_throws lineout_throws_won rucks_hit mauls_won errors knock_ons penalties_conceded ruck_infringements ten_metre_infringements yellow_cards red_cards red_cards_20min on_reports appearances starts minutes caps man_of_the_match`

Team: `points points_conceded tries tries_conceded penalty_tries possession_pct territory_pct turnovers_conceded scrums_won scrums_lost scrum_pct* scrum_penalties_won lineouts_won lineouts_lost lineout_pct* lineouts_stolen mauls_won rucks_won rucks_lost ruck_pct* sets_completed sets_total completion_pct* play_the_balls kicks_in_general_play forty_twenties goal_line_dropouts_forced sin_bin_minutes points_h1 points_h2 points_et tries_h1 tries_h2` plus the player totals.

Standings: `played wins draws losses byes points_for points_against tries_for tries_against try_bonus losing_bonus bonus_points deductions grand_slam_bonus table_points series_points`; derived `points_difference tries_difference win_pct`.

`*` = derived, never stored; formulas are in the Stat keys chunks.

## What to customize

- **Scoring system and tiebreakers.** Set `scoring_system` on each competition and name a tiebreaker preset, or write the published order, in a `House rules` lore chunk on the competition; without one the AI uses `pd_first` and offers to store the competition's own.
- **Squad sizes, interchange caps, half lengths**, the two-point field goal, the 20-minute red card, forfeit scores and the abandonment threshold: House rules per competition.
- **Fantasy scoring.** The Fantasy chunk's tables are defaults; the platform's actual table goes in the fantasy competition's `House rules` chunk per `rule/app_fantasy`.
- **Age-grade law variations** for youth sides: House rules, with the base's children rules unchanged.
- New stat keys: add them under `Extended keys`, dated.

## Known limits

- Bonus points need each side's tries on a team match line for every match; a match logged with only the score makes the table provisional, and the AI says so.
- Possession and territory are stored broadcaster figures and cannot be rebuilt or checked.
- No health information, ever: temporary replacements record who, when and that it was temporary, never why. Height and weight on person are optional and never requested.
- No odds or betting keys.
