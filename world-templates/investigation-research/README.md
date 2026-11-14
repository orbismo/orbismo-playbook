# Investigation & Research

**Keep a sourced record of what you know, where it came from, and what is still unconfirmed.** A reporter months into a story, an analyst keeping a standing watch on a few companies, a family historian piecing together who was who in a parish: each gathers documents, interviews and records, and each needs to answer the same questions later. What supports this? Who says otherwise? How sure are we? What is still open?

This template turns an Orbismo world into that record. Your documents stay where they are: in your files, your archive boxes, your browser bookmarks. The world holds the curated layer above them: each claim you care about, the sources that bear on it with a precise place in each source, how each source puts it (documented, stated, alleged, reported), how the claim stands now, and the questions still to chase. The assistant keeps that record as you talk, and builds your table, brief or write-up from it.

It is **not a document search tool**. It knows only what you have recorded in it. It does not read, store or search your documents, and "nothing recorded" never means "did not happen".

Like the [Commonplace Book](../commonplace-book/) and [Sports World](../sports-world/), it does not run on Orbismo's default schema. It ships its own Building Blocks, shared by three uses.

## Pick a use

The template comes in three uses. Each is a complete pair of world instructions and seed rules written for one kind of work, with its own wording, its own way of grading sources and its own output. All three run on the same Building Blocks. **Pick one use per world.**

| Use | For | What it produces |
| --- | --- | --- |
| [Investigation](uses/investigation/) | A reporter, nonfiction author or podcast researcher working a long story. | A claim-by-claim table for a fact-checker or lawyer: what stands, what is only alleged, who denies what, which leads are still open. |
| [Briefing](uses/briefing/) | A competitive or strategic analyst keeping a standing watch on a few organisations. | A short sourced brief: what changed since last time, what supports the current assessment, what would overturn it. |
| [Historical Research](uses/historical-research/) | A family or local historian working from records. | A sourced narrative, with conflicting records and uncertain identities left visible rather than smoothed over. |

How to choose: pick by the output you need to hand someone. If you are writing for publication and someone will check every line, it is Investigation. If you report to a team on a cadence and your view can change, it is Briefing. If your sources are registers, censuses and deeds and your hardest problem is "is this the same person?", it is Historical Research. Each use's README covers its rules, wording and first session.

## One investigation or several

Inside a world, each line of work is an **investigation**: a story, a watch, a line of research. A world can hold one or several. Claims and open questions belong to the investigation they came up in, and a claim that matters to two investigations is one record linked to both, with one standing. People, organisations, places and sources are shared, so a company that turns up in two stories is one record with everything said about it in either.

Most worlds hold one investigation, and then the assistant never asks which one you mean. Add a second to the same world when the two share people and sources and the same people may see both: two strands of one story, two families that intermarried, two competitors in one market.

**Start a new world when the sharing boundary changes, not the topic.** Everyone with access to a world can read everything in it. If a collaborator, editor or client should see one piece of work and not another, the other goes in its own world.

## The Building Blocks

`building-blocks.json` defines seven kinds and five connections.

| Kind | Records |
| --- | --- |
| `investigation` | One line of work: its question, status, the few people or organisations at its centre, and a log of every table, brief or narrative delivered. |
| `event`, shown as **Claim** | One thing that may or may not be true, dated or not: a happening, a role or link between two parties, an attribute, a "same person?" question, an assessment. Carries how it stands (open, confirmed, contested, refuted, withdrawn), dates only as precise as the sources give them, and when it last changed. Its key is `event` so that dated claims appear on Orbismo's timeline; you and the assistant both call it a claim. |
| `source` | Something that says something: a web page, article, filing, archive record, document, interview, statement, post, recording, book, dataset, or your own note. Its link, an archived copy, its reference number, when it was published, how far it is trusted, and the terms it was given on. The passages you cite are kept word for word, each under its page, timestamp or folio. |
| `actor` | A person, organisation, group, online account or object. Identification only: names, aliases, identifiers. Everything said about an actor is a claim. |
| `place` | A country, town, parish, site, plot, building or an organisation's address. |
| `question` | Something still to find out, do or watch for: a question, a lead, a check, a records request, a request for comment, an indicator. Each action taken on it is logged, including searches that found nothing. |
| `rule` | Standing guidance for the assistant, seeded by the use you pick. |

| Connection | Links |
| --- | --- |
| `PART_OF` | claim or question → each investigation it belongs to. |
| `ABOUT` | claim or question → the people, organisations, places, claims or sources it concerns, with a role (subject, other party, mentioned, location). |
| `BEARS_ON` | source → claim: where in the source, which way it points (asserts, supports, contradicts, denies, qualifies), how the source puts it, whether it only repeats another source on that point, and whether the person giving it was there at the time. Also claim → claim, for an assessment and the claims it rests on, and for two competing accounts. |
| `DERIVES_FROM` | source → source, when one is wholly a copy, rewrite, transcript or index of another. |
| `BY` | source → the person or organisation it comes from: author, speaker, issuer, publisher, informant, recorder. |

### Why links between people are claims

"Reyes was a director of Harbor Holdings from 2014 to 2019, according to the company's 2016 return" is recorded as one claim about Reyes and Harbor Holdings, with the return bearing on it. It is never a direct line from Reyes to Harbor Holdings. A direct line would read as fact wherever it appears, and it could not hold three sources that put it three ways, or a second account that gives different years. As a claim, the link keeps everything that makes it trustworthy or not: who says it, where, how it stands, and whether anyone disputes it.

The cost is reach. The assistant follows links one claim at a time, so a question like "how is Reyes connected to Lind?" is answered through recorded claims and at most one person or organisation in between, and it will say "nothing recorded within one step" rather than "no connection".

## Installation

1. Create a new Orbismo world.
2. Set up the structure. Open World Settings → Building Blocks, choose the JSON editor, and paste in the whole of `building-blocks.json`, replacing what is there. When you save, the editor first shows what will be new, changed, deleted for good and kept, and saves only after you confirm. The default kinds and connections that are not in the file are deleted, because nothing in a new world uses them yet; anything already in use would be kept and listed instead.
3. Choose your use. Set that use's `instructions.md` as the world instructions: paste it in the web UI, or give the file to your connected AI and ask it to update the world instructions.
4. In a fresh conversation (the assistant reads the world's structure once per session, so start after step 2), give that use's `world.json` to your connected AI and ask it to add everything in it, keeping names exactly as given. Expect this to take a while: each rule section is added with its own call, about 75 in all.
5. Ask the assistant to check its view of the world's structure against `building-blocks.json` and report any difference. The one to look for: the Claim kind must have the key `event`.
6. Say hello. Each use's onboarding asks what you are working on, sets up the first investigation with you and walks your first source through.

Install only one use per world. Mixing two uses' rules in one world gives the assistant two sets of wording and two grading schemes for the same record.

## What it deliberately does not do

- **Search, store or ingest your documents.** You bring the passages that matter; the rest stays in your files.
- **Collect anything on its own.** No scraping, monitoring, alerts or registry lookups. The assistant looks something up on the web only when you ask and only about public people and organisations, and records what it found as a source like any other.
- **Decide what is true.** The assistant records claims as unverified and can flag a conflict. Only you mark a claim confirmed, refuted or withdrawn.
- **State anything bare that you have not confirmed.** In every sentence of every answer, table, brief or narrative, a claim is told plainly only when you have confirmed it and a record or first-hand observation backs it. Everything else leads with who says it: "the census puts him at 4 Quay Row", not "he lived at 4 Quay Row". Accusations and a party's own statements stay attributed even after you confirm them.
- **Give legal conclusions or approve publication.** The investigation table is for your fact-checker and your lawyer, not a substitute for them.
- **Serve as evidence custody.** The record is not tamper-evident and is not a chain of custody for anything.
- **Analyse networks.** There are no graph views, link scoring or pattern finding. Links are answered from recorded claims.
- **Handle bulk data.** It suits a dossier of hundreds or a few thousand records you curate by hand, not a leaked database or a full registry dump. A table or list you paste is logged as one source, kept whole, with claims only for the rows that bear on your question or name someone already on file. When one message would create more than twenty records, the assistant shows you the plan and writes nothing until you say yes; it asks that once per session.

## Sensitive material

Read this before you put a source's name, or anything about a living person, into a world.

- **This is a hosted service.** Orbismo stores your world on its servers. The AI service you connect reads whatever the assistant reads. Anyone you share the world with reads everything in it. Decide what goes in on that basis.
- **Confidential sources are recorded by codename only.** "Source K", never a real name, and never their role, where they work, contact details, where you met, or details only they would know. The protection covers everything drawn from their material, not just the passage: a fact only they could have given is kept only as a plain "to check" question that does not point back to them, or left out. If you give the assistant identifying detail, it will not store it and will say why. A codename can still be identifiable through what a passage says; keep identifying passages out of the world.
- **Terms are kept with each source.** On the record, on background, off the record, licensed, restricted: the assistant reads the terms before quoting or naming anything from a source in an output. Published public documents (a public register, a published filing or notice, a public web page or article) are quotable without asking. For interviews, private documents, correspondence and paywalled material it asks you once, and until you answer it cites them without quoting.
- **Living private people get the minimum.** For a living person with no public role, the assistant records only what bears on a claim in your work: no home address, family members, movements, contacts, health or photos, and it never looks them up. People long dead are the subject of historical research, and their family links are recorded as claims like any other.
- **Identity is reused with care, and shared names prove nothing.** When you plainly mean someone already on file, or a name appears in the same form, role and setting as an existing record, the assistant attaches it to that record and tells you so, so you can correct it. Only when there is a real reason to doubt does it keep a separate record with a "same person?" claim and the evidence each way. Two existing records are joined only on your word, and a shared name or address is never treated as evidence of identity or of wrongdoing.

## Known limits

- Orbismo does not enforce value lists. The rules spell out every value and forbid inventing one; if the assistant ignores them, a wrong value is saved without warning.
- The timeline orders claims by their start date only. For a window ("2016 to 2019"), the assistant reads the investigation's dated claims in full and works out what overlaps, so long-running roles that started earlier are not lost. That costs more calls on a big investigation.
- How solid a claim is never shows in a list; the assistant has to read each claim before it tells you. On a large investigation a full table or brief takes a while to build, and the assistant should say so.
- Whether two sources are independent depends on what has been recorded about them. Five articles rewriting one press release count as one source only once someone has noted that they do.
- The three uses were exercised in scripted sessions with a connected assistant over four rounds, and the rules were revised after each. Some things still cost effort: every new session reads its rules again, roughly eight to ten calls before work starts; a hand install is about 75 calls, because each rule section is added separately; and a record renamed later keeps its original wording in its link name. Report what breaks.
