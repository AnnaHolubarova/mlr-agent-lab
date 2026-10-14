# PLAN

Read this with `README.md` first — see `AGENTS.md`.

## Done

- **2026-10-14** Repo scaffold in place: `AGENTS.md` (working rules), `CLAUDE.md`
  (loads it), `README.md` (goal + commands), `PLAN.md` and `RUNS.md` (this file and
  the log).
- **2026-10-14** Smoke test passes: `uv run hello.py` prints `hello`.
- **2026-10-14** Scaffold committed and pushed to `origin/main`.

## Next step

**Initialize the uv project properly** — there is no `pyproject.toml` yet, so
`uv run` works off an ambient env and `uv add <package>` would create the project
files implicitly mid-task. Do it deliberately instead:

```bash
uv init --bare .    # then confirm Python version, re-run uv run hello.py
```

## Later

- Replace `hello.py` with a real script once there is something to build; until
  then it stays as the smoke test.
- Add a lint/format step (`uvx ruff check .`) and fold it into the
  "check before you say it is done" list in `AGENTS.md`.
- Add a `.gitignore` for `.venv/` and `__pycache__/`.
- Decide what the lab actually measures: the run log in `RUNS.md` is the raw
  material, but there's no way to compare runs across models yet.

## Open questions

- Nothing yet. (`origin` → `github.com/AnnaHolubarova/mlr-agent-lab` is set up and
  `main` tracks it, so the backup gap noted when this file was written is closed.)
