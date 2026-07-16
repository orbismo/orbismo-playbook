# Orbismo Playbook

Starter world templates and mini apps for [Orbismo](https://orbismo.com/): ready-made building blocks you can copy into your own world and adapt.

Orbismo is a world-building knowledge platform. This repository provides two kinds of starters:

- **[World templates](world-templates/)**: complete starting points for a new world. Each pairs world instructions (free-form prose that shapes how an AI assistant behaves in your world) with a seed of entities that carry the template's behaviors.
- **[Mini apps](mini-apps/)**: portable behaviors (trackers, logs, routines) you install into an existing world. Each is a `rule` entity whose operating rules live as lore chunks in the world itself, routed from your world instructions.

## Getting started

1. Starting fresh? Browse [world-templates/](world-templates/) and pick the template that matches what you want your world to do. Already have a world? Browse [mini-apps/](mini-apps/) for behaviors to add to it.
2. Follow the starter's own README. Each one explains what it does, how to install it, and what to customize.
3. Adapt freely. Starters are starting points, not prescriptions: rename things, remove what you don't need, and tune the tone to your taste.

Installing needs no tooling. World instructions (`instructions.md`) can be pasted into your world in the Orbismo web UI, or set by your connected AI. The entity seed files (`world.json`, `app.json`) are written to be handed to your connected AI: share the file and ask it to add everything in it to your world.

New to Orbismo? The [help site](https://help.orbismo.com/) covers the underlying concepts: worlds, entities, relationships, lore, and instructions.

## Templates

| Template | What it turns your world into |
| --- | --- |
| [Personal Companion](world-templates/personal-companion/) | A life journal and personal knowledge graph, with movie and book trackers included. |
| [Tabletop RPG](world-templates/tabletop-rpg/) | A persistent single-player RPG with the assistant as Game Master and the rules engine living in the world. |
| [Wedding Planner](world-templates/wedding-planner/) | A shared, calm wedding-planning workspace covering vision, vendors, guests, and budget. |
| [Event Workspace](world-templates/event-workspace/) | A one-world-per-event workspace for galas, conferences, and parties: vendors, guests, budget, run of show, deadlines. |

## Mini apps

| App | What it does |
| --- | --- |
| [Book Tracker](mini-apps/book-tracker/) | A personal reading diary: shelves, progress, per-read ratings, authors, recommendations. |
| [Family Tree](mini-apps/family-tree/) | A personal family history: kinship edges, honest dates, attributed stories, heirlooms with provenance. |
| [Movie Tracker](mini-apps/movie-tracker/) | A personal movie diary: watchlist, per-watch ratings, co-watchers, favorites, stats. |
| [Recipe Box](mini-apps/recipe-box/) | Recipes with consistent ingredients, real provenance, and meals as social moments that carry the tweaks. |
| [Second Brain](mini-apps/second-brain/) | A personal notebook with verbatim recall: notes saved exactly as written, daily notes, backlinks. |
| [Travel Tracker](mini-apps/travel-tracker/) | Trips with a lifecycle, bookings, and a compounding atlas of places, verdicts, and recommendations. |

## Contributing

Improvements and new templates are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Released under the [MIT License](LICENSE) by GhzLab, Inc.
