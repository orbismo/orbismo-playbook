# Second Brain

A personal second brain: AI note-taking with verbatim recall. Save a thought, paste a passage, clip a link — it comes back exactly as you wrote it, byte for byte, never paraphrased. Quick thoughts land in a daily note that acts as the capture inbox; lasting subjects get their own note, linked to the people, projects, and places it's about, so your notebook and the rest of your world share one graph. Recall works even from half-memories: exact title and tag lookups first, then the graph, then semantic search over the notes themselves.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_second_brain` | The app itself. Five lore chunks carry the full spec: Data Model: Notes & Verbatim Capture; Data Model: Daily Notes, Links & the Note Index; Workflow: Capture a Note; Workflow: Recall — Word for Word; Workflow: Review & Tidy. |
| `interest/second_brain` | App hub. Its lore holds the "Note Index" — the notebook's controlled vocabulary of topic tags; the notes themselves are found by the `second-brain` tag. |

In use, the app creates `reference` entities for notes and daily notes (tagged `second-brain`; daily notes also `daily-note`), and links each note from the tracked entities it's about, so backlinks come free.

## Requirements

- Entity types `reference` and `interest` (Orbismo's common types).
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Second Brain** (`rule/app_second_brain`): triggers: saving notes and pasted text word-for-word, daily notes and journaling, web clippings, quoting an earlier note exactly, what did I note about X, notebook review. Tags: `second-brain`, `daily-note`
```

3. Test it: paste a short passage with deliberate formatting (a list, odd spacing, even a typo) and say "save this note as Fidelity Test", then ask "show me Fidelity Test word for word, in a code block, with a character count" and confirm the text and count match what you pasted — typo included.

## Notes

- Verbatim is the contract: your words are stored untouched (markdown, whitespace, typos and all) and quoted back exactly. Anything the assistant adds — a summary, structure — lives in its own labeled lore chunk, never mixed into your text.
- The daily note is the inbox: quick captures append to today's note with a timestamp, and anything that outgrows the inbox is promoted to its own note with the text moved untouched. Deterministic names ("Daily Note 2026-07-15") make "what did I note last Tuesday?" a direct lookup.
- Long notes split cleanly: Orbismo lore chunks hold ~10,000 characters each, so a long paste becomes ordered parts ("Note (1/3)") that recall reassembles seamlessly.
- The notebook stays tidy by review, not by rewriting: duplicates merge with both texts kept verbatim, out-of-date notes are marked superseded rather than deleted, and the Note Index keeps topic tags from sprawling.
- Pairs naturally with the other apps: a note about a relative links to the same person entity the [Family Tree](../family-tree/) tracks, and a note full of tweaks to Grandma's stew can sit right next to the [Recipe Box](../recipe-box/) recipe it refines.
