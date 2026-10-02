# Commonplace Book

A personal commonplace book: ideas, quotes, observations, questions, links, and the occasional person. The assistant becomes a fast, faithful note-taker that saves what you say in your own words, ties quotes to their sources, and answers "what have I got on X" by searching everything you have kept. Capture comes first and tidiness later, and nothing is rewritten, merged, or deleted without your say-so.

Unlike the other templates, this one does not run on Orbismo's default schema. It ships its own small set of kinds and connections in `building-blocks.json`, so installing it has one extra step.

## What's inside

**`building-blocks.json`** defines the world's structure, the part of a world the Orbismo portal calls Building Blocks (World Settings → Building Blocks). Four kinds and four connections:

| Kind | Records |
| --- | --- |
| `note` | The core unit: one idea per note, named with a short handle in the owner's words. `kind` is one of idea, quote, observation, question, or link; `status` is one of raw, developing, used, or archived; link notes keep their address in `url`. The owner's exact text lives in a lore chunk titled `Text`. |
| `source` | Where something came from: a book, article, podcast, site, talk, or conversation. `author`, `medium`, `url`. Created only when the owner names one. |
| `person` | Someone the owner mentions, kept light: `context` (who they are to the owner) and `where_met`. Not a contact database. |
| `rule` | Standing guidance for the assistant. `status` is one of active, draft, or archived; only active rules apply. |

| Connection | Links |
| --- | --- |
| `FROM` | note → source. A quote and the book it came from. |
| `MENTIONS` | note or source → person. |
| `BUILDS_ON` | note → note, where one idea grew out of another. |
| `RELATES_TO` | note ↔ note, a loose association. Suggested by the assistant, created only on the owner's yes. |

Topics are tags, not entities, so the structure stays this small no matter how the collection grows.

**`instructions.md`** defines the assistant's role and the defaults that always hold: keep the owner's wording, capture first and ask one question after, search for duplicates before creating, archive instead of delete, and never research a private person. The assistant's own commentary goes in a separate lore chunk titled `Agent note`, never mixed into the owner's text. A rule index routes situations to the rules below and asks the assistant to report index drift rather than fix it silently.

**`world.json`** seeds five `rule` entities, each carrying its guidance as lore:

| Rule | Covers |
| --- | --- |
| `rule/capture` | Capturing anything new: one note per idea, guessing the kind from the shape of what was said, and offering a merge when a near-duplicate turns up. |
| `rule/people` | When a passing mention becomes a `person`, filling it only from what the owner says, and what never gets recorded. |
| `rule/quoting_sources` | Verbatim quotes, one `source` per work, and how conversations and talks become sources. |
| `rule/retrieval` | Answering "what have I got on X": search names, tags, lore, and sources, answer in the owner's words, and offer connections as suggestions. |
| `rule/tidying` | Merging, splitting, retagging, archiving, and deleting, always on request and with the plan confirmed first. |

## Installation

1. Create a new Orbismo world.
2. Set up the structure. Open World Settings → Building Blocks (owner only) and recreate what `building-blocks.json` describes: add the `note` and `source` kinds with the details listed, trim `person` and `rule` to the details listed, and add the four connections with the kinds each one allows. Removing the default kinds and connections this template does not use (place, event, group, and the rest) is optional, but it keeps the assistant from filing things there. This step is done by hand; the portal has no Building Blocks import.
3. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
4. In a fresh conversation, give `world.json` to your connected AI and ask it to add its entities to the world, keeping names exactly as given. A connected AI reads the world's structure once at the start of a session, so the conversation must begin after step 2.
5. Start dropping things in. There is no onboarding interview: the first idea, quote, or link you share becomes the first note.

## Customizing

- **Kinds and statuses**: the `kind` and `status` lists live in two places, as enums in `building-blocks.json` and as the "What goes where" section of `instructions.md`. Change both together.
- **Rules**: add a behavior by creating a `rule` entity with its guidance as lore and adding a row to the rule index in `instructions.md`. Set a rule's `status` to `archived` to switch it off.
- **People**: the template keeps people deliberately thin and private. For a richer model of the people in your life, start from the [Personal Companion](../personal-companion/) template instead.
- **Tone**: the opening paragraph of `instructions.md` sets the register. The capture-first and keep-the-owner's-wording defaults are the point of the template; adjust everything else freely.
