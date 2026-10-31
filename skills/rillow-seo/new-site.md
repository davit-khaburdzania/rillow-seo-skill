# New site

Use this guide when the site is new, gets almost no search traffic, or is about to launch.
You should end with a site Google can crawl and understand, Search Console and Bing set up, the first pages a product site needs built or planned, and 5 to 10 searches a new domain can win in the content plan.

Work through the sections in order.
Content work is wasted while Google cannot crawl or index the pages.

## 1. Get the facts

1. Call get_project.
   If the site is not in Rillow yet, call list_projects, then create_project with the domain and the country the user sells to.
   create_project uses up to about 10.5 credits to pull the site and its competitors, so tell the user before you call it.
   Rillow then pulls the domain rating, ranked keywords and competitors in the background, and get_project shows data_ready.pulling until they are ready.
2. Read the memory.
   If it does not say what the product does, who it is for, what it costs, which competitors the user names and which tools it integrates with, ask the user and save the answers with remember in the product, audience and competitors sections.
3. Find the framework in the repository and where the production URL is configured, because canonicals and the sitemap are built from it.
4. If the site is live, fetch the home page as Googlebot, as the rules say.
   If it is not live yet, verify everything on a local production build now and again on production after launch.

## 2. The technical floor

This is the minimum for pages to be crawled, indexed and shown well.
The technical guide covers the reasons and everything beyond it.

| Check | Good |
|---|---|
| Title and description | One unique `<title>` and one meta description per page, in the server HTML |
| Canonical | One absolute, self-referencing `<link rel="canonical">` per indexable page, inside `<head>`, built from one configured site URL |
| Sitemap | `/sitemap.xml` or a sitemap index, listing only canonical URLs that return 200 |
| robots.txt | At the root, blocks nothing that indexable pages or their CSS and JS need, has a `Sitemap:` line with the absolute sitemap URL |
| Missing pages | HTTP status 404 with a helpful page, never a "not found" page with status 200 |
| HTTPS and one host | http and the unused host variant (www or bare) redirect with 301 or 308 to the same URL on the chosen host |
| Main content | Text, links, title, canonical and JSON-LD are in the HTML the server sends, not added later by JavaScript |
| Open Graph | `og:title`, `og:description` and `og:image` per page. Google also reads them as title and preview image candidates. |
| Preview and staging | Behind a password, or a noindex header if a password is not possible, while production has no noindex |

### Framework traps

- Next.js App Router: set `metadataBase` once in the root layout and `alternates.canonical` per route.
  A canonical set in the root layout is inherited by every page without its own, which points the whole site at the home page.
- Astro: `@astrojs/sitemap` needs `site` in `astro.config`, which absolute canonicals also need.
  It writes `sitemap-index.xml` and skips SSR routes, which need their own sitemap endpoint.
- Nuxt: an unset `site.url` gives wrong canonicals and sitemap URLs.
- Hugo: set `baseURL` to the production URL, and check the host serves `layouts/404.html` with status 404.
  `aliases` are meta refresh redirects, so put important redirects in the host config as 301s.
- WordPress: Settings > Reading > "Discourage search engines" adds a sitewide noindex, and two SEO plugins print two canonicals.

Next.js 15.2 and later can stream metadata.
When the page or `generateMetadata` does request-time work, the title, description and canonical go at the end of `<body>` for every client not on Next.js's list of HTML-limited bots.
Next.js says Googlebot still reads them there, but crawlers that do not run JavaScript, such as GPTBot, ClaudeBot and PerplexityBot, get the canonical in the body, where it is not valid.
Make the metadata prerenderable, or set `htmlLimitedBots: /.*/` in `next.config` to send it in `<head>` to everyone, at the cost of a slower first byte.
URL Inspection always shows the tags in `<head>`, so check with curl as Googlebot.

### Verify with curl

Run these against a local production build (build, then start or preview, not the dev server), and again on production after each deploy.

```bash
UA="Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
SITE="https://example.com"   # or http://localhost:3000

# Head tags in the server HTML. The canonical must print here, inside <head>.
curl -s -A "$UA" "$SITE/pricing" | perl -0777 -ne 'print $& if m{<head\b.*?</head>}s' \
  | grep -oiE '<title>[^<]*|<meta[^>]+(name="description"|name="robots"|property="og:)[^>]*>|<link[^>]+rel="canonical"[^>]*>'

# A sentence from the visible page should be in the raw HTML (count above 0).
curl -s -A "$UA" "$SITE/pricing" | grep -c "a sentence from the page"

# Missing pages: expect 404 for a random path and for a missing item on each dynamic route.
for p in /no-such-page-xyz /blog/no-such-post-xyz; do
  curl -s -o /dev/null -w "%{http_code} $p\n" -A "$UA" "$SITE$p"; done

# Host variants: expect 200 at the chosen URL after 1 redirect. 2 is fine for http plus www, more is a chain to fix.
for u in http://example.com/ http://www.example.com/ https://www.example.com/; do
  curl -s -o /dev/null -L -w "%{http_code} %{num_redirects} %{url_effective}  <- $u\n" "$u"; done

# robots.txt, then the sitemap its Sitemap: line names. Do not guess the path.
curl -s "$SITE/robots.txt"
SITEMAP="$SITE/sitemap.xml"   # replace with the URL from the Sitemap: line
curl -s "$SITEMAP" | grep -c '<loc>'   # must be above 0
curl -s "$SITEMAP" | grep -oE '<loc>[^<]+' | cut -c6- | while read u; do
  curl -s -o /dev/null -w "%{http_code} $u\n" -A "$UA" "$u"; done | grep -v '^200'   # prints only failures
```

If the `<loc>` count is 0, the sitemap URL is wrong or the sitemap is empty, and the last loop proves nothing.
If the sitemap is an index, run the loop on each sitemap it lists.
Also check that each sitemap URL is the canonical URL of its page.

## 3. Structured data

Structured data makes a page eligible for a search feature and never guarantees one.
Google's documentation describes no ranking effect from it, and AI Overviews and AI Mode need none.
Use JSON-LD in the server HTML, and mark up only what is visible on the page.

| Type | Page | What to include | What it earns |
|---|---|---|---|
| `Organization` | Home or about | `name`, `url`, `logo` (at least 112x112 px, crawlable, readable on white), `sameAs` links to the company's profiles | Helps Google pick the logo for results and knowledge panels. No property is required. |
| `WebSite` | Home page only (domain or subdomain root) | `name`, `url`, optional `alternateName` | The site name shown above results |
| `SoftwareApplication` or `WebApplication` | Product or pricing page | Add it only when you have all three required parts: `name`, `offers.price` (0 if free), and `aggregateRating` or `review` from reviews shown on that page. Add `applicationCategory` and `operatingSystem`. | The software app rich result |
| `Article` or `BlogPosting` | Each post | `headline`, `author` with `name` and `url`, `datePublished` and `dateModified` with a timezone, `image` | Helps Google show the right title, image and dates. No property is required. |
| `BreadcrumbList` | Pages that show breadcrumbs | The path a user follows, not the URL folders | The breadcrumb line, on desktop only since January 2025 |
| `Product` | Physical or purchasable goods only | `name`, `offers` with price and currency | Product snippets and merchant listings. For software, use `SoftwareApplication`. |

Never copy ratings from G2, Capterra, Trustpilot or an app store into markup.
Google's review rules require reviews that are collected by the site and visible on the page.

These no longer earn rich results, so do not add them for Google: `FAQPage` (shown for no site since 2026-05-07), `HowTo` (removed in 2023), and `SearchAction` for the sitelinks search box (removed in November 2024).
Existing markup of these types does no harm and does not need removing.

Validate each page template in the Rich Results Test (https://search.google.com/test/rich-results).
For a local build, paste the page HTML into its code tab.
After launch, the Enhancements reports in Search Console show what Google actually parsed.

## 4. Search Console, Bing and IndexNow

The user does the account steps.
Give them exact instructions, and do the repository steps yourself.

1. In Search Console, add a Domain property for the bare domain (`example.com`) and verify it with the DNS TXT record Search Console shows.
   A Domain property covers http, https, www and every subdomain.
   Data starts a few days after the property is added, so do this before launch.
2. Submit the sitemap URL in the Sitemaps report.
   The old sitemap ping endpoint no longer works.
3. Ask the user to inspect the home page in URL Inspection, run the live test and click Request indexing.
   Do the same for a few key pages only, because there is a daily quota and repeated requests do not speed anything up.
4. Ask the user to check that Settings > Search generative AI is set to Include, the default.
   Exclude removes the site from AI Overviews, AI Mode and Discover AI features.
5. Connect Search Console and Google Analytics to Rillow.
   get_project says whether they are connected and gives the user the link.
   Without Search Console, get_performance has no clicks or impressions and get_brief has no query data.
6. In Bing Webmaster Tools, import the site from Search Console.
   The import verifies the site and brings the sitemaps, and data appears after about 48 hours.
   Bing's index also feeds Copilot answers.
7. Add IndexNow, which tells Bing, Yandex, Naver, Seznam, Yep and Amazon when a URL is added, updated or deleted.
   Google does not use it.
   Bing says it reduces outdated references to the site in Copilot answers.
   Create a key of 8 to 128 letters, digits or dashes, serve it at `/<key>.txt` with the key as the file's content, and after each deploy POST only the changed URLs:

   ```bash
   curl -s -X POST https://api.indexnow.org/indexnow -H 'Content-Type: application/json; charset=utf-8' \
     -d '{"host":"example.com","key":"<key>","urlList":["https://example.com/pricing"]}'
   ```

   A 200 or 202 response means it was accepted.
   Check the CMS or host for an IndexNow setting first.
   Otherwise add the POST to CI or a deploy hook after the production deploy, because you are not there for each deploy.
   Without a repository, point the user to that setting or give them the command.

Do not use Google's Indexing API.
It is only for job posting and livestream pages.

## 5. The pages a product site needs, in order

Google gives no list of page types, so the order below is common practice, and the "Basis" column says where Google's guidance applies.

| Order | Page | What it does | Basis |
|---|---|---|---|
| 1 | Home | Says what the product is and who it is for, with the category term buyers search for in the title and main heading | Google's SEO starter guide |
| 2 | About, contact, privacy, terms | Shows who runs the site and how to reach them | Google: its quality guidance asks who created the content and why |
| 3 | Feature or use-case pages | One page per capability or job people search for, each with its own substance (workflow, screenshots, examples) | Common practice. Google's doorway policy rules out near-identical pages per audience or city. |
| 4 | Pricing | Real prices or a real range | Common practice. `SoftwareApplication` markup needs a visible price. |
| 5 | Alternatives and comparisons | "X alternative" and "X vs Y" pages for real product rivals, named by the user or found with get_competitors | Common practice, because these searchers are choosing a tool. Google's reviews guidance sets the bar: first-hand evidence, honest drawbacks, accurate claims about the competitor. |
| 6 | Integrations | One page per integration that exists, with what it does and how to set it up | Common practice. Google's spam policies rule out pages that differ only by partner name. |
| 7 | Docs or help | Server-rendered and crawlable | Common practice |
| 8 | Blog | Problem-led posts about problems the product solves, from first-hand experience | Google: original, non-commodity content. Avoid many unrelated topics. |

Decide the URL folders before anything is published, for example `/features/`, `/pricing`, `/compare/`, `/integrations/`, `/blog/`.
Renaming them later is a migration.
Google says keywords in the URL have hardly any effect.
Give each folder a hub page that links to every page in it, and link every page from at least one other page.
Read the content and writing guides before writing any of these pages, and the links guide for internal links.

## 6. First keywords for a new domain

A new domain has few links and no history, so it cannot win broad, competitive searches yet.
Aim for specific, long-tail, high-intent searches where the current top 10 already includes small sites or forum threads.
The keywords guide has the full method.

A find_keywords seed used for the first time uses about 0.5 credits and a get_serp call 0.06 to 0.12.
get_competitors uses about 0.5 for a competitor domain the first time, and without a domain up to 12 on a new site.
Saved results are free to read again.
Plan the calls and tell the user how many you will use.

1. Call find_keywords without a seed.
   It returns what the site already ranks for, often nothing on a new domain, and ideas saved from earlier seeds, at no cost.
2. Call get_competitors without a domain.
   On a new site Rillow takes them from Google's top 10 for the main topics in its memory.
   Then call it with one or two competitor domains the user named, ideally the smallest real one, to see the searches it ranks for.
3. Pick 2 to 4 seeds from the product, not from tools: the category term, the main problem it solves, a competitor name, a key integration.
   Call find_keywords with each.
4. Shortlist by intent first, then difficulty, then volume.
   Prefer searches where the person is choosing a tool or trying to solve the exact problem the product solves: alternatives, comparisons, "tool for X", "how to do X in Y".
   Difficulty is DataForSEO's estimate of the chance of reaching the top 10, on a logarithmic scale from 0 to 100, calculated from the link profiles of the current top 10 pages among other factors.
   It does not predict this site's chances.
   Low volume is fine for bottom-of-funnel searches.
5. Call get_serp on the 5 to 10 best candidates.
   Winnable signs: Reddit, Quora, Stack Overflow or other forum results, small or niche sites, thin or outdated pages, results that answer a slightly different question.
   Hard signs: the top 10 is all large brands, review sites or publishers, or the page type that ranks is one the user cannot make.
   Write down the page type that ranks: product page, comparison, list post, tutorial or docs.
6. Cluster them: one page per intent, not per phrasing.
   Searches whose top results overlap belong on one page.
   A cluster is a pillar page plus the posts that support it.
7. Show the user the clusters with volume, difficulty, intent, the page type that ranks and why each search looks winnable.
8. After the user picks, call update_plan to add the first 5 to 10 items, create the cluster and assign the items to it.
   Put bottom-of-funnel items first.
9. Save the chosen focus and anything the user ruled out with remember.

## 7. What to tell the user about timing

Google finds a new site's pages through links and sitemaps, so first impressions take days to weeks, and meaningful non-branded clicks usually take months.
In Ahrefs' study (updated May 2025), 1.74 percent of newly published pages reached the top 10 within a year, and low-volume keywords ranked faster than high-volume ones.
The measure guide has the usual time for each kind of change and how to judge one.

## Mistakes to avoid

- Writing a batch of blog posts before the home, feature, pricing and comparison pages exist.
- Shipping the staging noindex, a `Disallow: /`, or WordPress's "Discourage search engines" setting to production.
- Copying third-party ratings into `aggregateRating`, or adding `FAQPage` for rich results.
- Choosing keywords by volume.
  Head terms are out of reach for a new domain.
- Making a page for every keyword variation, city or integration name.
  That is scaled content or doorway abuse.
- Buying links, or buying an expired domain to reuse its rankings for unrelated content.
  Both break Google's spam policies.
- Requesting indexing again and again, or paying for "indexing" services.
