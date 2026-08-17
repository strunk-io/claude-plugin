# Strunk plugin

The install-once bundle of Strunk for Claude Code:

- **The Strunk remote MCP** (`.mcp.json`) — the atomic transport tools (`publish_doc`,
  `pick_google_doc`, `pull_comments`, `apply_revisions`, `reply_comment`, `resolve_comment`,
  `inspect_doc`, `get_preferences`, `set_preferences`, `get_version`, and `get_workflow`) served over
  Streamable HTTP at `https://strunk.io/mcp`, protected by WorkOS/AuthKit OAuth.
- **The review workflow** — the triage tiers, reply-before-revise ordering, and placeability policy —
  is *not* bundled as a skill. The MCP's `get_workflow` tool serves it live from the Strunk backend, so
  it stays current without a plugin update and works on any harness that connects to the MCP. The
  non-negotiable invariants also ride in the MCP `instructions` field, present the moment the server
  connects (before any tool call).

Install and usage: see the [repo README](../README.md).
