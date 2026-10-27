# Keywords and the content plan

Read this before you propose topics, add plan items, or decide which page targets which search.
End with the topics the user picked, each with its numbers and reason, added with update_plan, one page per search intent.

## 1. Start from the product, not a keyword list

The product and its buyers decide which keywords matter.

1. Read the memory from get_project.
   It should say what the product does, who buys it, what it costs, its competitors and its integrations.
2. Fill the gaps from the site: home page, pricing, features, docs, changelog and existing posts.
   Save each new fact with remember, in the product or competitors section.
3. Ask the user for their customers' words: support emails, sales call notes, reviews, onboarding answers, forum threads where buyers ask for a tool like theirs.
   Buyers search in their own words, not in the product's feature names.
4. List the pages the site already has: the routes and content folder in a repository, or sitemap.xml otherwise.
   You need this list in section 7.

Turn this into 5 to 10 seeds.
A seed is one to three words a buyer would type.
find_keywords returns the seed and searches that contain it, and saves the 50 with the most volume, so a broad seed fills up with head terms.
Use a longer seed to reach the long tail.

| Seed source | Example for an invoicing tool | What it finds |
|---|---|---|
| What the product is | invoice software | Category searches and "best" lists |
| Problems it solves | late invoice payments | Problem-led posts |
| Competitors | freshbooks alternative | Alternatives and comparison pages |
| Integrations | stripe invoicing | Integration pages |
| Audiences and use cases | invoicing for freelancers | Use-case pages |
| Existing rankings | find_keywords without a seed | Pages to improve and related topics |

Drop seeds the product has nothing first-hand to say about.
Google lists entering a topic without real expertise, mainly for search traffic, as a warning sign.

## 2. Get the data without wasting paid calls

Paid calls use the user's Rillow credits, and each result says how many.

| Call | Cost |
|---|---|
| find_keywords without a seed, or with a seed used before | Free, it reads saved data |
| find_keywords with a new seed | About 0.3 credits, then saved |
| get_competitors without a domain | Free, or up to 5 credits on a new site |
| get_competitors with a new domain | About 0.3 credits, then saved |
| get_serp | 0.03 to 0.06 credits, then free for 12 hours |
| get_serp with strength | About 0.7 credits more, then free for 7 days |
| get_keyword_metrics | Up to 0.5 credits for 200 keywords, then saved |
| get_performance with queries | Free |
| get_ranked_keywords | Up to 0.5 credits, then free for a day |
| get_brief | Up to 0.3 credits when uncached, then free for 7 days |

Work in this order.

1. Call find_keywords without a seed.
   already_ranking lists what the site ranks for, with position and URL.
   These positions count every element on the page, such as People Also Ask, so they can read lower than get_serp's organic position.
   position_min 4 and position_max 20 narrow it to pages that show below the top results, about positions 4 to 20 (a practitioner heuristic).
   Such a keyword needs its page improved, not a new page.
   With Search Console connected, get_performance with queries true lists such queries from Google's current data.
   Pass queries without a volume to get_keyword_metrics, because update_plan fills only numbers Rillow saved.
2. Call get_competitors without a domain.
   Pick one or two true search competitors: sites with a similar product and audience that share keywords with the site.
   Skip platforms and large publishers such as Reddit, YouTube, Wikipedia or news sites, because their keywords are not gaps a product site can fill.
3. Call get_competitors with that domain, filtered, for example with max_difficulty and intent "commercial".
   The result is a sample of the gap: the competitor's keywords the site does not rank for, among the 50 non-branded ones Rillow saved for it.
4. Call find_keywords with your best seeds, product-led ones first.
   Filters on a used seed are free, but only filter its saved ideas.
   A seed with fewer than 10 ideas also gets google_suggestions, what Google suggests as people type it.
   For a new category, also search Reddit and forums yourself for the words people use.
5. Skip keywords marked in_plan.
   They already have a plan item.

Before a batch of paid calls, tell the user how many you plan and why.

Read the numbers for what they are.

- Volume is a DataForSEO monthly average for the project's country, as rule 2 says.
  A seasonal search hides its peak in the average: peak_month names it, so publish two or three months before.
- yearly_change compares searches with a year before: a falling search is a weak bet.
- Tools miss much of the long tail.
  A comparison or alternatives search with tiny or zero volume can still bring buyers.
  A volume of 0 in Rillow can also mean DataForSEO has no number for the search.
  Keep it when customers use those words.

## 3. Judge what this site can win

Difficulty is DataForSEO's estimate of how hard it is to reach the top 10, on a logarithmic scale from 0 to 100, based on the link profiles of the current top 10 pages among other factors.
It is not a Google metric, it knows nothing about the user's site, and there is no difficulty value below which every site can win.
A site with a low domain rating should prefer low-difficulty keywords, but decide by comparing the site with the pages that rank, not by a cut-off.

The site's strength:

- How many keywords it ranks for and how high: data_ready.ranked_keywords and top_ranked_keywords in get_project, and already_ranking in find_keywords.
- Its domain rating, a DataForSEO estimate of link strength from 0 to 100.
  get_project returns the site's own, and get_competitors each competitor's.
  It is DataForSEO's backlink rank divided by 10, so it does not match the Ahrefs DR or Moz DA a user may quote.
- Whether it already ranks for searches close to this one.
  That is the strongest sign it can win this one too.

The pages that rank:

- Keyword results give top10_domain_rating and top10_referring_domains, the top 10's average site rating and linking sites per page, to compare with site_domain_rating.
  An average hides one weak page, so check the shortlist with get_serp, with strength true for each result's domain_rating, referring_domains and a strength_note.
- get_serp labels results that aren't articles with kind.
  Otherwise, judge by the kind of site.

| What you see in the top 10 | What it means for a new or small site |
|---|---|
| Forum threads, Reddit, Quora, Q&A pages | Often a sign that no specific page answers the search. A page with first-hand detail can win. |
| Small blogs, thin pages, outdated pages (an old year, old screenshots, a retired product) | Winnable |
| Pages on a broader topic that only partly answer the search | Winnable with a page that answers it exactly |
| Competitors with a domain rating far above the site's | A later target, once the site has links and related rankings |
| Large brands, government sites, Wikipedia and major publishers in most positions | Skip it for now |
| A page type the site cannot make, such as a marketplace or a calculator it does not have | Skip it, or make that page type if the product can |

A new or small site does best with specific, long-tail, high-intent searches where small sites, forums or thin pages already rank.

## 4. Read the results page before choosing a format

Call get_serp for each finalist, not for every idea.
The top 10 shows the intent, the page type and the depth Google rewards for that search, as rule 4 says.

1. Label each result with its page type: product or feature page, pricing page, list of tools, head-to-head comparison, how-to guide, definition, template or tool, documentation, forum thread, video.
2. Count the labels.
   The type that holds most of the top 10 is the format to make.
3. Check the intent from find_keywords against that count.
   When it disagrees with what ranks, trust what ranks.
4. Read People Also Ask and related searches.
   Most are questions for sections of the same page.
   Give one its own post only when its own results page shows a different page type.
5. Note the featured snippet.
   Its text shows the kind of answer Google shows for this search, such as a definition, steps or a comparison.

| Most of the top 10 | Make |
|---|---|
| Vendor product, feature or pricing pages | A product or feature page (format landing_page). A blog post rarely ranks there. |
| Lists of tools or alternatives | A list page with honest comparisons (format listicle) |
| Head-to-head comparisons | A comparison page (format comparison) |
| Step-by-step guides | A guide or tutorial post (format guide or tutorial) |
| Short definitions | A section in a broader post, or a glossary entry later |
| Templates, generators, calculators | A tool page (format tool_page) if the product can offer one, otherwise skip |
| A mix of types | Split intent. Pick the type that fits the product and cover the other need in a section, or skip it. |

Sometimes the right answer is a different page: the existing page that already ranks, a section added to an existing post (page_action add_section), or no page at all.
Say so when there is no gap the user can fill, for example when every result comes from a kind of site the user cannot become.

## 5. Estimate the clicks before the work

Estimate monthly clicks with a rough heuristic, showing the inputs and the arithmetic, as rule 1 says:

clicks ≈ monthly volume × click rate at the position the site can realistically reach × share of clicks left after other features

- Click rate: Backlinko's 2019 study of 4 million Google results put position 1 at 27.6 percent, position 6 at 4.9 percent and position 10 at 2.4 percent.
  Treat these as a starting point.
  When the user exports Search Console, use the site's own click rate at similar positions instead.
- Realistic position: the position the site can reach in the coming months given section 3, not position 1 by default.
  For a page that already ranks, start from its organic position in get_serp.
- Other features: get_serp shows an AI Overview, the featured snippet and People Also Ask, but not ads or shopping results.
  In Pew Research's study of US adults (March 2025), people clicked a regular result on 8 percent of visits to a results page with an AI summary, against 15 percent without one.
  With an AI Overview in get_serp, apply that discount.

Example: 400 searches a month, position 6 at about 5 percent, an AI Overview above the results: 400 × 0.05 × 0.5 = 10 clicks a month.

The number is for comparing topics with each other, not a forecast to promise, as rule 7 says.
When the number does not justify the work, say so before anyone writes.
A small number can still be worth it for a page close to a purchase.

## 6. Weigh business value, not volume

Ask how close the searcher is to needing the product.

| Stage | Example searches | Priority for a new product |
|---|---|---|
| Choosing a product | "x alternatives", "x vs y", "x pricing", "best x for freelancers", "x stripe integration" | First |
| Solving a problem the product solves | "how to <the job the product does>" | Second |
| Learning about the topic | "what is <term>", broad guides | Later, for coverage and links |

Grow and Convert showed one SaaS client's blog: bottom-of-funnel posts turned 0.3 to 4.3 percent of visits into signups, top-of-funnel posts 0.03 to 0.19 percent, and the best converter targeted a comparison search that keyword tools showed with zero volume.
That is one client's data, so treat it as a direction, not a number to forecast with.

Sort candidates by business value and winnability together.
A search with 200 a month that names the product's category and a buying word beats one with 20,000 that the product cannot answer.
Record the main reason in update_plan's signal field, for example high_intent, low_competition, quick_win or content_gap.

## 7. One page per search intent

Google answers many phrasings of one need with the same pages.
A plan item is one post, so give each intent one plan item and treat the other phrasings as its variants.
Pages for every variation compete with each other, and Google counts separate pages for each query variation, made to manipulate rankings, as scaled content abuse.

When you are unsure whether two keywords are one intent, call get_serp for both and count the URLs their top 10s share.
A common heuristic from SERP-overlap clustering:

| Shared URLs | Treat them as |
|---|---|
| 7 or more | One page. Add the stronger keyword and name the other in its note. |
| 4 to 6 | Usually one page. Make two only when the searches ask for a different job or page type, and link them to each other. |
| 2 or 3 | Related pages that link to each other |
| 0 or 1 | Separate topics |

Plurals, word order and plain synonyms ("invoice app" and "invoicing app") are one intent without checking.
A word that changes the job ("x pricing" and "x alternatives") makes a different intent without checking.

Before you add a keyword, check that no existing page already targets its intent:

- already_ranking from find_keywords, which gives the ranking URL
- the site's pages you listed in section 1
- the plan items in get_project, which shows the first 30, and the in_plan marks find_keywords puts on the rest

If a page exists, add the keyword with the page_action for updating an existing page (or add_section) and the page's URL in the note, as rule 5 says.
If two of the site's pages already compete for one intent, that is a merge for the audit guide, and rule 8 applies to any redirect.

## 8. Clusters and pillars

In Rillow, a cluster is a group of keywords served by one pillar post, with supporting posts for the narrower searches.

- Make a cluster when a topic has a broad search plus several narrower searches that each need their own page (they share some results with the broad search but ask for a different job or page type), and the product relates to all of them.
- One or two keywords do not need a cluster.
  Add them as plan items.
- The pillar keyword is the broadest one.
  Its top 10 shows overview pages, it usually has the most volume, and the narrower keywords' results overlap with it most.
- The pillar post covers the topic at overview depth and links to every supporting post.
  Each supporting post links back to the pillar.
- Write the pillar first or together with the first supporting posts, so they have a page to link to.

In update_plan, create the cluster with name, description and pillar_keyword, and add the pillar keyword and the supporting keywords in add with the same cluster name.
The pillar is set only when its keyword is in the same call's add with that cluster name.
If add skips the pillar keyword because it is already in the plan, move its item into the cluster with assign, then call update_plan again with the cluster's pillar_keyword and that keyword in add.

## 9. Propose, then update the plan

Propose 5 to 10 topics in one table, best first.

| Keyword | Volume | Difficulty | Top 10 now | Page | Clicks estimate | Why |
|---|---|---|---|---|---|---|

- Volume and difficulty come from find_keywords or get_competitors, labelled as rule 2 says.
- Top 10 now is one line on the page types and kinds of sites, for example "4 review-site lists, 3 vendor pages, 2 Reddit threads".
- Page is the type and either the planned URL or the existing URL to improve.
- Clicks estimate comes from section 5, with its inputs.
- Why gives the business value and the winnability in one sentence.

Then ask the user which to add, and wait for their choice before adding many items.
Call update_plan with the chosen keywords.
For each one, set format, signal, page_action, the cluster or strategy, and a note with the angle in one or two sentences: what the page will have that the current top 10 lacks.
If the result says Rillow has no volume for a keyword, get_keyword_metrics fetches it.

Save the decisions with remember.
Record the focus the user chose, and topics they rejected with the reason, as notes, so later sessions do not propose them again.

## Mistakes to avoid

- Sorting by volume.
  The top rows are often out of reach or useless to the business.
- Trusting the difficulty score or the intent label without reading the top 10.
- A plan item for every phrasing of one search.
- A new post for a search an existing page already ranks for in about positions 4 to 20.
- Dropping low-volume comparison and alternatives searches automatically.
- Proposing topics the product has no first-hand angle on.
- Presenting the clicks estimate as a forecast.
