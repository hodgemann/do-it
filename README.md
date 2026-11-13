# do it

A skill for Claude Code (and a routine for any coding agent) that works through a project's to-do list one queued batch at a time.

You keep a `TODO.md`. You mark the next piece of work with `(do it)`. Then you type **do it**, and the agent:

1. builds exactly that item and nothing else,
2. runs the tests that cover the change,
3. commits at logical points,
4. marks the item done the way your to-do file already does it (ticked, moved to a Done section, or removed),
5. tells you what it did and what the next "do it" will be.

When nothing is queued, it reads the whole to-do list and suggests the best next candidate, putting your reported bugs first, and waits for you to say yes. It never starts unqueued work on its own, never pushes, and keeps each step small enough to review.

So "do it" also works when you have time but no plan: the suggestion often brings back something you asked for weeks ago and forgot, and one small finished item tends to spark the next idea.

## Install

### Claude Code

As a plugin (recommended; `/plugin update` keeps it current):

```
/plugin install do-it --marketplace hodgemann/do-it
```

On Claude Code versions older than 2.1.275, add the marketplace first: `/plugin marketplace add hodgemann/do-it`, then `/plugin install do-it@do-it`.

Or as a plain skill, for all projects:

```sh
git clone https://github.com/hodgemann/do-it.git ~/.claude/skills/do-it
```

or for one project only, clone it into that project's `.claude/skills/do-it`.

Then type `do it` in a session. Ask "queue the export bug as the next do it" to queue work.

### Codex and other agents

Copy the settings section from `templates/AGENTS-snippet.md` into your project's `AGENTS.md`, followed by the body of `SKILL.md` (everything after the front matter). The body refers to `templates/`; those files are in this repository.

## Set up a project

1. Start a `TODO.md`; `templates/TODO.md` shows a layout that works well.
2. Optionally, add a `do it settings` section to `AGENTS.md` or `CLAUDE.md` (see `templates/AGENTS-snippet.md`) to name your test commands, make every batch a release (version bump, changelog line, tag), set a low-priority kind of work, and so on. Without it, the defaults in `SKILL.md` apply.
3. Queue the first item and say "do it".

## Files

- `SKILL.md`: the skill itself.
- `templates/TODO.md`: a sample to-do file.
- `templates/AGENTS-snippet.md`: project settings to paste into `AGENTS.md` or `CLAUDE.md`.
- `.claude-plugin/`: the plugin manifest and a one-plugin marketplace, so Claude Code can install the repository with `/plugin`.

## License

Public domain, under [the Unlicense](LICENSE): use, change and share it however you like, no attribution needed.
