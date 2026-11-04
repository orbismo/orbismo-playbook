# Sport pack: recreational softball

A sport pack for the Sports World template. It teaches the connected AI the vocabulary of recreational slow-pitch softball: the Tuesday-night parks-and-rec league, the company co-ed league, the bar league. Fast-pitch is covered with notes; `competition.match_format` is `slowpitch` (default) or `fastpitch`. Sport code: `softball`. Baseball is a separate pack (code `baseball`). Shared keys are spelled identically in the baseball and softball packs so a mixed world reads one way: `decided_by` values `run_rule`, `called_game` and `time_limit`; stat keys `outs_pitched` and `games_booked`; and the `exhibition` tag on match lines that never sum into a season.

## What it covers

- **Games as they really end**: seven innings or a time limit, run rule, extra innings and the international tiebreaker, ties, games called after they became official, rainouts before the first pitch (the event moves its date), games abandoned after it (replayed as a new event with `round: Replay`, or resumed as one `suspended` event with two dates), forfeits with an awarded score, and the friendly people play anyway after a forfeit.
- **Doubleheaders** as two games with `round: Week N Game 1` / `Game 2`.
- **Positions** for a ten-player slow-pitch field (`LCF`, `RCF`), the extra hitter (`EH`), utility, rover, and the fast-pitch `DH`/`FLEX`.
- **Stats people actually keep**: AB, H, 2B, 3B, HR, RBI, R, BB, K and friends, with batting average, OBP, slugging and OPS always derived, never stored. A short pitching group (slow-pitch pitching barely matters; fast-pitch uses the whole group) and a fielding group with errors.
- **Sporadic books**: the central fact of rec ball. The pack and its Game Night workflow make the AI ask "was the book kept?", store `null` for anything nobody counted, mark each line's `completeness` (full/partial/unknown), skip nulls when summing, and lead every average with its coverage ("over the 8 games with a book"). An optional line score goes in `event.periods` (one object per inning with `number`, `home`, `away`).
- **Standings**: W-L-T with ties as half a win, run differential, forfeit counts, `games_booked`, and a tiebreaker order you can override per league.
- **Season shape**: round robin then a single- or double-elimination tournament, spring/summer/fall seasons under one competition.
- **Co-ed lineups, subs and ringers, two teams in two leagues, the field at the complex ("Field 3"), umpires, the postgame spot, the organiser and treasurer roles.**

## What it assumes

- The base Sports World (its Building Blocks, `instructions.md` and the ten core rules) is installed first. This pack only adds vocabulary and one workflow on top.
- Adults playing for fun. People in this world are private; the AI records what the owner says and never looks anyone up.
- The owner's league publishes its own run rule, time limit, forfeit score, minimum players and tiebreakers. The pack ships common defaults and expects you to correct them (see Customize).
- A game's score lives on the match event; everything else numeric lives on stat lines. The AI computes averages and standings from stored lines and tells you how many games they cover.

## Install

On a built-in Sports World where you picked this sport at setup, this pack is already in: skip to step 3.

1. Install the base Sports World template: Building Blocks, world instructions, and its `world.json`.
2. Give this pack's `world.json` to your connected AI and ask it to add everything in it to the world. It creates two rules: `rule/sport_softball` (the pack) and `rule/softball_game_night` (the post-game workflow). Re-running is safe; existing entries are skipped.
3. Add a row to the world instructions' rule index for the Game Night workflow if you want it routed explicitly (the base already routes any softball competition to `rule/sport_softball`):
   `| A softball game just happened; softball box scores, averages, what's coming up | rule/softball_game_night |`
4. Start a conversation. Onboarding will ask what you want to track; say softball, name your league and team, and the AI creates the competition (`sport: softball`, `level: recreational`, `format: league_playoffs` or `round_robin`, `scoring_system: win_loss`) and links the pack to it with APPLIES_TO. There is no seeded competition: every league is local.
5. Tell it your league's house rules once (below). It stores them in a lore chunk titled **House rules** on the competition.

## Stat keys at a glance

| Group | Stored | Derived (formula) |
|---|---|---|
| Batting | `plate_appearances` `at_bats` `runs` `hits` `doubles` `triples` `home_runs` `rbi` `walks` `intentional_walks` `strikeouts` `hit_by_pitch` `sacrifice_flies` `sacrifice_bunts` `stolen_bases` `caught_stealing` `grounded_into_double_play` `reached_on_error` | `singles` = H-2B-3B-HR; `total_bases`; `batting_average` = H/AB; `on_base_percentage` = (H+BB+HBP)/(AB+BB+HBP+SF); `slugging_percentage` = TB/AB; `ops` |
| Pitching | `games_pitched` `games_started_pitching` `outs_pitched` `hits_allowed` `runs_allowed` `earned_runs` `walks_allowed` `strikeouts_pitching` `home_runs_allowed` `wins_pitching` `losses_pitching` | `innings_pitched` = outs/3; `earned_run_average` = ER×7/IP; `walks_per_game` |
| Fielding | `putouts` `assists` `errors` `double_plays_turned` `passed_balls` | `fielding_percentage` = (PO+A)/(PO+A+E) |
| Team, per game | `hits` `doubles` `triples` `home_runs` `walks` `strikeouts` `errors` `left_on_base` `double_plays_turned` `stolen_bases` (runs stay on the event) | |
| Team, season | `wins` `losses` `ties` `forfeit_wins` `forfeit_losses` `no_results` `runs_for` `runs_against` `games_booked` `points` | `win_pct` = (W+0.5T)/G; `run_differential`; games back |

Every rate stat is derived only over games where its components were recorded, and the AI says how many that is. Keys the owner adds later go in the pack's **Extended keys** chunk, dated, before they are used.

## What to customize

Say these to the AI once; they go into a **House rules** chunk on your competition and override the pack's defaults for that league:

- Run rule line (default 20 after 3, 15 after 4, 10 after 5) and the time limit.
- Forfeit score (default 7-0) and whether forfeits count in runs for/against.
- Official-game line for rain (default four complete innings).
- Extra innings: none, one, or international tiebreaker.
- Minimum players to start, fielders allowed, number of extra hitters, co-ed alternation and men-in-the-field caps.
- Standings: W-L-T vs a points table, run-differential cap per game, and the tiebreaker order.
- Qualifying minimum for batting leaders (default 2 PA per team game booked).
- Fast-pitch: set the competition's `match_format` to `fastpitch`; the pack's Overview lists what changes.
- Whether you want lineups tracked at all. A team that only wants scores and its own batting lines is a complete setup.

Several leagues under one pack are fine (a Tuesday rec league and a Thursday company league); each competition carries its own House rules.

## Privacy, deliberately

- **No injuries.** Someone who cannot play is `availability: unavailable` on their roster edge, with no reason. The AI does not ask why and drops any medical detail you type, saying so in one line.
- **No gender data.** Co-ed alternation is checked by you, not the AI; the pack refuses to add a gender key to do it.
- **Kids** on the sideline are not recorded. A 16- or 17-year-old on a waiver gets first name, last initial, position and team.
- **Money stays light.** `treasurer` is a staff role; dues, if you want a reminder, are one lore chunk on the team in your own words. No amounts on people, no payment details.
- **Nobody is looked up.** Players, subs, umpires and league reps are private people.

## Known limits

- Co-ed lineup alternation cannot be validated (by design, above).
- The ringer pattern is a one-night `PLAYS_FOR` edge with `arrangement: substitute`.
- A second event is created only for a replay of a game that had actually started (`round: Replay`); a rainout before the first pitch keeps its event and moves its date.
- Line scores by inning are not stat keys; they go in `event.periods`, and only when the owner wants them.
- No seeded competitions, teams or venues: everything here is local to your park.
