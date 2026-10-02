# World templates

A world template is a complete starting point for an Orbismo world. Each one pairs two files, with an optional third:

- **`instructions.md`**: the world instructions. Creator-authored prose that defines the assistant's role, tone, hard rules, and how it uses the world's tools.
- **`world.json`**: a seed export of entities to create in the world. Most of these are `rule` entities (the template's mini apps and workflows) whose full operating rules live as lore chunks on the entity itself. The instructions route to them; the entities carry the detail.
- **`building-blocks.json`** (optional): the world's structure, matching what the Orbismo portal calls Building Blocks. Most templates run on Orbismo's default schema and omit this file. A template that defines its own kinds and connections ships one, and its README covers setting them up.

This split is deliberate: the instructions stay short and stable, while behaviors live in the world where they can be inspected, tuned, paused, or extended without rewriting the prompt.

Portable apps a template bundles (ones that work in any world, like the movie and book trackers) have their canonical home in the [mini-app library](../mini-apps/), where they can also be installed on their own. Behaviors coupled to a template's own schema, like the wedding apps or the RPG rules engine, live only in the template.

## Catalog

| Template | What it turns your world into |
| --- | --- |
| [Personal Companion](personal-companion/) | A life journal and personal knowledge graph (people, places, events, memories) with a warm companion that remembers everything. Ships with Movie Tracker and Book Tracker mini apps. |
| [Story Bible](story-bible/) | For anyone writing a novel, series, memoir, or other long story. It keeps your characters, timeline, and decisions organized and consistent while you write. |
| [Tabletop RPG](tabletop-rpg/) | A persistent single-player tabletop RPG with the assistant as Game Master. A full rules engine (dice resolution, character progression, NPC disposition, quests and consequences, live saves) lives in the world as rule entities. |
| [Wedding Planner](wedding-planner/) | A shared wedding-planning workspace with a calm, jargon-free companion. Four mini apps cover the big picture, vendors and venues, the guest list, and the budget. |
| [Commonplace Book](commonplace-book/) | A personal commonplace book for ideas, quotes, observations, questions, and links, captured fast in your own words and tied to their sources. Ships its own Building Blocks (four kinds, four connections) instead of the default schema. |
| [Event Workspace](event-workspace/) | A one-world-per-event planning workspace for galas, conferences, and parties, shareable with that event's client and staff. Six mini apps cover the brief, vendors, guests, budget, run of show, and deadlines. |
| [Agent Team Workspace](agent-team-workspace/) | A shared workspace where several AI agents, from any vendor, work on one goal you set: they divide it up, keep each other posted, and check each other's work. Ships its own Building Blocks (six kinds, six connections) and a leaderless coordination protocol as its instructions, and is also offered as a built-in starting template on Standard plans and up. |

## Installing a template

1. Create a new Orbismo world (or pick an empty one). If the template ships a `building-blocks.json`, set up its kinds and connections first under World Settings → Building Blocks; a connected AI reads the world's structure once per session, so this comes before any conversation.
2. Set the template's `instructions.md` as the world's instructions. Two ways to do it: paste it into the world's instructions in the Orbismo web UI, or share the file with your connected AI and ask it to update the world instructions.
3. Give `world.json` to your connected AI and ask it to add everything in it to the world. Each entry's `entity_type`, `slug`, `data`, and `lore` describe exactly what to create, and the `relationships` block lists the links between them. Keep entity names exactly as given: slugs derive from names.
4. Start a conversation. Most templates include a first-session or onboarding workflow that takes the world from empty to working; just say hello and follow its lead. The Commonplace Book has none, and the first thing you share becomes the first note. The Agent Team Workspace has the first agent you sit with ask you for the master objective, and the rest of the team starts from that.

Each template's own README covers what it assumes and what to customize.

## Conventions

All templates in this catalog follow the same design rules:

- **Instructions are a router.** Behaviors that can live in the world (as `rule` entities with lore) do; the instructions carry only an app index and the rules that must always hold.
- **Rule entities are the source of truth.** If the instructions and an active rule entity disagree, the rule entity wins.
- **Apps own their tags.** Everything an app creates carries the app's `owns_tags`, so its data is always retrievable and never collides with the rest of the world.
- **Search before create.** Every template makes duplicate prevention a hard rule.
- **The lore is the schema.** Orbismo does not enforce entity property values; a wrong status or invented category is saved silently. The exact value lists in each rule's lore, and the "never invent a value" hard rule, are the enforcement layer, so templates spell vocabularies out precisely.
- **Pausable by design.** Setting a rule entity's `status` to `paused` or `archived` switches that behavior off without losing data.
- **Provenance is stamped.** Shipped rules carry a `source` property (e.g. `orbismo-playbook/event-workspace@1.0`) and an honest `trust` level: `imported` for playbook content, except the Tabletop RPG's rules, which ship `self-authored` because its trust-precedence mechanics depend on it. The Agent Team Workspace's rules are Orbismo's own starter content for that template, carried here unchanged, so they stamp `trust` only.
- **Installs are idempotent.** Seed files carry `skip_existing` / `skip_duplicates` flags matching the create tools' parameters; re-running an install skips what already exists instead of failing.
