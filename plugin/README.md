# Strunk plugin

The install-once bundle of Strunk for Claude Code.

- **The Strunk remote MCP** (`.mcp.json`), served over Streamable HTTP at `https://strunk.io/mcp` and
  protected by WorkOS/AuthKit OAuth. It carries the atomic transport tools: `publish_doc`,
  `pick_google_doc`, `pull_comments`, `apply_revisions`, `reply_comment`, `resolve_comment`,
  `inspect_doc`, `get_preferences`, `set_preferences`, `get_account`, `get_version`,
  `get_review_workflow` and `get_writing_rules`.
- **Slash commands** (`commands/`): `/strunk:pull-comments`, `/strunk:publish`, `/strunk:link-doc`.
- **A skill** (`skills/strunk-review/`) so a session reaches for Strunk when the author mentions a doc
  that came back with comments.
- **`SETUP.md`**, followed on install, which ends at the author's first review rather than at
  "connected".

**Every one of these is a launcher.** The review workflow, the triage tiers and the author's writing
rules are not bundled here. `get_review_workflow` and `get_writing_rules` serve them live from the
Strunk backend, so they stay current without a plugin update and work on any harness that connects to
the MCP. The non-negotiable invariants ride in the MCP `instructions` field, present the moment the
server connects and before any tool call.

That split is deliberate: a bundled copy of the judgment drifts from the server that owns it, which is
why the original bundled skill was removed. Commands and skills here point at the server. They never
restate it.

Install and usage: see the [repo README](../README.md).
