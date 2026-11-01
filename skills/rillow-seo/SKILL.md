---
name: rillow-seo
description: SEO for a website the user runs, with Rillow's search data, content plan and memory. Use it to set up a new site for search, audit and fix an existing one, pick keywords, write or refresh posts, build internal links, show up in AI answers, and check whether a change worked.
---

# Rillow SEO

Rillow holds the search data and memory for the user's site: keyword volumes and difficulty, live Google results, competitors, the content plan, published posts and how each one ranks.
You do the thinking, the writing and the changes to the site.
These guides say how to do SEO work well with that data.

The data comes from Rillow's MCP server, at https://rillow.ai/mcp.
If its tools (get_project, find_keywords, get_serp and the rest) are not available, ask the user to connect it first.
Sign-in is OAuth, and https://rillow.ai/seo-mcp-server has the steps for each agent.
Until then, work only from data the user gives you, such as a Search Console export, and say which steps you could not do.

Call get_project at the start of a session.
It returns what Rillow remembers about the site and names the guide that fits it.
Read that guide before you start that kind of work, not after.
Each guide is a file next to this one (keywords.md for keywords, and so on), and get_guide returns the same text.

## Guides

| Guide | Read it when |
|---|---|
| new-site | The site is new or gets almost no search traffic, or the user is launching one |
| audit | The user wants an existing site reviewed or fixed, or traffic or rankings dropped |
| technical | A change touches indexing, canonicals, redirects, sitemaps, robots, rendering or page speed |
| keywords | Before you propose topics, add plan items or decide which page targets which search |
| content | Writing or refreshing a post, or building a feature, comparison, best list, alternatives, integration or pricing page |
| links | Internal linking, or the user asks how to get backlinks |
| ai-search | The user asks about ChatGPT, Perplexity, Claude, Copilot, Google AI Overviews or AI Mode |
| agents | The user wants AI agents to find and use the site, or an audit reaches agent readiness |
| measure | After a change ships, when reviewing performance, or when the user asks how long something takes |
| weekly | The user asks for the weekly check, or a regular review of what to change next |
| writing | Before you write any text for the site: posts, pages, titles, descriptions, alt text or emails |

## Rules

These apply to every guide.

1. Take every number from Rillow's tools or the user's own data, such as a Search Console export, and never make one up.
   When you derive a number, like a click estimate, show the inputs and the arithmetic and call it an estimate.
   When the data is missing, say so.
2. Say where a number comes from and how old it is.
   Volume, difficulty, domain rating and the positions find_keywords returns are DataForSEO estimates, and the ranked keywords date from get_project's created_at.
   Search Console numbers are Google's own, as of the dates get_performance gives.
   get_serp and the daily rank checks are live.
3. Check the live site before you call anything missing or broken.
   Fetch the page as Google gets it, for example with `curl -sL -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" <url>`, and read the HTML.
   The repository can be ahead of production or behind it.
   On a 403 or a challenge page, retry with a browser user agent, since many CDNs block fake Googlebot.
   Only URL Inspection in Search Console shows whether the real Googlebot is blocked.
   Without a shell, a web fetch can drop status codes and head tags, so ask the user to run the command and paste the output.
4. Read the current Google results for a search (get_serp or get_brief) before you decide which page targets it.
   They show the intent, format and depth Google rewards.
5. Improve an existing page before you create a new one for the same search.
   Two pages for one search compete with each other.
6. Every recommendation names the exact URL and change, what it is worth according to the data, and how you will know it worked.
   "Improve the content" is not a recommendation.
7. Never promise rankings, traffic, indexing, AI citations or dates.
   Explain how the change works and give the usual range from the measure guide.
8. Ask before changes that are hard to undo: deleting or redirecting pages, changing URLs, editing robots.txt, adding noindex, or editing many pages at once.
   Show the list of pages first.
9. In the user's repository, follow how the site is already built.
   Check each change in the HTML a local build outputs, and the live page again after the user deploys.
   Without the repository, give the user the exact text to change on each page, ask them to tell you when it is live, then save the change note.
10. Save what you learn with remember: product, audience, voice and what to avoid in their sections, decisions and shipped changes as notes.
    Read the memory from get_project before asking the user something it already answers.
11. Report the findings that matter most first, at most five, each with what to do and its evidence.
    Group the rest below, and end with what you could not check.
    The first time you use an SEO term with the user, explain it in a few words, like "impressions (times Google showed the page)".
12. Leave a page alone until its last change is due, and never suggest deleting a page under about 6 months old.
    Draw no conclusion from under about 100 impressions a month (rules of thumb).
