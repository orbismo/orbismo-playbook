# League shape: 30-team professional baseball league, two leagues of three divisions

Seeds one generic competition (`competition/pro_league_30_teams_two_leagues`, sport `baseball`, level `pro`, format `league_playoffs`, scoring `win_loss`) and thirty placeholder franchises, `Team 1` to `Team 30`, tagged by league (`league-a`, `league-b`) and division (`league-a-east`, `league-a-central`, `league-a-west`, and the same for League B), five per division. It also adds the `APPLIES_TO` edge from `rule/sport_baseball` to the competition, so the baseball pack must be installed first.

It seeds no season, no people and no venues. The onboarding rule creates the season and the `COMPETES_IN` edges (`conference` "League A", `group` "East" and so on), and renames the placeholder teams to the real ones; rename before anything points at a team, since slugs derive from names. Set `governing_body` and `region` on the competition at the same time (they are seeded as `unknown`).

The shape matches a 162-game, two-league, six-division national league with a twelve-team postseason (wildcard best of three, divisional best of five, league championship and final best of seven). A league with a different shape (two leagues of six, one league of ten) is better created in onboarding without this starter.

Placeholder names collide across league shapes under `skip_existing`: install one `Team N` league shape per world, or rename its teams in the file before installing another. Structure as of November 2026.
