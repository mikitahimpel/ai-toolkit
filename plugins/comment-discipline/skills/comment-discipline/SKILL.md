---
name: comment-discipline
description: Rules for writing source-code comments that serve future readers, not the conversation that produced the code. Apply whenever writing or editing code. Prevents the very common failure mode where an agent adds comments documenting the chat history (what the user asked, what the agent changed, why an alternative was rejected) instead of documenting the code itself.
---

A comment belongs in source code only if a future reader who has **only this file** — no chat transcript, no PR description, no linked issue — finds it useful. Comments that document the path the author took through a conversation are noise to that reader; they age into nonsense within weeks.

The audience for a comment is not the person who wrote the code and not the agent that helped write it. The audience is someone six months later, possibly someone else, opening this file cold.

## When a comment earns its place

Write a comment only if it explains one of:

- **A non-obvious WHY** — the reason a less-obvious approach was chosen over the obvious one. "Using a deque here because pop-from-front is O(1) and list.pop(0) is O(n)" is useful; "loop through items" above a loop is not.
- **A hidden invariant** — something the code depends on that the code itself can't express. "Caller must hold the lock before invoking this" or "input is guaranteed sorted ascending".
- **A workaround for an external constraint** — a bug in a dependency, a quirk of an API, a platform limitation — with a concrete pointer to that constraint (issue URL, version number, ticket).
- **Genuinely surprising behavior** — code that looks wrong but is intentional, where a future reader's first instinct would be to "fix" it and break things.

If the comment doesn't fit one of those four, delete it.

## Self-check before keeping any comment

Mentally replace the comment with `// ???`. Does the file communicate as much to a cold reader? If yes, the comment was adding nothing — remove it. If no, the comment is doing real work and stays.

A second test: would the comment still make sense if read by someone who has never seen the conversation that produced this code? If "as requested", "per your suggestion", "switched to library X", or any reference to the conversation has to be removed for the comment to make sense, the whole comment is conversation residue and should be deleted, not edited.

## Anti-patterns — auto-fail, delete on sight

These show up constantly in AI-written code. Every one is a deletion candidate, not an edit candidate.

- **Conversation residue.** "As you suggested", "per the user's request", "switched to library X" — these document the chat, not the code. The chat is not the future reader's context.
- **What-not-why narration.** "Loop through items", "increment counter", "return the result". Well-named identifiers already say this. Restating the code in prose is dead weight.
- **Refactor archaeology.** "Previously did Y", "refactored from old approach", "removed unused param", "simplified from earlier version". The previous version is gone; the comment is a ghost. This belongs in the commit message or PR description.
- **Self-congratulation.** "Using library X here to make this easier" right above `import x from 'x'`. The call is visible; the comment is restating it with an editorial.
- **Decision-justification without invariant.** "We chose async here because it's more modern" — a value judgement with no constraint behind it. If there's a real constraint (a blocking call would freeze the UI), state the constraint, not the preference.
- **External pointers without context.** "See issue #42" with no explanation of what #42 says. Issues get closed, renumbered, migrated, deleted. The comment should encode enough of the context that the reader doesn't strictly need the link.
- **TODOs with no actor, date, or follow-up.** `// TODO: fix this` aged 4 years is not a TODO, it's clutter. Either fix it now, open a tracked ticket and link it with a one-sentence summary of what needs to happen, or delete the comment.
- **Section banners and decorative headers.** `// ==================== USERS ====================` — well-organized files don't need them; poorly-organized files need to be reorganized.
- **Restating the type signature.** `// returns a string` above a function whose return type is already `string`. The type system is the documentation here.
- **Author/timestamp comments.** `// Added by Mikita 2026-04-12`. Git blame already knows.

## What this skill is not

This is not a ban on documentation. Function-level documentation that explains intent, parameters, return semantics, and side effects in a docstring/JSDoc/etc. is valuable when the function is part of a public API or non-trivial internal contract. The rules above are for **inline comments inside function bodies** — the place where conversation residue accumulates most aggressively.

When in doubt, write fewer comments. The bar should be high. A file with three sharp comments that each capture a real WHY is better than a file with thirty comments that narrate the implementation.
