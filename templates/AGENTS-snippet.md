# do it settings

Paste this section into your project's `AGENTS.md` (read by Codex and other agents) or `CLAUDE.md`, and edit it to fit. Agents that do not load skills (Codex, for example) can use the full routine by also pasting the body of `SKILL.md` below it.

```markdown
## do it settings

- To-do file: `TODO.md`
- Tests: `pytest tests/` for server code; `npm test` for page logic; `npm run e2e` when the page or its markup changed.
- Versioning: every batch is a release. Bump `VERSION`, add a line to `CHANGELOG.md` and tag `vX.Y.Z`.
- Low-priority work: `Translate …` items under "Low priority". When it is the only work left, "do it" translates the first one: <the project's steps>.
- Finished items: tick them (`- [x]`) and leave them in place.
- Done commit: mark items done in a commit of their own.
```

Leave out any line where the default in `SKILL.md` already fits.
