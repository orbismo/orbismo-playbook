# Novel Studio

A story-development studio for writing novels — as many of them as one world can hold. Each novel is a project; its characters (with want/need/lie/ghost arcs), scenes (with cause and effect), plot threads (a promise/payoff ledger, so nothing planted is ever silently dropped), and loose ideas all hang off it. Writing techniques plug in as **packs**: adoptable beat sheets (Save the Cat, Hero's Journey) and genre packs (Mystery, Romance) that are pure data — the studio reads them; they never add rules of their own.

The studio tracks the story, not the manuscript: scenes are summaries and story facts, and your prose stays in whatever you write with. What you get in return is a story bible that answers questions — "what's still unresolved going into act 3?", "which beats have no scene yet?", "did I ever explain the locket?" — instead of making you re-read your own book.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_novel_studio` | The app itself. Eight lore chunks carry the full spec: Data Model: Novels & the Shelf; Characters; Scenes; Threads & Ideas; Packs; Workflow: Develop the Story; Plot Scenes & Threads; Review & Continuity. |
| `interest/novel_studio` | App hub. Its lore holds "The Shelf" — the one-line-per-novel catalog of the world's books (bootstrapped on the first novel; ships empty). |

In use, the app creates `project` entities for novels, `person` entities for characters, `event` entities for scenes, and `reference` entities for threads and ideas — all tagged `novel-studio`, each carrying a `novel` property naming its book — for characters, the book that introduced them; studio-wide ideas excepted — so several novels partition cleanly in one world.

## Packs

A pack is a small seed file that installs a `reference` entity carrying a technique as data. Novels adopt packs individually (recorded in the novel's Blueprint), so a mystery on Save the Cat and a freeform literary novel coexist happily.

| Pack | Type | What it adds |
| --- | --- | --- |
| [Save the Cat](packs/save-the-cat/) | Beat sheet | Blake Snyder's 15 beats, from Opening Image to Final Image. |
| [Hero's Journey](packs/heros-journey/) | Beat sheet | The 12-stage cycle, from Ordinary World to Return with the Elixir. |
| [Mystery](packs/mystery/) | Genre | Clue, red-herring, alibi, and secret threads; fair-play and reveal-integrity review lenses. |
| [Romance](packs/romance/) | Genre | Spark, wound, and rift threads; dual-POV balance and earned-ending review lenses. |

Packs are optional and installable any time; the core never depends on one. You can author your own by creating a `reference` entity in the same shape (see the app's "Data Model: Packs" lore).

## Requirements

- Entity types `project`, `person`, `event`, `reference`, and `interest` (Orbismo's common types).
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. For each pack you want, give its `pack.json` to your connected AI the same way (packs can also be added later).
3. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Novel Studio** (`rule/app_novel_studio`): triggers: start or develop a novel, create or interview a character, plot or log a scene, track plot threads and foreshadowing, story ideas, beat sheets and structure, review continuity or plot holes. Tags: `novel-studio`
```

4. Test it: say "start a novel called Test Flight — a lighthouse keeper finds a door in the sea" and confirm the assistant reads the rule's lore, creates the project with your logline verbatim, and starts The Shelf. Then add a scene and confirm it gets a sequence number and a WHAT CHANGES section.

## Notes

- **Canon is chosen, not assumed.** The assistant brainstorms freely — names, twists, what-ifs — but stores only what you approve. Anything stored is canon, and changing it later is a logged retcon, never silent drift.
- **The thread ledger is the heart.** Every promise to the reader — a mystery opened, a gun on the mantel — becomes a thread with `planted_in` and `resolved_in`. Open threads are a first-class state; the review workflow makes sure each one ends on purpose, even two books later.
- **Scenes carry cause and effect.** Proactive scenes record Goal/Conflict/Setback, reactive ones Reaction/Dilemma/Decision, and every scene ends with what changed. The review walks the chain and flags breaks.
- **Structure is opt-in.** No Blueprint means freeform — the structure lens simply skips. Pantsers get the bible without the beat sheet.
- **Deliberately not a drafting tool**: no word counts, no chapter files, no writing schedule. That keeps it beside Scrivener or a plain text editor, not in competition with them.
- Pairs naturally with the other apps: research clippings live as [Second Brain](../second-brain/) notes that a novel's entities can reference.
