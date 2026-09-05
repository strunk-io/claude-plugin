---
description: Publish a draft to Google Docs for review
argument-hint: [path to the draft, or nothing to use what is in context]
---

Publish a draft to Google Docs so reviewers can read and comment on it.

1. Call `get_writing_rules` first. It carries this author's own rules, it reaches you nowhere else, and
   it applies to a doc drafted from scratch as much as to a revision.
2. Publish with `publish_doc`.
3. Read the `warnings` and `voiceRules` from the response back to the author and let them decide what
   to change. Strunk rewrote nothing and refused nothing.

If `$ARGUMENTS` names a file, publish that. Otherwise publish the draft already in this conversation,
and ask which draft is meant if there is more than one.
