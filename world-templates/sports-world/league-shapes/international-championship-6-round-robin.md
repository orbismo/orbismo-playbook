# League shape: international championship, 6 nations, round robin

Seeds a generic **rugby union** annual international championship for the `rugby` pack: one competition (`level international`, `format round_robin`, `scoring_system union_bonus_margin`, `match_format union_15s`) and six placeholder national teams (`Nation 1` to `Nation 6`, `team_type national`), plus the APPLIES_TO edge from `rule/sport_rugby` to the competition. No people, no season, no venues.

The competition carries a `House rules` lore chunk with the typical rules of this shape: 4/2/0 match points, a try bonus for scoring three or more tries than the opponent, a losing bonus for losing by seven or fewer, three extra points for winning all five matches, and the `h2h_first` tiebreaker preset (head-to-head first, then points difference, tries scored, points scored, discipline, lots). Five rounds over seven weekends (the two rest weekends are not byes and create no events); home and away alternate each year.

Use it after installing the base Sports World and `packs/rugby/world.json`. During onboarding, rename the competition and the six nations, set `governing_body`, `region` and `country`, edit the House rules to the competition's published ones, then create the first season. Re-running the seed is safe (`skip_existing`, `skip_duplicates`).

Generic structure as of 2026-11; contains no real competition, union or team names.
