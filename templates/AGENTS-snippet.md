# do it settings

Paste this section into your project's `AGENTS.md` (read by Codex and other agents) or `CLAUDE.md`, and edit it to fit. Agents that do not load skills (Codex, for example) can use the full routine by also pasting the body of `SKILL.md` below it.

```markdown
## do it settings

- To-do file: `TODO.md`
- Tests: `pytest tests/` for server code; `npm test` for page logic; `npm run e2e` when the page or its markup changed.
- Versioning: `VERSION` and `CHANGELOG.md`; tag `vX.Y.Z`. Feature → minor, fix or adjustment → patch; docs, to-do and test-only commits keep the version.
- Low-priority work: `Translate …` items under "Translations". When it is the only work left, "do it" translates the first one: <the project's steps>.
- Never push.
```

Leave out any line where the default in `SKILL.md` already fits.
