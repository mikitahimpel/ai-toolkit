---
name: comment-discipline
description: Rules for writing source-code comments that serve future readers, not the conversation that produced the code. Apply whenever about to write or edit any source-code comment, docstring, or inline note — verify each proposed comment against these rules before producing the diff. Prevents the very common AI-coding failure mode where an agent adds comments documenting the chat (what the user asked, what alternative was rejected, what the agent changed) instead of comments that document the code.
---

A comment belongs in source code only if a future reader who has **only this file** — no chat transcript, no PR description, no linked issue — finds it useful. Comments that document the path the author took through a conversation are noise to that reader; they age into nonsense within weeks.

The audience for a comment is not the person who wrote the code and not the agent that helped write it. The audience is someone six months later, possibly someone else, opening this file cold.

## Decision rule — run before writing any comment

1. **Can the code itself be made clearer** (rename a variable, extract a helper with an intention-revealing name, use the type system, split a long function) so the comment becomes unnecessary? **Refactor first.**
2. Would the comment explain a **non-obvious WHY**, a **hidden invariant**, an **external-bug workaround**, or **genuinely surprising behavior**? If none of these — **delete**.
3. Would the comment still make sense to a reader who has only this file — no chat, no PR, no issue? If no — **delete or rewrite**.

If a proposed comment fails any of these, do not produce it.

## Refactor before commenting

The cleanest fix for most comment-worthy code isn't a better comment — it's better code. Before writing a comment, ask whether one of these would remove the need:

- **Rename.** `const d = u.filter(x => x.a > 0)` becomes self-documenting as `const activeUsers = users.filter(u => u.accountAgeDays > 0)`. No comment needed.
- **Extract.** A 30-line block doing one thing becomes `applyDiscountToCart(cart, code)`. The function name is the documentation.
- **Use the type system.** `// must be positive` becomes a `PositiveInt` branded type or a runtime assertion that throws on bad input. The constraint is enforced, not described.
- **Split.** A function doing four things becomes four functions, each named for its job. The comments that wanted to label sections become function names.

Reach for the comment only after these moves don't fit.

## When a comment earns its place

Write a comment only if it explains one of:

- **A non-obvious WHY** — the reason a less-obvious approach was chosen over the obvious one. "Using a deque here because pop-from-front is O(1) and list.pop(0) is O(n)" is useful; "loop through items" above a loop is not.
- **A hidden invariant** — something the code depends on that the code itself can't express. "Caller must hold the lock before invoking this" or "input is guaranteed sorted ascending".
- **A workaround for an external constraint** — a bug in a dependency, a quirk of an API, a platform limitation — with concrete pointers (issue URL, version number) and **enough context that the reader doesn't strictly need the link**.
- **Genuinely surprising behavior** — code that looks wrong but is intentional, where a future reader's first instinct would be to "fix" it and break things.

If the comment doesn't fit one of those four, delete it.

## Self-check before keeping any comment

Mentally replace the comment with `// ???`. Does the file communicate as much to a cold reader? If yes, the comment was adding nothing — remove it. If no, the comment is doing real work and stays.

A second test: would the comment still make sense if read by someone who has never seen the conversation that produced this code? If "as requested", "per your suggestion", "switched to library X", or any reference to the conversation has to be removed for the comment to make sense, the whole comment is conversation residue and should be deleted, not edited.

## Anti-patterns — delete on sight, with examples

Every one of these is a deletion candidate, not an edit candidate.

### 1. Conversation residue

The comment narrates the chat instead of the code.

```js
// ❌ Switched to map() per your suggestion
const ids = users.map(u => u.id);

// ✓ delete — the map() call is right there
const ids = users.map(u => u.id);
```

### 2. What-not-why narration

The comment restates what the code obviously does.

```py
# ❌ Loop through users
for user in users:
    notify(user)

# ✓ delete — the loop is self-evident. If something non-obvious is true,
# state the constraint instead:
# users is guaranteed sorted by signup date — notification order matters
for user in users:
    notify(user)
```

### 3. Refactor archaeology

The comment describes the previous version that no longer exists.

```ts
// ❌ Refactored from earlier nested-map approach
const grouped = groupBy(items, 'category');

// ✓ delete — belongs in the commit message, not the file
const grouped = groupBy(items, 'category');
```

### 4. Self-congratulation

The comment editorializes about a library call that's already visible.

```ts
// ❌ Use lodash here to make this simpler
const grouped = _.groupBy(items, 'category');

// ✓ delete — the lodash call is the code
const grouped = _.groupBy(items, 'category');
```

### 5. Decision-justification without invariant

The comment states a preference, not a constraint.

```ts
// ❌ We chose async here because it's more modern
async function loadConfig() { ... }

// ✓ either delete, or convert the preference into the real constraint:
// Must be async — caller holds the UI thread and disk I/O can block for tens of ms
async function loadConfig() { ... }
```

### 6. External pointer without context

The comment links to a moving target without encoding why.

```ts
// ❌ See issue #42
element.addEventListener('touchstart', handler, { once: true });

// ✓ encode the constraint so the link isn't load-bearing:
// Workaround: Safari < 17 fires touchstart twice on this element (issue #42).
// Remove when Safari 17 is the minimum supported version.
element.addEventListener('touchstart', handler, { once: true });
```

### 7. TODO without actor, date, or follow-up

A bare TODO ages into noise.

```py
# ❌ TODO: fix this
return data[0] if data else None

# ✓ either fix it now, OR encode the deferred work concretely:
# TODO(mh, 2026-06): replace with native AbortSignal once Node 22 LTS is min target — issue #142
return data[0] if data else None
```

### Other deletion-on-sight cases (same logic, no example needed)

- **Section banners / decorative headers** — `// ========= USERS =========`. Well-organized files don't need them; poorly-organized files need to be reorganized, not banner-decorated.
- **Restating the type signature** — `// returns a string` above a function whose return type is already `string`. The type system is the documentation.
- **Author/timestamp comments** — `// Added by Mikita 2026-04-12`. Git blame already knows.

## What this skill is not

This is not a ban on documentation. Function-level documentation that explains intent, parameters, return semantics, and side effects in a docstring/JSDoc/Rust-doc-comment is valuable when the function is part of a public API or a non-trivial internal contract. The rules above are primarily for **inline comments inside function bodies** — the place where conversation residue accumulates most aggressively. Doc-comments follow the same "audience = future reader with only this file" model, but the bar for inclusion is different: a public API function should have a docstring even when an inline comment in the same spot would be redundant.

When in doubt, write fewer comments. The bar should be high. A file with three sharp comments that each capture a real WHY is better than a file with thirty comments that narrate the implementation.
