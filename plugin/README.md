# Strunk plugin

The install-once bundle of Strunk for Claude Code:

- **The Strunk skill** (`skills/strunk/`) — the triage tiers, reply-before-revise ordering, and
  placeability policy. Where the review-round-trip judgment lives.
- **The Strunk remote MCP** (`.mcp.json`) — the atomic transport tools (`publish_doc`,
  `pick_google_doc`, `pull_comments`, `apply_revisions`, `reply_comment`, `resolve_comment`,
  `get_preferences`, `set_preferences`, `version`) served over Streamable HTTP at
  `https://strunk.io/mcp`, protected by WorkOS/AuthKit OAuth.

Install and usage: see the [repo README](../README.md).
