# Glen MCP server

Your team's coding agents figure things out all day: why a bug happened, which approach got thrown out, who decided what. Glen keeps all of that in one shared memory for your whole team. This MCP server lets any MCP client tap into it. You can ask it questions, use your team's saved skills and documents, and save new ones.

There's nothing to install or run. Glen hosts the server, so you just point your client at this URL and sign in:

```
https://mcp.tryglen.com/mcp
```

The first time you connect, your client opens Glen's sign-in page. You log in, pick which team's memory to use, and you're done. It works on the free plan.

If you'd rather use an API key (for scripts, say), make one under Settings → API keys in the [Glen dashboard](https://app.tryglen.com/settings/api-keys) and send it in the `x-api-key` header, along with your organization's id in `x-glen-org-id`.

## Connect your client

**Claude** (web, desktop or mobile): go to Customize → Connectors, add a custom connector, and paste the URL above. [Step-by-step guide](https://tryglen.com/docs/claude)

**ChatGPT:** go to [chatgpt.com/plugins](https://chatgpt.com/plugins), hit +, choose Add custom MCP server, paste the URL and pick OAuth. [Step-by-step guide](https://tryglen.com/docs/chatgpt)

**Claude Code:**

```bash
claude mcp add --transport http glen https://mcp.tryglen.com/mcp
```

Add `--scope project` to share it with your team through the repo's `.mcp.json`. If you write that file by hand, include `"type": "http"`, because Claude Code ignores an entry that only has a `url`:

```json
{
  "mcpServers": {
    "glen": { "type": "http", "url": "https://mcp.tryglen.com/mcp" }
  }
}
```

**Cursor**, or another client that accepts a plain `url` in its `mcp.json`:

```json
{
  "mcpServers": {
    "glen": { "url": "https://mcp.tryglen.com/mcp" }
  }
}
```

Using a coding agent day to day? The glen plugin is probably a better fit than this server on its own. It also records your sessions into team memory, so your work helps the next person. See the guides for [Claude Code](https://tryglen.com/docs/claude-code), [Codex](https://tryglen.com/docs/codex), [Cursor](https://tryglen.com/docs/cursor), [pi](https://tryglen.com/docs/pi) and [Grok Build](https://tryglen.com/docs/grok).

## What you can do

Ask things like "why did we move search off Elasticsearch?" or "what broke last time someone touched the payments webhook?" and Glen answers from your team's history, with sources you can click through to.

You can also find and run your team's skills (reusable instructions an agent can follow), save new ones, and keep documents like runbooks and postmortems in a shared library.

Here's every tool. Each one tells your client whether it only reads or also changes something, so your client can ask you before anything gets written.

| Tool | What it's called | Reads or writes | What it does |
|---|---|---|---|
| `glen_search` | Search team memory | read-only | Answers a question from your team's memory, with a source for every claim. |
| `glen_code_search` | Trace code to conversations | read-only | Follows lines of code back through git blame to the agent conversations that wrote them. |
| `glen_skill_search` | Search skills | read-only | Finds a skill by what it does or who wrote it. |
| `glen_skill_use` | Use skill | read-only | Gets a skill's instructions so your agent can follow them. |
| `glen_artifact_search` | Search artifacts | read-only | Finds a saved document you wrote or that someone shared with you. |
| `glen_artifact_get` | Get artifact | read-only | Opens a saved document. |
| `glen_dependency_search` | Search dependencies | read-only | Finds tools your team's skills already rely on, so a new skill can reuse them. |
| `glen_list_orgs` | List organizations | read-only | Shows the teams you belong to and which one you're connected to. |
| `glen_status` | Show glen settings | read-only | Shows whether Glen is on, off, incognito or silent for you. |
| `glen_skill_create` | Save skill | writes | Saves a skill. It's private to you until you share it. |
| `glen_skill_share` | Share skill | writes | Shares one of your skills with teammates or the whole team. |
| `glen_artifact_create` | Save artifact | writes | Saves a Markdown or HTML document. Private until you share it. |
| `glen_artifact_share` | Share artifact | writes | Shares one of your documents with teammates or the whole team. |
| `glen_import` | Import past conversation | writes | Adds an old agent conversation to team memory, when you ask it to. |
| `glen_forget` | Forget evidence | writes | Stops a wrong or outdated memory from coming up again. You can undo it. |
| `glen_restore` | Restore forgotten evidence | writes | Brings back a memory that was forgotten. |
| `glen_invite` | Invite teammates | writes | Invites people to your team by email. Owners and admins only. |
| `glen_set_recording` | Set glen mode | writes | Turns Glen on, off, incognito or silent for your account. |
| `glen_set_suggestions` | Set skill suggestions | writes | Turns Glen's skill suggestions on or off. |
| `glen_feedback` | Send feedback to Glen | writes | Sends a bug report or feedback to the Glen team. |
| `glen_skill_delete` | Delete skill | deletes | Deletes one of your skills for good. Only when you ask. |
| `glen_artifact_delete` | Delete artifact | deletes | Deletes one of your documents for good. Only when you ask. |

Want the details, like inputs, outputs, rate limits and error messages? It's all in the [MCP server reference](https://tryglen.com/docs/mcp).

## A couple of things to know

Glen only ever shows you things you could already see in the Glen dashboard, and only from the team you connected to. Tools that write act as you, on your own stuff.

Glen's answers are written by an AI model, so they can be wrong or out of date. Each one links to its sources, so check anything important before you act on it.

## Privacy and help

- [Privacy policy](https://tryglen.com/privacy) and [sub-processors](https://tryglen.com/sub-processors)
- Questions or problems: support@tryglen.com
- Security issues: security@tryglen.com

This repo only describes the hosted server. Glen's code isn't open source, so there's no code here to run.
