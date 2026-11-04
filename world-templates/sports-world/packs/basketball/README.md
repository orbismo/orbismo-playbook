# Sport pack: basketball

A sport pack for the Sports World template. It teaches the connected AI the vocabulary of basketball at every level: a national top-flight pro league and a women's pro league, the continental club competition and national-team play under international rules, college, adult rec leagues and pickup runs, and the sport-specific half of fantasy basketball. Sport code: `basketball`.

## What it covers

- **Games as each level plays them**: four quarters or two halves, stopped or running clock, 5 or 6 fouls to disqualification, overtime until someone leads (so no draws; a rec league may allow a tie at the horn), mercy rules (`run_rule`) and time limits, forfeits with an awarded score (`awarded`), abandoned, suspended and replayed games (`called_game`, status `suspended`, round `Replay`), walkovers and protests. Period scores go on the team match lines, not in `event.periods`. The differences between the pro, international, college and rec rule sets (three-point line, shot clock, bonus, possession arrow) are in the Overview, and each competition's **House rules** chunk picks one; `competition.match_format` stays empty in this sport.
- **Positions**: PG SG SF PF C and the combos modern rosters list (G, F, G-F, F-C), plus the rotation order (`depth_order`) and the roles starter / rotation / bench / reserve.
- **Stages**: preseason, regular season, in-season cup (group and knockout games count in the table, the final does not), all-star, play-in (7-8, 9-10, 8-seed games), best-of series as parent events with Game N children and the 2-2-1-1-1 home-court pattern, continental final four, national-team group-then-knockout, rec tournaments.
- **A full box score**: minutes, fgm/fga, fg3m/fg3a, ftm/fta, oreb/dreb/reb, ast, stl, blk, tov, pf, tf, flagrant fouls, disqualifications, pts, plus_minus, double/triple-doubles; team period scores, paint/fast-break/second-chance/off-turnover points, possessions and pace. Every rate (FG%, 3P%, FT%, eFG%, TS%, per-game averages, ratings, game score, EFF, PIR) is derived with its formula; usage, PER and the plus-minus family are import-only.
- **Standings and tiebreakers** for a 30-team pro league (head-to-head, division winner, division record, conference record, record against playoff-eligible teams, point differential; the multi-team order too), the women's league, international-style points tables, college conferences and rec defaults, with partial-table honesty when the owner logs one team.
- **Rosters at the top level**: 15 + 3 two-way contracts (`roster_list` standard / two_way / hardship), 13 actives, the 240-minute box-score check, two-way limits and the development-league affiliate as a loan-style second edge, list changes as `roster_move` events, 10-day and hardship deals, trades with salary matching and picks as text, voided trades, the lottery and the two-round draft, draft-and-stash rights, free agency and waivers, college scholarships and the portal.
- **Rec and pickup**: running-clock halves, rolling subs, ringers, co-ed rules kept as text (no gender key), scorebooks that often hold nothing but points (`null` for what nobody counted, every average led by its coverage), pickup runs as a competition with shirts/skins sides and no team entities, youth leagues with the minimum data.
- **Fantasy**: slots PG SG G SF PF F C UTIL BN IL, eligibility from real positions and combos, daily vs weekly lineups, H2H points, H2H categories (the standard nine: FG% FT% 3PM PTS REB AST STL BLK TOV), most-categories and rotisserie, default scoring tables, roster-size defaults. Rosters, waivers, trades and matchup mechanics come from the shared `rule/app_fantasy`.
- **Officials**: crew_chief, referee, umpire, alternate_official, replay_official, scorer, timer, shot_clock_operator, statistician.
- Two workflows: **Basketball Game Log** (`rule/basketball_game_log`) for the post-game routine of any team you play on or follow, and **Basketball Fantasy Week** (`rule/basketball_fantasy_week`) for daily lineups and closing a points or categories week.

## What it assumes

- The base Sports World (its Building Blocks, `instructions.md` and the ten core rules) is installed first. For fantasy leagues, the shared `rule/app_fantasy` from the base is installed too.
- Rosters use the base PLAYS_FOR properties as the pack maps them: `arrangement two_way` for a two-way contract, `roster_list` for the count a player is in (standard, two_way, hardship, affiliate), `role` for what they are to the team (starter, rotation, bench, reserve, captain), `depth_order` for the rotation order. List changes are `roster_move` events; a voided trade or lost waiver claim is an event with status `rejected`.
- Conference and division are two COMPETES_IN properties: `conference` ("Conference A") and `group` (the division alone, "North"). An in-season cup game is PART_OF both the season and the cup's parent event.
- Fantasy lineups: `slot` on the fantasy PLAYS_FOR edge is the lineup now; `PLAYED_IN.slot` on the weekly matchup is the snapshot; category weeks go on a round-scope team line, as `rule/stats` allows.
- Scores live on the match event; every other number on stat lines; the AI computes averages and standings from stored lines and says how many games they cover.
- People in rec, college, youth and fantasy worlds are private; nobody is looked up. Pro players' sporting facts may be looked up only when you ask.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World template: Building Blocks, world instructions, and its `world.json`.
2. Give this pack's `world.json` to your connected AI and ask it to add everything in it to the world. It creates three rules: `rule/sport_basketball` (the pack), `rule/basketball_game_log` and `rule/basketball_fantasy_week`. Re-running is safe; existing entries are skipped.
3. Optionally add rows to the world instructions' rule index so the workflows are routed explicitly (the base already routes any basketball competition to `rule/sport_basketball`):
   `| A basketball game just happened; box scores, averages, next game | rule/basketball_game_log |`
   `| Fantasy basketball lineups, categories, roto | rule/basketball_fantasy_week |`
4. Either create your competition in onboarding (say basketball, name the league and your team; the AI creates it with `sport: basketball`, the right `level` and `format`, and links the pack with APPLIES_TO), or, for a 30-team pro league in two conferences of three divisions, install `league-shapes/pro-league-30-two-conferences.json` and let onboarding rename the competition and `Team 1` to `Team 30`; when onboarding creates the first season it writes each team's COMPETES_IN `conference` and `group` from the team's `conference-*` and `division-*` tags. A rec league fits the generic round-robin-plus-tournament league shape; a fantasy league fits the generic head-to-head fantasy league shape.
5. Tell the AI your competition's rule set once (periods and length, clock, fouls to disqualification, shot clock, overtime, mercy rule, forfeit score, tiebreakers). It stores them in a lore chunk titled **House rules** on the competition.

## Stat keys at a glance

| Group | Stored | Derived (formula) |
|---|---|---|
| Player scoring | `minutes` `pts` `fgm` `fga` `fg3m` `fg3a` `ftm` `fta` `and_ones` | `fg2m` `fg2a`; `fg_pct` = fgm/fga; `fg3_pct`; `ft_pct`; `efg_pct` = (fgm + 0.5·fg3m)/fga; `ts_pct` = pts / (2·(fga + 0.44·fta)); `ppg` |
| Player everything else | `oreb` `dreb` `reb` (only when the book gives just a total) `ast` `stl` `blk` `tov` `pf` `tf` `flagrant_fouls` `disqualifications` `ejections` `plus_minus` `double_doubles` `triple_doubles` `blocks_against` `fouls_drawn` `starts` | `reb` = oreb + dreb; `ast_tov_ratio`; `rpg` `apg` `spg` `bpg` `mpg`; `game_score`; `efficiency`; `pir` |
| Player advanced (import only) | `usage_pct` `per` `oreb_pct` `dreb_pct` `ast_pct` `stl_pct` `blk_pct` `tov_pct` `off_rating` `def_rating` `net_rating` `bpm` `vorp` `win_shares` and friends, with `source_url` | |
| Team per game | `points_q1..q4` `points_ot1..` or `points_h1` `points_h2`; team totals under the player keys; `team_rebounds` `points_in_paint` `fast_break_points` `second_chance_points` `points_off_turnovers` `bench_points` `biggest_lead` `lead_changes` `times_tied` `possessions` (if published) | `possessions` estimate = fga − oreb + tov + 0.44·fta; `pace`; `off_rating` `def_rating` `net_rating`; shooting rates; `tov_pct` `oreb_pct` `ft_rate` |
| Team season (standings) | `wins` `losses` `ties` `points_for` `points_against` `home_*` `away_*` `division_*` `conference_*` `forfeit_*` `streak` `last_10_*` `standings_points` `series_wins` `series_losses` | `win_pct`; games back; `margin` |
| Fantasy | `fantasy_points` `projected_points` `games_counted` `games_scheduled`; category totals incl. stored `fg_pct` `ft_pct` `ato`; `roto_points` `categories_won/lost/tied` | |

Keys the owner adds later go in the pack's **Extended keys** chunk, dated, before they are used.

## What to customize

Say these to the AI once; they go into a **House rules** chunk on your competition:

- Periods and length, running or stopped clock, shot clock, overtime length (or none, and whether a tie stands), mercy rule, time limit.
- Fouls to disqualification (5 or 6), bonus rule, technical-foul ejection threshold.
- Forfeit score (default 20-0 rec, 2-0 pro) and roster minimum at tip.
- Standings: win-loss or a 2-1-0 points table, the tiebreaker order, point-differential cap per game.
- Playoff shape: how many qualify, play-in or not, series lengths, seeding by conference or a single table.
- Roster limits, two-way count, co-ed on-floor rules (text only), ringer policy.
- Qualifying minimum for leaders (default 70% of team games).
- Fantasy: lineup slots and counts, scoring format and table, roster size, IL spots, move limits, daily or weekly lineups.

Several competitions under one pack are fine (a pro league you follow, your Thursday rec league and a fantasy league); each carries its own House rules.

## Privacy, deliberately

- **No injuries, no reasons.** The daily availability report becomes `availability` only (Out → unavailable, Doubtful/Questionable → doubtful); body parts, illness, rest and personal reasons are never stored. A DNP is a game not played. Voided trades record the void, not the physical.
- **No gender key** for co-ed rules; you enforce them, the AI records them as text.
- **Kids** get first name and last initial, team score and points by default, no leaderboards of minors unless you ask, no height or weight, no profiles.
- **Private people** everywhere outside public pro leagues: nothing is looked up or enriched.
- **No betting keys** by default.

## Known limits

- Daily fantasy lineups are recorded as the current slot plus per-week PLAYED_IN edges and `games_counted`/`games_scheduled`; the day-by-day grid is kept only on request, in lore, and cannot be filtered.
- Game-day active/inactive is a per-game fact (`PLAYED_IN.played`), not a roster list, so "how many games was he inactive" is a count over edges.
- Pickup games use the side label (shirts, skins) in `PLAYED_IN.for_team` and have no team entities, so pickup records are per person only.
- PER, usage and the plus-minus family are import-only: this world cannot compute them honestly without league-wide data.
- No real league, team or player is seeded anywhere; the league shape is placeholders.
