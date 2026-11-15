# Investigation & Research: Briefing

**A standing watch on a few organisations, with a brief you can stand behind.** For a competitive or strategic analyst who follows a handful of competitors, suppliers, a sector or a market, and reports to a team or a client on a cadence. You bring the excerpts, filings, releases, articles and call notes; the assistant logs each as sources and claims, keeps announcements apart from things that actually happened, counts five rewrites of one press release as one source, and writes a short sourced brief on request: what changed since last time, the current assessment and how confident you are, what supports it, and what would change it.

This is one of three uses of the [Investigation & Research](../../) template. All three run on the same Building Blocks; this folder holds the briefing use's world instructions and seed rules. Read the template's [README](../../README.md) first for the model, the sensitive-material notes and the known limits.

## What it assumes

- **A watch is a standing line of work.** Each one has a question or decision it serves ("Should we enter the Velmar market before Lind Foods does?"), the few organisations or places at its centre, a cadence and an audience. A world can hold several watches; organisations and sources are shared between them, and a claim that matters to two watches is one record in both, so how it stands never drifts between them.
- **Claims, not facts.** Everything recorded is a claim with its sources, and each source says how it puts it: documented, stated, alleged, reported, observed or inferred. The assistant records new claims as **unconfirmed** and can flag **conflicting reports**; only you mark a claim **confirmed**, **disproved** or **retracted**. A denial from the organisation concerned is shown beside the claim wherever it appears, and accusations, a company's own statements and press reports stay attributed in the brief even after you confirm them.
- **An announcement is not the thing announced.** "Lind plans a plant for 2027" is recorded, and named, as an announcement, with 2027 kept as the expected date and the announcement date kept beside it. When two parties expect different things (two closing dates for one deal), both are kept as what each said, side by side, without either being marked as disputed. The plant opening is a separate claim, recorded only when a source shows it. In the same way, a regulator's notice is recorded as a claim that the notice was issued, kept apart from the conduct it alleges.
- **Sources are graded on the Admiralty scale** (`a_reliable` to `f_cannot_judge`), separately from how solid any one claim is. Each source also carries its terms: `public`, `licensed` (subscription content with quote limits), `client_confidential` or `internal`. Published public material (registers, regulators' notices, a company's published filings and releases, public web pages, articles at an open address unless you mention a subscription) is treated as public. For anything else, such as calls, letters or paywalled articles, the assistant never guesses: it asks one question when it logs them, and until you answer it names the source but does not quote it. Saying "we subscribe to Coldchain Review" covers every source from that publication.
- **Names are matched to what you already have.** When new material plainly refers to an organisation or person already recorded, the assistant files it there and tells you it did, so you can correct it. Only a real doubt (a different place, period or role, or just a similar name) gets a separate record and a "same organisation?" question for you to settle.
- **Documents you describe are treated as your account.** If you tell the assistant what a document says without pasting it, it records your summary as secondhand until you share the text.
- **Assessments are claims too.** A judgement such as "Lind Foods enters Velmar by 2028" rests on premise claims, carries a confidence (high, moderate, low) and a likelihood word (almost certain, likely, roughly even chance, unlikely, remote), and lists the signs that would change it. When a premise is disproved or retracted, the assessment is flagged for review rather than silently kept.
- **Freshness matters.** A figure older than 90 days (or the window you set) is given "as of" its date, never as current.

## What's inside

**`instructions.md`** sets the assistant's role, the rule index, the hard rules (no claim without a source, never state a claim from its name, no upgrades without you, "alleged" stays "alleged", identities never merged silently, approximate dates stay approximate, "not recorded" never means "did not happen", confidential sources by codename only, no profiling of private individuals), naming and tone.

**`world.json`** seeds fifteen `rule` entities, each carrying its operating detail as lore:

| Rule | Covers |
| --- | --- |
| `rule/core_model` | The seven kinds and five connections, where each fact lives, how a watch holds its claims, session start. |
| `rule/onboarding` | The first session: setting up the watch, logging the first source, asking audience and cadence at the first brief, and later changes to a watch. |
| `rule/vocabulary` | Every value this use defines: claim types, relations, source grades, terms, confidence, likelihood words, standing words and the bar for confirmed. |
| `rule/logging_sources` | Logging a source: counting records first, duplicate checks, naming, who a source is by, locators and passages kept word for word, with tables stored whole and only the rows that matter turned into claims. |
| `rule/recording_claims` | Turning what a source says into claims: one proposition each, neutral names, how each source puts it, links between parties, dates and freshness. |
| `rule/standing` | Who decides how a claim stands, the bar for confirmed, proposals, corrections, and reviewing conclusions when a premise moves. |
| `rule/independence` | Counting independent sources by following rewrites and repeats back to each origin, plus documents you describe but have not shared, corrections, retractions, regrades and dependence found later. |
| `rule/contradictions` | Two accounts of one thing kept side by side, denials, and what is not a conflict. |
| `rule/identity` | "Same company?" questions, trading names, group structure, and true duplicates. |
| `rule/open_questions` | Questions, leads, checks, requests, replies and indicators; searches that found nothing; what's open. |
| `rule/assessments` | Forming and revising judgements, what supports them, what would change them, and observing indicators. |
| `rule/output_brief` | What changed since the last brief, the brief's sections and wording, terms, and logging what was delivered. |
| `rule/retrieval` | Answering from the record: what's known about X, links between X and Y, timelines, current or stale. |
| `rule/people_and_confidentiality` | Confidential and expert-network sources, terms, people with and without a public role, web lookups. |
| `rule/tidying` | Reconciling a watch, safe repairs, never deleting claims, and reporting rule index drift. |

## Installation

1. Create a new Orbismo world.
2. Set up the structure: World Settings → Building Blocks, choose the JSON editor, and paste in the whole of the template's [`building-blocks.json`](../../building-blocks.json). Remove default kinds and connections the template does not use; the editor shows what will change before you save.
3. Set this folder's `instructions.md` as the world instructions: paste it in the web UI, or give the file to your connected AI and ask it to update the world instructions.
4. In a fresh conversation (the assistant reads the world's structure once per session, so start after step 2), give this folder's `world.json` to your connected AI and ask it to add everything in it, keeping names exactly as given.
5. Ask the assistant to compare its view of the world's structure with `building-blocks.json` and report any difference. The one that matters most: the Claim kind must have the key `event`.
6. Say hello.

Install only this use in the world. Mixing in another use's rules gives the assistant two wordings and two grading schemes for the same record.

## A first session

The assistant asks, one question at a time and skipping anything you have already said, what you are watching and what decision it serves, and which organisations are at the centre. It creates the watch (with a tag such as `velmar-watch`), then asks for one thing you read recently. Who reads the brief and how often are asked when you first ask for one. Paste a release, an article excerpt or your call notes and it logs the source, the organisations it names and the claims it makes, each with the exact place in the source. It ends by showing a claim the way it will always show claims, led by who says it, and listing what you can ask for:

> Logged: Lind Foods states it plans a Velmar plant for 2027 (12 June release, para 2); unconfirmed, one source. You can ask me to 'write the brief', 'what's changed', 'what's open' or 'what supports our view'.

After that, the everyday phrases are "here's this week's material", "what's changed since the last brief", "write the brief", "what supports our view on Velmar", "what would overturn it" and "what's open".

## Customising

- **Cadence, audience, freshness and quote limits** live in each watch's `Scope and standards` note, written during onboarding. Ask the assistant to change them at any time.
- **Value lists** (claim types, relations, source grades, terms, confidence) live in `rule/vocabulary`. If you change one, change it there and nowhere else, and never rename a value already in use: the assistant would stop finding the records that carry the old one.
- **The brief's shape** is in `rule/output_brief`. Reorder sections or change the length there.
- **The bar for confirmed** is in `rule/vocabulary`. A stricter bar for one watch can go in that watch's `Scope and standards` instead.
- **Pausing a behaviour**: set a rule's `status` to `paused`. Keep `rule/standing` and `rule/people_and_confidentiality` active; they carry the safety rules in full.

## What it deliberately does not do

- **Monitor anything.** No alerts, feeds, scraping or scheduled collection. The brief's due date is a reminder for you, not a job the assistant runs. It looks something up on the web only when you ask, only about public organisations and people with a public role, and records what it found as a source.
- **Decide what is true or what you should do.** It flags conflicts and proposes upgrades; you decide. Its assessments are drafted with you and adopted only on your word.
- **Treat an announcement as an outcome**, or a figure from months ago as current.
- **Store who your contacts are.** Confidential and expert-network sources are codenames only, the codename never appears in a brief (their material is given as "an industry contact, attribution as agreed"), and the assistant will decline to store identifying detail. A source whose terms you have not set is named but never quoted in a brief until you do. It will also flag material that looks like another company's confidential information and ask before recording it; whether you may use it is for you and your compliance rules.
- **Profile private people.** Employees, recruiters and other people without a public role get the minimum, and the assistant never looks them up.
- **Store your documents or handle bulk data.** It holds the passages that matter, not the filings or the subscription feed, and suits a curated watch of hundreds or a few thousand records.
