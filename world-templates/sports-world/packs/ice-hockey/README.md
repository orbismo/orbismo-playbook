# Sport pack: ice hockey

One pack for ice hockey at every level: a national professional league, a women's pro league, a continental pro league, international tournaments (the world championship, the winter games tournament), junior, college and university, minor hockey, and adult recreational "beer league". It also supplies the sport-specific half of fantasy ice hockey; the generic fantasy mechanics live in the base's `rule/app_fantasy`.

Code: `ice_hockey`. In conversation the assistant says "ice hockey". Nothing in this pack names a real league, team, player, federation or platform.

## What it covers

- **The game**: three periods, the single score number (goals), overtime formats (regular-season 3-on-3 then a shootout; 20-minute sudden-death playoff overtime; international variants; rec leagues with ties or no overtime), and how a shootout win is recorded (`decided_by: shootout`, the winner credited one goal, the loser keeping the loser point).
- **Positions**: C, LW, RW, D, G, with F as a catch-all; `depth_order` for line and pair numbers; captain and alternates as `role: captain` / `role: vice_captain`; line combinations, pairings and special-teams units in a dated "Lines" lore chunk on the team.
- **Stat keys**: a full skater set (gp g a a1 a2 pts plus_minus pim shots hits blocks takeaways giveaways toi_seconds shifts ppg ppa ppp shg sha shp evg eva gwg otg eng fow fol so_attempts so_goals so_gdg), a full goalie set (gp gs w l otl t ga sa sv so toi_seconds pim so_shots_faced so_saves so_ga qs rbs), team keys including period goals and special teams, and the standings keys. Every rate stat is marked stored or derived with its formula (sv_pct = sv / sa, gaa = ga × 3600 / toi_seconds, shooting_pct = g / shots, fo_pct = fow / (fow + fol), pp_pct, pk_pct, pdo, pts_pct). Time on ice is stored in seconds.
- **Standings**: the 2-1-0 loser-point system (W L OTL, points), the international 3-2-1-0 system, rec 2-1-0 with ties; the tiebreakers in order (points percentage, regulation wins, regulation + overtime wins, total wins, head-to-head, goal differential, goals for); the join the base's standings rebuild does between a team's `COMPETED_IN.result` and the event's `decided_by` to split L from OTL on derived lines (owner and official lines are never overwritten); conference and division from `COMPETES_IN.conference` and `COMPETES_IN.group`; the 16-team divisional playoff bracket with best-of-seven series as parent events and games as children.
- **Roster rules**: dressed lineup of 20 (18 skaters, 2 goalies), the active-roster limit, reserve lists as `role: injured_reserve` with `roster_list` (`ir`, `ltir`) and `roster_move` events for the dates, no reason stored; affiliate assignments and conditioning stints as the base's loan pattern, waivers, the seven-round draft, trades with retained salary and conditions in text, emergency backup goalies and rec-league subs as `arrangement: substitute`.
- **Officials**: referee, linesman, video_goal_judge, off_ice_official, scorer, supervisor.
- **Recreational**: rolling subs, sign-in sheets, goals-and-assists-only books with `completeness: partial` (nulls are skipped, never zero), shared and borrowed goalies, "sub goalie" ringers and playoff eligibility, tiers as divisions, mercy rules as `decided_by: run_rule`, youth defaults.
- **Professional**: the season calendar, trade deadline (`season.roster_deadline`), playoffs as series, cap and contracts as text, waivers and recalls, reserve lists, national-team edges as `arrangement: representative`, modern stats as Extended keys only, three stars, international tournaments.
- **Fantasy**: slots (C C LW LW RW RW D D D G G BN IR, with F/UTIL/IR+ variants) on `PLAYS_FOR.slot` and, where lineup history is kept, `PLAYED_IN.slot`; daily lineups, head-to-head categories (G A PPP SOG HIT BLK + goalie W GAA SV% SO), rotisserie, points-league default scoring, category keys on fantasy team lines. The workflows are the base's `rule/app_fantasy`.
- **Glossary, Season calendar, Forfeits and abandonments** (postponed keeps the event; suspended is one event with two dates; a replay from scratch is a new event with `round: Replay`), and a workflow rule `rule/ice_hockey_game_log` for game night.

## What it assumes

- The base Sports World (its Building Blocks, `instructions.md` and `world.json`) is installed first. The pack uses the base as it stands: `PLAYS_FOR.role injured_reserve`, `roster_list`, `depth_order`, `arrangement substitute` and `representative`, `PLAYED_IN.slot`, `stat_line.completeness`, `event.periods` for an optional line score, durations in seconds (`toi_seconds`, so `PLAYED_IN.minutes` stays empty), and the standings rebuild that reads `decided_by`.
- A fantasy league has two APPLIES_TO edges: the base's `rule/app_fantasy` and this pack.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World: set up its Building Blocks, set `instructions.md` as the world instructions, add the base `world.json`.
2. Add `packs/ice-hockey/world.json` to the world. It creates two rules: `rule/sport_ice_hockey` (the vocabulary) and `rule/ice_hockey_game_log` (the game-night workflow). Add a row for each to the instructions' rule index.
3. Create the competition in onboarding (a rec league, a junior team's league, an international tournament, a fantasy league), or add the generic league shape `league-shapes/pro-ice-hockey-league-32-two-conferences.json` for a 32-team, two-conference pro league shape and let onboarding rename the competition and the teams. The pack rule must have an APPLIES_TO edge to every ice hockey competition; onboarding adds it and the starter includes it.

## Stat keys at a glance

| Group | Keys |
|---|---|
| Skater counting | gp g a a1 a2 pts plus_minus pim shots missed_shots blocked_shot_attempts hits blocks takeaways giveaways toi_seconds pp_toi_seconds pk_toi_seconds shifts ppg ppa ppp shg sha shp evg eva gwg otg eng fow fol so_attempts so_goals so_gdg penalties_drawn penalties_taken |
| Skater derived | shooting_pct fo_pct pts_per_gp toi_per_gp pim_per_gp a1_share |
| Goalie counting | gp gs w l otl t ga sa sv so toi_seconds pim g a so_shots_faced so_saves so_ga qs rbs |
| Goalie derived | sv_pct gaa gsaa qs_pct |
| Team | goals_for goals_against goals_p1 goals_p2 goals_p3 goals_ot goals_so shots_for shots_against pp_opportunities pp_goals pk_times_shorthanded pk_goals_allowed sh_goals_for sh_goals_against en_goals_for en_goals_against pim penalties hits blocks giveaways takeaways fow fol; derived pp_pct pk_pct fo_pct shooting_pct save_pct_team pdo |
| Standings (team season line) | gp w l otl t pts rw otw sow row gf ga home_/away_/div_/conf_ splits so_w so_l streak_len last10_w last10_l last10_otl; derived pts_pct |
| Fantasy | fantasy_points projected_points, category totals (g a pts ppp shp sog hits blocks plus_minus pim fow w ga sa sv so toi_seconds), cats_won cats_lost cats_tied |

## What to customize

House rules go in a lore chunk titled "House rules" on the competition (per the base's `rule/sport_packs`), not in the pack:

- Points system (2-1-0, 3-2-1-0, 3 for any win, ties allowed), tiebreaker order, and whether the loser point needs a shootout.
- Overtime format and length, mercy-rule margin, running time, period length.
- Dressed and roster limits, playoff eligibility (minimum games), goalie-pool rules, sub and ringer rules.
- Forfeit score (1-0, 3-0, 5-0).
- Fantasy: slot counts, scoring table, category list, goalie minimum starts, waiver period, FAAB budget, roster size.
- New stat keys go under the pack's "Extended keys" chunk, dated.

## Known limits

- Standings need the `decided_by` of every game to split losses from overtime losses; a game logged with only a score and no `decided_by` counts as regulation. Only derived season lines are rebuilt; a line typed in by the owner or taken from an official table is reported as it stands.
- A fan who logs one team's games gets that team's record and streak, not a league table; the assistant says the table is partial rather than inventing it.
- Shot-attempt and expected-goals models are not pack keys; store them as Extended keys with the source named.
- Daily fantasy lineups are not logged per day by default; the week's category totals are the record.
- Reserve lists, scratches and absences are recorded as designations and availability only. The pack never stores why someone is out, and youth and junior players get the base's minimal-data treatment (first name and last initial, no birth or draft year).
