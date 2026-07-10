# Movie Tracker

A personal movie diary. Movies live as items; every watch is its own event with a rating and a one-line impression, so rewatches can score differently. Tracks your watchlist, favorites, who you watched with, and where, and can answer questions like "what did I watch with Sarah?", "what have I rated highest?", or give you a year in review.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_movie_tracker` | The app itself. Four lore chunks carry the full spec: Data Model, Log a Watch, Watchlist/Favorites/Edits, Queries & Stats. |
| `interest/watching_movies` | Collection anchor. Every movie item links here so the whole collection can be queried from one hub. |

In use, the app creates `item` entities for movies (tagged `movie-tracker`) and `event` entities for watch sessions, linked to the movie, the watchers, and the place.

## Requirements

- Entity types `item`, `event`, `interest`, `person`, and `place` (Orbismo's common types).
- An owner ("Me") entity to record as the default watcher.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Movie Tracker** (`rule/app_movie_tracker`): triggers: watching a movie/film, watchlist, movie ratings/reviews, rewatch, cinema/theater. Tags: `movie-tracker`
```

3. Test it: say "add Dune to my watchlist" and confirm a movie item appears with status `watchlist`.

## Notes

- Ratings live on watch events, never on the movie item, so each watch keeps its own score.
- Comes bundled with the [Personal Companion](../../world-templates/personal-companion/) template.
