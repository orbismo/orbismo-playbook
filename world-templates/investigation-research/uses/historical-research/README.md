# Investigation & Research: Historical Research

**A sourced research record for family and local historians.** Every baptism, census household, deed, gravestone and family story you find becomes a set of claims tied to the exact record and entry they came from. When records disagree, both accounts stay side by side. When a record plainly fits someone already on file, it is attached to them and you are told; when there is real doubt, the two stay separate until you decide. When you want it written up, you get a narrative where every sentence carries a citation and the uncertainties are still in the text.

This is one of three uses of the [Investigation & Research](../../) template. All three share one set of Building Blocks (`../../building-blocks.json`); each use is a complete pair of world instructions and rules. Pick one per world. The others are `investigation` (a reporter's claim-by-claim fact-check table) and `briefing` (an analyst's short sourced brief).

## Who it is for

- A family historian tracing an ancestor, a family across generations, or where a surname came from.
- A local historian working on a house, a farm, a street, a parish or a village's people.
- Anyone writing up a family story for relatives who wants it to stand up to a cousin asking "how do you know that?"

## What it assumes

- **You do the finding; the assistant keeps the record.** Orbismo holds the reference, the link, where you keep your copy, and the passages that matter, transcribed exactly. It does not search archives, store scans, or read images for you.
- **Your words and the records' words are kept as given.** Original spellings, Latin entries, abbreviations and your own uncertain readings stay as you transcribe them.
- **You decide what is proven.** Every claim starts as *unproven*. The assistant can mark a claim *conflicting* when a new record disagrees, and can suggest that something is *established* when the evidence meets the bar, but only you can settle it.
- **One line of research per world is the usual case** ("the Callan family of Dunmere"). A world can hold several; people, places, records and even individual claims are shared between them, so a finding made for one is never duplicated in another.

## What's inside

| File | What it is |
| --- | --- |
| `instructions.md` | The world instructions: the assistant's role, the rule index, the hard rules, naming and tone. Short by design; the detail lives in the rules. |
| `world.json` | Fifteen `rule` entities whose lore carries the operating detail. Rules only: no sample research, people or records. |

| Rule | Covers |
| --- | --- |
| `rule/core_model` | The one-kind model, where time and attribution live, membership and tags, paging and scope, session start. |
| `rule/onboarding` | The first session: what the research is about, its question, the people at its centre, family knowledge as a source, the first record. |
| `rule/vocabulary` | Claim types (birth, baptism, marriage, death, burial, residence, migration, land transaction, military service, court), relations (child of, spouse of, servant of, tenant of and the rest), reliability grades, access terms, confidence, the words for how a claim stands, and the bar for established. |
| `rule/logging_sources` | Logging a record: counting before any write, duplicate checks, one source per register entry, census household or certificate, locators, verbatim passages, origin, family stories and unseen material, a second passage on the same claim. |
| `rule/recording_claims` | Turning a record into claims: one proposition each, how the record puts it, informants and timing per point, census and baptism traps, old and approximate dates. |
| `rule/records_and_evidence` | Weighing records: originals versus indexes and images, who knew and when, ages as evidence of a birth year, charges, plans and bequests, and regrading or correcting a record. |
| `rule/standing` | Only you settle a claim; how the assistant proposes (showing what each record covers), handles denials, records your decision, and flags conclusions that rested on it. |
| `rule/independence` | Counting independent records: copies, transcripts and one informant behind two records count once. |
| `rule/contradictions` | Two records disagree: both kept, linked and shown together. |
| `rule/identity` | "Same person?": record-qualified names, the evidence for and against, aliases versus identity, true duplicates. |
| `rule/open_questions` | Things to look for, records to order, checks; what counts as finished; the search log, including searches that found nothing. |
| `rule/retrieval` | Answering questions: what we know about someone, how two people connect, timelines, which records put someone in a place, and being straight about gaps. |
| `rule/output_narrative` | Writing it up: every sentence traceable, conflicts and uncertain identities in the text, numbered citations, access terms, living people. |
| `rule/people_and_confidentiality` | Who counts as living and what they get, DNA, confidential informants by codename only, removing identifying detail properly, and only the dead and the public are looked up. |
| `rule/tidying` | Reconciling the record, repairing mistakes, merging only true duplicates, deleting a claim only after a warning, reporting rule drift. |

## Installation

1. Create a new Orbismo world (or pick an empty one).
2. Set up the structure. Open World Settings → Building Blocks (owner only), choose to paste a file from the Orbismo Playbook, and paste `../../building-blocks.json` (seven kinds, five connections). If any default kinds or connections remain afterwards, remove them so the assistant cannot file things there.
3. Set this folder's `instructions.md` as the world's instructions: paste it in the Orbismo web UI, or share the file with your connected AI and ask it to update the world instructions.
4. In a fresh conversation (the AI reads the world's structure once per session, so start after step 2), give `world.json` to your connected AI and ask it to add everything in it, keeping names exactly as given.
5. Ask the AI to check the setup: call `get_world_context` and compare it with `building-blocks.json`. The Claim kind must come through with the key `event` and the label "Claim", and the claim, source and question details must all be there.
6. Say hello. The onboarding rule asks what your research is about and walks the first record through with you.

## A first session

You say you are researching your great-great-grandfather Joseph Callan, who lived in Dunmere. The assistant asks what you most want to find out ("where he came from before Dunmere"), suggests a short label for the research, and records Joseph as the person at its centre. It asks what the family already says, and logs your aunt's story of him arriving by sea as a family story: a source graded low, with the claims it makes marked unproven.

Then you paste the 1881 census entry for his household. The assistant transcribes it into the record exactly as you give it, checks you have not logged it before, and records where the family lived, who was in the household and how each person relates to Joseph, with the census as the record behind each. Because you said "found them", it takes the Joseph in the census to be your Joseph and tells you so; a record that does not fit would be kept apart as a "same person?" question instead. Because the batch is large, it first lists the records and waits for your yes (once a session), and a census or register entry is open to quote without asking; it asks once about anything private, such as a cousin's letters. It ends by showing you one claim the way every answer will: "Joseph Callan born (about 1849, 1881 census age): unproven, worked out from one record. The census gives his age as 32; that puts his birth about 1849, which we worked out and I have marked as possible."

From there: log records as you find them, ask "what's left to look for", ask "were these two the same man?", and ask for a chapter whenever you want one.

## Customising

- **Your own standard of proof.** Add it to the research's `Scope and standards` note (the assistant writes this at onboarding); it adds to the bar in `rule/vocabulary`.
- **Claim types and relations.** The lists live in `rule/vocabulary`. To add one (say `education` or `guardian_of`), add it there with a one-line meaning; the assistant never invents a value that is not listed.
- **Citation style.** `rule/output_narrative`'s Citations chunk sets the default numbered-note style. Replace it with the style your society or journal uses.
- **Rules.** Pause a behaviour by setting its rule's `status` to `paused`. Add one by creating a `rule` entity with its guidance as lore and adding a row to the rule index in `instructions.md`.

## What it deliberately does not do

- **Search archives, read scans or store documents.** It keeps references, links and transcribed passages. Your files stay where you keep them.
- **Decide what is true.** It never settles a claim, never chooses between conflicting records, and never merges two people because the names match.
- **Fill gaps.** A search that found nothing is logged as where you looked, never as proof something did not happen, and general history is never written as if it were about your family.
- **Keep data about the living.** Living people get their name and place in the family line, nothing else: no addresses, contact details, health, photographs or DNA match data. Anyone is treated as living unless a death is recorded or they were born more than 100 years ago with nothing suggesting they are alive, and your word that someone is living always wins.
- **Build trees or charts.** There is no pedigree view, chart export or GEDCOM import. The record is claims and sources; a narrative or citation list is the output.

## Known limits

- **Values are not enforced.** Orbismo saves a wrong claim type or grade silently; the lists in `rule/vocabulary` and the "never invent a value" rule are the only enforcement.
- **Timelines read everything.** `get_timeline` filters on a claim's start year only, so the assistant fetches the whole timeline and filters after reading. A long family history makes this slow.
- **Connections reach one person in between.** "How are these two families connected" checks direct links and links through one intermediary; the assistant offers to go further step by step.
- **Independence depends on what is recorded.** An online index and the register it came from look independent until the link between them is logged.
- **The change date is kept by hand.** "What changed since my last write-up" depends on the assistant updating a claim's change date when its standing changes or a record contradicts it, and on finding new evidence through the records logged since; a missed stamp hides a change.