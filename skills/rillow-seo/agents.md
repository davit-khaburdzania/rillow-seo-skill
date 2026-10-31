# Make a site agent ready

Read this when the user wants AI agents to find and use their site, or when an audit reaches agent readiness.

Agent readiness helps AI agents use a site: read its pages as Markdown, find its API or MCP server, and sign in to it.
It does not help rankings or citations: Google says it ignores Markdown copies and AI text files, and no study links any of these files to more citations.
Suggest it after the ranking work, as a small separate job, and say what it does and what it doesn't.

Cloudflare's free scanner checks a live site and says how to fix each item.
Run it with `curl -X POST https://isitagentready.com/api/scan -H 'Content-Type: application/json' -d '{"url": "https://example.com"}'`, or send the user to https://isitagentready.com, before and after the changes.

## What to add

| Item | Which sites need it | The change |
|---|---|---|
| Content-Signal in robots.txt | Every site | One line in the `*` group, such as `Content-Signal: search=yes, ai-input=yes, ai-train=no`. Ask the user which of the three uses to allow |
| Markdown for agents | Every site with pages worth reading | When a request's Accept header asks for `text/markdown`, return the page's main content as Markdown with `Content-Type: text/markdown`, and HTML otherwise. On Cloudflare it is the Markdown for Agents setting |
| A `Link` header on the home page | Sites with an API, an MCP server or developer docs | Point to the files below with `rel="api-catalog"`, `rel="service-desc"` and `rel="service-doc"` (RFC 8288) |
| An API catalog at `/.well-known/api-catalog` | Sites with a public API | An `application/linkset+json` list with one entry per API, its spec and its docs (RFC 9727) |
| OAuth metadata at `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server` | Sites whose API or MCP server signs users in | The resource names its authorization server, and the auth provider usually publishes the server's metadata already |
| An MCP server card at `/.well-known/mcp/server-card.json` | Sites with an MCP server | The server's name, version, address and transport |
| An Agent Skills index at `/.well-known/agent-skills/index.json` | Sites that publish Agent Skills | Each skill's name, type, description, URL and SHA-256 digest |
| WebMCP tools | Sites with actions a browser agent could take, such as search or booking | Register each action with `document.modelContext.registerTool` (`navigator.modelContext` in older Chrome builds). Chrome is still trying the API |

- List only APIs, tools and sign-in that work, because agents act on what these files say.
- Do not publish Markdown copies or llms.txt as SEO work.
- Ask before changing robots.txt, as for any robots.txt change.
- The scanner also checks DNS records and payment protocols. Skip them unless the user asks.

## Make the changes

1. Scan the live site and list each failing item with the scanner's reason.
2. Drop the items that don't fit the site, using the table, and say why in one line each.
3. Ask the user which to make, and which uses to allow in Content-Signal.
4. Make them in the repository, the way the site already serves files and headers.
   Serve each JSON file with a JSON content type, not the site's HTML fallback page.
5. Deploy, scan again, and fetch each new address yourself to check it.
6. Record what shipped with remember as one dated note.
