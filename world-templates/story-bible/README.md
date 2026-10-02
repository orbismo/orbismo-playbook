# Story Bible

A story bible for a novel, series, memoir, or other long story. The assistant becomes a writing partner that keeps your characters, worldbuilding, timeline, scenes, and decisions organized and consistent while you write, and captures everything without locking anything in before you are ready. Every creative fork has a recorded state, every in-story date lands on one chronology, and the author owns every decision.

The template is built around a stage model. The work moves through `sketchbook`, `outline`, `drafting`, and `revision`, and what counts as settled changes with it: in sketchbook everything is tentative unless you lock it, while in revision changes are deliberate retcons logged with what they invalidate.

## What's inside

**`instructions.md`** defines how the partner works: a session-start routine that orients from the spine, a reserved status vocabulary (`tentative`, `proposed`, `working`, `locked`, plus named open decision points and penciled threads), authority and conflict rules, schema and vocabulary discipline so different sessions and models leave the world consistent, time discipline so every event is visible to the timeline, a promotion test for when prose in lore should become an entity in the graph, and a capture rhythm for brainstorming that keeps up with the author instead of filing in real time. Apps are discovered by tag rather than from a hardcoded index.

**`world.json`** seeds the four core apps as `rule` entities tagged `app`, each carrying its operating rules as lore.

| App | Covers |
| --- | --- |
| Decision Log | Every creative fork and its lifecycle (open, proposed, working, locked), so nothing is silently locked and nothing is lost. Answers "what's still open?" and runs the audit that resurfaces open forks when the stage is raised. |
| Timeline | In-story events on one canonical chronology, causality chained with `LED_TO`, and the calendar contract that decides how dates are recorded. The story-time axis. |
| Character Sheet | Creating and developing characters: role, engine (want, need, obstacle, change), voice, backstory, and what they know when. Sheet depth follows story weight. |
| Scene Map | Scenes, chapters, reading order, and POV, plus the drafting-stage manuscript ingestion loop. Scenes are `event` entities carrying both a story date and a reading-order position. The discourse-time axis. |

The spine, a `rule` entity named "Story Premise (working)" and tagged `spine`, is not in the seed. It is created in your first session, because its contents (premise, stage, calendar contract, open threads, a short "State of the World" summary) are yours. Worldbuilding systems such as magic, religions, economies, and languages also live as `rule` entities, tagged `system`, and are created as the story needs them.

The template runs on Orbismo's default schema: characters are `person`, settings are `place`, factions are `group`, the manuscript's development is a `project`, and so on. The "Where things live" section of `instructions.md` maps each story concept to a type.

## Installation

1. Create a new Orbismo world.
2. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
3. Give `world.json` to your connected AI and ask it to add its entities to the world, keeping names exactly as given.
4. Say hello, or just start talking about the story. With no spine in the world yet, the assistant builds one with you: the premise and genre, the stage (sketchbook by default), any notes or drafts you already have, and whether the title is decided. If you arrive mid-idea, the ideas are the interview; it captures as you go and asks at most one orienting question later.
5. Once the spine exists, the assistant offers to replace the base instructions with a version tailored to your story. Declining costs nothing.

## Customizing

- **Tailored instructions**: the template is written to be replaced, by consent, with a version specific to your story. When you do, keep the status vocabulary, time discipline, and schema discipline sections; they are what keeps the world queryable across sessions.
- **Series**: one `project` per book, each `PART_OF` a series project, with scenes attached to their book. Locking is per book, so a fact locked by a published volume stays locked while later volumes remain fluid.
- **Memoir and autofiction**: by default the author does not appear in the world. Say so and the narrator-character is created like any other `person`.
- **Calendar**: the Timeline app owns a calendar contract on the spine. It starts as Gregorian and tentative, is never an onboarding question, and is escalated by the Timeline app only when an event will not fit. Volunteering an invented-world calendar early is welcome; later changes are logged retcons.
- **More apps**: any new `rule` tagged `app` whose description names the nouns a request would contain (scenes, chapters, POV) is picked up by routing automatically. No index edit is needed.
