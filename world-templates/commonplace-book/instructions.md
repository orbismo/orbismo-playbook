# Commonplace Book

A personal commonplace book: ideas, quotes, observations, questions, links, and the occasional person. Fast capture matters more than tidiness, and the owner's own words matter more than polish.

## Rule index

Rules hold the detailed guidance for specific situations. Before you act on a topic below, read its rule with `get_entities` and `lore: {include_content: true}`. Most of a rule's substance is in its lore, not its properties. Only read the rules whose trigger matches what you are about to do.

| Trigger | Rule |
|---|---|
| Capturing anything new | `rule/capture` |
| Adding, editing or linking a person | `rule/people` |
| Quoting from a book, article, talk or conversation | `rule/quoting_sources` |
| Merging, splitting, retagging, archiving or deleting | `rule/tidying` |
| Answering "what have I got on X" or surfacing connections | `rule/retrieval` |

If a listed rule does not exist, or its `status` is not `active`, skip it and follow this document. An active rule overrides this document when the two conflict, and rules add detail this document leaves out — keep each piece of guidance in one place, not both. The owner overrides everything. When a new rule is added, add a row here. Occasionally — for instance when asked to tidy — check this index against the rule entities that actually exist, and report any drift (a rule with no row, or a row pointing at a missing rule) to the owner rather than fixing it silently.

## What goes where

- **note** is the core unit: one idea per note. Name it with a short handle, using the owner's words where possible. The owner's exact text goes in the note's first lore chunk, titled `Text`, however short it is; that chunk is the record. `short_description` is a one-line search aid, not the record: the owner's text itself when it fits on one line, otherwise a plain summary. `kind` is one of idea, quote, observation, question or link; a link note keeps its address in `url`. `status` is one of raw, developing, used or archived. New notes start as `raw`; move a note to another status only when the owner says so.
- **source** is where something came from, such as a book, article, podcast, site or conversation. It has `author`, `medium` and `url`. Create a source only when the owner names one.
- **person** is kept light: `context` (who they are to the owner) and `where_met`. This is not a contact database.
- **Topics are tags**, not entities. Use lowercase and hyphens. Check the existing tags (`search_entities` with `return: "tags"`) and reuse one before inventing a near-duplicate.

## Relationships

- FROM: note → source
- MENTIONS: note or source → person
- BUILDS_ON: note → note, where one idea grew out of another
- RELATES_TO: note ↔ note, for a loose association

Use only these four types. The platform's built-in semantic predicates (such as `inspired_by` or `rules_over`) are generic defaults and are not part of this world.

Create a link when the owner states it or it is plainly true, such as a quote and its source. For looser connections, suggest the link and wait for a yes.

## Defaults

- **Keep the owner's wording.** Never rewrite, condense or "improve" a captured note. Your own commentary — context you looked up at the owner's request, a hunch about what a note connects to, why you chose a tag — goes in a separate lore chunk titled `Agent note` on the entity concerned. Never mix it into the owner's text, and never let it grow longer than what it annotates.
- **Capture first, ask after.** If the kind or tags are unclear, save the note as `raw` with your best guess, then ask one question. Capture is never held up waiting for an answer.
- **Check for duplicates before creating.** Run a `search_entities` search first. A near-match does not block the capture: save the new note, then offer to merge it into the existing one.
- **Nothing is deleted without an explicit instruction.** Set a note to `status: archived` instead. Sources and people have no status; `rule/tidying` covers them.
- **People are private.** Record only what the owner tells you. Never search the web about a private person.
