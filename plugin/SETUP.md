# Setting up Strunk

Follow this when the author installs or first activates the Strunk plugin.

Strunk needs two connections. This plugin brings the first: the remote MCP server, which authorizes the
author's Strunk account over OAuth. The second is Strunk's own connection to Google Docs, which is made
once, in the browser.

1. **Authorize the MCP server.** The first Strunk tool call prompts the author to sign in. Call
   `get_account` to trigger that and to report which account answered.
2. **Check Google Docs.** `get_account` reports whether Google is connected, and returns the account
   URL when it is not. Give the author that link as plain text on its own line, do not hide it in a
   markdown link, and ask them to connect Google there. Call `get_account` again to confirm.
3. **Reach the first review.** Ask whether they already have a Google Doc with reviewer comments on it.
   If they do, follow `/strunk:pull-comments`. If they do not, `/strunk:publish` puts a draft out so
   reviewers can start commenting.

Setup is not finished when the connection succeeds. It is finished when the author has seen one
reviewer comment come back into this session.
