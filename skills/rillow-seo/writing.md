# Writing

Read this before you write any text for the user's site: a post, a landing, feature or pricing page, a title, a meta description, alt text, an email, or an edit to text that is already live.
get_brief includes these rules with every post brief, and save_article flags the patterns it can find in a saved post.

The other guides and the rest of get_brief's writing guide decide what a page covers and how it is built.
These rules decide how each sentence reads.
The voice section of the site's memory comes first: when it asks for something these rules advise against, such as bold terms or title case headings, follow the memory.
When you edit the user's own text, keep their meaning, facts, point of view and intended audience, and follow their instructions for the format.

The goal is text that is clear, specific and sounds like a person talking to another person.

## Fix meaning first

- Every noun, pronoun and comparison needs a clear reference.
  Introduce an object before calling it "the asset", "the process" or "the personal part".
  Name what it actually is.
- Complete comparisons.
  If you write "better", "faster", "cheaper" or "since", explain compared with what or since when.
  Remove comparisons you cannot support.
- State the useful point directly.
  Delete "Here's the thing", "Let me explain why this matters", "What if I told you" and other announcements of what the next sentence will say.
- Finish one concrete example.
  Explain the situation, what someone does and what happens next.
  Replace lists of unrelated capabilities with one example followed through to its outcome.
- Use at most one analogy, with one callback only if it adds understanding.
- Name actions plainly.
  Write "write an article" instead of "put it at the top of the calendar".
  Write "checked the pages" instead of "pointed the audit at the site".
- Each paragraph needs a specific point the intended reader can understand or use.
  Cut paragraphs that could be pasted unchanged into an unrelated article.

## Write connected sentences

- Do not stack sentences with the same opening.
  Combine repeated constructions when they express one continuous thought.
- Avoid runs of short fragments for drama: "More traffic. More leads. More sales."
  Use conjunctions when the ideas belong together.
- Remove staged reversals: "Not X, but Y", "It's not just X", "This isn't about X", "X won't do Y. Z will", and "The worst teams do X, the best teams do Y".
  State the useful action and its result directly.
  Explain real differences with specifics.
- Do not open with "Most people", "Most founders", "Most companies", "Most brands" or "Most teams" unless the task needs a population comparison you can source.
- Do not force every list, sentence or example into a group of three.
  Use the number the information needs.
- Vary sentence length naturally.
  Keep conjunctions such as "and", "but", "because", "so" and "which means" when they explain how ideas connect.
- Keep the same noun for the same thing.
  A spreadsheet can stay a spreadsheet, so do not cycle through "document", "workbook" and "asset" for variety.
- End on a useful action or specific fact.
  Do not add a clever symmetrical comparison to manufacture a strong ending.

## Use plain words

- Prefer "is", "are", "has", "wrote", "used", "moved", "tried" and "help" when they express the meaning accurately.
- Remove stock AI or promotional wording used as decoration: delve, unlock, elevate, empower, leverage, foster, enhance, showcase, underscore, tapestry, pivotal, crucial, robust, seamless, groundbreaking, revolutionary, game-changer, world-class, cutting-edge, holistic, nuanced, multifaceted, vibrant, valuable insights, rich, profound, myriad and "look no further".
- Also cut "serves as", "stands as", "is a testament to", "plays a key role", "marks a new era", "in today's landscape", "aligns with" and "resonates with" when they replace a precise statement.
- Do not swap a banned word for an equally vague synonym.
  Say what happened, who did it or what the reader should do.
- Avoid "shift" as a vague noun for change.
  Name what changed and, when known, by how much.
- Remove sentence endings that only comment on the sentence: ", highlighting...", ", underscoring...", ", showcasing..." or ", making it a...".
  Keep any new, useful information in a direct sentence.
- Remove "that's the part", "that's the half" and "pointed the X at Y".
  Name the subject and the actual action.
- Do not inflate importance, reputation or significance.
  Show the specific fact instead.
- Use ordinary transitions sparingly.
  Do not start paragraphs with "Additionally" or stack formal transitions.
- Keep a precise technical term when it is needed.
  A literal use of a word is different from decorative jargon: "rich results" and "robust standard errors" are terms, not decoration.

## Keep the author's voice

- Preserve an actual opinion and the speaker's level of confidence.
  Do not invent personal experience or a stronger claim.
- Keep natural contractions, occasional sentences beginning with "And", "But" or "So", and rhetorical questions that help the reader.
- Keep ordinary qualifiers such as "roughly", "perhaps", "mostly" or "tends to" when they are accurate.
- Do not deliberately insert typos or grammar mistakes.
  Correct grammar is fine.
  Do not add fake folksiness or pet names.
- Keep one English spelling variety throughout the piece, the one the site already uses.
- Do not flatten a natural mix of conversational and technical language into a corporate voice.

## Format for reading

- Use sentence case for headings.
  Make headings short and name the action or subject.
- Prefer prose.
  Use lists when the items are genuinely parallel or sequential, or when the page outline calls for them.
- Do not add bold or italic emphasis, emoji-led headings, or bullets shaped as "Bold term: explanation".
  Keep formatting the user or the site's voice asks for.
- Do not use em dashes, en dashes or spaced hyphens as punctuation.
  Rewrite with a comma, colon, parentheses or a separate sentence.
  Normal compound-word hyphens and number ranges like 2024-2026 are fine.
- Use straight quotation marks and apostrophes.
- Keep paragraphs focused.
  For social posts, avoid blocks longer than three sentences and avoid turning every sentence into its own paragraph.
- Do not add stock headings such as "Final thoughts", "Looking ahead" or "Conclusion".
  Name the specific final action if a heading is useful.
- Remove repeated explanations, section summaries and conclusions that restate a point without adding anything, unless the user asks for a recap.

## Keep facts and sources honest

- Preserve names, numbers, dates, prices, URLs, conditions and meaningful qualifications.
  Do not invent evidence, testimonials, urgency, guarantees, quotes, credentials or results.
- Remove unsupported "I've seen this happen", "Every founder I talk to" and similar claims of experience.
  Keep a personal example only when the user or the memory supplies it.
- Replace "Experts say", "Studies show", "Many believe" and other vague attribution with a verified source or remove the claim.
  Do not turn one source's opinion into consensus.
- Check factual claims when research is available.
  If an essential fact is missing, write TODO where it goes and ask the user outside the page (save_article won't pass a TODO).
  Otherwise omit or rewrite the unsupported claim.
- Never invent a source, DOI, ISBN, quotation or link.
  Keep quotations accurate and short.
  Remove AI attribution tracking parameters, like utm_source=chatgpt.com, from URLs.
- Do not add warnings, disclaimers, policy reminders or cautionary sections unless the user asks.
  Keep the material terms and required qualifications the user supplies.
- Keep assistant chatter out of the page: no "Certainly", "As an AI", "I hope this helps", notes about your editing or offers to continue.
- Remove unresolved placeholders, broken Markdown, citation-tool residue and incomplete sentences.
  Deliver a complete piece.

## Final pass

Read for meaning first, sentence rhythm second and style third.
For each sentence, check what its words refer to and whether the reader already knows it.
Remove unsupported comparisons, repeated openings, dramatic fragments, vague claims and duplicate points.
Read the result aloud in your head and restore conjunctions wherever a continuous thought was chopped up.
Preserve specific facts and the intended meaning.
Put only the finished text in the page, and tell the user anything else in your reply.
