# Sports World

**Keep the record of any sport you follow or play, at any level.** A pro league and the fantasy league built on it, a Tuesday-night softball team, a Sunday-league football (soccer) side, a Saturday cricket XI, a club tennis ladder, several of these in one world.

Published as the built-in **Sports World** template on every Orbismo plan. Like the [Commonplace Book](../commonplace-book/) and the [Agent Team Workspace](../agent-team-workspace/), it does not run on Orbismo's default schema; it ships its own kinds and connections.

Written for an international audience: the sport is "American football" (a national pro league is a competition inside it), association football is "football (soccer)" with the machine code `soccer`, and nothing shipped here names a trademarked league, team, platform or person. Those names enter a world only when its owner brings them in.

## The four pieces

Everything in this folder is one of four things. Knowing which is which is most of understanding the template.

| Piece | File | What it is | Sport-specific? |
| --- | --- | --- | --- |
| **Instructions** | `instructions.md` | The world instructions. Short: tone, hard rules, and a table saying "for topic X, read rule Y". | No. One file for every sport. |
| **Core rules** | `world.json` | Ten rule entities that live in the world: how to record a match, keep stats, run rosters, build standings, run a fantasy league, protect people. | No. |
| **Sport pack** | `packs/<sport>/world.json` | One sport's vocabulary as two to four more rule entities: positions, stat keys, stages, standings columns, tiebreakers, and how pro, rec and fantasy differ. The core rules say "take the stat keys from the pack"; the pack supplies them. | Yes. One per sport. A world needs at least one and can hold several. |
| **League shape** | `league-shapes/<shape>.json` | An empty league of a given shape: placeholder competition and teams, no seasons, people or results. Saves typing when your league has a big fixed structure. | Mostly. Optional; most owners never use one. |

Pro, recreational and fantasy are not separate packs or templates. They are the `level` of a competition, set during onboarding, and most packs' rules carry a Recreational, a Professional and a Fantasy chunk (softball covers rec play only; volleyball has no Fantasy chunk, so a fantasy league there starts from House rules). A pro season and the fantasy league built on it live in the same world, on the same players.

## Getting started

### In Orbismo (the normal way)

1. Create a world and pick **Sports World**. The schema and the instructions are set for you.
2. On the new world's dashboard, finish setup by picking your sport. That one step loads the core rules and that sport's pack. Skipping it leaves the world with instructions and nothing to run them on; the assistant will send you back to finish setup.
3. Say hello. The onboarding rule asks what you want to track and builds the competition, season and teams with you.

Nothing in Orbismo uses the words "pack" or "league shape". The only choice is the sport.

### Adding more later

- **A second sport:** give that pack's `world.json` (under [`packs/`](packs/)) to your connected AI and ask it to add everything in it, keeping names exactly as given.
- **A league shape:** give the file (under [`league-shapes/`](league-shapes/)) to the AI the same way, after its sport's pack is in. Then onboarding renames the placeholders.
- **A sport with no pack:** the AI drafts one from the contract in `rule/sport_packs`, shows it in full, and creates it once you approve.

### By hand, on any world

1. Create a new Orbismo world (or pick an empty one).
2. Recreate the schema under World Settings → Building Blocks from `building-blocks.json`: eight kinds with their properties and enums, fifteen connections with the kinds each allows. Remove the default kinds and connections this template does not use. The portal has no Building Blocks import.
3. Set `instructions.md` as the world's instructions: paste it in the web UI, or have the AI update the world instructions from the file.
4. In a fresh conversation (the AI reads the world's structure once per session, so start after step 2), give `world.json` to the AI and ask it to add everything in it.
5. Give it one sport pack the same way. Optionally one league shape.
6. Say hello.

## What it is good for, and what it is not

**Good for:** a fan who wants their team's season on the books with real standings and box scores; a player or organiser of a rec team who wants results, lineups and batting averages without a spreadsheet; a fantasy manager who wants rosters, moves and weekly scores in one place with the real players; a coach keeping a club's seasons and squads year over year; a cricket tragic who wants innings-by-innings scorecards and a net run rate that is actually computed; a tennis or pickleball player tracking a ladder.

**Not good for:**

- **Live scoring.** Orbismo stores what you tell it. The AI computes standings and averages from stored lines on request; it does not fetch scores on its own, and it only offers to look something up for a public professional competition when web search is available.
- **Multi-competitor events** (a race, a golf round, a swim meet) out of the box. The match model is two-sided: teams, or persons in individual sports. A pack for placings-per-event is possible but none ships yet.
- **Health tracking.** This template records whether someone is available to play and nothing about why. See Privacy below.

## Reference

### The schema: `building-blocks.json`

Eight kinds and fifteen connections. On a built-in Sports World they are there at creation; by hand you recreate them under World Settings → Building Blocks.

| Kind | Records |
| --- | --- |
| `competition` | The standing thing: a league, cup, tour, series or fantasy league. `sport`, `level` (pro … recreational, fantasy), `format` (league, league_playoffs, knockout, group_knockout, series, round_robin, ladder, exhibition), `match_format` (the form of the game when the sport has several), `scoring_system`. One per competition, never per year. |
| `season` | One edition of a competition. `label`, `status`, dates, `stage`, `current_round`, `rounds_total`, `roster_deadline`. |
| `team` | A side for its whole life: club, franchise, national, school, recreational, fantasy, select or pair (a doubles pairing). Season-specific facts live on its `COMPETES_IN` edge. |
| `person` | Player, coach, manager, official, executive, staff, fantasy manager or fan. What they do for a team is on `PLAYS_FOR` and `STAFF_OF` edges. In individual sports a person is also a side. Carries `is_minor` for the children's-data rules. |
| `event` | Anything dated. `event_type` match is the main case and carries `home_score`, `away_score`, `periods` (sets, innings, line scores), `score_display`, `result`, `decided_by`, `margin`, `stage`, `round`, `status` (scheduled … suspended, abandoned, forfeited, rejected). Drafts, trades, transfers, signings, releases, roster moves, staff changes, byes, tryouts, awards and meetings are the other types. Every event carries `date_year/month/day` so `get_timeline` orders it. |
| `venue` | A stadium, ground, field, park, gym or court. |
| `stat_line` | A block of numbers about one subject in one context: a box score, a set or innings, a season total, a career total, a fantasy week. `scope` (match, segment, round, season, career, split), `stats` (keys from the sport pack), `completeness` (full, partial, unknown), `source` (owner, official, import, derived), `as_of`. |
| `rule` | Standing guidance for the AI: the core rules, the fantasy app, each sport pack. |

| Connection | Links |
| --- | --- |
| `PART_OF` | season → competition or wider season; event → season and/or parent event (a tournament, a tie, a series); team → senior team. |
| `COMPETES_IN` | team or person → season: `conference`, `group` (division), `seed`, `entry`, `final_rank`, `outcome`, `status`. |
| `PLAYS_FOR` | person (or a fantasy team unit) → team: `position`, `jersey_number`, `depth_order`, `role` (starter … injured_reserve), `roster_list`, `arrangement` (contract, two_way, loan, representative, trial, substitute, amateur, volunteer, scholarship, fantasy_roster), `availability`, `availability_until`, `slot`, `acquired_via`, `fee`, `contract_until`, `designation`, plus started/ended. |
| `STAFF_OF` | person → team or competition: `role` (head_coach, manager, general_manager, owner, commissioner, trainer, team_parent …), `title`, `unit`. |
| `PLAYED_IN` | person → match: `for_team`, `started`, `played`, `minutes`, `entered_at`, `left_at`, `replaced_by`, `temporary`, `lineup_order`, `slot`. Not used in individual sports. |
| `COMPETED_IN` | team or person → match: `side`, that side's `result`, `formation`; on an edge to a parent tournament event also that tournament's `group`, `seed`, `final_rank`. |
| `OFFICIATED` | person or work team → match: `role`. |
| `INVOLVED_IN` | person or team → non-match event: `role` (drafted, traded_away, acquired, from_team, to_team, honoured …), `detail`. |
| `HELD_AT` | event → venue (`field`). `HOME_VENUE` team → venue. |
| `STAT_OF` | stat_line → person or team. `RECORDED_IN` stat_line → match or season. |
| `WON` | person or team → season or match: `title` (Champion, Most Valuable Player, Player of the Match …). |
| `RIVAL_OF` | team ↔ team, person ↔ person. |
| `APPLIES_TO` | rule → anything. Each sport pack applies to every competition in its sport; a fantasy competition also gets an edge from the fantasy app. |

### The instructions: `instructions.md`

A short router, the same for every sport: the rule index, the hard rules (search before create, never invent a value, one home per fact, no health data, children get the minimum, confirm bulk creates, nothing deleted, show your arithmetic), naming and tone.

### The core rules: `world.json`

Ten rule entities, all sport-agnostic. They say how to do things; the pack says what the words are.

| Rule | Covers |
| --- | --- |
| `rule/core_model` | What goes where, the one-home-per-fact map, how to find a competition's sport pack, what to do at session start. |
| `rule/onboarding` | First session: pick the pack, create the competition, season, teams, venues and the owner's own team, or rename a league shape's placeholders. |
| `rule/recording_a_match` | Scheduling and scoring a match, periods and margins, who played and who replaced whom, officials, forfeits and walkovers, postponed vs suspended vs abandoned-and-replayed, multi-day matches, ties, series and tournaments inside a season. |
| `rule/stats` | Stat lines: one per subject per context (per team for players), keys from the pack, derived vs stored rates, partial books and null-skipping, exhibition exclusion, aggregating match lines into season lines honestly, leaders. |
| `rule/rosters_and_staff` | Roster and staff edges; loans, two-way deals, affiliates and representative sides; depth charts and roster lists; every move as an event: signing, release, transfer, trade, draft pick, roster move, staff change. Availability without health detail. |
| `rule/seasons_and_standings` | Season lifecycle; standings rebuilt from results (and decided_by, and pack-named match keys) onto derived team season lines, never over owner-typed tables; tiebreakers from the competition's House rules first; byes; tournaments inside a season; playoffs; awards; rolling into the next season. |
| `rule/people_and_privacy` | Creating people; no health data for anyone; children get first name and last initial only; private people are never looked up. |
| `rule/retrieval` | Answering from the record: results, rosters as of a date, head-to-head, streaks, leaders, and being straight about gaps. |
| `rule/sport_packs` | The pack contract (the eight required lore chunks and the optional Recreational, Professional and Fantasy chunks), how to add a sport, where house rules go. |
| `rule/app_fantasy` | The shared fantasy app for any sport: leagues, managers, rosters and lineups (weekly or daily), waivers and FAAB, free agents, trades and vetoes, keepers and dynasty, drafts, head-to-head matchups with fantasy points as scores, category and rotisserie standings, playoffs, honours and punishments, platform imports. Each sport pack's Fantasy chunk supplies the slots, scoring table and categories. |

### The sport packs: `packs/<sport>/world.json`

One folder per sport. A pack is two to four more rule entities. The main one, `rule/sport_<code>`, carries the sport's vocabulary in a fixed set of lore chunks (Overview, Positions, Stages and rounds, Stat keys, Standings, Roster rules, Officials, Extended keys) and then Recreational, Professional and Fantasy chunks for where the level changes the rules. The rest are that sport's workflows (a game week, a scorecard entry). Packs seed no competitions, teams or people. Each pack's rules stamp `source` as `orbismo-playbook/sports-world/<pack>@1.0`, so a pack can be versioned on its own.

| Pack | Code | Covers |
| --- | --- | --- |
| [`american-football`](packs/american-football/) | `american_football` | Weeks and byes, playoffs, the draft, reserve lists as roster designations, depth charts, a full box-score vocabulary, split records for the tiebreaker order; flag and rec variants; fantasy slots and the point-per-reception scoring family. |
| [`basketball`](packs/basketball/) | `basketball` | Quarters and halves, overtime, the play-in, best-of series, the draft, two-way deals and affiliate assignment, a full box score with derived shooting rates; pickup and rec leagues; fantasy in points, head-to-head categories and rotisserie formats with daily lineups. |
| [`baseball`](packs/baseball/) | `baseball` | Innings and line scores, extra innings and suspended games, doubleheaders, the designated hitter and two-way players, batting, pitching (outs recorded) and fielding vocabularies, roster lists and options as roster moves, games back; youth and adult rec; rotisserie and head-to-head fantasy. |
| [`ice-hockey`](packs/ice-hockey/) | `ice_hockey` | Periods, overtime and shootouts with the loser point, skater and goalie vocabularies, lines and pairs, reserve lists, affiliate stints; beer league with sub goalies; fantasy categories and points. |
| [`football-soccer`](packs/football-soccer/) | `soccer` | Grassroots to top flight: league plus cup in one season, cards and suspensions, small-sided variants, youth defaults, extra time and shootouts, two-legged ties, transfers and loans with a parent club, national-team call-ups, nine tiebreaker presets chosen per competition, modern stats as optional keys; budget-game and draft fantasy. |
| [`rugby`](packs/rugby/) | `rugby` | Union (15s, 7s) and league (13s) with a per-competition code switch, bonus points, cards and sin bins, temporary replacements without a reason, club umbrella rosters, club-and-country edges; fantasy by position. |
| [`cricket`](packs/cricket/) | `cricket` | Innings on the event, declarations and follow-ons, draws, ties and no results, rain-rule targets, super overs, the toss, margins in runs or wickets, net run rate, series as seasons, balls as integers, dismissal counts; club cricket with partial scorebooks; fantasy. |
| [`rec-softball`](packs/rec-softball/) | `softball` | Slow-pitch rec and company leagues: doubleheaders, rainouts, run rules and time limits, co-ed lineup checks without a gender field, ringers, partial books, the batting stats people actually keep; fast-pitch as notes. |
| [`volleyball`](packs/volleyball/) | `volleyball` | Sets as the score with per-set scores in periods, the international points system, set and point ratios, libero rules, beach pairs, pools and seeds per tournament, work teams officiating. |
| [`tennis`](packs/tennis/) | `tennis` | The pattern for individual sports: a person as a side, doubles pairs as teams, tournaments as competitions with editions as seasons inside a tour year, draws with seeds and entry routes, sets and games, retirements and walkovers with no reason, rankings as season lines, team ties with rubbers, club ladders and box leagues; a chunk telling pickleball, padel, badminton, table tennis and squash what to copy. |

### The league shapes: `league-shapes/*.json`

Optional, and most owners never need one. A league shape is an empty league of the right shape: a placeholder competition and placeholder teams (`Team 1 … Team N`, `Conference A / Division North`) that onboarding renames. No seasons, no people, no results. Use one when your league has a big fixed structure you would rather not type in; otherwise onboarding builds a small league from the conversation. Each league shape links to its sport's pack rule, so load it after the pack. A placeholder's slug is fixed when it is created, so a second league shape with the same placeholder names collides under `skip_existing`: use one per world, or rename the placeholders in the file before importing it.

| League shape | Shape |
| --- | --- |
| `pro-league-32-two-conferences` | 32 teams, two conferences of four divisions, league then playoffs (American football). |
| `pro-league-30-two-conferences` | 30 teams, two conferences of three divisions (basketball). |
| `pro-league-30-two-leagues` | 30 teams, two leagues of three divisions (baseball). |
| `pro-ice-hockey-league-32-two-conferences` | 32 teams, two conferences of two divisions (ice hockey). |
| `cup-group-then-knockout` | 32 teams in eight groups, then a bracket (football, any tournament sport). |
| `international-championship-6-round-robin` | Six national sides, round robin with bonus points (rugby). |
| `rec-league-10-round-robin-tournament` | Ten rec teams, round robin then a tournament; sport set at onboarding. |
| `fantasy-league-12-head-to-head` | Twelve fantasy teams in two divisions, weekly matchups; sport set at onboarding. |
| `individual-tour` | A tour year with four tournaments (tennis and other individual sports). |

## Privacy

This template is built so that it cannot become a health record:

- **No health data about any person**, professional or amateur. The AI records that a player is `unavailable`, `doubtful` or `suspended` on their roster edge (with an `availability_until` date when known), which reserve list they are on (`roster_list`), who replaced them in a match (`replaced_by`), and that a match ended by retirement or walkover (`decided_by`) — and nothing about why. Injury, illness, surgery, rehab and concussion protocols are never stored, even when published for a public athlete, even when typed in by the owner.
- **Children get the minimum.** Anyone under 18 is `is_minor: true` with first name and last initial, no birth date, no contact details, no links. The youth-facing packs (football-soccer, rec-softball, baseball, basketball) make this the default for any team with kids on it.
- **Private people are never looked up.** Rec, club, school and fantasy worlds contain private people; the AI records what the owner says and never enriches from the web. Public professionals in public competitions may have sporting facts (position, team, number, nationality) looked up when the owner asks, with the source noted.
- **No protected attributes.** Co-ed lineup rules are checked by counts and positions; no pack stores a gender field.

## Customizing

- **House rules** go in a lore chunk titled `House rules` on the competition: tiebreaker order or preset, mercy rules, away goals, fantasy scoring and roster settings, points deductions. That chunk overrides the sport pack for that competition only.
- **Stat keys**: add a key by appending it to the pack's `Extended keys` chunk with the date. The AI never invents one.
- **Shared tags**: `book-kept` and `no-book` (ice hockey, softball), `exhibition` (softball, volleyball) and `scorecard-pending` (both cricket workflows) are deliberately the same tag across packs; `lineups-tracked` is world-wide, owned by no pack.
- **Lineups** are optional per competition. The AI asks once and remembers the answer as a `lineups-tracked` tag on the competition.
- **Adding a sport**: the AI drafts a pack from the contract in `rule/sport_packs`, shows it in full, and creates it with `trust: ai-generated` once approved. The tennis pack's "Other racket sports" chunk and the volleyball pack's beach pattern are the templates for the nearest sports.
- **Instructions**: short by design (under 7 KB of the 32 KB limit); if you add a rule, add its row to the index.

## Known limits

- `get_timeline` only orders the `event` kind, which is why matches are events rather than their own kind. Seasons and stat lines are found by tag and filter, not timeline.
- Edges are not searchable by property. Standings therefore read from each team's cached season stat line and are rebuilt from match results when stale; the AI says when it rebuilt. A head-to-head query walks both sides' `COMPETED_IN` edges.
- The two-sided match model does not cover multi-competitor events. A future pack could model placings as stat lines on one event.
- Orbismo does not enforce enums. The rule lore and the "never invent a value" hard rule are the enforcement; if the AI you connect ignores the rules, wrong values are saved silently.
- The template has been validated structurally against its building blocks and reviewed sport by sport against week-one scenarios. Live exercise with a connected assistant is light so far; report what breaks.
