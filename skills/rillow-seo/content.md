# Content

Read this before you write or refresh a post, or build a feature, comparison, best list, alternatives, integration or pricing page.
End with a page that answers the search better than what ranks now, saved with save_article with no issue marked fix, and recorded with mark_published once it is live.

## 1. The workflow

get_brief's writing guide covers the writing itself, and this guide covers the rest.

1. Take the plan item from get_project.
   If the page the user wants is not in the plan, add it with update_plan first, choosing it as the keywords guide says.
2. Call get_brief with its card_id.
   If the result has ranking_page, the site already ranks for this keyword with that page.
   Improve that page instead of writing a second one, as rule 5 says, unless the user asked for a new page.
3. Fetch the strongest pages in the top 10 and read them, as the writing guide says.
   Note which page type holds most of the top 10, and make that type.
4. Collect first-hand material before you write (section 2 says what counts).
   Ask the user for what the site and the memory do not have.
5. In a repository, write the page the way existing posts are written.
   - Find where posts live, for example `content/blog` or `src/content`, and read two or three of them.
   - Copy their frontmatter fields: title, description, dates, author, tags, hero image, draft flag.
   - Use the same components (callouts, figures, code blocks) and the same image folder.
   - Check how a file becomes a URL, so the slug you pass to save_article is the real path.

   Without a repository, write Markdown the user can paste into their CMS.
6. Make images with generate_image and place them as section 7 says.
7. In a repository, build the site locally and read the HTML of the new page, as rule 9 says: title, meta description, canonical, one main heading, dates, structured data, image paths with width and height.
   Check that the page appears in the built sitemap.
   Without a repository, run these checks on the live page after the user publishes it.
8. Call save_article with the whole file, frontmatter included.
   Fix every issue marked fix and save again.
   Read each warn and fix it when the fix is right for the page.
9. Leave committing and deploying to the user unless they ask you to do it.
   Use publish_article only when the user asks to publish through Rillow.
10. After the user deploys, fetch the live URL as rule 3 says, then call mark_published with the card_id and the URL.
11. Add 2 or 3 internal links to the new page from related pages on the site.
    The links guide says how.

## 2. What Google rewards

Before save_article, run the page through these questions from Google's "Creating helpful content" page.
Each "no" is something to fix.

- Does it give original information, research or analysis, beyond the obvious?
- Does it add substantial value compared with the other pages in the results?
- Does it show first-hand experience, such as having actually used the product?
- Does the title describe the content without exaggerating?
- Will a reader leave knowing enough to reach their goal, without searching again?
- Is it free of easily verified factual errors?
- Would the user's audience find it useful if they came to the site directly, not from search?

Google also lists warning signs: writing to a word count (it has no preferred one), mainly summarizing what others say, producing lots of content on many topics in the hope some of it performs, and changing dates to look fresh when the content has not substantially changed.

On a product site, first-hand detail means screenshots of the real product, numbers from the product or the user's own use, the steps as they really are including the limits, and customer situations the user may share.
Never invent numbers, quotes, customers, test results or credentials.
Write TODO where a fact is missing and ask the user.
save_article flags TODO as fix, so the page cannot pass with a gap left in it.

AI-written content is fine when it is accurate and useful and not made mainly to manipulate rankings.
Fact-check every claim before saving, including the title, meta description, alt text and structured data, because Google can show all of them.
Where a reader might ask "how was this made?", say how automation was used.
Do not list AI as the author, and never invent an author's name, photo or credentials.

## 3. What Google's spam policies punish

| Policy | What it covers | Where a product site meets it |
|---|---|---|
| Scaled content abuse | Many pages made mainly to manipulate rankings that give users little value, however they are made: AI pages without added value, content stitched from other pages, a page for each query variation or AI fan-out query | Bulk posts from a keyword list, templated pages |
| Doorway abuse | Substantially similar pages for similar searches, closer to search results than to a browsable section, or city and region pages that funnel to one page | Integration, use-case, alternatives or location pages that differ only in a name |
| Site reputation policy | Content from outside the site (freelancers, guest authors, partners, white-label services) published mainly to rank on the host site's signals. Third-party content alone is fine. | Sponsored or partner posts unrelated to the product |
| Keyword stuffing | Repeating words so often that it sounds unnatural, or blocks listing cities or terms | Forcing the keyword into every heading and paragraph |

## 4. Commercial pages

Google's reviews guidance covers head-to-head comparisons and ranked lists, including a vendor comparing itself.
Evaluate from the user's point of view, show evidence of your own use such as screenshots, give measurements where they exist, and discuss benefits and drawbacks from your own research.
Say what sets each option apart and which one is best for which use, and when you call something the best, say why.
When the product is on its own list, rank it by the same criteria and say the site makes it: a June 2026 study found Google's AI answers cite self-ranked lists but recommend a competitor 69% of the time.

| Page | Make it when | What makes it rank and convert |
|---|---|---|
| Best list or alternatives ("best x", "x alternatives") | Buyers search for options and the product is a real one | How you ranked them, a numbered heading per option with its facts, drawbacks and who it fits, a comparison table, prices with the date checked, an FAQ. For alternatives, first why people leave x, from reviews and forums. get_brief gives the outline |
| Comparison ("x vs y") | The product competes with x for the same buyers | The verdict first (who each is for), a table of the factors that decide the purchase, where the competitor is better, screenshots of both, the date checked. One page per pair, separate from the list, because its searcher has narrowed the choice to two. |
| Integration | The integration exists and works | What the combination does, setup steps with screenshots, limits, who it is for |
| Use case or audience ("x for freelancers") | The audience has its own workflow the product serves | Their problem in their words, the workflow in the product, examples and screenshots for that audience |
| Pricing | The product has public prices | Real numbers or ranges, what each plan includes, what costs extra, trial and billing terms, answers about total cost. |
| Feature | Buyers search for the capability by name or by the job it does | The job in buyer words first, how it works in the product, screenshots, limits. One page per capability, not per setting. |
| Glossary | The pages above exist, and the audience searches the term | A direct definition, an example from the product, links to the posts and features that go deeper. Never generated in bulk. |

Facts about competitors go out of date fast, and a wrong claim can bring legal trouble.
Take them from the competitor's live pricing page, docs and changelog, write the date you checked them, and ask the user to confirm the comparison before it ships.

save_article checks Markdown and MDX.
When the page is a component instead, run the same checks yourself on the built or live HTML: keyword in the title and main heading, meta description, links to the site's own pages, alt text.
mark_published still records its live URL on the plan item, so Rillow tracks its position.

## 5. Refreshing a post

The measure guide says which posts to refresh.

1. If the post is not a plan item, add its keyword with update_plan (the page_action for updating an existing page).
   If the plan item has no published_url in get_project, call mark_published with the post's URL.
   Then call get_brief with the card_id and refresh: true.
   Its Search Console queries are site-wide, not only this page's.
2. Fetch the live post and read it next to the current top 10.
3. Find what searchers want now.
   - Has the top 10 changed page type or angle since the post was written?
     Then the post needs rework, not only new facts.
   - Which of the Search Console queries does the post not answer?
   - Which People Also Ask questions does it miss?
4. Size the change to the problem.
   Google advises against changing pages that already perform well, so a post that still ranks well needs targeted fixes, not a rewrite.
5. Update facts, prices, product names, screenshots and examples.
   Add the missing subtopics and remove sections nobody needs any more.
   Rework the title and first paragraph if people now phrase the search differently.
6. Keep the URL.
   A new URL starts without the old one's history and needs a redirect, which rule 8 says to ask about.
7. Change the visible "Updated" date, dateModified and the sitemap lastmod only when the content changed meaningfully.
   A new date on unchanged content is one of Google's warning signs, and Google uses lastmod only when it is consistently accurate.
8. Call save_article with the same card_id, which saves a new version, and fix the issues.
9. After the user deploys, check the live page.
   Call mark_published again only if the URL changed.

## 6. Templated and programmatic pages

Templated pages are where scaled content abuse and doorway abuse start.

Go ahead only if every answer is yes.

1. Does each page carry unique, useful data a visitor cannot get from its sibling pages?
2. Is there real search demand for each page?
   Check a sample with get_keyword_metrics, which takes up to 200 keywords.
3. Would a visitor browsing the site want each page if search engines did not exist?
4. Will someone keep the data current?

Then:

- Build one page by hand first, and check that it gets indexed and earns impressions before you turn it into a template.
- Give the set a hub page that links to every page, and link each page back to the hub and to its closest siblings.
- Write titles and descriptions from each page's own data, readable and different on every page.
  Google calls titles that vary by only one piece of information boilerplate.
- Give the set its own sitemap file and submit it, because the Page indexing report filters by sitemap, not by folder.
- Publish in small batches.
  Before the next one, ask the user for that report filtered to the set's sitemap and the Performance report filtered to its folder: how many pages got indexed, and their impressions and clicks.
  A growing count of "Crawled, currently not indexed" usually means Google does not value the template's pages.
- For pages that earn no impressions after a few months indexed, improve them, merge them or remove them.
  Show the user the list and ask first, as rule 8 says.
- Never generate pages for integrations that do not exist or for places the business does not serve.

## 7. On-page details

### Titles and meta descriptions

Google builds the title link from the `<title>`, the main heading, `og:title`, other text on the page and the text of links that point to it.
It rewrites titles that are stuffed, boilerplate or do not match the page.
It sets no length limit, but it cuts titles and descriptions to fit the screen.
save_article warns above 60 characters, about where titles get cut, so put the words that decide the click first.
Write each title for the click: what this page gives and why it is the result to pick.
Put the site name once, at the start or the end, separated by a hyphen, colon or pipe.

| Page | Title | Description |
|---|---|---|
| Post | The search in the reader's words, then what they get | The answer, or what the post covers |
| Comparison | "X vs Y" and the difference that decides | Who each is for, what the table compares, when it was checked |
| Best list or alternatives | "Best X" or "X alternatives" and the audience, or a count if the count is real | The criteria and the options covered |
| Pricing | "Product pricing" and the plan range if it is current | Plans, prices, free tier or trial |
| Integration | "Product + Tool integration" and what it does | What syncs, how setup works, who it is for |
| Feature or use case | The job in buyer words, then the product name | What the reader can do with it |

Add a year only when the content was updated for that year.
Google does not use the meta description for ranking, and shows it only when it describes the page better than the page's own text.
save_article warns above 155 characters, because Google cuts it to fit the screen.

### Headings

Give the page one visible main heading that says what the title says, because Google reads it when it builds the title link.
save_article warns about a second H1 and skipped heading levels.
Fix them for readers and screen readers, even though Google does not rank pages on heading count or order.

### URL slugs

Use short, lowercase words with hyphens, in the reader's language, in the same folder as similar pages (`/blog/`, `/compare/`, `/integrations/`).
Leave dates out of evergreen slugs, so a refresh can keep the URL.
Use the keyword's words when they read naturally, and save_article warns when they are missing.
Google says words in the URL have hardly any ranking effect, so never change a live URL just to add them.

### Images

- Make images as get_brief's writing guide says. generate_image adds memory's style and style image.
  Keep words, steps and numbers out of images, and never generate logos, reviews or screenshots.
- generate_image returns a URL, a file name, width, height and alt text.
  In a repository, download the file into the site's image folder under that descriptive file name and link the local copy.
  Without a repository, give the user the URL and the alt text to upload.
- Use `<img>` or the framework's image component, not a CSS background, because Google does not index background images.
- Set width and height, so the page does not shift while the image loads.
- Lazy-load only images below the fold, never the hero.
- Put each image next to the text it illustrates, with alt text as the writing guide says and no keyword lists.
- Prefer real product screenshots to generated images wherever the page shows how the product works, because they are first-hand evidence.
- Set the hero image as `og:image`, so shared links and Google have a preview image.

### Author and date

Show a byline on posts and comparisons, linked to an author page with the person's background.
Show the date with a label such as "Published" or "Updated", and keep it the same as datePublished and dateModified in the markup.

### Structured data

Markup must describe what is visible on the page.
It makes a page eligible for a search feature, never guarantees one, and Google documents no ranking effect from it.
The new-site guide lists the types and properties and how to validate them in the Rich Results Test.

| Page | Markup |
|---|---|
| Post, comparison, alternatives | `BlogPosting` or `Article` with headline, author (name and url), datePublished and dateModified with a timezone, image |
| Pricing or product page | `SoftwareApplication`, only with a visible price and ratings from reviews shown on the page and collected by the site |
| A page that shows breadcrumbs | `BreadcrumbList` following the reader's path |

Do not add `FAQPage` markup for rich results, which Google shows for no site since 2026-05-07, or `HowTo` markup.
A visible FAQ section is fine when readers need it.
Google's list of supported features: https://developers.google.com/search/docs/appearance/structured-data/search-gallery

## Mistakes to avoid

- Inventing facts, numbers, quotes, customers or claims about competitors.
- Rebuilding the top 10's outline with nothing they lack.
- A comparison that only flatters the product.
- Templated pages that differ only in a name.
- Treating a clean save_article result as proof of quality.
  It means the basics are in place, not that the page beats the top 10.
- Calling mark_published before checking the live page.
