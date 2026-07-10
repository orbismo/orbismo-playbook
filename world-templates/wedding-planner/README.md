# Wedding Planner

A shared wedding-planning workspace with a calm companion that remembers everything and never talks like a database. It tracks the vision, the vendors, the guest list, and the budget, and keeps each fact in exactly one place so nothing drifts out of sync as plans change.

This world is designed to be shared: both partners can work in it, and others (a parent, a planner, a friend) can be given read-only access. The assistant never assumes who is speaking, and never assumes what a wedding "should" look like: no assumptions about budget, family structure, aesthetic, gender, or tradition.

## What's inside

**`instructions.md`** defines the companion's voice (warm, jargon-free, one gentle question at a time) and the hard rules that keep the data trustworthy: search-before-create, exact status vocabularies, a single home for each fact, and confirm-before-bulk-creates.

**`world.json`** seeds four mini apps as `rule` entities, each carrying its full data model and workflows as lore.

| App | Covers |
| --- | --- |
| Big Picture | The couple, the date, the vision and style, guest count, ceremony, plus the onboarding interview for the first session. |
| Vendors & Venues | Every vendor as a business plus an engagement with a lifecycle status (researching, quoted, booked, passed). Contacts, meetings, comparisons. |
| Guest List | Guests, households, invitations, RSVPs, meal choices, plus-ones, addresses, seating. |
| Budget | Budget lines linked to vendor engagements: deposits, payments, balances, and who's contributing. |

## Installation

1. Create a new Orbismo world.
2. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
3. Give `world.json` to your connected AI and ask it to add its entities and relationships to the world, keeping names exactly as given.
4. Say hello. With no wedding project in the world yet, the onboarding interview runs automatically (the couple, the date or "not set yet", and the broad strokes) before any tracking begins.
5. To plan together, share the world with your partner and anyone else who should see it.

## Customizing

- **Vocabulary**: the status and category lists live in each app's rule lore; extend them there rather than improvising values in conversation.
- **Apps**: the four apps are independent. Don't need budget tracking? Set that rule's `status` to `paused` and remove its line from the app index.
- **Beyond weddings**: the same structure (a central project, vendor engagements, a guest list, budget lines) adapts well to any large event; rename the wedding project and adjust the onboarding lore.
