---
description: The home of the UI shows a weekly digest. A digest gathers the entries that the group wrote in one week, from Monday to Sunday in CET. It has a lead, the top scores, the new subjects, and the counts. Memories show the entries of the same week in earlier years.
status: proposed
---

# Motivation
[motivation]: #motivation

The table of decision 00009 lists all entries by the time they were written, the newest first.
A small group writes few entries. A quiet week then looks empty. An endless feed also reads like social media.
The backlog has the item "redesign or rename table concept".

A weekly digest gathers the entries of one week on one page, like a small newspaper.
A quiet week then looks planned, not empty. A finished digest does not change. A person can then share its link in the chat of the group.
A food journal also remembers. The entries of the same week in earlier years come back as memories.

Expected outcome:
- the home shows the entries of this week so far, the digest of the last finished week, and the memories
- each week has a page at `/weeks/{date}` with its digest and every entry
- the archive at `/weeks` lists the weeks with entries
- the link of a digest shows a preview with the photo of the lead and the counts
- the weeks start on Monday at 00:00 in CET

# Scope
[scope]: #scope

In scope:
- the week and its time zone
- the operations `getCurrentWeek`, `getWeek`, and `listWeeks` of the new tag `weeks`
- the rules of the lead, the top scores, the new subjects, and the memories
- the home, the digest page, and the archive of the UI
- the link previews of the digest pages
- the words and the components of the design system for the digest

Out of scope:
- a weekly email. The group talks in its own chats. See "Won't do" in the backlog.
- memories of one month or six months ago. Memories are whole years.
- the memories of one reviewer on the journal
- a digest that a person edits or curates
- a week that starts on Sunday, and a time zone for each user
- comments, likes, or reactions on a digest. Decision 00000 puts social features out of scope.
- an average score of a week

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

The design canvas "Weekly digest mockup" has the five screens of this decision.
They are the home on a Saturday and on a Monday, the digest page, the archive, and a link preview.

## The concepts

- **Week**: Monday 00:00 to the next Monday 00:00 in the time zone of the site. The date of its Monday identifies it, such as `2026-11-16`.
- **Digest**: the page of one week. It shows the entries that were written in the week. A week that ended has a finished digest. The current week has a digest in progress.
- **Lead**: the first story of a digest. It is a subject that two or more people reviewed in the week, or the best entry.
- **Top scores**: the best entry of each kind after the lead.
- **New on the table**: the restaurants, recipes, and pantry items that users added in the week.
- **Memory**: an entry with a `date` in the same days of an earlier year. "On this day" marks a memory with the month and the day of today.
- **Archive**: the list of the weeks with entries at `/weeks`.

Examples:
- On Monday 23 November 2026, Ana opens Olive. "This week so far" says "Nothing yet this week". Below it is the digest of last week. Its lead is "Priya and Marco both went to Luigi's Trattoria" with the photo of Priya.
- Priya copies the link `/weeks/2026-11-16` into the chat of the family. The preview shows the photo of the lead and "14 entries from 6 people".
- Marco writes a review on Monday 23 November at 00:30 CET about the dinner of Sunday. It is in the week of 23 November. The week of an entry comes from the time it was written. Its card still shows "Visited on 22 Nov 2026".
- On Saturday 28 November, Jules opens the home. The memories show the pho that Ana ate on 28 November 2025 with "On this day". They also show the banana bread that Marco cooked on 24 November 2023.
- Jules opens `/weeks/2026-08-03`, a week without entries. The page says "A quiet week" and links to the weeks before and after it.

## The home

```
SATURDAY 28 NOVEMBER
On the table
What everyone ate, cooked, and tasted lately.

THIS WEEK SO FAR               4 entries from 4 people
  Kin Khao Noodle Bar · Broth worth the queue      8.8
  Shakshuka · Better with feta                     7.5
  Comté 24 months · Nutty, with a long finish      9.0
  Osteria del Ponte · Lovely room, slow kitchen    6.5
  Open this week

══════════════════════════════════════════════════════
Last week
16–22 November · 14 entries from 6 people · 3 new on the table
  [photo of the lead]                            (8.4)
  LUIGI'S TRATTORIA · 2 VISITS
  Priya and Marco both went to Luigi's Trattoria
  Priya  The best carbonara in town ........... 8.6
  Marco  Loud but worth it .................... 8.2
Top scores
  ATE OUT  Casa Pepe .......................... 8.8
           Jules · Croquetas to remember
  COOKED   Dal makhani ........................ 8.4
           Ana · Worth the slow simmer
  TASTED   Rioja Reserva ...................... 8.1
           Sam · Soft tannins, ripe cherry
New on the table
  (Luigi's Trattoria) (Miso cod) (Rioja Reserva)
  Read the digest    All weeks

Memories
A YEAR AGO THIS WEEK
  [On this day] Pho 24 · Rare beef, clear broth    8.0
3 YEARS AGO THIS WEEK
  Banana bread · Too sweet, great crust            7.0
```

- "This week so far" shows the five newest entries of the current week as short rows. Its link "Open this week" opens the page of the current week.
- without entries in the current week, it says "Nothing yet this week". Below it is "The first visit, cook, or tasting shows here." A signed in user then sees "Add review".
- the digest is the last finished week with entries. Its heading is "Last week" for the week before the current week. An older week has "Week of 2 November".
- a double rule in `foreground` starts the digest, like the masthead of a newspaper
- "Read the digest" opens the page of the week. "All weeks" opens the archive.
- the memories are the memories of the current week. Without memories, the section is not there.
- the heading of the memories is "Memories". The groups are "A year ago this week" and "3 years ago this week".
- a signed out visitor sees the note of decision 00009 above "This week so far"
- the home has no `KindSwitch`. The digest shows all kinds on purpose.
- without any entries, the home shows the empty state "Nothing on the table yet" of decision 00009
- without a finished week with entries, the home has no digest section
- `BottomNav` and `AppHeader` keep "Table". It opens the home.

## The parts of a digest

The home and the digest page show the parts of a digest in the same way.

- the lead is a photo card, like the photo form of `EntryCard`. The score stamp sits on its photo.
- the kicker of a shared lead is the subject and the count, such as "Luigi's Trattoria · 2 visits". The words are "visits", "cooks", and "tastings".
- the heading of a shared lead is its headline. Below it, each entry of the lead has one line.
- such a line has the reviewer, the title, a dotted leader, and the overall
- the stamp of a shared lead shows the average overall of its entries. 8.6 and 8.2 give 8.4.
- the UI gets the average from the exact values and shows one decimal, like each overall. See decision 00005.
- a lead with one entry is the photo card of that entry with its title
- a lead with one entry has a headline such as "Ana tasted Comté 24 months". It names the reviewer, the verb, and the subject.
- the archive rows use the headline of each lead
- the top scores are one card, like a menu. Each kind has a line with the kind, the subject, a dotted leader, and the overall.
- the reviewer and the title of a top score are below its line
- "New on the table" is a row of chips with the icon of the kind and the `accent` fill. More than six subjects add the text "and 2 more".
- "On this day" is a tag with the `accent` fill above the subject of a memory. `saffron` stays only for the score stamp. See decision 00009.

## The digest page

- the page of a week is at `/weeks/{date}`. A date that is not a Monday opens the week of the date. The URL then changes to its Monday.
- the heading is "Week of 16 November 2026". The current week has "In progress" below it.
- the page has the counts, the lead, the top scores, "New on the table", the memories, and "Every entry"
- the counts have the kinds below them, such as "6 visits · 5 cooks · 3 tastings"
- the memories of a digest page have the dates of their year as group headings, such as "16–22 November 2025"
- "Every entry" lists all entries of the week as short rows, the newest written first
- the `KindSwitch` of "Every entry" has "All", "Ate out", "Cooked", and "Tasted". `?kind=` keeps the selection.
- the links at the end are "Week of 9 November", "Week of 23 November", and "All weeks"
- they open the weeks before and after with entries, and the archive
- a week without entries says "A quiet week". It still has the links.
- a week after the current week shows the not found screen

## The archive

- `/weeks` lists the weeks with entries, the newest first
- the heading is "All weeks", with "The weeks with entries, the newest first." below it
- each row has "Week of 16 November 2026", the counts, and the headline and the photo of the lead
- a lead without a photo shows the icon of its kind. A row has no score.
- the row of the current week has the tag "In progress". Its counts end with "so far".
- "Load more weeks" loads the next weeks
- the digest page and the home link to it

# Implementation Details
[implementation-details]: #implementation-details

## The time zone

- `REVIEWS_TIME_ZONE` sets the time zone of the weeks. It is an IANA name. The default is `CET`.
- `CET` has the summer time CEST from the last Sunday of March to the last Sunday of October
- an unknown name stops the app at start with an error, like the other environment variables
- the Docker image has the `tzdata` package. `time.LoadLocation` finds the zone files there.
- the database has `created` in UTC. The server changes the start and the end of a week to UTC for the queries.
- the week of 23 to 29 March 2026 has 167 hours. The week of 19 to 25 October 2026 has 169 hours.
- only the weeks and `today` use the zone. The `date` check of decision 00005 keeps UTC+14.

## A week

`GET /weeks/2026-11-16` sends:

```json
{
  "start": "2026-11-16",
  "end": "2026-11-22",
  "starts_at": "2026-11-15T23:00:00Z",
  "ends_at": "2026-11-22T23:00:00Z",
  "today": "2026-11-28",
  "finished": true,
  "counts": {
    "entry_count": 14,
    "reviewer_count": 6,
    "restaurant_review_count": 6,
    "recipe_review_count": 5,
    "pantry_item_review_count": 3,
    "new_restaurant_count": 1,
    "new_recipe_count": 1,
    "new_pantry_item_count": 1
  },
  "lead": [
    {
      "kind": "restaurants",
      "review": {
        "id": "r7k2m9x4p1q8w3z",
        "restaurant": "k3x9m2p7q1w8z4a",
        "restaurant_name": "Luigi's Trattoria",
        "date": "2026-11-21",
        "overall": 8.6,
        "title": "The best carbonara in town",
        "creator": { "id": "u2p8r4y6a1c3e5g", "name": "Priya" }
      }
    },
    {
      "kind": "restaurants",
      "review": {
        "id": "b4n8c2v6x0z3m7l",
        "restaurant": "k3x9m2p7q1w8z4a",
        "restaurant_name": "Luigi's Trattoria",
        "date": "2026-11-19",
        "overall": 8.2,
        "title": "Loud but worth it",
        "creator": { "id": "m9a3r5c7o1b2d4f", "name": "Marco" }
      }
    }
  ],
  "top": [],
  "new_subjects": [
    { "kind": "restaurants", "id": "k3x9m2p7q1w8z4a", "name": "Luigi's Trattoria" }
  ],
  "latest": [],
  "memories": [],
  "previous": "2026-11-09",
  "next": "2026-11-23"
}
```

The example leaves out properties of the reviews and the items of `top`, `latest`, and `memories`. The response has complete reviews.

| Property | Value |
|---|---|
| `start`, `end` | the dates of the Monday and the Sunday |
| `starts_at`, `ends_at` | the start and the end of the week as RFC 3339 timestamps. The UI uses them for the `created` filters of "Every entry". |
| `today` | the date of today in the time zone of the site |
| `finished` | `true` when `ends_at` is in the past |
| `counts` | the numbers of entries, reviewers, and new subjects of the week |
| `lead` | the entries of the lead. It is empty for a week without entries. |
| `top` | at most one entry of each kind |
| `new_subjects` | at most six subjects, the newest first. `counts` has the totals. |
| `latest` | at most five entries, the newest written first |
| `memories` | at most six entries |
| `previous`, `next` | the Monday of the nearest earlier or later week with entries, or `null` |

- `WeekEntry` is a `oneOf` of `RestaurantWeekEntry`, `RecipeWeekEntry`, and `PantryItemWeekEntry`. Its `discriminator` is `kind`.
- `kind` has the values of `kind` of `listTags`: `restaurants`, `recipes`, and `pantry`. `review` is the complete review of the kind.
- `WeekSubject` has `kind`, `id`, and `name`
- a review can be in more than one section, such as `lead` and `latest`

## Operations

| Operation | Path | Result |
|---|---|---|
| `getCurrentWeek` | `GET /weeks/current` | the `Week` of today |
| `getWeek` | `GET /weeks/{date}` | the `Week` with the date |
| `listWeeks` | `GET /weeks` | a page of `WeekSummary` items, the newest first |

- the operations need no token. All data is public. See decision 00000.
- they have no `x-rate-limit`, like the other read operations
- `date` of `getWeek` has the form `YYYY-MM-DD`. It must exist and be `1900-01-01` or later.
- a date after the current week gets `404`. A week without entries gets `200` with empty sections.
- `listWeeks` has `limit` and `cursor`. It lists only the weeks with entries. The current week is in it when it has entries.
- `WeekSummary` has `start`, `end`, `counts`, and `lead`

## The rules

All rules use the entries that were written in the week. `created` gives the week of an entry, as on the table of decision 00009.

The lead:
1. A shared subject has entries of two or more reviewers in the week. The shared subject with the most reviewers is the lead. Its entries of the week are the entries of the lead, the newest first.
2. Without a shared subject, the lead is the entry with photos and the highest overall.
3. Without photos in the week, the lead is the entry with the highest overall.

- in rule 1, a tie goes to the higher average overall of the entries of the week. The next tie goes to the newest entry.
- in rules 2 and 3, a tie goes to the newest `created`, then to the higher ID
- one reviewer with two entries of a subject does not make it shared
- the headline of a shared lead names the reviewers, such as "Priya and Marco both went to Luigi's Trattoria"
- three or more reviewers get "all", such as "Priya, Ana, and Marco all cooked Shakshuka". The verbs are "went to", "cooked", and "tasted".
- the photo of the lead is the first photo of the newest entry with photos

The top scores:
- each kind has the entry with the highest overall that is not in the lead
- the order is "Ate out", "Cooked", and "Tasted". A kind without such an entry is not there.
- a tie goes to the newest `created`, then to the higher ID
- the rows show no rank numbers

New on the table:
- the restaurants, recipes, and pantry items with a `created` in the week, the newest first
- a subject without reviews is also there

The counts:
- `entry_count` is the sum of the three review counts
- `reviewer_count` counts each user with an entry one time
- the UI writes "14 entries from 6 people · 3 new on the table". It leaves out a count of zero.

The memories:
- a memory has a `date` from the Monday to the Sunday of the week in an earlier year
- 29 February shifts to 28 February in a year without it
- the memories come from at most three years, the nearest first. Each year has at most two memories.
- in the current week, a memory with the month and the day of `today` goes first in its year
- then a memory with photos goes first, then the higher overall, then the newest `created`
- the UI shows "On this day" on these memories of the current week. On a finished week, no memory has it.
- the memories use `date`, not `created`. A memory is about the meal.
- a user writes a review today of a dinner of 2023. It is a memory of the week of that dinner.
- the memories are the same for all readers. The digest has no part for one reader.

## The server

- the new package `app/api/internal/weeks` has the week, the counts, and the rules. `publicapi` and `webui` use it.
- its functions get the time of now as a parameter. The Go tests then use fixed weeks.
- `weeks` reads the reviews and the subjects of all kinds through `subjects.All`. A new kind then joins the digest.
- `listWeeks` reads the `created` of all reviews and groups them by week in Go. SQLite has no time zones.
- this grouping is fast for a few thousand reviews. The backlog expects a low number of users.
- the memories use one query of each kind with the ranges of the earlier years
- the server makes each week on each request. It has no cache. The digest then shows each edit and each delete at once.
- `publicapi` changes the result to the types of `specs/api/sdk/server/go`. The spec validation of responses checks it. See decision 00001.

## Link previews

- `webui` writes the page meta of `/weeks/{date}`. See decision 00002.
- the title is "Week of 16 November 2026". `og:type` is `article`.
- the description has the counts, such as "14 entries from 6 people: 6 visits, 5 cooks, and 3 tastings."
- the description of the current week starts with "So far".
- `og:image` is the photo of the lead. A week without photos has the image of the site.
- `og:url` has the Monday of the week
- a week after the current week gets `404` with the default `index.html`
- `/weeks` keeps the tags of the site, like the other list screens
- the `<title>` of the page meta has the format of `usePageTitle`, as for the other pages

## The UI

- `TablePage.vue` at `/` loads `getCurrentWeek`. It then loads `getWeek` with `previous` for the digest.
- `WeekPage.vue` is at `/weeks/:date`. `WeeksPage.vue` is at `/weeks`.
- "Every entry" uses `useEntries` with `created[gte]` of `starts_at`, `created[lt]` of `ends_at`, and `sort=-created`
- the short rows use `EntryCard` with `variant="row"`
- `lib/weeks.ts` has the two headlines of the lead, the average of a shared lead, and the texts of the counts
- it also has the group headings of the memories
- `start`, `end`, and `today` are dates without a time. The UI reads them with `parseDay`. See decision 00005.
- `groupByWritten` of `lib/entries.ts` goes away. The journal keeps `groupByMonth` and `useEntries`.
- `specs/design-system` gets the lead, the top scores, "New on the table", the memories, and the week rows
- the words of the design system change. "Past week" and the month groups go away.
- the home gets "This week so far", "Last week", and "A year ago this week"
- the digest page gets "Week of 16 November" and "Every entry"

## Corner cases

- the first week of an install has no finished digest. The home then shows only "This week so far".
- an edit of a score after the end of a week can change its lead. A delete can remove the lead. The digest shows the new result.
- an edit does not move a review to another week. `created` does not change.
- an entry written on Monday at 00:30 CET is in the new week. In UTC, it is 23:30 on Sunday.
- the week of 28 December 2026 to 3 January 2027 has memories of two months. They are from December and January.
- the first year of an install has no memories. Old reviews that a superuser adds in the dashboard can fill it. See the backlog.

## Changes to other decisions

When this decision is implemented, these decisions change with it:

| Decision | Change |
|---|---|
| 00002 | the page meta of `/weeks/{date}` and its row in the table of the pages |
| 00009 | the concept "Table", the home, the two sections "The table", and `?kind=` on the home. The table sections point here. |

`AGENTS.md` gets the tag `weeks`, the package `weeks`, the pages, and `REVIEWS_TIME_ZONE`.
The design system README gets the new words and components.
The backlog item "redesign or rename table concept" is done.

## Work order

1. the spec, the generated SDKs, the package `weeks`, the operations, the time zone, and the page meta. One pull request.
2. the home, the digest page, the archive, and the design system. A second pull request.

Each pull request has its Go, unit, and end-to-end tests.

- the Go tests check the rules with fixed data and a fixed time of now
- they check the ties of the lead and the top scores
- they check the memories of 29 February and of the turn of the year
- they also check the weeks of the summer time changes and the `404` after the current week
- a Go test checks that `/weeks/current` reaches `getCurrentWeek`, not `getWeek`
- all projects of one end-to-end run share one app and one week. The end-to-end tests check only their own entries.
- an end-to-end test finds its entry in "Every entry" by a name from `uniqueTag`

# Alternatives
[alternatives]: #alternatives

| Alternative | Result |
|---|---|
| The digest in the browser from the review lists | Not selected. The memories need a `date` filter and nine requests. The link previews need the lead rule in Go. |
| The time zone of the browser | Not selected. A shared link must show the same digest to each reader. |
| Weeks in UTC | Not selected. An entry of Monday at 00:30 CET would be in the week before. |
| A time zone for each user | Not selected. The group lives in one time zone. |
| The live digest of the current week as the home | Not selected. Its lead would change all week. The home shows the last finished digest. |
| Saved digests from a weekly job | Not selected. Each edit and each delete of a review would need to change them. |
| A weekly email | Not selected. The group talks in its own chats. A person shares the link of a digest there. |
| The ISO week in the URL, such as `2026-W47` | Not selected. A date is easier to read. ISO week years also have corner cases. |
| The top score first as the lead rule | Not selected. A shared subject tells more about the group. |
| The overall of one entry on the stamp of a shared lead | Not selected. The stamp would show one entry as the score of the subject. |
| A headline only for a shared lead | Not selected. Each row of the archive needs a headline. |
| `saffron` for the tag "On this day" | Not selected. The design system keeps `saffron` for the score stamp. |
| Rank numbers on the top scores | Not selected. They make the digest a leaderboard. People then avoid honest low scores. |
| An average score of the week | Not selected. A low average would read like a bad week. |
| `KindSwitch` for the whole digest | Not selected. The digest shows all kinds on purpose. "Every entry" has it. |
| Memories of one exact day | Not selected. A small group has few entries on one day. A week has about seven times more. |
| Memories of one month or six months ago | Not selected. Memories are whole years. |
| Memories by `created` | Not selected. A memory is about the meal. It uses `date`. |
| The own memories of the reader first | Not selected. The digest is the same for all readers. |

# Unresolved questions
[unresolved-questions]: #unresolved-questions

- `WeekEntry` is the first `discriminator` of the spec. The first pull request checks the generated Go and TypeScript code. If a generator fails, the response gets one array of entries for each kind.
