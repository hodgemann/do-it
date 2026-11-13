---
name: do-it
description: Work through a project's to-do list one queued batch at a time. Use when the user opens a fresh session with just "do it" (do the next queued item), when they ask to add an item to the "do it" queue (anywhere in it, or as the next "do it"), or when they ask what the next "do it" should be.
---

# do it

A small routine for steady, reviewable progress on a project. The to-do list holds the work. The user marks the next piece with `(do it)`. Opening a fresh session and saying "do it" builds that piece, tests it, commits it, marks it done and reports what comes next. Queueing and suggestion requests prepare work for a later session; they never start implementation.

Each "do it" is a quick thing: one small, independently verifiable result that leaves the project working. A complex item becomes a series of quick things once it is split into batches, one batch per fresh-session "do it".

## Project settings

Before anything else, read the project's agent instruction file (`AGENTS.md`, `CLAUDE.md` or similar) for a `do it` settings section (see [the settings template](templates/AGENTS-snippet.md), relative to this skill's directory). Settings there override the defaults below:

| Setting | Default |
|---|---|
| To-do file | `TODO.md` at the repository root |
| Queue marker | `(do it)` right after the checkbox: `- [ ] (do it) …` |
| Tests | Whatever the project's instructions and test setup name, limited to the ones that cover the change. A project can ask for its full suite instead |
| Versioning | None: leave version numbers and tags alone, as the project's own release process handles them. A project can make every batch a release (see step 4) |
| Low-priority work | None. A project can name a kind of item (for example `Translate …` items) that ranks below other suitable work. It still needs to be queued by the user before implementation |
| Finished items | Whatever the to-do file already does: tick them (`- [x]`) or move them to a "Done" section. If it shows no habit yet, remove them; git history keeps the record |
| Done commit | The to-do change that marks an item done goes into the batch's last commit. A project can ask for a separate commit instead |
| Pushing | Never. Commits stay local until the user pushes |

The project's other rules (testing, safety, style, private files) still apply in full while doing a batch.

## "do it": the whole message

When the user opens a fresh session with the whole request "do it", do the **first unchecked item marked `(do it)`** in the to-do file, and nothing else. A native skill selector may accompany that request; selecting the skill does not authorize any unqueued work.

1. **Commit stray to-do edits first.** If the to-do file has changes that are not committed (items the user added by hand or with a tool), commit them on their own, separate from your work, before starting. Leave any other uncommitted changes as they are and keep them out of your commits; if they touch files the item needs, ask the user before going on.
2. **Build it.** If the item is split into batches, build only the first unchecked batch. Otherwise build every sub-item it lists. Follow the item's notes either way.
   - For a fix, start at the reported defect and its references. Inspect only what you need to find the defect, fix it and verify the fix.
   - If the project has tests, add focused tests for the change in its existing setup, at the layer the change touches (no browser tests for a change confined to a backend script).
   - Leave follow-up work that the project tracks as separate items (for example translating new text) for those items: record it where the project says, do not do it unasked.
3. **Test it.** Run the tests the settings call for, and they must pass. A failure that was already there before your change is not yours to fix: report it and add it to the to-do file if it is not already there. To check, run the failing test on the last commit in a temporary worktree (`git worktree add`, removed afterwards); do not stash the user's uncommitted work.
4. **Finish and commit.** Commit at logical points, one commit per coherent piece. Leave unrelated untracked files out of commits.

   If the project's settings make every batch a release, follow their steps; where they leave something out, use semantic versioning:
   - a breaking change raises the major number (1.38.0 → 2.0.0);
   - a new feature raises the minor number (1.38.0 → 1.39.0);
   - a fix or small adjustment to existing behaviour raises the patch number (1.38.0 → 1.38.1);
   - batches that change only documentation, the to-do list or tests keep the version and do not create a release;
   - only when the version changes, add a line at the top of the changelog and create a new version tag.

   Before the final work commit, mark the finished batch or sub-item done the way the project handles finished items, and note in the parent item what is done (and in which version, if this batch is a release). When the last batch of an item is done, mark the whole item done. Include these to-do updates in the final work commit. If the project requests a separate done commit, commit the work first, then commit the completion update separately.

   For a release, tag the final work commit after creating it (`git tag v1.39.0`). A separate done commit does not receive another release tag.
5. **Report and stop.** Say which batch is done (and in which version, if this batch is a release) and what the next fresh-session "do it" will do: the next unchecked batch of the current item, the next `(do it)` item, or, if none is left, your suggestion (below). Do not start another batch in this session.

### When you get stuck

If the item is unclear, the tests cannot be made to pass, or the work turns out much larger than the item suggests: stop, commit nothing half-done, report what blocked you, and suggest how to reword or split the item.

## When nothing is queued

When no unchecked `(do it)` item remains, do not mark, queue or start anything on your own. Review the **whole** to-do file, including notes at the bottom, and suggest the best candidate for the next `(do it)`:

- It fits the usual size: one quick item, or one manageable chunk of a larger one.
- Items under "Decisions needed" or similar wait for the user's choice. Do not suggest building them; you may remind the user that they are waiting.

Use this priority order both for suggestions and for inserting an item when the user has not specified its place:

1. Reported defects and problems the user highlighted in specific places (a line of code, a screen).
2. Small behavior or usability changes, such as clearer wording for a button.
3. Other planned work.
4. Work the project explicitly names as low priority, even if it would otherwise fit a higher category.

For equal priority, preserve the existing order. Do not just suggest whatever sits at the top of the backlog.

The user decides whether to queue the suggestion. Wait for that, even if only low-priority work remains. If they approve queueing it, follow "Queueing" and stop. Implementation waits for the user to open a fresh session and say "do it". If nothing suitable is left, say so and stop.

The user can always choose any item explicitly, including a low-priority one ahead of the rest.

## Queueing

When the user asks to queue something ("add X to do it", "queue X as the next do it", "queue X after the export fix"):

1. Write it as one unchecked item in the to-do file's queue section (create a `## Do it` section at the top of the file if there is none), marked `(do it)`. The user controls the queue, so put the item where their words say: "next" or "first" puts it at the top, ahead of everything already queued; "after Y", "before Y", "last" or "at the end" put it there. With no ordering word, insert it using the priority order in "When nothing is queued", after existing items of equal priority. Preserve the relative order of existing queued items.

   The user can reorder the queue at any time, by hand or by asking.
2. Make the item self-contained: what to build, the relevant files, functions and endpoints you already know, constraints the user stated, and how to tell it is done. A later session, or another agent, must be able to build it from the item alone.
3. If the item is too large for one quick "do it", split it into numbered batches inside the item, each one a small, independently verifiable result that leaves the project working.
4. Remove the item from wherever it sat before (backlog, notes) so it is not listed twice.
5. Commit the to-do change on its own ("Queue … for do it").
6. Tell the user where it landed: its place in the queue (for example "third of five") and the items just before and after it, so they can move it if they disagree. Stop here; the queued work waits for a fresh-session "do it".

## To-do file shape

A suggested layout is in [the to-do template](templates/TODO.md), relative to this skill's directory. The routine relies only on:

- Markdown checkboxes (`- [ ]`) for open items;
- the `(do it)` marker on queued items;
- the order of items, which is the reading order.

Everything else (sections, headings, notes) is the project's own choice.
