# Strunk for Claude Code

*The bridge between your agentic harness and the docs your reviewers already use.* · [strunk.io](https://strunk.io)

Author and revise inside your harness — where the repo, tools, and data context live — and get
feedback in **Google Docs**, where your reviewers already are. Strunk publishes your doc out for
review, pulls anchored reviewer comments back, lets you triage and draft revisions with full
context, then replies and resolves threads in place. No copy/paste, no PR, no one switching tools.

This repo is the public Claude Code plugin: a small install-once bundle of the **Strunk skill**
(the review-round-trip workflow and judgment) and the **Strunk remote MCP** (the atomic tools,
served over Streamable HTTP at `https://strunk.io/mcp` and protected by WorkOS/AuthKit OAuth).

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

Just say what you want in plain language and the skill drives the loop over the MCP tools:

- *"Publish this for review"* → `publish_doc` returns a Google Doc URL to share.
- *"Load Google Doc X"* → `pick_google_doc` connects an existing doc.
- *"What came back on the doc?"* → `pull_comments` brings anchored comment threads back.
- *"Address the comments"* / *"push my revisions"* → triage, draft, `apply_revisions`, `reply_comment`, `resolve_comment`.

You approve every substantive revision in the harness before it's pushed — that approval is the gate.
Google Docs version history is the undo.

## What's in here

```
.claude-plugin/marketplace.json   # marketplace listing (this repo)
plugin/
  .claude-plugin/plugin.json      # plugin manifest
  .mcp.json                       # remote MCP: https://strunk.io/mcp
  skills/strunk/SKILL.md          # the Strunk skill
  README.md
```

## Links

- Product: [strunk.io](https://strunk.io)
