---
description: Connect an existing Google Doc to Strunk
argument-hint: [doc URL, or nothing to browse]
---

Connect a Google Doc the author already has, so its comments can be pulled and revisions applied to it.

Call `pick_google_doc`. Pass the URL if `$ARGUMENTS` carries one, so the Picker opens on that exact doc
and the author approves it rather than searching for it again.

Once the doc is linked, offer to pull its comments.
