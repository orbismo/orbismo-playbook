# Book Tracker

A personal reading diary. Books live as items with shelves (to-read, reading, read, DNF) and progress while you read; finishing a book creates a read-through event with a rating and impression, so rereads can score differently. Authors are first-class entities linked to their books, which makes "what have I read by Le Guin?" a graph query instead of a string match, and taste-based recommendations follow from your high-rated reads, favorite authors, and even your DNF patterns.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_book_tracker` | The app itself. Four lore chunks carry the full spec: Data Model, Reading Lifecycle, Read List/Favorites/Edits, Queries/Stats/Recommendations. |
| `interest/reading` | Collection anchor. Every book item links here so the whole collection can be queried from one hub. |

In use, the app creates `item` entities for books (tagged `book-tracker`), `person` entities for authors (tagged `author`, kept distinct from personal contacts), and `event` entities for completed read-throughs.

## Requirements

- Entity types `item`, `event`, `interest`, `person`, and `reference` (Orbismo's common types).
- An owner ("Me") entity to record as the default reader.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Book Tracker** (`rule/app_book_tracker`): triggers: reading/starting/finishing a book, reading list / to-read pile, book ratings/reviews, reread, audiobooks, series, book recommendations. Tags: `book-tracker`
```

3. Test it: say "add The Left Hand of Darkness to my reading list" and confirm a book item appears with status `to_read` and a linked author.

## Notes

- Ratings live on read-through events, never on the book item, so each read keeps its own score. A read-through is created on finishing, not per sitting; progress lives on the item while reading.
- A DNF is recorded without rating pressure; it feeds recommendations as taste signal.
- Comes bundled with the [Personal Companion](../../world-templates/personal-companion/) template.
