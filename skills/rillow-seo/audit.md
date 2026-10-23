# Audit an existing site

Use this guide when the user wants an existing site reviewed or fixed, or when its search traffic or rankings dropped.
You end with a short, prioritised list of fixes, and then you make the ones the user approves.

## Pick the shape of the audit

| The user asks | What you do |
|---|---|
| Traffic or rankings dropped | Gather the evidence, run the drop triage below, and stop at the first confirmed cause |
| Review the site, or get more search traffic | Gather the evidence, audit the layers in order, then look for quick wins |
| Why one page does not rank or is not indexed | Gather the evidence for that page, then audit the layers in order for that page and its template |

Take the narrowest shape the request allows, because a full audit buries one urgent problem.

## Gather the evidence first

1. Call get_project.
   Read the memory, the plan, which data sources are connected, the top ranked keywords and the competitors.
   If Rillow is still pulling data, say so and read the ranked keywords later.
2. Call find_keywords without a seed, with limit 100, because the default is 25.
   It returns the keywords the site ranks for, with position and URL, as of when the project was created.
   Use them to choose pages to check, and take current numbers from the Search Console export.
3. Call get_performance.
   It returns site totals for 28 days against the 28 before, and each published plan post with its position and clicks, but no other pages.
   If its window ends more than a few days ago, ask the user to open the Traffic page of the Rillow dashboard, which fetches Google's latest data when it opens, then call it again.
4. Ask the user for a Search Console export.
   In the Performance report for Search results, set the date to compare the last 3 months with the previous period, and export the Queries and Pages tables.
   For a drop, also ask for the last 16 months by week, so you can see seasonality.
   If Search Console offers the branded queries filter, ask for a second export with Non-branded selected.
   Ask what the Manual actions, Security issues and Page indexing reports show, with the example URLs for each indexing reason.
   In claude.ai, ask the user to paste or upload the files.
   If the user cannot, continue and list them under what you could not check.
5. Read the repository, if you have one.
   Find where titles, descriptions, canonicals, robots rules, the sitemap, redirects and 404 pages are produced, and which routes render on the server.
   Read the recent commits that touched those files.
6. Fetch the live site as Googlebot, as the rules say.
   Fetch robots.txt, the sitemap, the home page, and one or two URLs of each template.
   For each URL, record the status code, any redirects, the title, the meta description, the robots meta tag and X-Robots-Tag header, the canonical, the main heading, and whether the main content is in the HTML.

## Reconcile the repository with production

If the live home page title, description or main heading differs from the source, ask the user which one is current before you edit anything.

Old search demand describes the past, not where the product is going.
If the memory or the live site shows the product changed, queries about the old product are legacy.
Decide per page whether to keep it, redirect it to an equivalent current page, or retire it, based on its clicks and the memory.
Do not optimise pages for demand the product no longer serves.

## Audit in this order

Each layer depends on the ones before it, so when an earlier layer has a problem, fix it before you judge the later layers for the affected pages.

| Layer | What to check | Evidence |
|---|---|---|
| 1. Manual actions and security issues | Any manual action or security issue | The user's Search Console reports. Rillow cannot see them |
| 2. Indexing | robots.txt rules, noindex in meta tags or headers, canonicals, status codes, redirect chains, sitemap contents, main content in the server HTML, Page indexing reasons | Live fetch, repository, the user's Page indexing report. The technical guide has the fixes |
| 3. Search intent | Whether each important page is the type of page Google shows for its main search, and whether two pages compete for one search | get_serp or get_brief for the page's keyword, Search Console queries |
| 4. Technical quality | Core Web Vitals, mobile rendering, HTTPS, intrusive interstitials, scripts that hijack the back button, structured data that matches visible content, duplicate URL variants, unique titles and descriptions | Live fetch, PageSpeed Insights, repository |
| 5. Content | Whether pages are helpful and accurate, outdated facts, thin pages, comparisons that follow Google's reviews guidance | Reading the pages, Search Console trends |
| 6. Links | Internal links first: orphan pages, links to pages close to page one. Then external links | Repository or the live pages, the Search Console Links report from the user. The links guide has the fixes |

Notes on the layers:

- A manual action is the diagnosis, so work on it before anything else.
- "Crawled, currently not indexed" usually points to quality or duplication (a practitioner reading, Google does not say why), so improve or merge the page instead of resubmitting it.
  "Discovered, currently not indexed" means Google postponed the crawl, usually because it expected to overload the site.
- Your live fetch shows what a page declares.
  URL Inspection, through the user, shows what Google indexed and which canonical it chose.
- For a plan post whose brief was fetched in the last 7 days, read get_brief, which is then free.
  Otherwise use get_serp, 0.03 to 0.06 credits, while an uncached get_brief uses up to 0.3.
- For Core Web Vitals, follow the technical guide.
  The PageSpeed Insights API always returns lab data, adds field data only when Chrome has enough traffic for the page or site, and may fail with 429 without a key.
  Report lab data as lab data, not as a ranking problem.
- Run Google's self-assessment questions on the most important pages and on the pages that lost the most clicks: https://developers.google.com/search/docs/fundamentals/creating-helpful-content

## Count problems per template

A defect in a template, such as the blog post layout, is on every page built from it, and one fix in the component fixes them all.

- Fetch one or two URLs per template, not every URL.
  The sitemap or the routes in the repository give you the list.
- Report a template problem once: the template, one example URL, the number of pages it covers, and the file that produces it.
- Judge content page by page.

## If traffic or rankings dropped

Work through these steps in order and stop at the first cause you can confirm with evidence.

1. Confirm the drop in Search Console.
   If Analytics fell and Search Console clicks did not, search did not drop.
   Look at tracking (a removed tag, a consent banner change, a filter) or at other channels.
2. Ask the user whether the Manual actions or Security issues report shows anything.
   If one does, that is the diagnosis.
3. Find the shape of the loss with the table below.
   Compare the last 3 months with the previous period and with the same months last year, and sort pages by the clicks they lost.
4. Check whether the pages that lost the most are still indexable.
   Fetch them live for status, noindex and canonical, and ask the user for their URL Inspection results.
5. Check recent deploys.
   Read the commits around the drop date for changes to robots.txt, noindex, redirects, canonicals, rendering or templates.
6. Compare the drop date with Google's update dates on the Search Status Dashboard: https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history
   A drop that lines up with a core update means Google reassessed the site, not that a page broke.
   Judge it only after the rollout ends.
   A small drop, like position 2 to 4, needs no drastic action, and Google advises against changing content that already performs well.
   A large drop across many searches, like position 4 to 29, calls for a self-assessment of the whole site.
7. Read the current results for the lost searches with get_serp to see a new page type, a featured snippet or People Also Ask above the site.
   It shows an AI Overview but not ads, so ask the user to search the top lost queries in a private window and say which ads appear.
8. Rule out seasonality and changing demand with the 16-month view and the same period last year.
   Check the top lost queries in Google Trends.
9. Ask the user to check the Search Console Links report for lost links.
   Rillow's domain rating dates from when the project was created, so it cannot show them.
10. If nothing is conclusive, read the pages that now outrank the site for 10 to 20 lost searches.
    List concretely what they have that the site lacks.

| Shape of the loss | Most likely cause |
|---|---|
| One page, a few queries | A stronger competitor, a change in intent, or a newer page of the site competing for the same search |
| One page, all its queries, suddenly | A noindex, canonical change, 404, redirect or template change on that page |
| Whole site, suddenly | robots.txt, a site-wide noindex, server or DNS errors, an HTTPS failure, or a manual action |
| Whole site, gradually, during an update's rollout | A core or spam update |
| Impressions steady, clicks down | The title or snippet, or a change on the results page such as an AI Overview, ads or a richer result |
| One section or template | A template change, or Google reassessing that type of page |

Google's guide to traffic drops has more detail: https://developers.google.com/search/docs/monitor-debug/debugging-search-traffic-drops

## Prioritise

Score every finding, then sort from high to low.
The score orders the work, it does not forecast results.

`priority = traffic at stake × confidence ÷ effort`

Take traffic at stake from data:

| Finding | Traffic at stake | Source |
|---|---|---|
| A page that lost traffic or could lose it, such as a noindex, wrong canonical, 404 or redirect | Its clicks in the last 3 months, or in the 3 months before the drop | Search Console export, get_performance for plan posts |
| A page close to page one, or with a low click rate | Its impressions and clicks in the last 3 months | Search Console export |
| Any page, when there is no Search Console data | The volume and position of the keywords the URL ranks for | get_ranked_keywords with the URL, or find_keywords without a seed (dated to when the project was created), labelled as DataForSEO estimates |
| A template defect | The sum over the pages on that template | As above |

To estimate what a page could gain, multiply its impressions by the click rate the site already gets at a better position, both from the export, and show the inputs as the rules say.

Confidence is a heuristic: about 0.9 when the mechanism is direct, such as a page blocked from indexing, about 0.5 for a likely cause such as an intent mismatch, and about 0.2 when you cannot name how the fix would move traffic.
Effort is the user's time in hours or days, so ask the user when you do not know it.

For example, a stray noindex on 12 docs pages that earned 2,400 clicks in the 3 months before scores 2,400 × 0.9 ÷ 0.5 days = 4,320.

Keep findings with little traffic at stake out of the top 5.
A common starting point is a floor of 100 impressions a month.
The exception is a page the business depends on, such as pricing or signup.

## Quick wins on an existing site

Look for these before you propose any new page.

| Signal | How to find it | The change |
|---|---|---|
| A page at about positions 4 to 20 (a practitioner heuristic) with real impressions | get_performance with queries true, the Pages and Queries export, or find_keywords with position_min 4 and position_max 20 as a dated starting list | Improve the page against the current top results, rewrite its title and description, and add 2 or 3 internal links to it from related pages that already get traffic |
| A page has impressions but a click rate far below the site's own rate at that position | The Queries and Pages export, compared with the site's average click rate at the same position | Check get_serp for an AI Overview and ask the user about ads, which it does not show, then rewrite the title and description to say what the page offers |
| Two pages compete for one search | Search Console filtered by that query, Pages tab: two URLs trading impressions over weeks for queries with the same intent | Merge into the stronger page, redirect the weaker one to it with a 301 or 308, update internal links, and drop the old URL from the sitemap |
| A post is decaying | Clicks down against the previous period and the same period last year, not explained by seasonality or an update | Refresh it with the content guide. The measure guide says which posts to refresh first |
| A page is thin or outdated | Few or no impressions for months, or facts that are no longer true | Improve it, merge it into a stronger page on the same intent, noindex it if it helps visitors but not searchers, or remove it as the last resort |

- Compare click rates with the site's own numbers, not with published click curves, because rates vary a lot between sites and results pages.
- Two pages ranking for one query is fine when they serve different intents.
  Read the queries each page earns before you merge anything.
- Do not fix competing pages with noindex, with a canonical between pages that are not duplicates, or by deleting one.
- Redirect a removed page only to an equivalent page.
  A page with no equivalent returns 404 or 410.
  If you redirect it to the home page, Google may treat that as a soft 404.

## Write the report

Use this structure:

1. Verdict: one or two sentences on what limits the site most and what it is worth.
2. Top 5 fixes, highest priority first.
   For each: the URL, or the template with its page count, and what rule 6 asks for, with the source of each piece of evidence.
3. Everything else, grouped by type (indexing, intent, technical, content, links), one line each.
4. What you could not check, and why, such as no Search Console export or no URL Inspection results.

A good finding reads like this:
"/pricing sends `X-Robots-Tag: noindex` (live fetch, 2026-10-06), and its clicks fell from 410 to 12 between the last two 3-month periods (Search Console). Remove the header in `middleware.ts`. Done when the live fetch shows no noindex and URL Inspection shows the page indexed."

- Give every finding one outcome with a reason: fixed, deferred or not needed.
  Do not drop a finding silently.
- Treat a noindex, canonical or robots rule as an observation until the user confirms it is a mistake, because it may be deliberate.
- Missing data is a finding.
  If Search Console is not connected, say so near the top, because nobody can prioritise well without it.

## Make the fixes

1. Ask the user which fixes to make.
2. Make the approved changes in the repository, the way the site already does them.
   Fix a template defect in the template, not page by page.
3. Keep each page's change small and datable.
   Several changes to one page in the same week cannot be judged apart later.
4. Check the build and then the live pages as the rules say, including the sitemap and robots.txt the build produces.
   For the most important pages, suggest the user request indexing in URL Inspection, which has a daily quota, and resubmit the sitemap if it changed.
5. Record each shipped change with remember as a note: the date, the URL, what changed and why, and when to judge it.
   For example: "2026-10-06: merged /blog/x into /blog/y with a 301, to target 'z'. Judge after 2026-11-03."
   Put fixes that shipped together in one note, because get_project shows only the 30 newest notes.
   Suggest the user add the same note as a Search Console annotation.
6. Tell the user how long the change usually takes and why, from the measure guide.

## Mistakes to avoid

- Working on the content or links of a page that is noindexed, blocked, or canonicalised to another URL.
- Listing warnings that cost no traffic, or treating title and description length as rules.
  Long ones get cut on screen, which can affect clicks, and that is all.
- Treating DataForSEO positions, volume or domain rating as Google's data, or a missing row as zero.
- Changing many things at once after a drop, so nobody can tell what worked.
- Disavowing links after a core update.
  Disavow only when many spammy links caused, or will likely cause, a manual action.
- Spending get_serp on every keyword instead of the pages that matter.
- Optimising for searches the product no longer serves.
