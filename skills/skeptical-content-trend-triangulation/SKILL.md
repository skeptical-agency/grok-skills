---
name: skeptical-content-trend-triangulation
description: "Pick daily content topics by triangulating Trends RSS, Techmeme, and trade press; never trust one AI search. Use when choosing today's news topics for a channel, newsletter or shorts pipeline, or when an AI search tool claims a real story does not exist."
when-to-use: "Choosing today's news topics for a channel, newsletter or shorts pipeline, an AI search tool claims a real story \"doesn't exist\", or you need a repeatable daily research procedure."
---

# Content trend triangulation (daily topic selection)

## What this does (plain English)

AI-powered search tools can be confused about the date and will confidently say a real, hours-old story does not exist. This skill gives your AI assistant a repeatable daily way to pick topics instead: check Google Trends RSS for volume, Techmeme for what matters in tech, trade press in your lane, then a web search with an explicit date, and use X only for reaction. It never rejects a topic because one AI search tool denied it, and it requires at least two independent non-LLM sources first. It also covers how to choose between candidates and how to log the pick and the runner-ups.

## Responsible use

Use public feeds and pages politely and follow each source's terms. Verify a story against its primary source before you publish it, rather than quoting an aggregator.

## Install what you need

Nothing to install. The one command uses `curl`, which is included on macOS, current Windows 10 and 11, and most Linux systems.

## Worked example

Pick today's topics using the source stack below, in order.

1. Pull the Google Trends RSS feed and read the `<title>` entries (replace `<browser UA>`). Filter hard by your lane: it is a volume signal, not an editorial one.

```
curl -s -A "<browser UA>" "https://trends.google.com/trending/rss?geo=US"
```

2. Read Techmeme (the river plus the story-ranking headers) for what actually matters in tech today.
3. Check trade press for your lane. When 3+ outlets in the same vertical cover a story within hours, it is the story of the day.
4. Run a web search with an explicit date in the query ("top tech news today <Month D YYYY>").
5. Use X search last, for reaction and color only.
6. Score each candidate on size, lane fit, freshness, confirmation, balance against yesterday, and content policy. Log the chosen topic and why, plus the runner-ups you rejected with one-line reasons.

---

The full production recipe follows.


## When to Use
- Choosing today's news topics for a channel/newsletter/shorts pipeline
- An AI search tool claims a real story "doesn't exist"
- You need a repeatable daily research procedure, not vibes

## The golden rule
NEVER treat a single AI-powered search tool's denial as a negative signal.
LLM-backed search tools can be date-confused (they believe the current year is
the future) and return confident "No such announcement exists" answers for
REAL, hours-old stories. Triangulate with at least two independent
non-LLM sources before rejecting a topic.

## Source stack (in order)

1. Google Trends RSS: 

   ```
   curl -s -A "<browser UA>" "https://trends.google.com/trending/rss?geo=US"
   ```
   Parse <title> entries. Caveat: heavily polluted with sports/celebrity/
   politics - filter by your lane; it's a volume signal, not an editorial one.
2. Techmeme: the river (techmeme.com/river) plus the story-ranking headers.
   Best single "what actually matters in tech today" signal; multiple
   outlets converging on one story = real trend.
3. Trade press RSS/direct: MacRumors, IGN, PC Gamer, Eurogamer, Kotaku etc.
   per your lane. When 3+ outlets in the same vertical cover a story within
   hours, it's the story of the day.
4. General web_search WITH an explicit date in the query ("top tech news today
   <Month D YYYY>") to catch anything the above missed.
5. X/Twitter search last (if at all): date-staleness makes it unreliable for
   news discovery; use it for reaction/color only.

## Selection criteria (per candidate)
- Is it the biggest story of the morning in your lane? (size)
- Lane fit: does it match your channel's beats? Explicitly reject off-lane
  topics (e.g. a tech channel skips politics/sports entirely).
- Freshness: reject stale stories resurfacing (check original publish date -
  a June story trending again in October is not news).
- Confirmation: reject thin/unconfirmed single-source leaks unless the leak
  itself is the story.
- Balance: rotate against yesterday's picks (don't run the same sub-lane two
  days straight; pair contrasting topics across a day's slots).
- Content policy: reject anything violating your channel's banned topics.

## Discipline
- LOG the decision: chosen topic + why, plus runner-ups REJECTED with one-line
  reasons. This is what makes a daily pipeline auditable and improves
  tomorrow's picks.
- When a topic involves AI-generated content or controversy, verify the primary
  source statement directly (article fetch), don't quote the aggregator.
- Run the same stack at each daily slot; morning and evening trends differ.
