---
name: strunk-review
description: >-
  Use when the author wants to work through reviewer comments on a Google Doc, publish a draft to
  Google Docs for human review, or reply to and resolve comment threads a reviewer left. Triggers
  include a doc that has come back with comments, feedback that needs addressing, sharing a draft for
  review, asking what reviewers said, or any mention of Strunk.
---

# Reviewing with Strunk

Strunk carries a draft out to Google Docs for human review and brings anchored reviewer comments back,
so the author stays in this session and the reviewer stays in the doc they already use.

## Fetch the judgment rather than reproducing it

This skill holds no triage rules, no writing rules and no workflow, deliberately. They live on the
server, they are per-author, and they change without this file changing.

- Before triaging comments, call `get_review_workflow`.
- Before authoring or revising any text, call `get_writing_rules`.
- When it is unclear which account is connected, call `get_account`.

## The shape of a review

1. `pick_google_doc` connects a doc the author already has. `publish_doc` creates a new one.
2. `pull_comments` returns each comment with the span the reviewer highlighted and the tab it lives in.
3. Triage as `get_review_workflow` describes, then reply, then revise, then resolve, in that order.
4. `apply_revisions` writes approved changes. Pass back the tab id the comment named.

The author approves every substantive change before it lands, and every reply goes out under their
name. The server's `instructions` carry the rest of the invariants and arrive before any tool call.
