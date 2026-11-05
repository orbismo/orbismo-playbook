# Sport pack: Baseball

Code `baseball`. One pack for the sport at every level: a national professional league and its affiliated minor leagues, the top leagues of other countries, college and school ball, youth leagues from tee-ball up, independent and summer leagues, adult recreational hardball, and fantasy baseball built on any of them. Softball is a separate pack (code `softball`). Shared keys are spelled identically in the baseball and softball packs so a mixed world reads one way: `decided_by` values `run_rule`, `called_game` and `time_limit`; stat keys `outs_pitched` and `games_booked`; and the `exhibition` tag on match lines that never sum into a season.

## What it covers

- **The game.** Nine innings, extra innings and the automatic runner (whose run is unearned), seven-inning doubleheader games, called games, run rules at youth level, time limits, ties where a league allows them, forfeits at 9-0, suspended games (`status: suspended`, one event with two dates) that resume from the point of stoppage, abandoned games replayed as a new event with `round: Replay`, and an optional line score in `event.periods` (one object per inning with `number`, `home`, `away`).
- **Positions.** P SP RP CL C 1B 2B 3B SS LF CF RF DH, the catch-alls IF OF UT, the two-way player `TWP`, and the on-the-day-only codes PH PR DEF.
- **Stats.** Batting (plate appearances through GIDP, with AVG OBP SLG OPS ISO BABIP derived), pitching with innings stored as `outs_pitched` so thirds add up (ERA WHIP K/9 BB/9 derived), decisions stored and never derived, fielding (putouts, assists, errors, double plays, fielding percentage derived, catcher running-game keys), team box-score totals, and the fantasy keys. Every line carries `completeness` (full/partial/unknown); nulls are skipped when summing and never read as zero. A two-way player's game is one stat line holding both groups.
- **Standings.** W L PCT GB WCGB RS RA DIFF HOME AWAY L10 STRK, games back and wild-card games back derived, the top-level tiebreaker order (head-to-head, intradivision, interdivision, last-half intraleague), and an honest refusal to show a division table when only one team's games are on file.
- **Rosters and transactions.** 26/28-man active roster inside a 40-man reserve list, lists as `PLAYS_FOR.roster_list` values (`active`, `forty_man`, `il_10`, `il_15`, `il_60`, `dfa` and so on) with `role: injured_reserve` for any injured list, every one-team list move a `roster_move` event, options and recalls through the base loan pattern with affiliates as teams `PART_OF` the parent, DFA, waivers, outrights, injured lists as list names with dates and never reasons, trades with cash and players to be named later, the twenty-round amateur draft, two-way eligibility, youth pitch-count tables.
- **Officials.** Plate and base umpires, numbered base and outfield umpires for larger crews, crew chief, replay official, official scorer.
- **Fantasy.** Slots (C 1B 2B 3B SS OF OF OF UTIL CI MI SP SP RP RP P P BN IL NA), rotisserie 5x5 categories (R HR RBI SB AVG / W SV K ERA WHIP) and common swaps, head-to-head categories, a default points table, daily and weekly periods, roster-size defaults. The mechanics (lineups, waivers, FAAB, trades, keepers, matchups, category standings) belong to the shared `rule/app_fantasy` in the base.
- **Two workflow rules.** `rule/baseball_game_day` (logging one game from score to season lines, and how to quote averages honestly) and `rule/baseball_transactions` (every roster move as a dated event with the edge changes it implies).

## What it assumes

- The base Sports World (its Building Blocks, instructions and ten core rules) is installed first.
- `rule/app_fantasy` in the base owns the fantasy workflows; this pack supplies only the baseball vocabulary for it.
- The pack uses these base properties and values directly: `event.status: suspended`, `event.event_type: roster_move`, `event.periods` (optional line score), `event.regulation_length` (`"7"` for a seven-inning game, with the tag `seven-inning`), `PLAYS_FOR.roster_list`, `PLAYS_FOR.role: injured_reserve`, `PLAYS_FOR.arrangement: substitute` for ringers, `PLAYS_FOR.depth_order` for the rotation, `PLAYED_IN.slot` for fantasy weekly history, `stat_line.completeness`, and `COMPETES_IN.conference` for the league with `group` holding the division alone.
- Nobody's health is recorded. An injured-list placement is a list name and a date; a youth pitcher's rest is read from the league's pitch-count table and the schedule, not from anything about the pitcher.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World: Building Blocks, `instructions.md`, `world.json`.
2. Give `packs/baseball/world.json` to your connected AI and ask it to add everything in it to the world, keeping names exactly as given (slugs derive from names). It creates three rules and nothing else.
3. Create your competition in onboarding (the AI will attach the pack with `APPLIES_TO`), or install the generic league shape `league-shapes/pro-league-30-two-leagues.json` for a 30-team, two-league, three-division shape with placeholder teams you rename.
4. If you run a fantasy league, set `scoring_system` on the competition (`roto_5x5`, `roto_custom`, `h2h_categories`, `h2h_points`, `points_season`) and put the league's categories or points table in a `House rules` lore chunk on the competition.

## Stat keys at a glance

| Group | Stored | Derived |
| --- | --- | --- |
| Batting | plate_appearances at_bats runs hits doubles triples home_runs rbi walks intentional_walks hit_by_pitch strikeouts sacrifice_flies sacrifice_bunts stolen_bases caught_stealing grounded_into_double_play reached_on_error pitches_seen | singles total_bases extra_base_hits batting_average on_base_percentage slugging_percentage ops isolated_power babip strikeout_rate walk_rate stolen_base_percentage |
| Pitching | games_pitched games_started_pitching games_finished complete_games shutouts outs_pitched batters_faced hits_allowed runs_allowed earned_runs walks_allowed intentional_walks_allowed strikeouts_pitching home_runs_allowed hit_batters wild_pitches balks pitches strikes pickoffs wins_pitching losses_pitching saves save_opportunities blown_saves holds quality_starts | innings_pitched era whip k_per_9 bb_per_9 hr_per_9 k_bb_ratio strike_percentage opponent_batting_average |
| Fielding | games_fielding innings_fielded_outs putouts assists errors double_plays triple_plays passed_balls stolen_bases_allowed_catching caught_stealing_catching pickoffs_catching outfield_assists | total_chances fielding_percentage caught_stealing_percentage |
| Team match | hits errors left_on_base team batting totals double_plays_turned pitchers_used pitches_thrown_team time_of_game_seconds RISP line (the line score itself is `event.periods`, not a stat key) | |
| Team season | wins losses ties runs_for runs_against home/away, division/league/interleague, one-run and extra-innings splits last_10 streak forfeit_wins forfeit_losses games_booked | games_played win_pct run_differential games_back wildcard_games_back elimination_number pythagorean_wins |
| Fantasy | fantasy_points projected_points | category ranks and rotisserie points |

Innings are always stored as outs (`outs_pitched 20` for 6.2) and written back in thirds notation. Published advanced metrics (WAR, wRC+, FIP and the like) are stored only when imported, with a source, and never computed here.

## What to customize

Everything a league decides for itself goes in a `House rules` lore chunk on the competition, which overrides the pack for that competition: whether the DH is used, scheduled game length, the run-rule table, time limits and ties, the official-game threshold, forfeit score, tiebreaker order, re-entry and continuous-batting rules, youth pitch-count and rest tables, qualifying minimums for averages, fantasy categories or points, roster sizes and IL slots, the fantasy scoring-period calendar. Age divisions are tags on the competition (`u12`, `over_35`), never data on a person.

## Known limits

- Standings need every team's games on file; a fan logging one team gets that team's record and splits and a plain statement that the rest is not on file.
- Daily fantasy lineups are not stored; the matchup totals are. A dictated lineup goes in lore on the matchup.
- Pitching decisions, earned runs and errors are the official scorer's calls, so the owner's or the box score's word is stored and never recomputed.
- Advanced metrics that need league constants or tracking data are import-only.
- Service time, option years and contract structure are kept as the owner writes them in lore; the AI counts options used in a season but does not compute service time.
