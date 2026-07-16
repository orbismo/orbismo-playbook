# Family Tree

A personal family history. The tree lives as real kinship edges (parents, spouses), so "how am I related to her?" is a graph walk instead of guesswork; dates stay as precise as the family actually knows them ("abt 1893" is recorded as said, never sharpened); and the stories are the point — captured faithfully, attributed to whoever told them, with an interview mode for sitting down with a relative while the stories can still be asked for. Heirlooms carry their provenance chain, and the family shoebox of documents attaches as links.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_family_tree` | The app itself. Five lore chunks carry the full spec: Data Model: People & Kinship; Data Model: Milestones, Heirlooms, Places & Sources; Workflow: Build the Tree; Workflow: Capture a Story (Oral History); Workflow: Queries & Rendering the Tree. |
| `interest/family_history` | App hub. Its lore holds the tree-wide "Open Threads" list of unknowns; relatives themselves are found by the `family-tree` tag. |

In use, the app creates (or reuses) `person` entities for relatives (tagged `family-tree`; deceased forebears also `ancestor`), `event` entities for storied milestones like weddings and immigrations, `item` entities for heirlooms, `reference` entities for documents, and `place` entities for the places the family keeps returning to.

## Requirements

- Entity types `person`, `event`, `place`, `item`, `reference`, and `interest` (Orbismo's common types).
- An owner ("Me") entity — the tree is walked from here, so "my grandfather" resolves through Me.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Family Tree** (`rule/app_family_tree`): triggers: ancestors and earlier generations, the family tree, how relatives are related, maiden names, where the family came from, immigration and family stories/legends, interviewing a relative, heirlooms. Tags: `family-tree`
```

3. Test it: say "my great-grandfather Peter was born in Bavaria around 1868" and confirm a person appears with `birth_date: "abt 1868"`, `birth_year: 1868`, and tags `family-tree` and `ancestor`.

## Notes

- Kinship is stored only as PARENT_OF / SPOUSE_OF / PARTNER_OF edges (SIBLING_OF only when the shared parents are unknown); grandparents, aunts, and cousins are always derived by walking the tree, so the graph can't drift out of sync with itself.
- Dates keep the family's honest uncertainty: qualified strings ("abt 1893", "bef 1901") in `birth_date`/`death_date`, with numeric `birth_year`/`death_year` alongside for sorting. Precision is never invented — that rule extends to names and places, and unknowns land on the anchor's "Open Threads" list instead of being papered over.
- Stories are one-per-lore-chunk, attributed ("— as told by Ruth, June 2026"), and legends stay labeled as legends. Two tellings that differ are both kept.
- Living relatives already in your world are reused and woven in, not duplicated. Deceased forebears carry the `ancestor` tag so they stay out of everyday name disambiguation, the way the Book Tracker keeps authors out of your contacts.
- Pairs naturally with the [Personal Companion](../../world-templates/personal-companion/) template, and with [Recipe Box](../recipe-box/): the relative a family recipe came from is the same person entity as her place in this tree.
