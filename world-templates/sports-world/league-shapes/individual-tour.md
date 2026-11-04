# League shape: individual tour (tennis)

Seeds a generic shape for an individual sport that is played on a tour of tournaments, in the tennis pack's mapping (see `packs/tennis/README.md`):

- `competition/individual_tour`: the tour year itself, `format league`, `scoring_system ranking_points`. Its seasons ("2026") hold the players' ranking stat lines; each tournament's season for that year is PART_OF the tour season and also carries the tour-year tag. It has no matches of its own.
- `competition/tournament_1` … `competition/tournament_4`: four placeholder tournaments, `format knockout`, `match_format best_of_3`, tagged `tournament`, `tier_unknown`, `mens_singles`. Each edition of a tournament is a season; players enter it with COMPETES_IN from the person (seed, entry, final_rank); its matches are events by round with COMPETED_IN from each player.
- APPLIES_TO edges from `rule/sport_tennis` to all five.

No seasons, no people, no venues, no real names. Onboarding renames the tour and the tournaments, fixes the tier tags, discipline tags and `match_format`, adds or removes tournaments, opens the current season of each and links each tournament season to the tour season with PART_OF. For another racket sport, change `sport` and the tag to that sport's code and point the APPLIES_TO edges at its pack.

As of: 2026-11, pack `orbismo-playbook/sports-world/tennis@1.0`.
