# Personal Companion

A life journal and personal knowledge graph. The assistant becomes a warm, curious companion that maps your life as you talk (the people, places, projects, and events that matter to you) and remembers all of it: it journals your days, surfaces connections, and answers questions like "what's coming up this weekend?" or "when did we last see Kevin?"

## What's inside

**`instructions.md`** defines the companion's role and the rules that keep the graph trustworthy: search-before-create, faithful journaling that never invents details, timeline-first date handling, a consistent tagging strategy, and a full stop if the connection to Orbismo drops.

**`world.json`** seeds the world with:

| Entity | Role |
| --- | --- |
| `rule/onboarding` | First-session interview that builds the initial graph: you, your home, your key people, what's going on in your life. Archives itself when done. |
| `rule/app_movie_tracker` | Movie diary mini app: watchlist, per-watch ratings and reviews, co-watcher history, favorites, year-in-review stats. |
| `rule/app_book_tracker` | Reading diary mini app: shelves (to-read, reading, read, DNF), progress, per-read ratings, series, authors as first-class entities, recommendations. |
| `interest/watching_movies`, `interest/reading` | Collection anchors the trackers hang their items on. |

The two trackers are bundled copies of the [Movie Tracker](../../mini-apps/movie-tracker/) and [Book Tracker](../../mini-apps/book-tracker/) packages from the [mini-app library](../../mini-apps/), included here so the template installs in one step.

## Installation

1. Create a new Orbismo world.
2. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
3. Give `world.json` to your connected AI and ask it to add its entities and relationships to the world, keeping names exactly as given.
4. Say hello. With no "Me" entity in the world yet, the onboarding workflow runs automatically and interviews you to build the starting graph.

## Customizing

- **Tone**: the role and tone live in the opening paragraph and the closing "Tone" section of `instructions.md`; adjust them freely.
- **Apps**: add or remove trackers by creating or archiving `rule` entities, then keeping the app index in `instructions.md` in sync. The two included trackers are good models for writing your own.
- **Tags**: the tagging strategy section lists suggested tags; adapt them to your own vocabulary before the graph grows.
