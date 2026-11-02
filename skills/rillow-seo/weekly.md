# Weekly check

Read this when the user asks for the weekly check, or for a regular review of how the site is doing.
You end with a short list for the week: at most two changes to existing pages and at most two new pages, each with its numbers and a check day.
Most pages get "not due" or "leave it", and that is the expected result.

## Before you start

1. Call get_project.
   Read changes_due, changes_not_due, brand, the money_pages section of the memory, and plan items with update, which are pages the user picked on Rillow's Overview or Research page.
2. If money_pages is empty, ask the user which pages lead to signups, bookings or sales, and the search each should win.
   Their analytics or payment tool shows the pages people visit right before they buy.
   Save the list with remember, at most 20 pages.
3. If people search for the brand by names other than the ones under brand, save them with remember and brand_names.
   The non-branded numbers are only as good as this list.
4. Call get_performance without other arguments.
   Read the non-branded line, the posts table and the indexed column.
5. Without Search Console, say so, give the user the link get_performance returns, and do only the steps that use the daily rank checks.
   A site with no impressions yet has nothing to review, so write the next planned page instead, as the new-site guide says.

## Work through these in order

1. Judge the changes that are due.
   For each item in changes_due, compare before with now and decide as the measure guide says: worked, no clear change, made it worse, too early or too small to tell.
   Save the verdict as a note and remove the old note.
   A change that is stuck goes to one of the next steps: a better title, internal links, links from other sites, or more content.
2. Pages the user picked on Traffic.
   Plan items with update are the user's own picks, so they come before the steps below.
3. Posts that fell.
   In the posts table, a post 3 or more places below its best is a refresh candidate, unless it changed recently.
4. Searches close to page one.
   Call get_performance with queries true.
   For each of the top rows, check that the page in the page column has the exact search in its title and that 3 to 5 related pages link to it with that search as the anchor.
   Never make a new page for these: the page exists, and rule 5 applies.
5. Titles that get few clicks.
   Call get_performance with queries true, position_min 1, position_max 5 and min_impressions 200.
   A ctr well below site ctr means people see the page and pick another result.
   Rewrite the title as the content guide says, and change one title at a time so each can be judged.
6. Pages that lost clicks.
   Call get_performance with pages true and read the reason column before you touch any page.
   "fewer searches" needs nothing.
   "position fell" needs get_serp for the page's main search, to see what the page that overtook it does better.
   "click rate fell" needs get_serp, to see an AI answer or a new result type above the page.
   A page with a date in changed is waiting for its check day.
7. Pages on page two with many impressions.
   The pages view lists them after the table.
   Links decide these more than rewrites: internal links first, as the links guide says.
8. Once a month, look further out.
   Call get_performance with patterns true and pick one kind of search to build next, such as "vs" or "alternative".
   Read the money pages the pages view lists as not indexed.
   Ask the same fixed questions in AI assistants, as the ai-search guide says.

## Pick the week's list

Rank what you found by the traffic at stake (impressions and clicks), by business value (money pages first), and by effort, cheapest first: a title or internal links before a rewrite, a rewrite before a new page.

- At most two changes to existing pages and at most two new pages, a rule of thumb that keeps each change judgeable.
  Fewer is fine.
- On a new site, about one new page a week is enough.
  Never plan more than one a day.
- Skip any page whose last change is not due, and say "not due".
- Skip queries and pages with under about 100 impressions a month.
- Overrule the data when the business says so: searches people who could never buy make, a page for a product being retired, or a kind of search where the site has nothing better to say than the current top 3.
- When Google shows the home page or an unrelated page for a search that needs its own page, plan a new page.
  First list the searches the current page already gets clicks from, so the new page doesn't take them.

## Do the work

For each change, name the URL and the exact change, as rule 6 says, then make it.
After it ships, save a note with remember, with page and check_on from the measure guide's table.
For each new page, add it with update_plan, then write it from get_brief.

## Report to the user

Keep the report short:

1. The changes you judged and the verdicts.
2. This week's list, at most four items, each with its numbers and check day.
3. How many pages are not due, in one line.
4. Anything that needs the user: a page to approve, a money pages list, a Search Console connection.

## Mistakes to avoid

- A long list of recommendations.
- Changing a page again before its check day.
- Judging the week by total clicks, which include people searching for the brand.
- Quoting the site-wide average position.
- Rewriting a page that lost clicks because fewer people searched.
- Treating impressions without clicks as a win: Google may be testing the page low on the results.
