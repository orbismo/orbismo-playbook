# League shape: 32-team pro league, two conferences

Seeds the shape of a national professional American football league: one competition (`competition/pro_league_32_teams_two_conferences`, sport `american_football`, level pro, format league_playoffs, scoring win_loss) and 32 placeholder franchises `Team 1` to `Team 32`, tagged by conference (`conference-a`, `conference-b`) and division (`division-east`, `division-north`, `division-south`, `division-west`), four per division. It links the American football pack rule and the game-week workflow to the competition with APPLIES_TO.

It seeds no season, no people and no venues, so it carries no COMPETES_IN edges and no `conference` or `group` values. Install the base Sports World and `packs/american-football/world.json` first, then this file, then let onboarding rename the competition and each team (full name, short_name, abbreviation, city) and create the current season. Onboarding writes each team's COMPETES_IN `conference` ("Conference A") and `group` (the division alone, "East") from the team's `conference-*` and `division-*` tags, renamed to whatever the league calls its conferences and divisions. The tags can stay as they are.

Generic as of 2026-11: 32 teams, 2 x 4 x 4. If the league you follow has a different shape, create the competition in onboarding instead.
