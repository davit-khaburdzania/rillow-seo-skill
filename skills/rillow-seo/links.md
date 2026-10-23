# Links

Read this before internal linking work, or when the user asks how to get backlinks.
End with internal links added in the repository where they help most and, if the user asked, a realistic plan for earning links that fits a small site.

## Internal links come first

Internal links are the part of linking the user controls.
Google finds most new pages through links, reads anchor text to learn what the target page is about, and uses links as one ranking signal among many.
An internal link is live after one deploy and counts once Google recrawls the page.
Links from other sites take months to earn, and the user controls neither their anchor nor their timing.
So fix internal links before you plan any outreach, and tell the user why.

## What Google says about links

From Google's [link best practices](https://developers.google.com/search/docs/crawling-indexing/links-crawlable):

- Google reliably follows only `<a>` elements with an `href` that resolves to a URL.
  `<span>` links, `onclick` navigation, `href="javascript:..."` and buttons that call a router are not reliably followed.
- Links inserted by JavaScript work for Google when they use real `<a href>` markup.
  Many AI crawlers do not run JavaScript, so prefer links that are in the server HTML.
- Anchor text should be descriptive, reasonably concise and make sense on its own.
  "Click here", "read more" and "this article" say nothing about the target.
- Do not cram keywords into anchors.
  Keyword stuffing breaks the spam policies.
- Do not put links right next to each other.
  Each link needs its own words around it.
- For an image link, the image's `alt` text is the anchor text.
- Every page the user cares about needs a link from at least one other page on the site.
  There is no ideal number of links per page.
- Link to the canonical URL, not to a URL that redirects or to a duplicate variant.
- Linking out to sources is fine and can build trust.
  Use `rel="nofollow"` only for sources you do not trust, `rel="sponsored"` for paid links, and `rel="ugc"` or `nofollow` for links users post in comments or forums.

Google's [spam policies](https://developers.google.com/search/docs/essentials/spam-policies#link-spam) define link spam as links made mainly to manipulate rankings.
That includes:

- buying or selling links, paid in money, goods, services or a free product
- excessive link exchanges, and partner pages that exist only to cross-link
- automated link building
- requiring a followed link in terms of service or a contract
- links with optimized anchor text in guest posts, advertorials or press releases spread across other sites
- low-quality directory and bookmark links, links in widgets, and links in the footers or templates of many sites
- forum comments and signatures with optimized links
- low-value content made to pass links, such as a network of sites built to link to one site

A paid link is allowed when it carries `rel="sponsored"` or `rel="nofollow"`.

## Map the internal links

Call get_project first.
Its top ranked keywords show which URLs rank and where, its clusters show which posts belong together, and its memory may hold earlier linking decisions.

1. List every URL that should be indexed.
   In a repository, read the routes, the content folders and the sitemap generator, then fetch the live `/sitemap.xml` and compare.
   Add the published posts that get_performance lists.
2. Collect the links on each page from the HTML the server sends.
   Run the production build and read its output, or start the production server locally and fetch pages with curl.
   Without a repository, fetch the live pages with curl or your web fetch.
3. For a quick first pass on a Markdown blog, grep the content folder for relative links, links to the site's own domain and `href` attributes, with that domain in place of example.com: `grep -rnoE '\]\((/|https?://(www\.)?example\.com)[^)]*\)|href="(/|https?://(www\.)?example\.com)[^"]*"' content/`.
   Navigation, footer and related-post links live in layout components, so read those too.
4. If your web fetch returns text instead of HTML, or the site has hundreds of pages, tell the user the map is partial and ask for a crawl export from a crawler they already use.
   Rillow has no crawler.
5. Record for every URL: its status code, its click depth from the home page (breadth-first over the links), the pages that link to it with their anchor text, and whether each link sits in body text or in navigation, footer or sidebar.

From the map, list:

- Orphan pages: in the sitemap, the plan or a Search Console export, but linked from nowhere.
- Deep pages: pages that should rank but sit many clicks from the home page.
  A common rule of thumb is three clicks or fewer.
  Google gives no number.
- Links that point at redirects, 404s, `http://` or non-canonical variants such as a different trailing slash or letter case.
- Links that only exist after JavaScript runs, or that navigate with click handlers.
- Generic anchors, and targets that get the identical anchor from every page.
- Important pages that only get navigation or footer links.
  Those links appear on every page, so they say little about which page matters.

## Decide which pages get links

Spend links where the data says they can move something.

| Target | How to find it | Why |
|---|---|---|
| Pages at about positions 4 to 20 (a practitioner heuristic) for a keyword that matters | find_keywords without a seed (positions and URLs as of when the project was created, DataForSEO estimates), get_performance for published posts, or the user's Search Console export by page | They already show for the search, so they have the most to gain |
| Product, feature, pricing, comparison and alternatives pages | The repository and the navigation | They convert, so they deserve more body-text links than most blog posts |
| A post published today | get_project plan status | New pages are found through links. Link it from older related pages on the day it ships |
| Orphan and deep pages | The link map | Google may find them late or not at all |
| Posts in a cluster | get_project clusters | The pillar and its posts should link to each other |

Then pick the source pages for each target.

- Relevance first.
  The reader of the source page should want the target.
  Sharing a keyword is not enough reason.
- Then strength: pages with the most Search Console clicks, pages in get_project's top keywords, pages with the most external links (get_backlinks with view top_pages, or Search Console's Links report), the home page and section hubs.
- A common starting point is 2 or 3 new body-text links per target, each from a different page.

## Write the links

1. When the change touches more than a few pages, show the user the list first: source URL, the sentence that gets the link, the anchor and the target.
   The rules ask for approval before editing many pages at once.
2. Put each link in body text, in a sentence where the target helps the reader.
   If no sentence fits, add one that is useful on its own.
3. Write an anchor that says what the target page is, in words people search for, for example `<a href="/compare/linear-vs-jira">how Linear and Jira compare for sprint planning</a>`.
   Vary the wording across source pages.
   The same exact keyword on every link reads as engineered and gives Google less context.
4. One link to a given target per source page is usually enough.
5. Use the site's own link markup: Markdown links, plain `<a href>`, or the framework component that renders one (Next.js `Link`, Nuxt `NuxtLink`).
   Do not navigate with click handlers.
6. Use the canonical path in the site's trailing slash style.
7. Leave a post's updated date and `dateModified` alone when you only added links.
   Dates change only with a substantial content change.
8. Build locally and read the output HTML to confirm each link is an `<a href>` with the right anchor.
   After the user deploys, fetch the live pages again.
9. Without the repository, give the user each change as source URL, sentence, anchor and target.
10. Save what you did with remember in one short note, for example which pages now link to a target and why, so a later session does not redo or undo it.

## Navigation, hubs and breadcrumbs

- Keep the main navigation to the sections that matter, such as product, pricing, blog and docs.
  A menu that links hundreds of pages makes every page look equally important.
- Give each section a hub page that links to every child page, such as `/blog/`, `/integrations/`, `/compare/` and `/docs/`.
  The children link back to it.
- Paginate blog and archive lists with real links to `/blog/page/2/` and on, each page with its own canonical.
  A "load more" button with no paginated URLs hides older posts from crawlers.
- Show breadcrumbs as visible links that follow the path a reader takes.
  BreadcrumbList markup is optional, and Google shows breadcrumbs only on desktop results since January 2025.
- Choose related posts by topic or cluster, not by date.
  A "latest posts" block links whatever is newest.
- Keep the footer to a few important pages and the legal pages.

## Clusters, pillars and new posts

In Rillow, a cluster is a group of keywords served by one pillar post, with supporting posts for the narrower searches.

- The pillar links to every post in its cluster, and each post links back to the pillar.
- Each post links to one or two sibling posts where the reader would want them.
- The pillar links to the product or comparison page it supports, so readers have a path to the product.
- Publish the pillar before its posts or together with them.
- Use update_plan to put plan items in the right cluster, so get_project shows the structure next time.

When you write a post:

1. Call get_brief.
   It returns the site's own pages worth linking to for that post.
   Link to the ones that fit the text.
2. Pick 2 or 3 older pages on the same topic and plan a link from each to the new post.
3. Call save_article and fix what it marks about links to the site's own pages.
4. After the user deploys, add the incoming links if they were not in the same deploy, call mark_published, and check the links on the live pages.

## Earning links from other sites

Do this when the user asks, and after the internal links are in order.
Be honest with the user.
Links usually take months to appear and to count, many efforts produce none, and no link guarantees a ranking.
Google says links from other sites come naturally over time, and that promoting the site, in communities and by word of mouth, helps people find it.
Mentions and links from other sites are also what studies tie most closely to AI answers, as the ai-search guide says.

Read the product, audience and integrations from get_project memory, then choose from these:

| Way to earn links | What to do | When it fits |
|---|---|---|
| Listings where the product belongs | Directories that real buyers in the niche browse, integration partners' app marketplaces, platform and plugin directories, launch sites the audience reads, curated lists on GitHub. Test: would the user list there if links did not count? | Always. Many of these links are nofollow, which is fine because the value is the visitors |
| Something people cite | Original data from the product (aggregated, anonymous and allowed by the privacy policy), a free tool or calculator, a template, a statistics page for the category kept current | The user has data or can build a small tool |
| Communities | Answer questions as the user, say they built the product, and link only when the link is the answer | Always. Dropping links in posts or signatures is link spam |
| Being a source for writers | Answer journalist request services with a short, specific reply that includes a number. Offer data or a quote to writers who cover the category | The founder has data or a clear view |
| Comparison and alternatives pages | Fair, accurate pages that writers comparing tools can link to. Read the content guide before writing them | There are known competitors |
| Open source and docs | A library, SDK, example repository or docs page that developers link from READMEs and answers | Developer products |
| Mentions without a link | Search the web for the product name, and ask writers who mentioned it to link the page they described | The product is already discussed |
| Lost pages with links | Ask the user for Search Console's Links report and the 404 list in Page indexing. With the user's approval, redirect a 404 URL that has external links to the live page that replaced it, when one covers the same topic. Leave it a 404 when none does | The site moved or deleted pages |

In one 2026 vendor study of B2B SaaS sites, statistics pages earned the most links relative to how many were published.
Treat that as directional.

To find where similar products are listed, take the competitors from get_project or get_competitors and, if you have web search, search their names with words such as "alternatives", "directory" and "integrations".
get_backlinks with a competitor's domain lists the sites that link to it for up to about 0.7 credits, and is then free for a day.
Use competitors with a similar product and audience, not large sites that only share keywords.
A site that links to two or more competitors but not to the user is the likeliest prospect.
Open its page before you suggest it, and drop it if it only sells links.

Write the plan as five to ten actions.
Give each one the target page on the user's site, the place, the effort and how the user will know it worked.
Give the user the full plan in your reply.
Save a one-line summary with remember as a note, such as the target pages and the tactics chosen, because a note holds at most 300 characters.

Do not suggest buying links, paid guest posts, link exchanges, private blog networks, guest posts at scale with keyword anchors, free product in exchange for a link, footer or widget links on other sites, directories that sell listings for links, or comment and forum links.
Each is link spam under Google's policies.
A sponsorship is fine when its link carries `rel="sponsored"`.

## Domain rating and disavow

- The domain rating that get_project, get_competitors and get_backlinks return is a DataForSEO estimate built from backlinks.
  It is not a Google metric, and Google says third-party tools have no access to its ranking data.
  Use it to compare sites roughly, never as a goal.
- Keyword difficulty is DataForSEO's estimate, calculated from the link profiles of the current top 10 pages among other factors.
  A site with a low domain rating should target low difficulty keywords, as the keywords guide explains.
- Do not suggest a disavow unless Search Console shows a manual action for unnatural links, or the user or someone they hired built or bought a considerable number of spammy links that could cause one.
  In most cases Google decides on its own which links to trust, and a tool's "toxic links" score is not a reason.
- The disavow tool does not work for Domain properties.
  If a disavow is justified and the user has only a Domain property, they first add and verify a URL-prefix property for the site.
  Then point them to [Google's disavow page](https://support.google.com/webmasters/answer/2648487) and let them upload the file.

## Measure

- Internal links: note the date of the change with remember and suggest a Search Console annotation.
  After a few weeks, compare the target pages' positions in get_performance for published posts, and in a Search Console export filtered by page for other pages.
  find_keywords positions date from when the project was created and do not update, so use get_ranked_keywords with the page's URL for current ones.
  The measure guide covers the comparison.
- External links: get_backlinks shows the site's referring domains from DataForSEO's crawl, which misses some links Google knows.
  Search Console's Links report (top linked pages, top linking sites, top linking text), as an export or a paste from the user, is Google's own list.
- Judge earned links by what they lift: the linked page's ranking and its referral visits in Google Analytics, not the link count or the domain rating.

## Mistakes to avoid

- Adding links that do not help the reader of the source page.
- Using the same exact-match anchor on every link to a page, or "click here" anchors.
- Links that only exist after JavaScript runs, or that use click handlers.
- Linking to redirecting or non-canonical URLs.
- Counting navigation and footer links as if they were body-text links.
- Changing dates on posts where you only added links.
- Recommending paid links, exchanges, keyword-anchor guest posts or link directories.
- Treating domain rating as a Google metric, or recommending a disavow without a manual action.
