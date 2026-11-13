# do it

A skill for Codex and Claude Code (and a routine for any coding agent) that works through a project's to-do list one queued batch at a time.

You keep a `TODO.md`. You mark the next piece of work with `(do it)`. Then you open a fresh session and type **do it**, and the agent:

1. builds exactly that item, or its first unfinished batch,
2. runs the tests that cover the change,
3. marks the completed work done the way your to-do file already does it (ticked, moved to a Done section, or removed) and includes that update in the final work commit,
4. tells you what it did and what the next fresh-session "do it" will be, then stops.

When nothing is queued, it reads the whole to-do list and suggests the best next candidate, putting your reported bugs first. It waits for your approval to queue the suggestion. After it is queued, you open a fresh session and say "do it" to start the work. This applies even when only low-priority work remains. The agent never starts unqueued work, never pushes, and keeps each step small enough to review.

"do it" is mainly for quick things: one small, independently verifiable result that leaves the project working. Complex work fits too: it is split into a series of quick batches, and each fresh-session "do it" does the next one.

So "do it" also works when you have time but no plan: the suggestion often brings back something you asked for weeks ago and forgot, and one small finished item tends to spark the next idea.

## Install

### Claude Code plugin

The recommended way for Claude Code; manage updates in the `/plugin` panel:

```
/plugin install todo-do-it --marketplace hodgemann/do-it
```

On Claude Code versions older than 2.1.275, add the marketplace first: `/plugin marketplace add hodgemann/do-it`, then `/plugin install todo-do-it@todo-do-it`.

Open a fresh session in the project and say `do it`. The plugin's skill is named `todo-do-it:do-it`, so you can also type `/todo-do-it:do-it` to call it directly.

### Native skills for Codex or Claude Code

Clone the repository once, outside either agent's skill directory:

```sh
mkdir -p ~/.local/share
git clone https://github.com/hodgemann/do-it.git ~/.local/share/do-it
```

If you already have a checkout, use its location in the copy commands below. Both agents install the same `skills/do-it/` folder, which contains the instructions, templates and Codex picker metadata.

#### Codex

For all projects:

```sh
mkdir -p ~/.agents/skills
cp -R ~/.local/share/do-it/skills/do-it ~/.agents/skills/
```

Or, from a project's root, install it for that project only:

```sh
mkdir -p .agents/skills
cp -R ~/.local/share/do-it/skills/do-it .agents/skills/
```

Copying just the shared skill folder gives Codex the name `do-it`. A full-repository install or a symlink back into it can pick up the Claude plugin manifest and add a plugin prefix. Codex loads the installed `SKILL.md` and its bundled templates directly; only optional project settings belong in `AGENTS.md`. See the [official Codex skill documentation](https://learn.chatgpt.com/docs/build-skills).

Open a fresh Codex session in the project and say `do it`. If Codex does not select it from the plain wording, select **do it** through `/skills` or mention `$do-it` alongside the `do it` request.

#### Claude Code plain skill

For all projects:

```sh
mkdir -p ~/.claude/skills
cp -R ~/.local/share/do-it/skills/do-it ~/.claude/skills/
```

Or, from a project's root, install it for that project only:

```sh
mkdir -p .claude/skills
cp -R ~/.local/share/do-it/skills/do-it .claude/skills/
```

Open a fresh Claude Code session in the project and say `do it`, or type `/do-it` to call the skill directly.

For each agent, choose one installation method and scope per project. To update a native installation, pull changes in the checkout and repeat its copy command, then open a fresh session.

### Other agents

For an agent without native skill support, copy the settings section from `skills/do-it/templates/AGENTS-snippet.md` into your project's agent instruction file, followed by the body of `skills/do-it/SKILL.md` (everything after the front matter). Copy `skills/do-it/templates/` into the project as `do-it-templates/` and update the two template links in the pasted body to point there.

## Set up a project

1. Start a `TODO.md`; `skills/do-it/templates/TODO.md` shows a layout that works well.
2. Optionally, add a `do it settings` section to `AGENTS.md` or `CLAUDE.md` (see `skills/do-it/templates/AGENTS-snippet.md`) to name your test commands, make every batch a release (version bump, changelog line, tag), set a low-priority kind of work, and so on. Without it, the defaults in `skills/do-it/SKILL.md` apply.
3. To queue work, say "add the export bug to do it". The agent inserts it by priority: reported problems, small behavior or usability changes, other planned work, then configured low-priority work. Equal-priority items keep their order. It tells you where the item landed. Say "as the next do it" to put it first, or name another position explicitly.
4. Open a fresh session and say "do it". Each session completes one batch, then stops.

By default, version numbers and tags are left alone and completion updates go into the final work commit. The settings template keeps release-per-batch and separate completion commits as optional examples.

## Files

- `skills/do-it/SKILL.md`: the one shared skill used by both agents.
- `skills/do-it/templates/TODO.md`: a sample to-do file.
- `skills/do-it/templates/AGENTS-snippet.md`: project settings to paste into `AGENTS.md` or `CLAUDE.md`.
- `skills/do-it/agents/openai.yaml`: Codex skill picker metadata.
- `.claude-plugin/`: the plugin manifest, its icon and a one-plugin marketplace, so Claude Code can install the repository with `/plugin` and load the shared folder under `skills/`.

## Support

Report bugs and ask questions in [GitHub issues](https://github.com/hodgemann/do-it/issues).

## Privacy

do it is a set of written instructions, not a program. It contains no code, collects no data and contacts no service. Your agent follows the instructions inside your project: it reads and edits project files, runs the commands your settings name, and makes local commits that it never pushes. Anything your agent's provider does with your session is covered by that provider's own privacy policy.

## License

Public domain, under [the Unlicense](LICENSE): use, change and share it however you like, no attribution needed.
