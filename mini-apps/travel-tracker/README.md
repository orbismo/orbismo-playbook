# Travel Tracker

A travel memory that compounds. Trips are projects with a real lifecycle (dreaming, planning, booked, underway, done), bookings attach as references the moment they exist, and every journey feeds a growing atlas: places with visit history, one-line verdicts ("go back", "skip"), the dish worth ordering, and who recommended it. The next trip starts from everything the last one learned.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_travel_tracker` | The app itself. Five lore chunks carry the full spec: Data Model: Trips & Bookings, Data Model: The Atlas, Workflow: Trip Lifecycle & Closeout, Workflow: Log a Visit, Workflow: Queries. |
| `interest/traveling` | Collection anchor. Every trip links here so the full travel history can be queried from one hub. |

In use, the app creates `project` entities for trips, `reference` entities for bookings, `event` entities for dated visits, and maintains `place` entities as the atlas: cities containing their venues, `last_visited` and `visit_count` kept fresh, verdict tags (`verdict:return` / `verdict:fine` / `verdict:skip`), and recommendation credits as lore plus REFERRED links.

## Requirements

- Entity types `project`, `place`, `event`, `person`, `reference`, and `interest` (Orbismo's common types); `item` only if you want gear debriefs.
- An owner ("Me") entity to record as the default traveler.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Travel Tracker** (`rule/app_travel_tracker`): triggers: trips (planned, booked, underway, wrapping up), destinations, flights/hotels/bookings, places visited while traveling, restaurant and stay experiences, travel recommendations, trip recaps and verdicts. Tags: `travel-tracker`
```

3. Test it: say "we're thinking about Portugal next spring" and confirm a trip appears at `trip_status: "dreaming"`, linked to the Traveling anchor.

## Notes

- The trip lifecycle lives in `trip_status` on the trip, mapped onto the built-in project `status` (completed only when the trip is done). It is never encoded as tags.
- Repeat visits are dated events, so they never collide; the VISITED edge upgrades to FREQUENTS on the third visit by an exact procedure that respects Orbismo's one-active-edge rule.
- Verdicts are deliberately blunt: one of three tags per place, revised trip over trip, plus one line of why in the place's lore.
- Pairs naturally with the [Personal Companion](../../world-templates/personal-companion/) template and its movie and book trackers.
