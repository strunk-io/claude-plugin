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

## If a second Strunk server appears

An author who already added Strunk as a connector, and then installs this plugin, ends up with two
entries pointing at the same URL: the connector, already authorized, and `plugin:strunk:strunk`, which
reports "Needs authentication". Both work once authorized, and having both is only confusing. Tell them
they can keep either one and remove the other, and that the connector is the one already signed in.
