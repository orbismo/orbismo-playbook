# League shape: 10-team recreational league, round robin then tournament

Seeds the shape of a local recreational league in any sport: one competition (`competition/rec_league_10_teams_round_robin_and_tournament`, level recreational, format league_playoffs, scoring win_loss) and ten placeholder teams `Team 1` to `Team 10` with team_type recreational. It is sport-agnostic, so its `sport` is `unknown` and it carries no APPLIES_TO edge; onboarding sets the sport code, tags the competition and links the sport pack.

It seeds no season, no people and no venues. Install the base Sports World and the sport's pack first, then this file, then let onboarding set the sport, rename the competition and teams, mark the owner's own team `mine`, and create the current season. Set unused placeholder teams to `status: inactive` rather than deleting them.

Fits softball, football (soccer), basketball, volleyball, ice hockey beer leagues, club cricket and most parks-and-rec formats. For a league with divisions, use one of the pro-league starters or build it in onboarding.
