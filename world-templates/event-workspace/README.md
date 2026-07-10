# Event Workspace

A complete planning workspace for one event: a gala, a conference, a corporate offsite, a fundraiser, a big party. The assistant becomes a calm, organized planning companion that tracks the vendors, the guest list, the budget, the minute-by-minute schedule, and every deadline, and keeps each fact in exactly one place so nothing drifts as plans change.

**One world per event, by design.** Orbismo grants access per world (read/write or read-only), so the world boundary is the privacy boundary: you can invite this event's client, staff, and helpers without exposing any other client's budget, guest list, or vendor pricing. When the next event comes, start a fresh world from this template; when the event wraps, this world becomes its archive.

## What's inside

**`instructions.md`** defines the companion's voice (professional, warm, jargon-free) and the hard rules that keep the data trustworthy: search-before-create, exact status vocabularies, a single home for each fact, headcount counted from attendance edges, and confirm-before-bulk-creates.

**`world.json`** seeds six mini apps as `rule` entities, each carrying its full data model and workflows as lore.

| App | Covers |
| --- | --- |
| Event Brief | The event itself: working name, category, date, client (person or organization), coordinator, headcount target, formality, vision. Includes the first-session onboarding interview. |
| Vendors & Venues | Every vendor as a business plus an engagement with a lifecycle status (discovered through booked, deposited, paid, or passed). Contacts, quotes as structured lore, tours and meetings. |
| Guest List | Guests with list segments (VIP, speaker, sponsor, press, staff, general), RSVP lifecycle, meal choices, plus-ones as real people, headcount from attendance edges. |
| Budget | Budget lines as the single home for money: expenses vs. contributions (sponsorships offset, never inflate spend), payment statuses, payers as links, live-computed totals. |
| Run of Show | The minute-by-minute schedule: timed blocks with owners and locations, chronological run sheets straight off the timeline, conflict and gap review. |
| Tasks & Milestones | Deadlines dated as events: open/done/blocked statuses, owners, vendor links, what's-due and overdue queries. |

## Installation

1. Create a new Orbismo world.
2. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
3. Give `world.json` to your connected AI and ask it to add its entities and relationships to the world, keeping names exactly as given (slugs derive from names).
4. Say hello. With no event project in the world yet, the onboarding interview runs automatically: the event, the client, the date, the rough headcount.
5. Share the world with the client and anyone else who should see or edit it (read/write or read-only).

## Customizing

- **Vocabulary**: vendor categories, guest segments, and budget categories are exact lists in each app's rule lore; extend them there rather than improvising values in conversation.
- **Apps are independent.** A small party may not need Run of Show; a conference may not need meal choices. Set an unused rule's `status` to `paused` and remove its line from the app index.
- **Weddings**: this template deliberately has no household/envelope model; the [Wedding Planner](../wedding-planner/) template covers household-unit invitations and wedding-specific onboarding, and remains the better fit for weddings.
- **Multi-day events**: set `event_end_date` on the project and qualify schedule block names per day ("Doors Open (Day 2)").
