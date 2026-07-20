# How-To Library

A library of how you do things. Every procedure you'd otherwise re-derive — winterizing the house, onboarding a volunteer, restoring the database, Grandma's canning routine — becomes a guide: captured once, walked through step by step on demand, and improved every time it runs. Guides link to the people, places, projects, and tools they involve, so "how do we do X?" resolves through your world's graph, and each guide carries its own change log, so you can always see how the procedure evolved and why.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_how_to_library` | The app itself. Five lore chunks carry the full spec: Data Model: Guides; Data Model: The Guide Index & Links; Workflow: Capture a Guide; Workflow: Run a Guide; Workflow: Revise & Review. |
| `interest/how_to_library` | App hub. Its lore holds the "Guide Index" — the one-line-per-guide catalog of the library; the guides themselves are found by the `how-to` tag. |

In use, the app creates `reference` entities for guides (tagged `how-to`), each with a fixed lore layout — Overview, Steps, and a dated Log — and links each guide from the tracked entities it involves.

## Requirements

- Entity types `reference` and `interest` (Orbismo's common types).
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **How-To Library** (`rule/app_how_to_library`): triggers: how do I / how do we do something, walk me through a task, document or save a procedure's steps, make a how-to, update a guide after doing it differently, review the how-to library. Tags: `how-to`
```

3. Test it: describe a small three-step procedure and say "save this as a how-to called Coffee Ritual", then ask "walk me through Coffee Ritual" and confirm the assistant reads the guide's lore and gives you step 1 alone — not the whole list, and not steps from memory.

## Notes

- Guides are structured, not verbatim: unlike a [Second Brain](../second-brain/) note, a guide is the assistant's organized write-up of your procedure. It may reorder and format freely, but it never invents a step, tool, or value you didn't give — unknowns stay visibly marked until you fill them in.
- Running a guide is where it improves: the assistant walks you through one step at a time, notes where reality diverged from the doc, and offers to fold the changes back in afterwards. Revisions are recorded in the guide's Log with the old way noted, never silently erased.
- The Log stays quiet: only notable runs (a deviation, a failure, an update) get a log entry — routine walkthroughs don't clutter the history.
- Deliberately not a to-do app: guides document *how*, never *whether* — no due dates, no recurrence, no completion state. That keeps it compatible with any scheduling or habit system you add later.
- Works for both halves of life: "Winterize the House" linked to your home and "New Volunteer Onboarding" linked to a project and its people live comfortably in the same library.
- Pairs naturally with the other apps: a how-to's rough source material can live as a [Second Brain](../second-brain/) note it cites, and a cooking procedure can reference the [Recipe Box](../recipe-box/) recipe it serves.
