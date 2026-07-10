# Recipe Box

A recipe box that remembers the table, not just the food. Recipes are saved with a consistent ingredient list and honest provenance: the cookbook they came from (linked as a real book), the website (linked as a reference), or the relative whose recipe it is (a real link to that person). Cooking gets recorded as **meals**: dated social events that capture who was at the table, what was served, and what you changed this time. Tweaks that stick get promoted into the recipe's "Our Version", so the box always reflects how your family actually makes it.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_recipe_box` | The app itself. Five lore chunks carry the full spec: Data Model: Recipes, Data Model: Meals, Workflow: Capture a Recipe, Workflow: Log a Meal, Workflow: Queries & What to Make. |
| `interest/cooking` | Collection anchor. Every recipe links here so the whole box can be queried from one hub. |

In use, the app creates `item` entities for recipes (with "Ingredients" and "Instructions" lore in one exact format), `event` entities for meals (attendees, place, occasion, tweak notes), plus cookbook items, website references, and CREATED_BY links to relatives as provenance demands. Ingredients are deliberately **not** entities; they live in lore, where `search_lore` can still find "the recipe with saffron".

## Facet tags

`family-recipe`, `favorite`, `eat-again`, `not-again`, `quick` (a known total time of 30 minutes or less), `make-ahead`, `crowd-pleaser`, `holiday`. Exact spellings, case-sensitive, at most one of the two verdict tags per recipe, revised freely as tastes change.

## Requirements

- Entity types `item`, `event`, `person`, `place`, `reference`, and `interest` (Orbismo's common types).
- An owner ("Me") entity to record at the table by default.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Recipe Box** (`rule/app_recipe_box`): triggers: recipes, ingredients, cookbooks, cooking or baking something, meals and dinners together, what to make or cook, family dishes, logging that we cooked or ate. Tags: `recipe-box`
```

3. Test it: say "save my grandma's dinner roll recipe" and confirm a recipe appears with an Ingredients lore chunk and a CREATED_BY link once you name her.

## Notes

- For cookbook and website recipes, the app records the ingredients and *your* changes and points to the page or link for the method, rather than transcribing published text.
- Meals can serve several recipes, belong to a bigger occasion ("Thanksgiving Dinner" as part of "Thanksgiving 2026"), and are worth logging even without a saved recipe when the moment matters.
- Pairs naturally with the [Book Tracker](../book-tracker/) (a cookbook is the same book item in both apps) and the [Personal Companion](../../world-templates/personal-companion/) template.
