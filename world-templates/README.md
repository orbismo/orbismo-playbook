# World templates

A world template is a complete starting point for an Orbismo world. Each one pairs two files:

- **`instructions.md`**: the world instructions. Creator-authored prose that defines the assistant's role, tone, hard rules, and how it uses the world's tools.
- **`world.json`**: a seed export of entities to create in the world. Most of these are `rule` entities (the template's mini apps and workflows) whose full operating rules live as lore chunks on the entity itself. The instructions route to them; the entities carry the detail.

This split is deliberate: the instructions stay short and stable, while behaviors live in the world where they can be inspected, tuned, paused, or extended without rewriting the prompt.

Portable apps a template bundles (ones that work in any world, like the movie and book trackers) have their canonical home in the [mini-app library](../mini-apps/), where they can also be installed on their own. Behaviors coupled to a template's own schema, like the wedding apps or the RPG rules engine, live only in the template.

## Catalog

| Template | What it turns your world into |
| --- | --- |
| [Personal Companion](personal-companion/) | A life journal and personal knowledge graph (people, places, events, memories) with a warm companion that remembers everything. Ships with Movie Tracker and Book Tracker mini apps. |
| [Tabletop RPG](tabletop-rpg/) | A persistent single-player tabletop RPG with the assistant as Game Master. A full rules engine (dice resolution, character progression, NPC disposition, quests and consequences, live saves) lives in the world as rule entities. |
| [Wedding Planner](wedding-planner/) | A shared wedding-planning workspace with a calm, jargon-free companion. Four mini apps cover the big picture, vendors and venues, the guest list, and the budget. |
| [Event Workspace](event-workspace/) | A one-world-per-event planning workspace for galas, conferences, and parties, shareable with that event's client and staff. Six mini apps cover the brief, vendors, guests, budget, run of show, and deadlines. |

## Installing a template

1. Create a new Orbismo world (or pick an empty one).
2. Set the template's `instructions.md` as the world's instructions. Two ways to do it: paste it into the world's instructions in the Orbismo web UI, or share the file with your connected AI and ask it to update the world instructions.
3. Give `world.json` to your connected AI and ask it to add everything in it to the world. Each entry's `entity_type`, `slug`, `data`, and `lore` describe exactly what to create, and the `relationships` block lists the links between them. Keep entity names exactly as given: slugs derive from names.
4. Start a conversation. Every template includes a first-session or onboarding workflow that takes the world from empty to working. Just say hello and follow its lead.

Each template's own README covers what it assumes and what to customize.

## Conventions

All templates in this catalog follow the same design rules:

- **Instructions are a router.** Behaviors that can live in the world (as `rule` entities with lore) do; the instructions carry only an app index and the rules that must always hold.
- **Rule entities are the source of truth.** If the instructions and an active rule entity disagree, the rule entity wins.
- **Apps own their tags.** Everything an app creates carries the app's `owns_tags`, so its data is always retrievable and never collides with the rest of the world.
- **Search before create.** Every template makes duplicate prevention a hard rule.
- **The lore is the schema.** Orbismo does not enforce entity property values; a wrong status or invented category is saved silently. The exact value lists in each rule's lore, and the "never invent a value" hard rule, are the enforcement layer, so templates spell vocabularies out precisely.
- **Pausable by design.** Setting a rule entity's `status` to `paused` or `archived` switches that behavior off without losing data.
- **Provenance is stamped.** Shipped rules carry a `source` property (e.g. `orbismo-playbook/event-workspace@1.0`) and an honest `trust` level: `imported` for playbook content, except the Tabletop RPG's rules, which ship `self-authored` because its trust-precedence mechanics depend on it.
- **Installs are idempotent.** Seed files carry `skip_existing` / `skip_duplicates` flags matching the create tools' parameters; re-running an install skips what already exists instead of failing.
