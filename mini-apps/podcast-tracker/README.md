# Podcast Tracker

A personal listening log. Shows live as items with a follow status (to try, following, paused, dropped, finished). Each episode you queue, hear, or talk about is its own item, linked to its show, with a rating, a one-line impression, and your takeaways and quotes kept as lore so you can find them again. Hosts and guests are first-class people, so "which episodes had her on?" is a graph query, not a string match. The books and movies that come up in an episode are linked to it, and the app remembers who recommended what.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_podcast_tracker` | The app itself. Five lore chunks carry the full spec: Data Model: Shows & Episodes, Data Model: People/Links/Mentions, Shows & the Queue, Listening/Takeaways/Mentions, Queries & Stats. |
| `interest/listening_to_podcasts` | Collection anchor. Every show item links here so the whole collection can be queried from one hub. |

In use, the app creates `item` entities for shows and episodes (tagged `podcast-tracker`), `person` entities for hosts and guests (tagged `public-figure` to keep them distinct from personal contacts; people another app already created are reused untouched), `reference` entities for the show and episode links you share, and plain `item` entities for books, movies, and other works that come up in an episode.

## Requirements

- Entity types `item`, `interest`, `person`, and `reference` (Orbismo's common types).
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Podcast Tracker** (`rule/app_podcast_tracker`): triggers: podcasts and podcast episodes, listening to or finishing an episode, following/trying/pausing/dropping a show, podcast queue or save-for-later, podcast links, episode ratings, takeaways and quotes, hosts and guests, books or movies that came up on a podcast, who recommended a podcast. Tags: `podcast-tracker`
```

3. Test it: say "I listened to the latest Hardcore History, it was a 9" and confirm a show item and an episode item appear, with the episode's status `listened` and its rating set.

## Example prompts

Talk about podcasts the way you normally would. These show what the app responds to and what happens next.

**Shows**
- "I've started listening to 99% Invisible": adds the show as one you follow, with its host linked.
- "Sam says I should try Heavyweight": adds it as a show to try and remembers that Sam recommended it.
- "I gave up on that true crime show, too many ads": marks it dropped and keeps the reason.

**The queue**
- "Save this for later: <link>": queues the episode, reads the title and guest from the page, and keeps the link.
- "What's in my queue?" lists what you're partway through first, then the rest by show.
- "Add the last 10 episodes of this show: <feed link>": adds them as unplayed, skipping written posts in the same feed. None of them joins your queue until you say so ("queue the one with Robert Caro").

**Listening**
- "Listened to the Acquired episode on Costco, easily a 9": logs the listen with its rating.
- "From that episode: the membership fee is most of the profit": saves the takeaway on the episode, in your words.
- "She said '<quote>' around 41 minutes in": saves the quote word for word, with the speaker and the timestamp.

**What came up**
- "The guest was on to talk about her new book, <title>": links the book to the episode and to the guest as its author. With the Book Tracker installed, it also asks once whether the book goes on your reading list.
- "They kept bringing up Blade Runner": links the movie to the episode as a mention.

**Questions**
- "Which episodes had Ezra Klein on?" · "What did Sam recommend?" · "What books have come up on podcasts?" · "What did I take away from that Costco episode?" · "What should I listen to next?"

## Notes

- Only episodes you queue, hear, or talk about are tracked. The app never fills in a show's back catalog on its own. When you ask for a show's recent episodes, they are read from a feed or page (never from the assistant's memory) and added as `unplayed`.
- The queue is your to-listen list. An episode is `unplayed` (on file), `queued` (on the list), `in_progress`, `listened`, or `abandoned`. Only audio or video episodes count; written posts in a show's feed are skipped.
- Episode details (title, number, date, guests) come from you or from a page you share, never from the assistant's memory, because they are easy to get wrong. Show-level facts like the host and genre are filled in for you when the assistant is sure of them.
- Takeaways and quotes are yours. The assistant hasn't heard the episode, so it never writes a summary or supplies a quote on its own.
- Links are saved as `reference` entities, one per distinct URL, and only when you share one. A show can carry several (its Spotify page, YouTube channel, website, merch store), each named for where it points, so "what's the merch link for that show?" works. The assistant never guesses or recalls a URL; the only links it saves on its own are episode pages read from a feed you asked it to add episodes from.
- Ratings live on episodes. A show has no rating of its own: its score is the average of the episodes you rated.
- There are no listen events. A listen is recorded on the episode itself (status, date, rating), since episodes are rarely heard twice.
- With the [Book Tracker](../book-tracker/) or [Movie Tracker](../movie-tracker/) also installed, you're asked once whether a book or movie that comes up should go on your reading list or watchlist; if so, it is created by that app's rules. Otherwise it is kept as a plain item linked to the episode.
- "Who recommended it" is stored as a name on the show or episode. No person entity is created just for a recommendation.
