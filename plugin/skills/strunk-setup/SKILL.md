---
name: strunk-setup
description: >-
  Use when the author has just installed or enabled Strunk, asks how to get started with it, asks
  whether Strunk is connected or which account it is using, or hits a Strunk tool that reports no
  Google connection. Walks the two connections Strunk needs and ends at their first review.
---

# Setting up Strunk

Strunk needs two connections. This plugin brings the first: the remote MCP server, which authorizes the
author's Strunk account over OAuth. The second is Strunk's own connection to Google Docs, made once, in
the browser.

1. **Authorize the MCP server.** The first Strunk tool call prompts the author to sign in. Call
   `get_account` to trigger that and to report which account answered.
2. **Check Google Docs.** `get_account` reports whether Google is connected, and returns the account URL
   when it is not. Give the author that link as plain text on its own line, do not hide it in a markdown
   link, and ask them to connect Google there. Call `get_account` again to confirm.
3. **Reach the first review.** Ask whether they already have a Google Doc with reviewer comments on it.
   If they do, follow `/strunk:pull-comments`. If they do not, `/strunk:publish` puts a draft out so
   reviewers can start commenting.

Setup is not finished when the connection succeeds. It is finished when the author has seen one reviewer
comment come back into this session.

## If the author already had Strunk as a connector

In Claude Code, installing this plugin **takes over** the entry for `https://strunk.io/mcp`. The
author's existing connector stops appearing in `claude mcp list`, and `plugin:strunk:strunk` appears in
its place reporting "Needs authentication". Their previous authorization is not reused, so Strunk looks
signed out until they authorize the plugin's entry.

Nothing is lost. Authorizing once restores it, and uninstalling the plugin brings the original
connector back exactly as it was, still connected.

So do not tell them the installation is broken, and do not tell them to keep the connector and remove
the plugin's entry: while the plugin is installed, the plugin's entry is the one in use. Tell them to
authorize it, which the first Strunk tool call prompts for.
