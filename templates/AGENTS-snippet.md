# do it settings

Paste this section into your project's `AGENTS.md` (read by Codex and other agents) or `CLAUDE.md`, and edit the test commands to fit. Both Codex and Claude Code can load the routine as a native skill; installation is described in the README.

```markdown
## do it settings

- To-do file: `TODO.md`
- Tests: `pytest tests/` for server code; `npm test` for page logic; `npm run e2e` when the page or its markup changed.
```

Leave out any line where the default in `SKILL.md` already fits. Without overrides, the routine leaves version numbers and tags alone, follows the to-do file's existing habit for finished items, and includes completion updates in the final work commit.

Optional overrides: copy only the lines you want into the same settings section.

```markdown
- Versioning: make each implementation batch a release. Bump `VERSION`, add a line to `CHANGELOG.md` and tag `vX.Y.Z`. Documentation, to-do and test-only batches do not create releases.
- Done commit: mark items done in a commit of their own.
- Finished items: tick them (`- [x]`) and leave them in place.
- Low-priority work: `Translate …` items under "Low priority". Suggest these after other suitable work; they still need the user's approval to queue them and a fresh-session "do it" before implementation.
```
