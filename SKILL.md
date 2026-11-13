---
name: do-it
description: Work through a project's to-do list one queued batch at a time. Use when the user opens a fresh session with just "do it" (do the next queued item), when they ask to queue an item as the next "do it", or when they ask what the next "do it" should be.
---

# do it

A small routine for steady, reviewable progress on a project. The to-do list holds the work. The user marks the next piece with `(do it)`. Saying "do it" builds that piece, tests it, commits it, marks it done and reports what comes next. Nothing else gets touched.

## Project settings

Before anything else, read the project's agent instruction file (`AGENTS.md`, `CLAUDE.md` or similar) for a `do it` settings section (see `templates/AGENTS-snippet.md`). Settings there override the defaults below:

| Setting | Default |
|---|---|
| To-do file | `TODO.md` at the repository root |
| Queue marker | `(do it)` right after the checkbox: `- [ ] (do it) …` |
| Tests | Whatever the project's instructions and test setup name; run only the ones that cover the change |
| Versioning | If the project has a `VERSION` file or version field and a `CHANGELOG.md`: semantic versioning, one changelog line per version, a `vX.Y.Z` tag. Otherwise no versioning |
| Low-priority work | None. A project can name a kind of item (for example `Translate …` items) that ranks below everything else |
| Finished items | Whatever the to-do file already does: tick them (`- [x]`) or move them to a "Done" section. If it shows no habit yet, remove them; git history keeps the record |
| Pushing | Never. Commits stay local until the user pushes |

The project's other rules (testing, safety, style, private files) still apply in full while doing a batch.

## "do it": the whole message

When the user's whole message is "do it", do the **first unchecked item marked `(do it)`** in the to-do file, and nothing else.

1. **Commit stray to-do edits first.** If the to-do file has changes that are not committed (items the user added by hand or with a tool), commit them on their own, separate from your work, before starting. Leave any other uncommitted changes as they are and keep them out of your commits; if they touch files the item needs, ask the user before going on.
2. **Build it.** If the item is split into batches, build only the first unchecked batch. Otherwise build every sub-item it lists. Follow the item's notes either way.
   - For a fix to an existing feature, start at the reported defect and its code references. The feature exists: do not spend effort checking or explaining whether it does. Inspect only what you need to find the defect, fix it and verify the fix.
   - Add focused tests for the change in the project's existing test setup. Do not add tests for unrelated layers (no browser tests for a change confined to a backend script).
   - Follow the project's rules for anything the change creates on the side (for example new page text that the project translates later: record it where the project says, do not translate it unasked).
3. **Test it.** Run only the tests that cover the changed code, and they must pass. A failure that was already there before your change is not yours to fix: report it and add it to the to-do file if it is not already there. To check, run the failing test on the last commit in a temporary worktree (`git worktree add`, removed afterwards); do not stash the user's uncommitted work.
4. **Commit it.** Commit at logical points, one commit per coherent piece. If the project uses versioning:
   - a new feature raises the minor number (1.38.0 → 1.39.0);
   - a fix or adjustment (a small change to an existing control, script or behaviour) raises the patch number (1.38.0 → 1.38.1);
   - commits that change only documentation, the to-do list or tests keep the version;
   - add the changelog line at the top and tag the commit (`git tag v1.39.0`).

   Leave unrelated untracked files out of commits.
5. **Mark it done** in its own commit, the way the project handles finished items (see the settings). Mark the finished batch or sub-item, and note in the parent item what is done (and in which version, if the project is versioned). When the last batch of an item is done, mark the whole item done.
6. **Report.** Say which batch is done (and in which version, if the project is versioned) and what the next "do it" will do: the next `(do it)` item, or, if none is left, your suggestion (below).

### When you get stuck

If the item is unclear, the tests cannot be made to pass, or the work turns out much larger than the item suggests: stop, commit nothing half-done, report what blocked you, and suggest how to reword or split the item.

## When nothing is queued

When no unchecked `(do it)` item remains, do not start anything on your own. Review the **whole** to-do file, including notes at the bottom, and suggest the best candidate for the next `(do it)`:

- It fits the usual size: one quick item, or one manageable chunk of a larger one.
- Concrete defects the user reported, and notes the user left on specific elements or lines, come first. Do not just suggest whatever sits at the top of the backlog.
- Items under "Decisions needed" or similar wait for the user's choice. Do not suggest building them; you may remind the user that they are waiting.
- Low-priority work (if the project names any) ranks below every suitable other task.

The user decides whether to queue it. Wait for that.

If the user says "do it" with nothing queued:
- If a suitable non-low-priority task exists, suggest it and wait. Do not mark or start it.
- If only low-priority work is left, do the first unchecked low-priority item, following the project's own steps for that kind of work.
- If nothing is left, say so and stop.

The user can always choose any item explicitly, including a low-priority one ahead of the rest.

## Queueing

When the user asks to queue something ("queue X as the next do it", "make that the next do it"):

1. Write it as one unchecked item at the top of the to-do file's queue section (create a `## Do it` section at the top of the file if there is none), marked `(do it)`, with the date it was queued. The new item comes first: anything already queued moves down one place and is done after it.
2. Make the item self-contained: what to build, the relevant files, functions and endpoints you already know, constraints the user stated, and how to tell it is done. A later session, or another agent, must be able to build it from the item alone.
3. If the item is too large for one "do it", split it into numbered batches inside the item, each one a sensible commit.
4. Remove the item from wherever it sat before (backlog, notes) so it is not listed twice.
5. Commit the to-do change on its own ("Queue … as the next do it").

## To-do file shape

A suggested layout is in `templates/TODO.md`. The routine relies only on:

- Markdown checkboxes (`- [ ]`) for open items;
- the `(do it)` marker on queued items;
- the order of items, which is the reading order.

Everything else (sections, headings, notes) is the project's own choice.
