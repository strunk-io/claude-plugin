---
description: Pull reviewer comments on a Google Doc and work through them
argument-hint: [doc URL or localId, or nothing to choose one]
---

Work through reviewer comments on a Google Doc with Strunk.

1. If `$ARGUMENTS` names a doc, use it. Otherwise call `pick_google_doc` with no URL. It returns a
   Google Picker link the author opens to choose and approve the doc.
2. Call `get_review_workflow` before triaging anything. Do not restate the triage tiers from memory:
   the server serves the current version, and it is the only source of it.
3. Call `pull_comments`, then follow that workflow, including its ordering of reply, then revise, then
   resolve.

A pulled comment names the tab its span lives in. Pass that tab id back on `apply_revisions` and
`inspect_doc`, because indices are per-tab and an untagged edit lands in the first one.
