# do it

A skill for Claude Code (and a routine for any coding agent) that works through a project's to-do list one queued batch at a time.

You keep a `TODO.md`. You mark the next piece of work with `(do it)`. Then you type **do it**, and the agent:

1. builds exactly that item and nothing else,
2. runs only the tests that cover the change,
3. commits at logical points (raising the version and tagging, if the project is versioned),
4. marks the item done in its own commit, the way your to-do file already does it (ticked, moved to a Done section, or removed),
5. tells you what it did and what the next "do it" will be.

When nothing is queued, it reads the whole to-do list and suggests the best next candidate, putting your reported bugs first, and waits for you to say yes. It never starts unqueued work on its own, never pushes, and keeps each step small enough to review.

## Install

### Claude Code

Personal (all projects):

```sh
git clone <this repo> ~/.claude/skills/do-it
```

Or for one project only, clone it into that project's `.claude/skills/do-it`.

Then type `do it` in a session. Ask "queue the export bug as the next do it" to queue work.

### Codex and other agents

Copy the settings section from `templates/AGENTS-snippet.md` into your project's `AGENTS.md`, followed by the body of `SKILL.md` (everything after the front matter). The body refers to `templates/`; those files are in this repository.

## Set up a project

1. Start a `TODO.md`; `templates/TODO.md` shows a layout that works well.
2. Optionally, add a `do it settings` section to `AGENTS.md` or `CLAUDE.md` (see `templates/AGENTS-snippet.md`) to name your test commands, versioning and any low-priority kind of work. Without it, the defaults in `SKILL.md` apply.
3. Queue the first item and say "do it".

## Files

- `SKILL.md`: the skill itself.
- `templates/TODO.md`: a sample to-do file.
- `templates/AGENTS-snippet.md`: project settings to paste into `AGENTS.md` or `CLAUDE.md`.
