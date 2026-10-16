# Vinyl Tracker

A record collector's log. Each physical copy you own or want is its own item. That means two pressings of the same album stay separate, each with its own optional collector details: size and speed, label, catalog number, country, variant, runout matrix, and media/sleeve grades on the Goldmine scale. Note what came in the package (inserts, OBI strip, download code) and when each record was last cleaned. Track what you paid and where, follow pre-orders until they arrive, hunt a wantlist with grails, grade floors, and price limits (Record Store Day picks included), flag duplicates, trade bait, and copies you want to upgrade, log spins with ratings, and keep tabs on loans, sales, and trades. Artists are first-class entities linked to their records, so "what do I have by Coltrane?" is a graph query, not a string match.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_vinyl_tracker` | The app itself. Six lore chunks carry the full spec: Data Model: Records, Data Model: Artists/Shops/Events, Collection & Wantlist, Spins/Condition/Care, Loans/Sales/Trades, Queries & Stats. |
| `interest/record_collecting` | Collection anchor. Every record item links here so the whole collection can be queried from one hub. |

In use, the app creates `item` entities for records (tagged `vinyl-tracker`), `person` entities for solo artists and `group` entities for bands (tagged `artist` to keep them distinct from personal contacts; artists another app already created are reused untouched), `place` entities for the record stores you buy from, `event` entities for spins and for handoffs (a loan, gift, sale, or trade with someone you know), and `reference` entities for release pages you share.

## Requirements

- Entity types `item`, `event`, `interest`, `person`, `group`, `place`, and `reference` (Orbismo's common types).
- An owner ("Me") entity to record as the default listener.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Vinyl Tracker** (`rule/app_vinyl_tracker`): triggers: vinyl records/LPs/singles, record collection, wantlist, crate digging/record stores, buying/pre-ordering/selling/trading/lending records, Record Store Day, pressings, condition grading, cleaning records, spinning a record. Tags: `vinyl-tracker`
```

3. Test it: say "I picked up Kind of Blue at Rough Trade for $28" and confirm a record item appears with status `owned`, linked to Miles Davis, with `purchased_from` set to the store.

## Example prompts

Talk about your records the way you normally would. These show what the app responds to and what happens next.

**Collecting**
- "I picked up a used Kind of Blue at Rough Trade for $28": adds the copy and the store, then offers once, in one line, to note grades, pressing details, and whether it needs a clean.
- "Here's my shelf: Rumours, Blue Train, Purple Rain…": adds the whole list in one go (a photo of your spines works too) and flags any it isn't sure about.
- "Here's the Discogs page for my copy: <link>": proposes the label, catalog number, and country from the page, asks which runout matrix is on yours, and saves only what you confirm.

**Hunting**
- "Add an original UK pressing of Rumours to my wantlist, VG+ or better, under $60. It's a grail."
- "I pre-ordered the RSD clear vinyl of Rumours from the webstore for $45, ships in December": tracks it as a pre-order, kept out of your collection until "my Rumours pre-order arrived".
- "I'm at a record store, what should I look for?": lists your grails and high-priority wants with their grade floors and price limits, then the copies you'd like to upgrade.

**Listening and care**
- "Spinning Rumours tonight with Sam": logs the spin, asks who Sam is the first time, and asks for a rating only on a record's first spin.
- "Cleaned the Blue Note stack today": checks which records you mean, then marks them cleaned.

**Lending and trading**
- "Sam borrowed my Kind of Blue", then later "Who has my records?"
- "I traded my spare Rumours for Sam's Abbey Road": retires the outgoing copy, keeping its history, and adds the incoming one.

**Questions**
- "What should I spin?" · "What did I spend this year?" · "What do I have more than one copy of?" · "I need a list for insurance"

## Notes

- Copy details (format, pressing, condition, package) are only recorded from what you tell it or a page you share, never filled in from general knowledge, because copies of one album differ. Album-level facts like the original release year and genres are filled in for you.
- Share a Discogs (or similar) release page for your copy and the assistant offers to fill in the pressing details from it, asking which runout matrix is on yours and writing only what you confirm.
- Every pressing, condition, package, and care field is optional. A record with just an album and an artist is enough; add detail when you care to.
- Ratings live on spins, never on the record. Only the first spin of a record asks for a rating; later spins take one only when you offer it.
- The app tracks what you paid, not market value. "What has my collection cost?" works; "what is it worth?" gets the cost and a note that market value isn't tracked. The same holds for the insurance inventory report: it lists each copy's details and what you paid, not replacement value.
- A pre-order (paid for before the record is released or in stock) isn't counted in the collection until it arrives. "What pre-orders are outstanding?" flags any past their expected ship date when you ask; there are no reminders.
- A mail order of an in-stock record, such as a Discogs purchase, counts as owned from the day you buy it.
- Records that leave the collection (sold, traded, given away, lost) are kept with their history, never deleted, and are excluded from collection counts.
- Loans, along with gifts, purchases, sales, and trades with people you know, are recorded as handoff events with you and the other person attending, so "who has my records?" and "what did Sam give me?" are answerable. Anonymous marketplace buyers and sellers are never created as people.
- Buying from a shop in person gives you a "visited" link to it the first time, if you don't already have one. With the [Travel Tracker](../travel-tracker/) also installed, that link won't be counted in the travel app's visit history, since this app never updates it after creating it.
