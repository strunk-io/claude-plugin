---
name: strunk
description: >-
  Review and revise a doc through Strunk — the bridge between your harness and Google Docs. Use when
  publishing a harness-authored doc for human review, pulling reviewer comments back, triaging and
  drafting revisions with full repo/data context, and replying/resolving threads without leaving the
  harness. Triggers on "publish for review", "what came back on the doc", "address the comments",
  "push my revisions", "reply to / resolve the comments".
---

# Strunk — review round-trip

You are the author's harness. You hold the repo/data/decision context the reviewer's doc tool never
sees — so **triage and drafting happen here, in you**, using the Strunk MCP tools as the transport.
The tools are atomic; this skill is the workflow and the judgment.

## Preferences (check once per session)

Call `get_preferences` before triaging and honor it:
- **`postReplies: false`** → don't post reply acknowledgments (skip `reply_comment` for summaries; the
  server also won't post one). `true` (default) → reply in the author's voice.
- **`autoResolve: false`** → don't resolve threads yourself; leave them open for the reviewer to resolve.
  `true` (default) → resolve once the author-approved fix is applied.

The author changes these with `set_preferences` (e.g. "stop auto-resolving my comments").

## Starting point

When the author says they want to "work on a doc", "start a doc", or "set up a doc" but does not make
clear whether it is new or existing, ask one short question before calling Strunk tools:

> Do you want to draft a new doc in Claude, or pick an existing Google Doc as the starting point?

Then route accordingly: draft first and `publish_doc` for a new doc; `pick_google_doc` for an existing
Google Doc.

## The loop

1. **Publish** — `publish_doc { localId, title, body }`. `body` is markdown; it renders to real Docs
   formatting. Re-calling with the same `localId` re-publishes as a targeted diff that preserves
   reviewer comments. Share the returned URL.
   To work on an **existing** Google Doc ("load Google Doc X", "work on doc X", "pull comments from doc
   X"), first call `pick_google_doc { localId }`; ask the author to open the returned pick URL and choose
   the exact doc in Google Picker, then continue from step 2. Do not guess a Strunk `localId` from repo
   paths, filenames, or titles unless the author explicitly gives that localId or says to use the currently
   connected Strunk doc. For self-host/dev only, `pick_google_doc { localId, url }` can use a pasted URL
   when Strunk is connected with broad Drive access.
2. **Pull** — `pull_comments { localId }`. If the author says "pull comments from doc X", first run the
   existing-doc pick flow above, then call `pull_comments` with that same `localId`. Each comment carries
   `quotedText`, `resolved`, threaded
   `replies`, a **`placeability`** (`unique` | `ambiguous` | `not-found` | `unanchored`), and an
   **`occurrence`** (1-based). Non-unique quotes are auto-pinned to the exact occurrence via the doc's
   export anchor, so many once-`ambiguous` comments come back `unique` with the right `occurrence`.
3. **Triage** each open comment into a tier (below) using your context.
4. **Address** in the right order (below).
5. Repeat until the doc is approved.

## Triage tiers (autonomy is a function of stakes)

- **Mechanical** — typos, dead links, formatting, terminology. Draft the fix; on the author's ok (a
  blanket "handle the mechanical ones" is enough), apply it, reply, and resolve.
- **Judgment call** — a tradeoff (tone, scope, a claim's strength). **Never decide for them.** Surface
  the options and the tension; once the author chooses, write it in their voice, apply, reply, and
  **resolve** — their choice closes it. Leave open only while still waiting on their decision.
- **Needs context** — you usually *have* it (repo, tickets, data) — use it. When you genuinely don't,
  ask; never fabricate. Once addressed, reply and resolve; leave open only while waiting on the author.

The rule is **never close a comment the author hasn't dispositioned — but once they have, close it.**
Autonomy is a function of stakes: Mechanical resolves on a blanket ok; Judgment/Needs-context resolve
only after the author decides that specific comment. Optimize for decisions surfaced clearly, not for
threads left hanging after they're handled.

## Addressing a comment — the order matters

For each comment you're addressing:

1. **Reply first, while the anchor is intact** (if `postReplies`). `reply_comment { localId, commentId,
   content }` with a short note in the author's voice describing what you changed and why. Doing this *before* the edit
   means the acknowledgment stays tied to the reviewer's original span — if the edit later deletes that
   span, Google shows "Original content deleted", but the thread still carries the context.
2. **Apply the revision** — `apply_revisions { localId, revisions: [{ quotedText, newText, occurrence }] }`.
   - `quotedText` is the comment's exact `quotedText`; pass its **`occurrence`** so a non-unique quote
     lands on the right one. `newText` renders **inline markdown** — use `[label](url)` to add a link,
     `**bold**`, `*italic*`, `` `code` `` — it lands formatted, not as literal characters.
   - Still `ambiguous` (couldn't be pinned) or `not-found` (text changed since) come back un-applied —
     **surface them to the author** with the current text and let them restate. Never guess.
3. **Resolve once addressed** (if `autoResolve`) — `resolve_comment { localId, commentId }`. After the
   fix is applied and the author is satisfied (any tier), close the thread; the reply stays in history.
   Leave a thread open **only** while it's still awaiting the author's decision, or when the author
   wants the reviewer to weigh in further. (Use `reply_comment { resolve: true }` for the case where you
   answer a comment and close it *without* a text edit — it replies and resolves in one call.)

## Guardrails

- Nothing substantive is applied without the author's ok in the harness — that approval *is* the gate
  (the Docs API can't create suggestions). Google version history is the undo.
- Re-publish only ever targets the same doc via diff; it never clobbers reviewer comments.
- Reply in the author's voice, not a bot's.
