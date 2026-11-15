# Investigation & Research: Investigation

**Keep a sourced record of a long story, and hand a fact-checker or a lawyer a table they can check claim by claim.** For reporters, nonfiction authors and podcast researchers working a story over weeks or months: what is known, from which document or interview, how it stands, who denies it, and what is still to report out.

This is one of three uses of the [Investigation & Research](../../) template. All three share one schema (`../../building-blocks.json`); each use is its own pair of instructions and rules, and a world runs exactly one. The other two are [Briefing](../briefing/) (a standing watch on a few organisations) and [Historical Research](../historical-research/) (family and local history).

Orbismo holds the curated record above your documents: the reference to each source, the passages that matter, and every claim with its evidence. It does not store, search or monitor the documents themselves.

## What's inside

| Piece | File | What it is |
| --- | --- | --- |
| **Schema** | [`../../building-blocks.json`](../../building-blocks.json) | Seven kinds and five connections, shared by all three uses. |
| **Instructions** | `instructions.md` | The world instructions: the assistant's role, a rule index, the hard rules, naming and tone. Short by design. |
| **Rules** | `world.json` | Fifteen rule records, each carrying its full operating detail as lore. No sample story, people or companies. |

The model in one paragraph: a **story** (stored as an investigation) has **claims**, each one proposition that may or may not be true, dated or not. A claim is backed by **sources** (documents, filings, articles, interviews, your own notes), and each source's link to it says where in the source (a locator) and how the source puts it: documents, observes, states, alleges, reports or infers. Claims are **about** people, companies and places. A link between two of them, such as "Reyes was a director of Harbor Holdings", is itself a claim with its own sources, never a bare line between two names. **Questions** hold the leads, checks, records requests and requests for comment. How a claim stands (unverified, stands, disputed, shown false, withdrawn) is yours to decide; the assistant only ever records new claims as unverified and marks a claim disputed when new material conflicts with it.

### The rules

| Rule | Covers |
| --- | --- |
| `rule/core_model` | The one-kind model, where time and attribution live, story membership and tags, the exact call shapes and limits, session start. |
| `rule/onboarding` | Setting up a story, the first or another: its question, tag and the people or companies at the centre; walking in the first source. Read only when a story is being started. |
| `rule/vocabulary` | Every value list: claim types, relations, reliability grades and the defaults the assistant may set, source terms (on the record, background, off the record), confidence, the words for how a claim stands, the bar for "stands". |
| `rule/logging_sources` | Logging a source and what it says: counting before writing, duplicate checks that confirm it is the same document, the one terms question, a second passage on the same claim, who a source is by, naming, locators, verbatim passages and transcripts, and material you describe but have not shown. |
| `rule/recording_claims` | One proposition per claim, neutral names, how a source puts it (and the traps: a complaint is an allegation even in a court filing, and that it was filed is a separate claim), links between actors, dates and spans, change dates. |
| `rule/standing` | Who decides how a claim stands; proposing that one stands, part by part with every date covered; the one procedure every change goes through, including flagging conclusions that rest on it; corrections; conclusions drawn from several claims and "how sure are we". |
| `rule/independence` | Counting independent sources at output time (wire copy, one informant telling two outlets, a company's filing and press release, a body's register and its audit), dependence between sources, and corrections, regrades and retractions with every affected claim updated. |
| `rule/contradictions` | Two accounts of the same thing, kept side by side and never chosen between. A denial is not a contradiction. |
| `rule/identity` | "Is the A. Reyes on the 2018 filing our Ana Reyes?": names that plainly match an existing record are attached to it and you are told; real doubt is kept separate, asked as a question, decided by you; the full procedure for a record created twice by mistake. |
| `rule/open_questions` | Leads, checks, records requests, what's still open, searches that found nothing. |
| `rule/retrieval` | When a claim may be said plainly; "what do we know about X", "everything linking X to Y", timelines, what is only alleged, who disputes it, what changed; every answer kept to the story you asked about. |
| `rule/people_and_confidentiality` | Confidential sources by codename only, cleaning identifying detail out of stored material, source terms in answers and in copy for others, private individuals, looking people up. |
| `rule/tidying` | Keeping story membership consistent, fixing wrong values, deletes only on your word after a warning. |
| `rule/output_fact_check_table` | The table: what stands, what is only alleged, what is disputed or unverified, what was dropped, open leads and pending replies, with every source, locator, term and denial. |
| `rule/right_of_reply` | Requests for comment, chasing, logging a response or a lawyer's letter, denials (shown beside the claim, never changing how it stands), "declined to comment" and "no response by". |

## What it assumes

- You talk; the assistant writes. It works through Orbismo's tools on your behalf and needs a connected AI with read and write access to the world.
- You have the documents. You paste passages, transcripts and links, or describe where something is; the assistant keeps the reference and the words that matter.
- You decide what stands. The assistant proposes when every part of a claim, dates included, meets the bar (an original filing or record, or two independent firsthand sources, and no unanswered denial), but never marks one itself. It sets a first reliability grade or confidence only from a defaults table and tells you; later changes are yours.
- English-language reporting conventions for source terms (on the record, on background, deep background, not for attribution, off the record, embargoed). Change them in `rule/vocabulary` if your desk uses others.

## Getting started

1. Create a new Orbismo world (or pick an empty one).
2. Open World Settings → Building Blocks and paste the contents of [`../../building-blocks.json`](../../building-blocks.json) into the JSON editor ("Paste a file from the Orbismo Playbook"). Remove default kinds and connections the file does not list if any remain.
3. Set this folder's `instructions.md` as the world's instructions: paste it in the web UI, or share the file with your connected AI and ask it to update the world instructions.
4. In a fresh conversation (the AI reads the world's structure once per session, so start after step 2), give it this folder's `world.json` and ask it to add everything in it, keeping names exactly as given.
5. Ask it to check the setup: "Compare the world's kinds and connections with building-blocks.json and tell me anything that differs." The one that matters most: the Claim kind must come through with the key `event` and the label Claim, or claims will not appear on timelines.
6. Say hello.

## A first session

The assistant asks, one question at a time and skipping anything you already said: what the story is, the question you are trying to answer, and who or what is at the centre (two to five names is plenty). Who reads the table, and any deadline, it asks when you first ask for the table. It asks once whether you work with confidential sources and, if you do, explains in two sentences how they are kept. It opens the story and asks for the first thing you have.

You paste a passage from a filing, or an interview transcript. The assistant logs the source, quotes the passage with its page or timestamp, records what it says as claims, and asks what it needs: one terms question for anything that is not published or your own note, and at most one other question. If one message would create more than twenty records, it shows you the plan first and writes nothing until you say yes; once you have said yes to large batches, that holds for the rest of the session. It ends by showing one claim the way the world keeps it:

> Ana Reyes director of Harbor Holdings (2014 to 2019): the 2016 annual return records it, p. 3. Unverified, one source.

From then on, say things like "here's my interview with Okafor", "what do we know about Reyes, and how solid is it?", "everything linking Reyes to Lind", "the 2016 return says she resigned in 2015", "what's only alleged?", "timeline of the grant money, 2016 to 2019", "here's their lawyer's reply", "mark the directorship as standing, I have both filings", "what do I still need to report out?" and "give me the fact-check table".

## What it is good for, and what it is not

**Good for:** keeping every claim tied to the exact page, paragraph or timestamp that backs it; keeping allegations as allegations all the way to the final table; seeing when five articles are really one press release; tracking who was asked for comment, when, and what they said; handing a fact-checker or a lawyer a table that shows denials next to the claims they deny.

**Not good for, by design:**

- **Holding or searching your documents.** No ingestion, scraping or monitoring. The assistant keeps the passages you give it.
- **Evidence custody.** Nothing here is a chain of custody or a forensic record.
- **Registry, sanctions or background lookups.** The assistant looks up public organisations and public figures in their public role only when you ask and web search is available, and never private individuals.
- **Legal conclusions or publication approval.** The table says what stands under your bar and who says what. Whether anything is defamatory, lawful or safe to print is for you and your lawyer.
- **Graph views or network analysis.** Links are answered within one intermediary, read exactly, and the assistant offers to look further rather than claiming "no link".

## Privacy and confidential sources

- **Confidential sources are codenames only** (Source K). Their real name, role, employer, contact details, and anything that could point back to them are never stored, because everything in a world can be read by Orbismo, by the AI provider you connect, and by anyone you share the world with. Identifying detail in a transcript is removed before it is stored, marked "[identifying detail removed]", and the assistant tells you what it removed. If something identifying was stored already, the chunk is deleted and recreated clean, because an edit would keep the old text in its history. The codename itself is a label between you and the assistant: it never appears in the table or anything else meant for other readers.
- **Source terms are respected in every output.** Background and not-for-attribution material appears as "attribution as agreed", naming no one. Off-the-record, deep-background and embargoed material is never quoted or cited; it can only point to things to check. A source whose terms you have not given is cited but never quoted, and the assistant asks you once. Your own notes count as your own knowledge, attributed to you and flagged as unsourced.
- **Private individuals get the minimum:** only what bears on a claim in the story. No home address, family, movements, contacts, health or photographs, even when asked. The one exception is a single family link a source gives when the link is itself the point of a claim (a payment to a director's brother). Anyone is treated as living unless a death is recorded or they were born more than 100 years ago with nothing suggesting otherwise.

## Customizing

- **Your bar for "stands":** write any stricter standard (two sources for everything, no single-source allegations) in the story's `Scope and standards` note during onboarding, or ask the assistant to add it later. It wins over the default bar in `rule/vocabulary`.
- **Value lists:** claim types, relations, reliability grades, terms and confidence words are in `rule/vocabulary`. Change them there, and only there; the assistant never invents a value. The fixed lists (how a claim stands, how a source puts it, question types) are in the shared schema and are the same in all three uses.
- **Table layout:** columns and groups are in `rule/output_fact_check_table`.
- **Rules:** pause one by setting its status to `paused`; add one as a rule record with its guidance as lore and a row in the instructions' rule index.

## Known limits

- **Values are not enforced.** Orbismo saves a wrong value silently. The rules' exact lists and the "never invent a value" rule are the enforcement; check the world's values in use from time to time (ask the assistant to tidy).
- **Change dates are kept by hand.** "What changed since the last table" finds new evidence through the sources logged since, and everything else through each claim's change date, which is stamped when its standing changes, when a contradiction or denial is added, or when a source behind it is corrected or regraded. A source regraded after it backs thirty claims means thirty updates; a missed one hides that claim from the next "what changed".
- **Story membership lives in two places** (the claim's link to the story, and the story's tag on it). Tidying reconciles them; until it runs, a claim with a missing tag drops out of lists. A claim two stories share is one record with one standing.
- **Identity is reused by judgement.** A name that plainly matches an existing person or company (same form, role and setting) is attached to it, and the assistant tells you each time; real doubt gets a separate record and a "Same person?" question. Correct it when it guesses wrong.
- **Large reads take time.** "Everything linking X to Y" at around 100 claims each side is about 15 reads; a 150-claim table is a few dozen. The assistant warns you first.
- **Independence depends on recorded dependence.** Two articles from one unnamed wire story look independent until someone records that they share it.
- **The claim's name is neutral,** so a quick one-line answer late in a long session is where an allegation is most likely to be stated as fact. The rules guard against it; tell the assistant when it slips.
- **Tested in scripted sessions, not yet in long real use.** This use was exercised with a connected assistant over four rounds of scripted sessions and revised after each; report what breaks.
