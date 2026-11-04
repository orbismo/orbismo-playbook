# League shape: cup, group stage then knockout

Seeds one generic football (soccer) tournament shape: a competition `Group-Then-Knockout Cup` (format `group_knockout`, level `international`, scoring `3-1-0`) and 32 placeholder teams `Team 1` … `Team 32`, tagged `group-a` … `group-h` four at a time, plus the `APPLIES_TO` edge from `rule/sport_soccer`. No season, no people, no venues.

What it is for: a national-team tournament, a continental championship, a world club tournament, or any 32-team cup that plays groups and then a bracket. The competition's `House rules` chunk already names the `world_tournament_group` tiebreaker preset (defined in the pack's Standings chunk), `away_goals: no` and the extra-time-then-penalties knockout rule (a shootout is a win for the side that won it, with the kicks in `score_display`); its `Structure` chunk says how to stretch the shape to 48 teams with a round of 32.

Install: the base Sports World, then the football (soccer) pack, then this file. In onboarding, rename the competition and the teams (slugs derive from names, so rename before anything points at them), set `team_type` to `club` for a club tournament and `level` to `pro`, set `match_format` if it is not 11v11, create the season, and set `group` on each `COMPETES_IN` edge from the team's `group-x` tag.

As of 2026-11-04. The shape is stable; the real regulations of whichever tournament you rename it to are not, so replace the `House rules` chunk with that edition's rules.
