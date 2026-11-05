# Sport pack: Football (Soccer)

Vocabulary for association football (the game called soccer in some English-speaking countries) inside the Sports World template. Machine code `soccer`; rule slug `rule/sport_soccer`. One pack covers every level: a Sunday-league side, a club with a first team, reserves and an over-35s, a youth club team, a small-sided or futsal league, a local league you only follow, the professional and international game, and fantasy football.

## What it covers

- **Positions**: GK, CB, LB, RB, LWB, RWB, SW, DF, CDM, CM, CAM, LM, RM, MF, LW, RW, ST, CF, FW, plus futsal and small-sided roles (FIXO, ALA, PIVO, UTIL). Formations go on the team's match edge (4-4-2, 2-3-1).
- **Match format**: `match_format` on the competition (11v11, 9v9, 7v7, 5v5, futsal, walking, indoor), overridable per event.
- **Stages and rounds**: preseason, friendly, exhibition, qualifying, regular_season, league_phase, group_stage, knockout, knockout_playoff, round_of_32, round_of_16, quarterfinal, semifinal, third_place, final, super_cup, playoff, promotion and relegation play-offs, and the post-season stages of a conference league; rounds written as Matchday 14, Third Round, Replay, Leg 1, Game 2. `decided_by`: regulation, extra_time, penalties, golden_goal, aggregate, away_goals, seeding, walkover, awarded, drawing_of_lots, coin_toss.
- **Results**: a shootout is a win for the side that won it (equal goals, `decided_by penalties`, the kicks in `score_display`) and a draw for W-D-L records; two-legged ties have an aggregate parent event; away goals are a per-competition house rule; suspended matches keep one event across two dates; replays are a new event with round `Replay`.
- **Stat keys**: goals, assists, penalties, shots, clean sheets, saves, cards (yellow, red, second-yellow, straight, sin bin), `minutes_played` (so `PLAYED_IN.minutes` stays empty), appearances, Player of the Match tallies; team keys for the table. Rate stats (goals per 90, save percentage, shot conversion) are derived, never stored. Optional professional keys (`xg`, `npxg`, `xa`, `progressive_passes`, `ppda`, `psxg` and so on) in the Professional (continued) chunk.
- **Standings**: P W D L GF GA GD Pts with 3-1-0 by default, deductions, walkover handling, promotion and relegation on the team's season edge, and nine tiebreaker presets (`gd_first` is the default; `gd_first_deep`, `h2h_first`, `h2h_first_minitable`, `continental_group`, `continental_league_phase`, `world_tournament_group`, `wins_first`, `short_season`) that a competition picks in its `House rules` chunk.
- **Roster rules**: squads and substitutes (rolling subs in amateur football, recorded with `entered_at`, `left_at`, `replaced_by`, `temporary`), registration windows and `roster_deadline`, the player-manager, ringers as a one-day `substitute` edge, the club-with-several-sides structure with a shared club roster, one stat line per player per side per season.
- **Officials**: referee, assistant_referee, club_assistant, fourth_official, var, avar, reserve_assistant_referee, referee_observer, match_delegate, match_secretary.
- **Optional chunks**: Recreational (small-sided forms, club structure, match fees kept as notes only), Professional and Professional (continued) (formats, two-legged ties, continental league phase, national teams with `representative` edges, a conference league, squads and managers, modern stat keys), Fantasy (slots, default scoring table, gameweeks, roster sizes; the workflows are in the base `rule/app_fantasy`), Forfeits and abandonments, Discipline (cards, totting-up, suspensions with `availability_until`), Youth defaults, Season calendar, Glossary.
- **Workflow rules**: `rule/soccer_matchday_log` (the Sunday routine for a side you play for or run, and the weekend batch for a league you follow), `rule/soccer_club_campaign` (one professional club across league, cups and continental competition: the matchday routine, the per-competition `House rules` chunk and how its tiebreaker preset is applied, international breaks, manager changes, boardroom events) and `rule/soccer_transfer_window` (windows, fees as text, pre-contracts, loans with a parent club, `contract_until`, squad numbers and registration lists, and the roster mechanics of salary-budget leagues as `designation`).

## What it assumes

- A match's single score is goals; draws are a normal result; shootout kicks are not goals (they live in `score_display`).
- Your league plays 3-1-0 unless you say otherwise. Cups are a separate competition entered by the same team. A friendlies container is a competition with format `exhibition`.
- A club with several sides is a club team (no fixtures of its own) with each side its own team, `PART_OF` the club; a player rostered to the club may appear for any of its sides.
- A player on a national team has a second open `PLAYS_FOR` edge with arrangement `representative`; caps are `PLAYED_IN` edges on international matches.
- **Children's data is minimal by default.** In a youth competition, or on any team whose name carries an age group, every player is recorded as first name and last initial, flagged as a minor, with no date of birth, contact details, photos or links, and is never looked up online. You can ask for a full name; nothing else is added. Dates of birth for team sheets belong in the league's own system, not here.
- No health data for anyone. A player who cannot play is `unavailable` with no reason; a substitution carries no reason; a banned player is `suspended` with `availability_until` and the ban's terms (matches, dates, the offence as the league stated it).
- Match fees are a note on the match in your own words, never a running account.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World first (its `building-blocks.json`, `instructions.md` and `world.json`).
2. Give this folder's `world.json` to your connected AI and ask it to add everything in it to the world. It creates `rule/sport_soccer`, `rule/soccer_matchday_log`, `rule/soccer_club_campaign` and `rule/soccer_transfer_window`; re-running skips what exists.
3. Add a row for each workflow rule to the rule index in the world instructions if the AI has not done it (the base already routes any football (soccer) competition to `rule/sport_soccer`).
4. Start a conversation and say what you want to track ("our Sunday side in Division 3 and the county cup", "my U10 7v7 team", "the local league table", "my club's season across the league, the cup and the continental competition"). The onboarding rule creates the competition, season, teams and your own side, and links the pack to the competition with `APPLIES_TO`.
5. If you track a league you only follow, you can type in the published table; it is stored as the league's official line and never overwritten by the AI's own arithmetic.

No league shape ships with this pack. The generic league shapes that fit are `league-shapes/cup-group-then-knockout.json` (32 teams in eight groups then a bracket, with the `world_tournament_group` preset already in its House rules) and `league-shapes/pro-league-32-two-conferences.json` for a conference league (set `sport` to `soccer`, `scoring_system` to `3-1-0` and the preset to `wins_first` when you rename it); a top flight is a plain 20-team `league` competition the onboarding rule builds from a list.

## Stat keys at a glance

| Group | Keys |
|---|---|
| Appearances | appearances, starts, sub_appearances, minutes_played, unused_sub |
| Attacking | goals, assists, penalty_goals, penalties_missed, penalties_won, free_kick_goals, headed_goals, own_goals, shots, shots_on_target, hat_tricks, braces, shootout_goals, shootout_misses |
| Goalkeeping | clean_sheets, goals_conceded, saves, penalties_saved, shootout_saves, gk_minutes |
| Discipline | yellow_cards, red_cards, second_yellow_reds, straight_reds, sin_bins, fouls_committed, fouls_suffered |
| Other | player_of_match_awards, captain_appearances |
| Derived (never stored) | goals_per_90, goal_contributions, shot_conversion, save_percentage, form, and the professional rates (pass_completion_pct, shot_accuracy_pct, xg_per_shot, goals_minus_xg, xg_difference, dribble_success_pct, per_90) |
| Team season (table) | played, wins, draws, losses, goals_for, goals_against, goal_difference, points, points_deducted, clean_sheets, failed_to_score, home_/away_ splits, disciplinary_points |
| Team match (optional) | shots, shots_on_target, corners, offsides, fouls_committed, yellow_cards, red_cards, possession_pct, clean_sheet, failed_to_score, own_goals_for; professional: xg_for, xg_against, shots_for, shots_against, corners_for, corners_against, ppda, field_tilt_pct, big_chances_for, big_chances_against |
| Professional player (optional) | xg, npxg, xa, xgot, big_chances, big_chances_missed, big_chances_created, key_passes, touches_in_box, passes_attempted, passes_completed, progressive_passes, progressive_carries, tackles, interceptions, blocks, clearances, recoveries, aerials_won, duels_won, psxg, sweeper_actions, bps, bonus_points and the rest listed in the Professional (continued) chunk |
| Fantasy | fantasy_points, projected_points, bonus_points, bps, price, selected_by_pct; team: total_points, gameweek_rank, overall_rank |

## What to customize

- **House rules** go in a lore chunk titled `House rules` on the competition: the tiebreaker preset or your own ordered list, away goals yes or no, qualification and relegation places, points deductions, transfer window dates, points for a win in a 5v5 league, a bonus point for scoring five, walkover recorded as points only, totting-up thresholds, whether abandoned-match stats stand.
- **Lineups**: the AI asks once per competition whether you want lineups and minutes, and remembers the answer as a tag.
- **Match format**: set `match_format` on the competition (11v11, 9v9, 7v7, 5v5, futsal, walking, indoor) and tag it with the same word.
- **New stat keys**: ask the AI to add them under the pack's `Extended keys` chunk; it will not invent one.
- **Fantasy scoring**: the default table is in the Fantasy chunk; a league's own table and settings go in its House rules chunk per `rule/app_fantasy`.

## Known limits

- Rolling substitutions are stored as a total in `minutes_played` plus text lists of stints in `entered_at` and `left_at`; there is no per-stint record.
- Standings are computed by the AI from stored results each time; a large followed league with many seasons means many reads. The team season line caches the table so hello-messages stay quick.
- Tiebreaker steps that need every team's results (opponents' records in a league phase) stop at the last step the stored lines can answer when you log only your own club's matches; the AI says which step it stopped at.
- Registration lists, squad quotas and salary-budget rules are recorded, never enforced: the AI reports what is on file and never rules a signing permitted or not.
- Money is deliberately out of scope beyond a one-line match-fee note and transfer fees kept as text.
- A player who appears for two sides in the same league season has two season lines (one per `for_team`); club-wide totals are a sum shown with its arithmetic.
