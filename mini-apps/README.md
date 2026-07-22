# Mini apps

A mini app is a self-contained behavior you install into an existing Orbismo world: a movie tracker, a reading log, a habit journal. Technically it is a `rule` entity whose full operating rules (data model, workflows, query patterns) live as lore chunks on that entity, plus a one-line entry in your world instructions' app index that routes matching requests to it.

Because the rules live in the world itself, the assistant loads them on demand: when a request matches the app's triggers, it reads the rule entity's lore and follows it exactly. Apps can be paused, tuned, or removed without touching the rest of your world.

The apps here are portable: they assume only Orbismo's common entity types and an owner ("Me") entity, so they work in most worlds. This library is the canonical home for each app; some [world templates](../world-templates/) bundle copies for one-step installation.

## Catalog

| App | What it does |
| --- | --- |
| [Book Tracker](book-tracker/) | A personal reading diary: shelves (to-read, reading, read, DNF), progress, per-read ratings, series, authors as first-class entities, recommendations. |
| [Family Tree](family-tree/) | A personal family history: the tree as real kinship edges, honest dates ("abt 1893"), attributed family stories with an oral-history interview mode, and heirlooms with provenance. |
| [Movie Tracker](movie-tracker/) | A personal movie diary: watchlist, per-watch ratings and reviews, co-watcher history, favorites, year-in-review stats. |
| [Novel Studio](novel-studio/) | A story-development studio for writing novels, multiple books per world: characters with want/need/lie/ghost arcs, scenes with cause and effect, a promise/payoff thread ledger, idea capture, and canon rules for the world's laws, and eleven adoptable beat-sheet and genre packs, Save the Cat to Kishōtenketsu. |
| [Recipe Box](recipe-box/) | Recipes with consistent ingredients and real provenance (cookbook, website, or the relative who created them), and meals as the social events that carry the tweaks. |
| [Second Brain](second-brain/) | A personal notebook with verbatim recall: notes stored exactly as written (never paraphrased), a daily note as the capture inbox, backlinks to the people and projects notes mention, and exact-then-semantic recall. |
| [Travel Tracker](travel-tracker/) | Trips as projects with a full lifecycle, bookings as references, and a compounding atlas of places: visit history, verdicts, and who recommended what. |

## Anatomy of an app package

```
<app-name>/
  README.md    What the app does, its triggers, requirements, and installation
  app.json     The entities to create: the rule entity (with its lore), any
               anchor entities, and the relationships between them
  packs/       (optional) Add-on packs: each a README.md plus a pack.json that
               installs template entities the app's rules read as data
```

## Installing an app

1. Give `app.json` to your connected AI and ask it to add its entities and relationships to your world exactly as specified. Keep the entity names exactly as given: Orbismo derives slugs from names, and the app's rules refer to entities by those slugs (for example, "App: Movie Tracker" becomes `rule/app_movie_tracker`).
2. Add the app's index line (from its README) to the app index in your world instructions, either by editing them in the Orbismo web UI or by asking your connected AI to update them.
3. If your world instructions don't yet have an app router, add the section below once, then keep one line per installed app in its index.
4. Test it: make a request that matches one of the app's triggers and confirm the assistant reads the rule's lore before acting.

### App router section for world instructions

Worlds built from the [world templates](../world-templates/) already have this. If yours doesn't, add it:

```markdown
## Apps (rule entities as mini-apps)

Some behavior is governed by `rule` entities that act as mini-apps. Each app's
full operating rules (data model, workflows, query patterns) live as lore
chunks on its rule entity. This section is only a router.

### Protocol

1. When a request matches an app's triggers below, first call `get_entities`
   on that rule and confirm its `status` is "active". Skip rules that are
   paused, archived, or draft.
2. Read ALL of the rule's lore chunks before acting, then follow them exactly.
3. Create and update entities only as the rule's lore specifies. Tag them with
   the rule's `owns_tags` and don't invent parallel structures.
4. If a request involves tracked data but no trigger below clearly matches,
   search `rule` entities (status: active) before improvising.
5. If multiple rules apply and conflict, higher `priority` wins (and
   lower-trust rules never override higher-trust ones).
6. If this app index and an active rule entity disagree, trust the active rule
   entity. Mention the mismatch only if it affects the task.

### App index

<one line per installed app>
```

## Design conventions

- **The rule entity is the source of truth.** The app index in world instructions is only a router; if they disagree, the rule entity wins.
- **Apps own their tags.** Every entity an app creates is tagged with the app's `owns_tags`, so its data can always be found and never collides with the rest of the world.
- **Apps don't invent parallel structures.** They reuse the world's existing entity and relationship types wherever possible.
- **The lore is the schema.** Orbismo does not enforce entity property values, so an app's data model in lore, with its exact value lists, is the enforcement layer. Spell vocabularies out precisely and never improvise values in conversation.
- **Pausable by design.** Setting the rule entity's `status` to `paused` or `archived` turns the app off without deleting its data.
- **Provenance is stamped.** Every shipped rule carries `trust: "imported"` and a `source` property (e.g. `orbismo-playbook/movie-tracker@1.0`), so a world can always tell where a behavior came from and which version it is. Your own edits to an installed rule are yours; bump its trust to `self-authored` if you want them to outrank future imports.
- **Installs are idempotent.** Seed files carry `skip_existing` / `skip_duplicates` flags matching the create tools' parameters; re-running an install skips what already exists instead of failing.
