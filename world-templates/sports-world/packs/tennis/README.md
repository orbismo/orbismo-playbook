# Sport pack: tennis

A sport pack for the Sports World template. It teaches the connected AI the vocabulary of tennis at every level: tour and major tournaments a fan follows, national-team ties, college dual matches, association and inter-club team leagues, club championships, box leagues, ladders and the Tuesday hit the owner wants remembered. Sport code: `tennis`.

It is also the **pattern pack for individual and racket sports**. Pickleball, padel, badminton, table tennis and squash each get their own pack with their own stat keys, but they copy the structure set here (see the chunk `Other racket sports`).

## What it covers

- **A person is a side.** A singles match is person vs person: the player's COMPETED_IN edge is the appearance and no PLAYED_IN edge is used (PLAYED_IN.for_team is only filled in doubles and ties). No shadow teams, ever.
- **Doubles pairs** are a `team` with `team_type pair` ("Lee / Patel"): the pair has the record, the seed, the ranking and the title; both partners PLAYS_FOR it.
- **Tournaments** are competitions whose editions are seasons (the spring major, women's singles, 2026). Entrants enter the season with COMPETES_IN carrying `seed`, `entry` (direct, qualifier, wild_card, lucky_loser, protected_ranking, alternate) and `final_rank`. The **tour year** is a competition of its own whose season holds the ranking lines; each tournament season is PART_OF the tour season (season to season) and also carries the tour-year tag.
- **Rounds**: `round_of_128` … `quarterfinal`, `semifinal`, `final`, plus qualifying, round-robin groups and consolation draws.
- **Scores**: `home_score`/`away_score` are sets won (games won in pro sets and timed play; rubbers won on a team tie); `score_display` carries the full line as tennis writes it: `6-4 3-6 7-6(5)`, `4-6 6-3 [10-7]`, `8-6`, `6-4 3-6 2-5 ret.`, `w/o`; the set-by-set line can also go in `event.periods` when the owner wants it queryable; `margin_type` is `sets` or `games` with `margin`.
- **Match formats** (`match_format` on the competition, overridable per event): best of five, best of three, match tiebreak in place of a third set, short sets, pro sets, single set, timed, tiebreak only.
- **Retirements, walkovers and defaults** as sporting facts only: `decided_by retired | walkover | default`, with no reason recorded anywhere, in any wording.
- **Match stats** a serious amateur or a broadcast box score would give: aces, double faults, first serves in, serve and return points, break points won/played and faced/saved, winners, unforced and forced errors, net points, totals; every percentage derived from counts, or stored with null components when only the percentage is known; per-set splits as `scope segment` lines with `segment "Set 3"`; every line carries `completeness`.
- **Rankings and ratings** as stored snapshots on season or career lines (`ranking`, `ranking_points`, `rating`, `ladder_position`), never computed by the AI.
- **Team tennis**: a tie is a parent match event between two teams, each rubber a child event between people or pairs; lineup slots S1–S6, D1–D3; team standings with ties, rubbers, sets and games.
- **Club life**: box leagues (cycle = season, box = group, promotion and relegation), ladders (challenge rules, position moves), club championships, social play, sporadic stats.
- **Officials**: chair umpire, line umpire, referee, review official, roving umpire, supervisor, scorer; most rec matches have none.
- **Other racket sports**: what pickleball (side-out and rally scoring, dynamic ratings), padel, badminton, table tennis and squash copy unchanged and what each changes (the score number, `match_format` values, stat keys).

## What it assumes

- The base Sports World (Building Blocks, `instructions.md`, the core rules) is installed first. The pack uses the base as it stands: `person` as a source of `COMPETED_IN` and `COMPETES_IN`; `team_type pair`; `COMPETES_IN.entry` and `final_rank`; `match_format` on competition and event; `event.periods`, `margin_type` and `margin`; `stat_line.scope segment` and `completeness`; `PLAYED_IN.replaced_by`; PART_OF from a tournament season to its tour season; `decided_by retired | walkover | default` with no reason.
- A match's score lives on the event; everything else numeric lives on stat lines; the AI computes records, percentages, box tables and ladders from stored lines and says how many matches they cover.
- People in club, junior and league worlds are private; the AI records what the owner says and never looks them up.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World template.
2. Give this pack's `world.json` to the connected AI and ask it to add everything in it. It creates `rule/sport_tennis` (the pack) and `rule/tennis_match_log` (the post-match workflow). Re-running is safe.
3. Optionally add a row to the world instructions' rule index: `| A tennis match, ladder challenge, box match or tournament round just happened; tennis records, head-to-head, rankings, the draw | rule/tennis_match_log |`. The base already routes any tennis competition to `rule/sport_tennis`.
4. Start a conversation. Say what you follow or play: a tour, a tournament, a club ladder, a box league, a team league. Onboarding creates the competition (`sport: tennis`, the level and format from the pack's mapping) and links the pack to it with APPLIES_TO. For a tour-plus-tournaments shape you can seed `league-shapes/individual-tour.json` first and rename its placeholders.
5. Tell the AI your house rules once (below). They go into a lore chunk titled **House rules** on the competition.

## Stat keys at a glance

| Group | Stored | Derived (formula) |
|---|---|---|
| Serve | `aces` `double_faults` `first_serves_in` `serve_points_played` `first_serve_points_won` `second_serve_points_played` `second_serve_points_won` `service_games_played` `service_games_won` `break_points_faced` `break_points_saved` `fastest_serve_kph` `average_first_serve_kph` `average_second_serve_kph` | `first_serve_pct` = in/played (stored if only the % is given); `first_serve_win_pct`; `second_serve_win_pct`; `hold_pct`; `break_points_saved_pct` |
| Return | `return_points_played` `return_points_won` `first_serve_return_points_won` `second_serve_return_points_won` `return_games_played` `break_points_won` `break_points_played` | `return_win_pct`; `break_pct` = won/played |
| Rally | `winners` `unforced_errors` `forced_errors` `forehand_winners` `backhand_winners` `forehand_unforced_errors` `backhand_unforced_errors` `net_points_played` `net_points_won` `volley_winners` `overhead_winners` `drop_shot_winners` | `net_pct` |
| Totals | `total_points_played` `total_points_won` `games_won` `games_lost` `sets_won` `sets_lost` `tiebreaks_played` `tiebreaks_won` `longest_rally_shots` `distance_covered_m` `match_duration_min` `challenges_made` `challenges_won` `code_violations` `time_violations` | `points_win_pct`; `dominance_ratio` |
| Player season / career | `matches_played` `matches_won` `matches_lost` `walkovers_received` `walkovers_given` `retirements` `defaults` `titles` `finals` `semifinals` `ranking` `ranking_points` `ranking_high` `rating` `ladder_position` `ladder_challenges_won` `ladder_challenges_lost` `prize_money` | `match_win_pct`; `games_win_pct`; `tiebreak_win_pct` |
| Team season (ties) | `ties_played` `ties_won` `ties_lost` `ties_drawn` `rubbers_won` `rubbers_lost` `sets_won` `sets_lost` `games_won` `games_lost` `points` `walkover_rubbers_received` `walkover_rubbers_given` | rubber, set and game differences |

Pairs use the player keys on the pair's own lines. Keys the owner adds later go in **Extended keys**, dated, before use.

## What to customize

- `match_format` per competition (best of three with a match tiebreak is the club default), no-ad scoring, tiebreak-at-12-12 variants.
- Which ranking or rating system a competition uses, and its currency for `prize_money`.
- Box-league points scheme (default 3 for a win, 1 for a loss with a set won), promotion and relegation counts, how unplayed matches are scored.
- Ladder rules: how many places up a challenge may go (default 3), the acceptance window, whether an unaccepted challenge is a walkover.
- Team-league rules: number of singles and doubles rubbers, whether a player may play both, lineup-order rules, level caps, the league's points scheme and tiebreakers.
- Whether to keep match stats at all. Score-only is a complete setup.
- Age-group, wheelchair and adaptive categories are tags and House rules on the competition, never facts about a person.

## Privacy, deliberately

- `ret.`, `w/o`, `def.` and `withdrew` are the whole fact. The AI stores the result and, if you type a reason, says once that the reason stays out.
- A skill level, rating or age group describes the competition or a stat line, never the person; the AI does not infer a birth year from U14 or 60+.
- Juniors get first name and last initial, `is_minor true`, and are never looked up.
- Rating-system profiles: the AI records the number and an `external_id` for matching imports, nothing else from the profile.
- Mixed doubles is a competition category; the pack refuses to add a gender key.
- Entry fees, court fees and ladder dues are not recorded on people.

## Known limits

- Rankings are never computed; the pack does not reproduce any ranking system's rolling window.
- A scheduled match whose opponent is "winner of ..." carries one side until the other is known.
- A walkover has no score: `home_score`/`away_score` stay null and `margin_type` blank; records count it under `walkovers_received` / `walkovers_given`, not `matches_played`.
- Fantasy tennis is uncommon; the pack ships no slots or scoring table, only a note on bracket pools; any fantasy competition links to the base's `rule/app_fantasy` as well as this pack.
- No real tours, tournaments, teams or players are seeded; `league-shapes/individual-tour.json` is a renameable placeholder shape.
