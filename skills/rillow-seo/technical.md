# Technical SEO

Use this guide for any change that touches indexing, canonicals, redirects, sitemaps, robots rules, JavaScript rendering, page speed, language versions or URLs.
You should end with the exact fixes made in the repository, verified in the HTML a local production build serves, and checked again on production after the user deploys.
Without a shell, ask the user to run the commands below and paste the output, as the rules say.

## Facts that change how you build

- Googlebot reads only the first 2 MB of the HTML and of each CSS or JS file, uncompressed and including headers (64 MB for PDFs), and indexes what arrived before the cut.
  Keep the title, meta tags, canonical and JSON-LD early in `<head>`, and keep base64 images and large inline scripts out of the HTML.
  Sources that say 15 MB are out of date.
- Google indexes the smartphone render, so content, links and markup missing on mobile do not count.
- A page with a non-200 status may not be rendered.
  A `noindex` in the initial HTML may stop rendering, so JavaScript cannot reliably remove it.
  The canonical must be the same before and after rendering, or be set only by JavaScript.
- Rendering is stateless, so local storage, session storage and cookies are cleared between pages.
- GPTBot, ClaudeBot and PerplexityBot do not run JavaScript (Vercel's measurement, December 2024), so they see only the server HTML.

## Diagnose a page that is not indexed

Rillow has no URL Inspection or Page indexing data, so ask the user for what only Search Console shows.

1. Fetch the URL as Googlebot and read the status, redirects, headers and head.
   A 403 or a challenge page does not prove Googlebot is blocked, as the rules say.

   ```bash
   UA="Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
   curl -sIL -A "$UA" "$URL" | grep -iE '^(HTTP/|location:|x-robots-tag:)'
   curl -s -A "$UA" "$URL" | perl -0777 -ne 'print $& if m{<head\b.*?</head>}s' \
     | grep -oiE '<meta[^>]+name="(robots|googlebot)"[^>]*>|<link[^>]+rel="canonical"[^>]*>'
   curl -s -A "$UA" "$URL" | wc -c   # bytes of HTML, keep well under 2 MB
   ```

2. Read the live robots.txt and check the path against its rules.
3. Check that the URL is in the live sitemap and linked from at least one page with a real `<a href>`.
4. Ask the user to inspect the URL in Search Console and paste the Page indexing status, the Google-selected and user-declared canonicals, the last crawl date, and the rendered HTML from Test live URL.
   For many URLs, ask for the Page indexing report reasons with example URLs.

| Page indexing reason | Meaning | Fix |
|---|---|---|
| Discovered, currently not indexed | Known, crawl postponed. Google says usually because crawling was expected to overload the site | Check response times and errors in the Crawl stats report, then add links from strong pages and a sitemap entry |
| Crawled, currently not indexed | Crawled and not kept. Google says only that it may be indexed later and needs no resubmission | The usual practitioner reading is quality or near-duplication, so improve or merge the page |
| Duplicate, Google chose different canonical than user | Your canonical was overruled | Point redirects, canonical, sitemap and internal links at one URL, or make the pages differ |
| Soft 404 | A "not found" or empty page returned 200 | Return 404, or give the page real content |
| URL marked noindex | A meta tag or `X-Robots-Tag` header says noindex | Find the layer that adds it: framework metadata, CMS, plugin, CDN rule, staging config |
| Blocked by robots.txt | A `Disallow` rule matched | Decide: crawlable, or excluded with noindex. Not both. |
| Indexed, though blocked by robots.txt | Indexed from links without a crawl | To remove it, allow crawling and add noindex |
| Redirect error | Chain too long, a loop, or a bad URL | One hop to a 200 page |
| Server error (5xx) | The server failed under crawl | Server logs and the Crawl stats report |
| Blocked (401 or 403) | Auth, a firewall or bot protection answered Googlebot | Allow verified Googlebot. Staging protection left on production is a common cause. Confirm with the URL Inspection live test first. |

Do not judge indexing with a `site:` search, because its counts are estimates.
If Google chose another URL as the canonical of the page you want to improve, fix the canonical first, because every signal you add goes to the other URL.

## Canonicals and duplicates

- Every indexable page gets one self-referencing canonical with an absolute URL, in `<head>`, because Google ignores `rel="canonical"` in `<body>`.
- Signals, strongest first: redirects, `rel="canonical"`, sitemap inclusion.
  Point all of them and every internal link at the same URL.
- The canonical target returns 200, is indexable and does not canonicalize somewhere else.
- Do not canonicalize with robots.txt, `noindex` or the URL removal tool.
- Common duplicate sources: http and https, www and bare host, trailing slash, letter case, `index.html`, tracking and sort parameters.
  Fix host, protocol, slash and case with one 301 or 308, and point parameter URLs' canonicals at the clean URL.
- Duplicates within a site are not penalized, but they split signals and waste crawling.

## robots.txt versus noindex

| Control | Stops crawling | Stops indexing | Use for |
|---|---|---|---|
| `Disallow` in robots.txt | Yes | No | Large URL spaces with no search value, such as filter combinations |
| `<meta name="robots" content="noindex">` | No | Yes | Pages users need that should not appear in search |
| `X-Robots-Tag: noindex` header | No | Yes | PDFs and other non-HTML files |
| Password | Yes | Yes | Staging and private areas |

A page blocked by robots.txt cannot show its noindex, because Googlebot never fetches it, and it can still be indexed from links.
To remove a page, allow crawling, add noindex, and wait until it drops out.

- Google reads only `user-agent`, `allow`, `disallow` and `sitemap`, so `noindex` and `crawl-delay` lines do nothing in Google.
- One file per host and protocol, at the root, UTF-8, at most 500 KiB.
  Rules are case-sensitive, and the most specific matching rule wins.
- Google caches robots.txt for up to 24 hours.
- A 4xx on robots.txt, except 429, means no restrictions.
  A 429 or 5xx stops Google crawling the site for 12 hours, then it uses the last good copy for up to 30 days.
- Never block the CSS, JS or API calls a page needs to render.

Protect preview and staging hosts with a password rather than noindex, because a noindex is one deploy away from production.
Rules for AI crawlers are in the ai-search guide.

## Redirects and status codes

| Situation | Response |
|---|---|
| Page moved for good | Server-side 301 or 308. Next.js `redirects()` with `permanent: true` sends 308. |
| Temporary move | 302 or 307. The old URL stays in results. |
| Removed, with an equivalent page | 301 to the closest equivalent |
| Removed, nothing equivalent | 404 or 410 with a helpful page. Google treats them the same. |
| Many removed pages | Never redirect them all to the home page. Google may treat that as a soft 404. |
| Server overloaded | 503 or 429 slows crawling. 401 and 403 do not, so never use them for that. |
| Missing item in a single-page app | A JavaScript redirect to a URL that returns 404, or a noindex added by JavaScript |

- Redirect in one hop.
  Googlebot follows up to 10 hops, and Google advises ideally no more than 3 and fewer than 5.
- When you add a redirect, update internal links, canonicals and the sitemap to the final URL.
- Keep permanent redirects at least a year, ideally forever.
- Persistent 5xx errors drop URLs from the index.
- Google sees a JavaScript redirect only after rendering, so use one only when a server redirect is impossible.

Check a redirect with `curl -s -o /dev/null -L -w '%{http_code} %{num_redirects} %{url_effective}\n' -A "$UA" "$OLD_URL"`.
Expect `200 1` and the intended final URL.

## Sitemaps

- List only canonical, indexable URLs that return 200: no redirects, noindex pages or 404s.
- Use absolute URLs, at most 50,000 URLs or 50 MB uncompressed per file, and a sitemap index above that.
- Google uses `lastmod` only when it is consistently accurate, and only for significant changes to content, structured data or links.
  Stamping every URL with the build date (`lastModified: new Date()` in `app/sitemap.ts` is the usual cause) makes it useless.
  Google ignores `priority` and `changefreq`.
  Bing reads `lastmod` as a freshness signal for search and Copilot.
- Submit it in Search Console and list it in robots.txt.
- On larger sites, split sitemaps by section, because the Page indexing report can be filtered by sitemap.

## Parameters, filters and pagination

- Filter and sort URLs that do not need to be in search: block their parameter patterns in robots.txt and keep an unfiltered listing crawlable.
- Tracking parameters such as `utm_source`: the canonical points at the clean URL, and internal links never carry them.
- Pagination: each page has its own URL and a self-referencing canonical, linked with real `<a href>` links.
  Do not canonicalize page 2 to page 1 or noindex the series, because items deeper in the list lose their crawl path.
  Google no longer uses `rel="next"` and `rel="prev"`.
- Infinite scroll and "load more" need paginated URLs behind them, because Google does not scroll or click.
- On product sites this mostly means blog tag archives, docs search and changelogs.
  A common practice is to noindex or remove tag pages with one or two posts.

## JavaScript frameworks

- Server-render or statically generate every page that should rank.
  Client-only rendering is for screens behind a login, and dynamic rendering (a prerendered copy for bots) is a deprecated workaround.
- Links are `<a href="/real/path">`, which router link components render.
  `onclick` handlers, buttons and `#fragment` routes are not followed.
- Title, description, canonical, robots meta and JSON-LD come from the framework's server-side metadata API.
  Never use client-side JavaScript to flip indexability or change a canonical.
- Lazy-load only content below the fold, with `loading="lazy"` or IntersectionObserver.
- Content that appears only after a click, from personalization, or from local storage is not indexed.

Next.js specifics:

- Streaming metadata, in 15.2 and later.
  When the page renders at request time, or `generateMetadata` reads runtime data (`cookies()`, `headers()`, `searchParams`, and `params` with Cache Components on) or fetches uncached data, the tags are sent at the end of `<body>` to every client not on Next.js's list of HTML-limited bots.
  Next.js says Googlebot still reads them because it runs JavaScript, but crawlers that do not, such as GPTBot, ClaudeBot and PerplexityBot, get the canonical in `<body>`, where it is not valid.
  Make the metadata prerenderable (static data, `generateStaticParams`, or `'use cache'` inside `generateMetadata` with Cache Components on), or set `htmlLimitedBots: /.*/` in `next.config` to send it in `<head>` to everyone, at the cost of a slower first byte.
  Google-InspectionTool is on the list, so URL Inspection always shows the tags in `<head>`.
  Check with curl as Googlebot that the canonical is inside `<head>`.
- `notFound()` called inside `<Suspense>`, after the response has started streaming, sends status 200 with a noindex tag.
  For a real 404, check that the item exists before streaming starts: with `generateStaticParams` and `dynamicParams = false`, in the page outside Suspense when Cache Components is off, or in `proxy` (called `middleware` before Next.js 16) when it is on, because then every dynamic route streams a static shell first.
- Put permanent redirects in `next.config` or `proxy`, not in a `router.push` inside `useEffect`.

## Core Web Vitals

| Metric | Good | Measures |
|---|---|---|
| LCP | 2.5 s or less | When the largest visible element renders |
| INP | 200 ms or less (poor above 500 ms) | Delay from a tap, click or key press to the next paint |
| CLS | 0.1 or less | Unexpected layout movement |

Field data is the 75th percentile of real Chrome page loads over a rolling 28 days, mobile and desktop separately.
Ranking systems use Core Web Vitals, but relevance wins over speed, so fix them for users and conversion.

1. Field data: call the PageSpeed Insights API.

   ```bash
   curl -s "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<url>&strategy=mobile"
   ```

   `loadingExperience` holds the page's field p75 values and `originLoadingExperience` the whole site's, when Chrome has enough data, so say which one you report.
   Without an API key the call may fail with 429, so ask the user for a free key to add as `&key=`.
   Without a shell, ask the user for the field data from pagespeed.web.dev or the Search Console Core Web Vitals report.
   Google has said it plans to stop including field data in this API.
2. No field data means too little traffic, not a pass.
   Report it as unavailable and use lab data only to find causes.
3. Lab data: run Lighthouse on a production build, `npx lighthouse <url> --only-categories=performance --output=json --output-path=lh.json --chrome-flags="--headless"`, three runs, and take the median.
   Lab tests cannot measure INP, and Total Blocking Time is its lab proxy.
4. After a fix, re-run the same lab test.
   Check field data again about four weeks after the deploy, when the 28-day window has rolled over.

Usual fixes on framework sites:

- LCP: static or CDN-cached HTML, and the hero image as an `<img>` in the server HTML, not lazy-loaded, with `fetchpriority="high"` and a right-sized AVIF or WebP.
- INP: less client JavaScript (server components, islands, code splitting), long tasks split up, and chat, analytics and testing tools loaded on interaction or when idle.
- CLS: `width` and `height` or `aspect-ratio` on images and embeds, reserved space for banners, and a metric-matched fallback font.

web.dev's guide for server response time (TTFB) is 0.8 s or less.
TTFB is not a Core Web Vital, so treat a slow one as a cause of slow LCP, not as a ranking threshold.
When you audit third-party scripts, also look for history manipulation, because back button hijacking has been a Google spam policy since 2026-06-15.

## Language and country versions

Add hreflang only when real translated or regionally different versions exist, and then follow Google's rules (https://developers.google.com/search/docs/specialty/international/localized-versions).
Each version is self-canonical, never canonicalized to the original.
Do not redirect by IP or browser language, because Googlebot crawls mostly from the US and would see one version.
Rillow's keyword data is for the project's country, so agree with the user how to research a new market before translating.

## URL changes and migrations

1. Map every old URL to its closest new URL, or to 404 when nothing matches.
   Collect old URLs from the routes, the live sitemap, and the user's Search Console exports of Pages (last 16 months) and the Links report.
   An export holds at most 1,000 rows, so on a larger site add server logs or a crawl export.
2. Change one thing per release where you can, so a drop can be traced to its cause (common practice).
3. Ship one-hop server-side 301 or 308 redirects with the new URLs, never a catch-all to the home page, and update internal links, canonicals, structured data and the sitemap.
4. Check every old URL on a preview build and on production.
   No output means each one redirects once to a 200 page.

   ```bash
   while read old; do
     curl -s -o /dev/null -L -w "%{http_code} %{num_redirects} %{url_effective}  <- $old\n" -A "$UA" "$old"
   done < old-urls.txt | grep -v '^200 1 '
   ```

5. Submit the new sitemap, plus one of the old URLs so the user can watch them leave the index.
6. For a domain move, verify both domains in Search Console, submit a Change of Address for every verified variant of the old domain, and keep the old domain registered.
7. Do not block old URLs in robots.txt, because Googlebot must crawl them to see the redirects.
8. Google says most pages of a small or medium site move in a few weeks.
   Record the launch date and what moved with remember as a note, ask the user to add a Search Console annotation that day, and judge it with the measure guide.

## Mistakes to avoid

- Adding noindex to a page that robots.txt blocks.
- Redirecting removed pages to the home page, or using 302 for a permanent move.
- Canonicalizing paginated pages to page 1, or translations to the original.
- Calling Core Web Vitals failing from lab data or code alone, or claiming a field improvement on the day of the deploy.
