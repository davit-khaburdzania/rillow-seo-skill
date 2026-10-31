# Rillow SEO skill

Free SEO guides for your AI agent, packaged as an Agent Skill and a Claude Code plugin.
Your agent reads the guide for the job (an audit, keyword research, a new post, AI search) and gets the numbers from [Rillow](https://rillow.ai/?ref=rillow-seo-skill)'s MCP server: keyword volumes and difficulty, Google's live results and AI Overview, competitors, your Search Console data, a content plan and daily rank tracking.

It works in Claude Code, Codex, Cursor, Gemini CLI and any agent that reads skills and connects to MCP servers.
The guides are free and open source under Apache 2.0.
Rillow starts free with 30 credits, no credit card.

## What's inside

Ten guides in [`skills/rillow-seo`](skills/rillow-seo), and the rules that apply to all of them in [`SKILL.md`](skills/rillow-seo/SKILL.md).

| Guide | Your agent reads it when |
|---|---|
| [new-site](skills/rillow-seo/new-site.md) | The site is new or gets almost no search traffic |
| [audit](skills/rillow-seo/audit.md) | You want a site reviewed or fixed, or traffic or rankings dropped |
| [technical](skills/rillow-seo/technical.md) | A change touches indexing, canonicals, redirects, sitemaps, robots, rendering or page speed |
| [keywords](skills/rillow-seo/keywords.md) | Before it proposes topics or decides which page targets which search |
| [content](skills/rillow-seo/content.md) | Writing or refreshing a post, or building a comparison, best list, alternatives or pricing page |
| [links](skills/rillow-seo/links.md) | Internal linking, or you ask how to get backlinks |
| [ai-search](skills/rillow-seo/ai-search.md) | You ask about ChatGPT, Perplexity, Claude, Copilot, Google AI Overviews or AI Mode |
| [agents](skills/rillow-seo/agents.md) | You want AI agents to find and use your site, or an audit reaches agent readiness |
| [measure](skills/rillow-seo/measure.md) | After a change ships, or when you ask whether something worked |
| [writing](skills/rillow-seo/writing.md) | Before it writes any text for your site: posts, pages, titles, descriptions or emails |

A few of the rules: every number comes from a tool or your own data, never a guess.
Every recommendation names the exact URL, the exact change and how you will know it worked.
The agent asks before changes that are hard to undo, such as redirects, noindex or edits to many pages.

## Install

### Claude Code

```
/plugin marketplace add davit-khaburdzania/rillow-seo-skill
/plugin install rillow-seo@rillow
```

The plugin adds the skill and Rillow's MCP server together.
Run `/reload-plugins`, then `/mcp`, choose rillow and sign in to Rillow.

### Codex, Cursor, Gemini CLI and other agents

Add the skill with the [skills CLI](https://github.com/vercel-labs/skills):

```
npx skills add davit-khaburdzania/rillow-seo-skill
```

Or copy the `skills/rillow-seo` folder into your agent's skills folder.

Then connect Rillow's MCP server at `https://rillow.ai/mcp`:

- **Codex:** run `codex mcp add rillow --url https://rillow.ai/mcp`. Codex opens your browser to sign in. If it does not, run `codex mcp login rillow`.
- **Cursor:** add `{ "mcpServers": { "rillow": { "url": "https://rillow.ai/mcp" } } }` to `~/.cursor/mcp.json`, then sign in to rillow under MCP in Cursor's settings.
- **Gemini CLI:** run `gemini mcp add --scope user --transport http rillow https://rillow.ai/mcp`, then `/mcp auth rillow` in Gemini CLI.
- **claude.ai:** open Settings, then Connectors, choose Add custom connector and paste `https://rillow.ai/mcp`.

Sign-in is OAuth, so there is no API key to copy.
The [setup page](https://rillow.ai/seo-mcp-server?ref=rillow-seo-skill) has the steps for each agent.

## Try it

Ask your agent in plain words:

- "Audit my site's SEO and fix the five biggest problems."
- "Which keywords could my site rank for? Add the best three to my plan."
- "Write the next post in my plan."
- "Do Google's AI answers cite my site for my main topics?"
- "I changed my titles two weeks ago. Did it work?"

## Without Rillow

The guides are plain Markdown, and much of their advice stands on its own.
They never let the agent guess a number, so without Rillow connected the agent works from data you give it, such as a Search Console export, and tells you which steps it skipped.

## Sources

The guides follow Google Search Central's documentation and build on several open-source SEO skills.
[NOTICE.md](skills/rillow-seo/NOTICE.md) lists them.
Rillow's `get_guide` tool returns the same guides, so the skill and the server stay in step.

## Privacy

The skill itself sends nothing anywhere.
Rillow's MCP server handles your data as its [privacy policy](https://rillow.ai/privacy) says.

## License

Apache 2.0. Made by the team behind [Rillow](https://rillow.ai/?ref=rillow-seo-skill).
