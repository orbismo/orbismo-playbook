# League shape: 12-team head-to-head fantasy league

Seeds the shape of a friends' fantasy league in any sport: one competition (`competition/fantasy_league_12_teams_head_to_head`, level fantasy, format league_playoffs) and twelve placeholder fantasy teams `Team 1` to `Team 12`, six tagged `division-east` and six `division-west`. It links the base's `rule/app_fantasy` to the competition; onboarding adds the APPLIES_TO edge from the real sport's pack once the sport is known.

It seeds no season, no managers and no real players. Install the base Sports World and the real sport's pack first, then this file, then let onboarding set the sport, the scoring system and the House rules (roster slots, scoring table, waiver and trade settings from the pack's Fantasy chunk), rename the teams, add the managers, and create the season. Real players are persons who PLAYS_FOR their real team and, separately, the fantasy team that rosters them.

For a categories or rotisserie league, keep the same starter: `rule/app_fantasy` covers both formats and the pack's Fantasy chunk lists the categories.
