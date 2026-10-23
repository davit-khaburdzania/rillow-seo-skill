# AI search

Read this when the user asks about ChatGPT, Perplexity, Claude, Copilot, Google AI Overviews or AI Mode.
End with an honest picture of what is known, the few changes worth making on the user's site, and a way to measure visibility that does not invent numbers.

## The honest picture

- Google documents that AI Overviews and AI Mode run on its normal index and ranking systems, so normal SEO is the work.
- OpenAI, Anthropic, Perplexity and Microsoft document their crawlers and controls, but not how their assistants choose sources.
- Third-party studies disagree on the numbers.
  They agree that a brand is mentioned mostly when one of its own pages is among the pages the assistant retrieved.
- Nobody can make an assistant cite or recommend a site, and the same prompt gets different answers from run to run.

Tell the user this before you propose any plan.

## Google AI Overviews and AI Mode

From Google's [guide to optimizing for generative AI features](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) (updated July 2026) and its AI features page:

- Both features retrieve pages from the Search index with the same ranking and quality systems.
  They also use query fan-out: the model runs several related searches and draws on those results too.
- A page is eligible when it is indexed, can be shown with a snippet, and the site is not excluded with the Search Console setting below.
  There are no other technical requirements.
- Google says optimizing for its AI features is still SEO, whatever it is called ("AEO", "GEO").
- Things Google says you can ignore for its Search: llms.txt and other AI text files or Markdown copies, special schema, "chunking" pages into small pieces, rewriting content for AI, covering every long-tail variant, and seeking inauthentic mentions.
- Making a separate page for each variation or fan-out query, to manipulate rankings or AI answers, is scaled content abuse.
  Since May 2026 the spam policies state that they cover attempts to manipulate generative AI responses.
- What Google says helps: non-commodity content based on first-hand experience, clear structure for readers, crawling allowed in robots.txt and by the CDN or host, internal links, good page experience, important content in text, relevant images and video, and structured data that matches the visible page.
- For shops and local businesses, Google points to Merchant Center feeds and a Business Profile.
- AI Overviews appear only where Google decides they add something, so many searches have none.
  AI Mode and AI Overviews use different models, so the links they show differ.

Google's controls:

- `nosnippet`, `data-nosnippet`, `max-snippet` and `noindex` limit what Google shows from a page, in AI features as well.

## Other assistants

| Assistant | How it finds pages | Documented controls and reports |
|---|---|---|
| ChatGPT search | OAI-SearchBot crawls for it. OpenAI says it may also get results from a third-party search provider, which it does not name | Allow OAI-SearchBot to be eligible for summaries and citations. A blocked site can still appear as a bare link, and `noindex` prevents that. Referrals carry `utm_source=chatgpt.com` |
| Microsoft Copilot | Bing's index, crawled by Bingbot. Partners that use Bing results draw on the same index | `noarchive` keeps a page out of Copilot answers, `nocache` limits Copilot to the URL, title and snippet. Bing Webmaster Tools has an AI Performance report |
| Perplexity | Its own index, crawled by PerplexityBot | robots.txt for PerplexityBot. Perplexity-User fetches a page when a user asks |
| Claude | Claude-SearchBot indexes for search, Claude-User fetches when a user asks. In 2025 reporters matched Claude's web results to Brave Search, which may have changed | robots.txt for each bot |

Bing's webmaster guidelines give specific advice for AI answers, which Bing calls grounding.
They ask for one topic per URL, key information early on the page, facts stated explicitly, consistent entity names, pages that make sense without outside context, and accurate updates.
They tie citations to plain SEO work too: unique titles and descriptions, one canonical URL per page, accurate sitemap `lastmod` dates, and IndexNow when a page changes, so Copilot does not cite an outdated page.
`nosnippet` and `data-nosnippet` can lower the quality of a Copilot citation.
Bing names prompt injection and AI manipulation as abuse.

What studies suggest:

| Study | Finding |
|---|---|
| Ahrefs, March 2026 | 38 percent of AI Overview citations ranked in the top 10 for the same query, against 76 percent in July 2025. Ahrefs changed its parsing and points to fan-out searches, so the two are not fully comparable |
| Ahrefs, August 2025 | About 12 percent of URLs cited by ChatGPT, Gemini, Copilot and Perplexity were in Google's top 10 for the prompt |
| Stage model paper, September 2026 | In about 35,000 GPT and Gemini runs, a brand was mentioned in about 3 percent of answers with none of its pages among the sources, and in about half with one. One author |
| SE Ranking, November 2025 | Across 129,000 domains, the number of linking sites predicted ChatGPT citations best. Recently updated pages and pages with more data points were cited more. FAQ markup and llms.txt showed no benefit |
| Ahrefs, January 2026 | Branded web mentions correlated with AI Overview citations, word count did not |

None of them shows a cause.
Quote them as studies with their dates, never as rules.

## Check crawler access

Each crawler token has its own purpose.
Blocking a training crawler does not remove the site from AI answers, except that Google-Extended also covers Gemini app grounding.
Blocking a search crawler does.

| Purpose | Tokens | What blocking it does |
|---|---|---|
| Search and AI features | Googlebot, Bingbot, Applebot | Removes the site from Google Search and its AI features, from Bing and Copilot, or from Siri, Spotlight and Safari search. Do not block on a site that wants search traffic |
| Assistant search indexes | OAI-SearchBot, PerplexityBot, Claude-SearchBot | Removes or reduces citations in ChatGPT search, Perplexity or Claude |
| User-triggered fetches | ChatGPT-User, Perplexity-User, Claude-User | Stops the assistant reading a page a user asked about, where the bot follows robots.txt. Claude-User does. OpenAI and Perplexity say robots.txt may not apply to theirs |
| Gemini training and grounding | Google-Extended | Stops use for Gemini training and for grounding answers in Gemini Apps and Vertex AI, so it can reduce the site's use in Gemini app answers. No effect on Google Search, AI Overviews or AI Mode |
| Model training | GPTBot, ClaudeBot, Applebot-Extended, CCBot | Opts out of training for those models. CCBot feeds the Common Crawl corpus many models train on. No documented effect on search or citations |

Check the current names on the operators' pages before you write a rule: [OpenAI](https://developers.openai.com/api/docs/bots), [Anthropic](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler), [Perplexity](https://docs.perplexity.ai/guides/bots) and [Google](https://developers.google.com/crawling/docs/crawlers-fetchers/google-common-crawlers).

1. Fetch the live robots.txt with curl or your web fetch.
   For each token in the table, find the group that applies to it: its own `User-agent` group if it has one, otherwise the `*` group.
   List every `Disallow` that covers pages the user wants found.
2. In the repository, find where robots.txt comes from (a static file, `app/robots.ts`, a robots module or a CMS setting) and compare it with the live file.
3. Check the CDN and firewall, because Google asks site owners to allow crawling there too.
   OpenAI and Perplexity ask site owners to allow their published addresses there, and Perplexity documents the firewall rule.
   The lists are at openai.com/searchbot.json, perplexity.com/perplexitybot.json and claude.com/crawling/bots.json.
   Anthropic's bots do not pass a CAPTCHA, so a challenge page blocks them.
   Cloudflare can block AI crawlers by group (search, agent, training), has done so by default on many new domains since July 2025, and can add rules to the robots.txt it serves.
   A training block can also catch crawlers that serve both search and training, such as Googlebot and Bingbot, so confirm that search crawlers are still allowed.
   Ask the user to check the bot settings of their CDN or host, or to search their server logs for the crawler names.
   A curl request with a crawler's user agent that gets a 403 or a challenge page shows a rule that matches that name.
   For Googlebot and Bingbot, the rule may block only requests from unverified addresses, so confirm with URL Inspection in Search Console or Bing Webmaster Tools before you report the crawler as blocked.
   A 200 does not prove the real crawler gets through, because CDNs often verify crawlers by IP address.
4. Fetch the key pages without running JavaScript, as the rules describe, and confirm the main content, title and links are in the HTML.
5. Look for `noarchive`, `nocache`, `nosnippet` and `max-snippet` in meta robots tags and `X-Robots-Tag` headers, and say what each one limits.
6. Report which bots can reach which pages.
   A robots.txt change takes about a day to reach ChatGPT search and Perplexity.
   Do not change robots.txt or CDN rules without asking.
   Explain the trade-off from the table: search and user-triggered crawlers make citations possible, and training crawlers are a business choice.
   Save the user's decision and the reason with remember.

## Search Console's generative AI setting and report

- Setting: "Search generative AI" in Search Console settings is Include by default.
  Exclude removes the whole property from AI Overviews, AI Mode and Discover AI features within one to two days.
  It does not change regular rankings or AI training.
  If the user wants AI visibility, ask them to confirm it says Include.
- Report: the Generative AI performance report shows impressions in AI Overviews and AI Mode by page, with no queries and no clicks.
  get_performance does not include it, so ask the user to export it by page and compare it month to month.
  The measure guide says how to read it.
- Bing Webmaster Tools has an AI Performance report with citations, cited pages and grounding queries, which are the phrases Bing searched, not what users typed.
  Its data is sampled and has no clicks.

## What plausibly helps

None of these guarantees a citation.
Label each one as plausible when you recommend it.

| Change | Why it plausibly helps | Evidence |
|---|---|---|
| Rank in the top results for the searches behind the prompts, including the narrower sub-questions | Google retrieves from its index, and Copilot grounds in Bing's | Documented by Google and Bing. Studies disagree on how much rank matters |
| A direct answer near the top of the page, and under each heading that asks a question | Bing asks for key information early | Documented by Bing. Google says not to chunk or rewrite for AI, so write it for readers |
| Specific facts and numbers, with sources and dates | Bing asks for explicit facts, and SE Ranking found pages with more data points and expert quotes were cited more | Bing documented, one correlation study |
| Pages kept current, with a visible updated date when the content really changed | Bing asks for accurate updates | Documented by Bing, and SE Ranking's study agrees |
| A clear entity: one product name used everywhere, an about page, Organization markup with `sameAs` links, and the same description on the user's profiles elsewhere | Bing asks for consistent entity names | Bing documented, the rest is practitioner advice |
| Real mentions on sites people trust: reviews, community threads, comparison articles, press | Assistants repeat what the web says about a product | Two correlation studies, on mentions and on linking sites. Google says inauthentic mentions do not help. The links guide covers earning them |
| Comparison, alternatives and pricing pages with visible facts | Many buyer prompts ask "X vs Y", "alternatives to X" or "how much does X cost" | Practitioner advice. The content guide covers these pages |
| Main content, links and metadata in the server HTML | Many AI crawlers do not run JavaScript | Measured by Vercel in December 2024 |

To apply them:

1. Pick three to five pages that matter: the home page, pricing, the main feature page, comparison pages and the posts with the most clicks.
2. Fetch each live page and check it against the table.
3. Propose exact edits for each URL.
   Call get_serp for the search behind a buyer prompt only when you need to see what ranks, because each call uses the user's credits.
   It returns Google's AI Overview when there is one, with the sites it cites, so check whether the user's site is among them.
4. Do not invent product facts to make a page more quotable.
   Write `TO CONFIRM` placeholders, ask the user, and save the confirmed facts with remember.
5. Cover a topic's sub-questions on one strong page or in pages linked to it.

## Measure AI visibility honestly

You usually cannot open ChatGPT, Perplexity, Copilot or AI Mode yourself.
Give the user the prompt list and a table to fill in.
If the user has API access to an assistant with web search, you can run the prompts there, but label those runs, because API answers can differ from the apps.

1. Write 10 to 20 prompts a real customer would type.
   Mix research prompts ("how do I ..."), comparison prompts ("best ... for ...", "alternatives to <competitor>") and evaluation prompts ("is <product> good for ...", "<product> pricing").
   Take them from get_project's keywords, People Also Ask in briefs, the user's Search Console queries, and the questions customers ask in sales and support.
   Do not write prompts designed to force the brand into the answer.
2. Give the user the full list to keep, and save the prompts with remember as notes, several per note, since each holds at most 300 characters.
   Keep the monthly results in a file or sheet the user owns, not in memory.
3. Run each prompt three to five times in each assistant, in fresh sessions with memory off where possible.
4. Record each run: date, assistant and mode, prompt, whether the brand is mentioned and in what words, which of the user's URLs are cited, which competitors are named, and any wrong fact about the product.
   Keep failed or empty runs in the log, but leave them out of the counts.
5. Report rates with the sample size, for example "mentioned in 6 of 40 runs, site cited in 2 of 40".
   Keep three outcomes apart: cited (a page is a source), mentioned (the name appears) and recommended (the product is on the shortlist).
   Group the other cited domains by kind (a competitor's page, a list or review article, a forum thread, a video), because that mix shows where the brand needs to appear.
6. Repeat monthly with the same prompts.
   Treat small changes as noise, and note when an assistant changed its model.
7. Ask the user for AI referral traffic in Google Analytics, by session source: chatgpt.com, perplexity.ai, claude.ai, copilot.microsoft.com and gemini.google.com.
   get_performance has Analytics totals only.
   Some apps send no referrer, so this undercounts.

Do not report a visibility score, an "AI share of voice" or any number that is not a count over a stated sample.

## llms.txt

- llms.txt is a proposal for a Markdown file at `/llms.txt` that summarizes a site for agents when they answer.
- Google Search ignores it.
  It neither helps nor harms.
- No study has linked it to more citations.
  A study of about 300,000 domains found no relationship.
- Search crawlers rarely request it.
  A July 2026 log study of 83 sites counted 7 requests from OpenAI's crawlers in 12 weeks, against 3,990 for robots.txt.
- Lighthouse has an experimental, unscored llms.txt check in its Agentic Browsing category.
  It checks agent readiness, not ranking.
- It is fine to publish one if the user wants it for other tools, for example so coding agents can read a developer product's docs.
  Keep it accurate, and do not present it as SEO or AI search work.

## Mistakes to avoid

- Treating GPTBot or ClaudeBot as the bots that put a site into assistant answers.
  They are training crawlers.
- Blocking Google-Extended to stay out of AI Overviews.
  Use the snippet controls or the Search Console setting.
- Creating a page per prompt or per fan-out sub-question.
- Hidden text or instructions aimed at AI models.
  Google treats it as spam and Bing as abuse.
- Splitting pages into fragments, adding FAQ markup, or rewriting in a special style for AI.
- Reporting one run as a result, or reporting a score.
