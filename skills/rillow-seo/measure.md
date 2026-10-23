# Measure results

Use this guide after a change ships, when you review how the site and its posts are doing, or when the user asks how long something takes.
You end with an honest answer about whether a change worked, or that it is too early or too small to tell, and with the posts to refresh next.

## Read get_performance

Call get_performance without card_id for the whole site.

- Site totals: Search Console clicks, impressions and average position, and Analytics sessions and users, for the 28 days ending on the latest day Rillow has synced, against the 28 days before.
  If that day is more than a few days ago, ask the user to open the Traffic page of the Rillow dashboard, which fetches Google's latest data when it opens, then call get_performance again.
  The Search Console totals cover web search only.
  Analytics users are daily users added up, so they count a returning person more than once.
  Compare them only with the same number from another period.
- Published posts, biggest position drop first, then most clicks.
  Each row has the post's keyword, its position now, 7 and 28 days before and its best in 90 days, its movement, and the page's Search Console clicks and impressions in the same 28 days.
  ">100" means not in the top 100, and "-" means no check.
  Movement compares the latest position with the one 28 days before, or 7 days when there is no older check, and "flat" means within one position.
  "new" means there is no earlier check to compare with.
  The list stops at 50 posts.
- Only plan posts marked published are tracked.
  For any other page, use the user's Search Console export.

Call get_performance with queries true for the site's Search Console queries at an average position of 4 to 20 over the same 28 days, most impressions first.
They show on page one or two already, so the page that ranks for each is the one to improve.
The table marks queries already in the plan and shows Rillow's volume when it has one.

Call get_performance with a card_id for one post: its daily positions for 90 days, and a weekly table of Search Console clicks, impressions and average position with Analytics visitors.
Weeks start on Monday, and the first and last weeks can be partial, so leave partial weeks out of any comparison.

The two positions are different measures, so do not compare one with the other:

| | Rillow's daily position | Search Console average position |
|---|---|---|
| What it is | One desktop check a day, through DataForSEO, of Google's results for the post's keyword in the project's country. It counts every result block, so its number can be larger than the organic position get_serp shows | The topmost position of the site, averaged over every impression of every query the page showed for |
| Good for | Following one keyword over time | The page's real exposure across all its queries |
| Limits | One query, one check, not what every searcher sees | Moves when the mix of queries changes |

The site totals include branded searches, so they cannot show on their own whether SEO work grew traffic.

## What Search Console numbers mean

- Clicks and impressions are the measures that matter.
  Google calls them "ultimately the measure of success".
- Average position is the topmost position of the site for each query, averaged across impressions, and it is recorded only when the site got an impression.
  When a page starts showing for new long-tail queries far down the page, its average position gets worse while its clicks rise.
  Report positions per query, never the site average as a result.
- An AI Overview takes one position, and every link in it gets that position.
  Clicks and impressions from AI Overviews and AI Mode are counted in the web totals.
- Rare queries are anonymised.
  They are left out of the query table but kept in the chart totals, so the rows add up to less than the total, and any query filter drops them too.
- A missing value is not zero.
  The export writes "-" and "~" as zeros, and a page with no row may have had impressions that a row limit cut off.
- The newest days are preliminary and can still change.
  End every window on a complete day, as get_performance does.
- Search Console keeps 16 months of data.
  For year-over-year work, ask the user for the export before the older months expire.
- The interface exports at most 1,000 rows per table.
  On a larger site the export misses part of the long tail, so say so before you draw conclusions from it.
- Web, image, video and news searches are separate search types, and multimodal searches (Lens, Circle to Search) are reported apart since September 2026.
  Compare the same search type in both periods.
- The branded queries filter splits branded from non-branded searches on eligible sites, using Google's own classification.
  Without it, ask the user to filter out queries that contain the brand name.
- Search Console clicks and Analytics sessions never match exactly.
  Do not treat the gap as a problem.

Google explains clicks, impressions and position in detail at https://support.google.com/webmasters/answer/7042828

## Prove a change worked

1. Find the change and its date in the memory from get_project.
   If it is not there, check the commits and ask the user when it went live.
2. Pick two windows of equal length, in whole weeks, so both cover the same days of the week.
   Use at least two weeks each, and more for a page with little traffic.
   The before window ends the day before the change went live.
   Leave out the days between the deploy and Google's recrawl when the user can see the crawl date in URL Inspection.
3. Filter to the changed pages and their queries.
   Use the weekly table from get_performance with the post's card_id, or a Search Console export filtered by the page.
4. Leave out branded searches when you judge SEO work.
5. Compare with similar pages that did not change, such as other posts on the same template.
   If they moved the same way, the season, an update or the whole site moved them, not the change.
6. Check the windows for anything else that moves traffic.
   Look for Google updates on the Search Status Dashboard (https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history), seasonality in the same weeks last year, other deploys, and tracking changes.
   After a core update, wait a full week after it finishes, then compare with a week before it started.
7. Decide on one verdict: worked, no clear change, made it worse, too early, or too small to tell.
   For a page with few clicks, look at its weekly numbers before the change.
   If the after weeks stay inside the range of the before weeks, call it too small to tell.

Report it with the windows, the source and the comparison, and say that timing does not prove the cause.
For example: "Non-branded clicks to /blog/x rose from 84 to 131 between two equal 4-week windows (Search Console), while five similar posts stayed flat. The rise started after the title change shipped on 2026-09-08, which fits the change but does not prove it caused the rise."

- A percentage change from zero cannot be computed, so give the before and after counts.
- Do not turn a change into a yearly figure or a forecast.
- Do not judge a change from day-to-day positions, because daily swings are larger than most effects.

## How long each change takes

Google removed its "four months to a year" estimate from its page on hiring an SEO in June 2026, so do not quote it as Google's.
Google's general advice is that changes can take hours to several months to show, and to wait a few weeks before you judge one.

| Change | When you can judge it | Why |
|---|---|---|
| robots.txt edit | Google uses the new file within about a day, and pages change as they are recrawled | Google caches robots.txt for up to 24 hours |
| Title or meta description | The new title shows a few days to a few weeks after the recrawl, then judge on at least two full weeks of data | Google changes the title link only after it recrawls the page, and the click rate needs enough impressions to read |
| Removed noindex, or a new page on an established site | Crawling and indexing take a few days to a few weeks, positions settle over weeks to months | Google must find and crawl the page first. Requesting indexing in URL Inspection can shorten the wait for key pages |
| Canonical fix | A few weeks | After the fix, Google may keep the pages in a duplicate cluster for up to two weeks |
| Redirects, merged pages or changed URLs | A few weeks for a medium site, longer for a large one | Google must recrawl every old URL to move its signals |
| Refreshed or new post | Wait a few weeks, and expect some posts to take months | Google recrawls the page, then ranks it again against the others |
| Site-wide quality work after a core update | Several months, sometimes the next core update | Google's systems take months to confirm a site now has helpful content. Smaller core updates that Google does not announce can show it sooner |
| Core Web Vitals fix | Lab results at once, field data after about 4 weeks | Field data is a rolling 28-day window of real visits |
| Internal links | After the linking pages are recrawled. Google documents no timing | Google counts a link when it crawls the page that holds it |
| Search generative AI setting in Search Console | 1 to 2 days | Google's documented delay |
| robots.txt rule for an AI search crawler | About a day for ChatGPT search and Perplexity | OpenAI and Perplexity document up to 24 hours |
| New site | First impressions in days to weeks, meaningful non-branded clicks usually after months | Discovery is fast, but a new site has few links and no history |

Ahrefs' study (updated May 2025) found that only 1.74 percent of newly published pages reached the top 10 within a year.
Use it to set expectations, not to predict a page.

## Pick what to refresh next

Refresh a post before you write a new one for the same search, as the rules say.
Read get_performance and, when you have it, the Search Console export, and look for these, in this order:

1. Posts that dropped.
   The movement is "down" and the position is clearly worse than 28 days before.
   First check whether the whole site dropped, with the drop triage in the audit guide.
   A small drop, like position 2 to 4, needs no drastic change, and Google advises against changing content that already performs well.
2. Posts stuck at about positions 4 to 20 (a practitioner heuristic) for weeks, with real impressions.
   They already show for the search and need a better page, a better title or 2 or 3 internal links to reach the top results.
3. Posts with impressions but few clicks.
   A click rate far below the site's own rate at that position points to the title and description, or to a results page with an AI Overview or rich results above the post.
   get_serp shows an AI Overview with the sites it cites, a featured snippet and People Also Ask, but not ads.
4. Old posts on topics that changed.
   The product, prices, screenshots or facts are out of date, or the current results now show a different type of page.

Rank the candidates by the traffic at stake, which is their impressions and clicks, and by their value to the business.
Pick one to three at a time, so each refresh can be judged on its own.

For each chosen post:

1. Call get_brief with refresh: true and the post's card_id.
   It adds the post's rank history and the site's Search Console queries from the last 28 days that contain the keyword.
   Those queries are site-wide, not for that page alone.
   An uncached call uses up to 0.3 credits, for the results plus up to five pages it measures, and is then free for 7 days.
2. Read the content guide and rework the post from the brief.
3. After it ships, update the visible date and dateModified only if the content changed substantially.
4. Record the refresh in the change log below.

## AI visibility

- The Generative AI performance report in Search Console shows impressions in AI Overviews and AI Mode by page, country, device and date.
  It shows no queries and no clicks.
  Clicks from these features are counted in the main Performance report.
  Ask the user to export it.
  It tells you which pages appear in AI features and the trend, not for which questions.
  Details: https://support.google.com/webmasters/answer/16984139
- get_serp returns Google's AI Overview for a search, with the sites it cites, or null when Google shows none.
  One call is one sample, and the answer changes between requests.
- For ChatGPT, Perplexity, Claude and Gemini, no report shows what they answer.
  The only way to measure their answers is to ask the same fixed prompts several times on each assistant and report a rate, such as "cited in 3 of 10 runs".
  Record the date, the assistant and the exact prompts, and repeat them later to compare.
- In each run, record whether one of the site's own pages is among the sources.
  Studies find a brand is mentioned mostly when one is, so a brand that is never a source needs ranking and crawl access work first, not new wording.
- Answers change between runs and between models.
  Never report an AI visibility score, and never claim a change caused a citation.

The ai-search guide covers the prompt protocol in full, Bing's AI Performance report, AI referral traffic, and how to improve AI visibility.

## Keep a change log

Later sessions can judge a change only if they know what changed and when.

- After each change ships, add a note with remember.
  Write the date, the URL, what changed, the target search, and when to judge it, in one or two sentences.
  For example: "2026-10-06: rewrote the title and description of /blog/x for 'y'. Judge after 2026-10-27."
- Suggest the user add the same note as an annotation in Search Console.
  An annotation holds up to 120 characters and everyone with access to the property sees it.
- At the start of a measuring session, read the change notes in get_project.
  Judge each one whose date has passed, then save the verdict as a new note, such as "2026-10-06 title of /blog/x: no clear change after 4 weeks", and remove the old note with remember and remove_note.
- A note holds at most 300 characters and a section 1,500, and get_project shows only the 30 newest notes.
  If notes_left_out is set, older change notes are hidden, so ask the user for earlier change dates or read the commit log.
  Keep each verdict to one line, or remove the note, so old notes do not crowd out the rest.
- Change one thing per page per window when you can.
  Two changes to one page in the same weeks cannot be judged apart.

## Mistakes to avoid

- Reporting a better or worse site-wide average position as the result.
- Comparing windows of different lengths, partial weeks, or windows that end on preliminary days.
- Reading a missing row, "-" or a zero in the export as no traffic, or adding up query rows as the total.
- Comparing Rillow's daily position with Search Console's average position.
- Saying a change caused a rise because the rise came after it.
- Judging a change in its first days, or from daily rank swings.
- Counting branded searches as growth from SEO work.
- Promising a date, or quoting "four months to a year" as Google's estimate.
- Refreshing a post that performs well only because it is old.
