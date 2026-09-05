# Strunk for Claude Code

*The bridge between your agentic harness and the docs your reviewers already use.* · [strunk.io](https://strunk.io)

Author and revise inside your harness, where the repo, tools, and data context live, and get
feedback in **Google Docs**, where your reviewers already are. Strunk publishes your doc out for
review, pulls anchored reviewer comments back, lets you triage and draft revisions with full
context, then replies and resolves threads in place. No copy/paste, no PR, no one switching tools.

This repo is the public Claude Code plugin: a small install-once bundle for the **Strunk remote MCP**
(the atomic tools, served over Streamable HTTP at `https://strunk.io/mcp` and protected by
WorkOS/AuthKit OAuth). The review-round-trip workflow and judgment ships *with* the connector: the
MCP's `get_review_workflow` tool serves it live and always current, so there's no separate skill to
install or keep in sync, and it works the same on any harness that connects to the MCP.

## Install

From an interactive Claude Code session:

```
/plugin marketplace add strunk-io/claude-plugin
```

```
/plugin install strunk@strunk
```

On first use of a Strunk tool, Claude Code opens the WorkOS OAuth flow in your browser (one-time
consent). Manage or reconnect the connection any time with `/mcp`.

## Use it

Say what you want in plain language and Claude drives the loop over the MCP tools, calling
`get_review_workflow` for the review workflow and `get_writing_rules` before it drafts. Three slash
commands start the common paths directly:

- `/strunk:pull-comments` → work through the comments on a doc
- `/strunk:publish` → put a draft out for review
- `/strunk:link-doc` → connect a Google Doc you already have

Or in plain language:

- *"Publish this for review"* → `publish_doc` returns a Google Doc URL to share.
- *"Load Google Doc X"* → `pick_google_doc` connects an existing doc.
- *"What came back on the doc?"* → `pull_comments` brings anchored comment threads back.
- *"Address the comments"* / *"push my revisions"* → triage, draft, `apply_revisions`, `reply_comment`, `resolve_comment`.

You approve every substantive revision in the harness before it's pushed, and that approval is the
gate.
Google Docs version history is the undo.

## What's in here

```
.claude-plugin/marketplace.json   # marketplace listing (this repo)
plugin/
  .claude-plugin/plugin.json      # plugin manifest
  .mcp.json                       # remote MCP: https://strunk.io/mcp
  SETUP.md                        # followed on install, ends at your first review
  commands/                       # /strunk:pull-comments, /strunk:publish, /strunk:link-doc
  skills/strunk-review/           # so a session reaches for Strunk unprompted
  README.md
```

The review workflow is not bundled here. The MCP's `get_review_workflow` tool serves it live from the
Strunk backend, so it stays current without a plugin update, and `get_writing_rules` does the same for
your own writing rules. Everything in `commands/` and `skills/` points at those tools rather than
repeating what they say.

## Links

- Product: [strunk.io](https://strunk.io)
