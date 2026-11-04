# Sport pack: American Football

Vocabulary for American football at every level, for the Sports World template. It covers a national professional league (32 teams in two conferences of four divisions, 17 games over 18 weeks, 14-team playoffs), college and school leagues, adult rec tackle, and flag football, plus how real players are shared with a fantasy league in the same world.

## What is in it

`world.json` holds two rule entities:

- `rule/sport_american_football` (name "Sport: American Football"): the pack. Required chunks in contract order: Overview, Positions, Stages and rounds, Stat keys, Stat keys (continued), Standings, Roster rules, Officials, Extended keys. Optional chunks after them: Recreational, Professional, Fantasy (the sport-specific half of fantasy football: slots, scoring tables, roster sizes and the real keys they read; every workflow is in the base `rule/app_fantasy`), Glossary, Season calendar, Forfeits and abandonments.
- `rule/american_football_game_week`: a workflow for the weekly routine of someone who follows or plays on one team (result, box score, story, roster news, standings refresh, next game), plus loading a whole schedule and recording a bye.

No competitions, teams or people are seeded by the pack. A generic 32-team league shape lives at `league-shapes/pro-league-32-two-conferences.json`.

## What it assumes

- The base Sports World (its Building Blocks, instructions and ten core rules) is installed first; for a fantasy league in the same world, the base `rule/app_fantasy` too.
- Rosters use the base PLAYS_FOR properties as the pack maps them: `role` says what a player is to the team (starter, bench, captain, practice_squad, injured_reserve, reserve), `roster_list` says which league list they count against (active, practice_squad, injured_reserve, pup, nfi, suspended, exempt), `depth_order` is the depth-chart position (QB1 = position QB, depth_order 1). List moves are `roster_move` events.
- A bye is an event with `event_type bye`, linked to the team with INVOLVED_IN; it never enters standings.
- Divisions and conferences are two COMPETES_IN properties: `conference` ("Conference A") and `group` (the division alone, "East").
- The single score number is points; results are home_win, away_win or tie (never draw); decided_by is regulation, overtime, run_rule (a game stopped early at a margin) or called_game (an abandoned game counted as final). Quarter scores live on the team match lines, not in `event.periods`.
- Season labels are the year the regular season starts.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World: set up its `building-blocks.json`, set `instructions.md` as the world instructions, add the base `world.json`.
2. Give this folder's `world.json` to your connected AI and ask it to add everything in it. Names must stay exactly as written: slugs derive from them.
3. Create your competition in onboarding (the AI asks for the sport, picks this pack and adds the APPLIES_TO edge), or add `league-shapes/pro-league-32-two-conferences.json` for the 32-team shape and rename the placeholder competition and teams in the first conversation.
4. Say hello. The game-week workflow takes over from the first result you log.

## Stat keys at a glance

Lowercase snake_case, one key per quantity, numbers only. Rate stats are derived, never stored, with formulas in the pack.

- Passing: pass_attempts, pass_completions, passing_yards, passing_touchdowns, interceptions_thrown, sacks_taken, sack_yards_lost, longest_pass, passing_first_downs, passes_20_plus. Derived: completion_pct, yards_per_attempt, passer_rating, touchdown_pct, interception_pct.
- Rushing: rush_attempts, rushing_yards, rushing_touchdowns, longest_rush, rushing_first_downs, rushes_20_plus. Derived: yards_per_carry.
- Receiving: targets, receptions, receiving_yards, receiving_touchdowns, longest_reception, receiving_first_downs, yards_after_catch, drops. Derived: yards_per_reception, yards_per_target, catch_pct.
- Ball security and scoring: fumbles, fumbles_lost, fumbles_recovered_own, two_point_conversions. Derived: scrimmage_yards, all_purpose_yards, total_touchdowns.
- Defense: tackles_solo, tackles_assisted, tackles_for_loss, sacks, qb_hits, interceptions, interception_return_yards, interception_touchdowns, passes_defended, forced_fumbles, fumble_recoveries, fumble_return_yards, fumble_touchdowns, safeties, blocked_kicks. Derived: tackles_total, defensive_touchdowns.
- Kicking and punting: field_goals_made/attempted with distance bands, longest_field_goal, extra_points_made/attempted, kickoffs, touchbacks, punts, punt_yards, longest_punt, punts_inside_20, punt_touchbacks, punts_blocked, punt_return_yards_allowed. Derived: field_goal_pct, kicking_points, punt_gross_average, punt_net_average.
- Returns: kick_returns, kick_return_yards, kick_return_touchdowns, longest_kick_return, punt_returns, punt_return_yards, punt_return_touchdowns, longest_punt_return. Derived: the two averages.
- Discipline and usage: penalties, penalty_yards, games_started, snaps_offense, snaps_defense, snaps_special_teams.
- Team match line: points by quarter, first downs by type, total_plays, total_yards, rushing and net passing, sacks_allowed, third- and fourth-down conversions and attempts, red zone trips and touchdowns, penalties, turnovers, takeaways, defensive_sacks, defensive_touchdowns, return_touchdowns, safeties, blocked_kicks, time_of_possession_seconds, explosive_plays. Derived: yards_per_play, third_down_pct, red_zone_pct, turnover_margin.
- Team season line (standings cache): wins, losses, ties, points_for, points_against, division/conference/home/away splits. Derived: point_differential, win_pct, strength_of_victory, strength_of_schedule.

## What to customize

- House rules for a specific competition (rec overtime format, mercy line, flag scoring, co-ed receiver rules, a 2-1-0 points table, forfeit score, roster caps) go in a lore chunk titled "House rules" on that competition. The pack's Recreational chunk lists the common variations.
- Tiebreakers: the pack lists the pro league's full order; most other leagues use the first four steps then points allowed or a coin toss. Say which in House rules.
- Extra stat keys (advanced metrics from a stats site, a league's own flag categories) go under the pack's Extended keys chunk, dated, with the source named.
- Fantasy scoring, slots and roster sizes: the Fantasy chunk; the league's own settings go in the House rules chunk on the fantasy competition, in the template `rule/app_fantasy` gives. A fantasy competition carries sport `american_football`, level `fantasy`, and two APPLIES_TO edges: this pack and `rule/app_fantasy`.

## Known limits

- Standings need every game in the division to be on file. A fan who logs only their own team gets their own record, division and conference records, and streak; strength of victory and schedule are reported as not computable. The AI says when a table is partial rather than inventing the rest.
- Cap and contract figures are text only; nothing computes cap space.
- Draft picks are text inside trade and draft events, not entities, so "who owns our 2027 second-rounder" is answered by reading the trade events, not by a query.
- Injury reports are reduced to availability (available, doubtful, unavailable) and reserve lists to a `roster_list` value and a role, with no reason stored, at every level. This is deliberate: the world holds no health data about anyone.
- Minutes are not tracked (unlimited substitution); use snap counts when the owner has them.
