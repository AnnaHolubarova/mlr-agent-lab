# Working Rules

**Read first, every session:** `README.md` (what this is, how to run it), then
`PLAN.md` (where it stands, what's next). Read more only when the plan or your
change reaches files that actually exist.

## Python: uv only

Never install into a global or system environment.

- Run: `uv run <script.py>` · Tool: `uvx <tool>`
- Add a dependency: `uv add <package>` — it edits `pyproject.toml` and syncs `uv.lock`
- Commit `pyproject.toml` and `uv.lock` together. Never commit `.venv/`.
- No `pip install`, no bare `python3 <script>`, no ad-hoc virtualenvs.

If `pyproject.toml` is missing, say so and propose `uv init --bare .` — creating the
project is the user's call, not a side effect of a task.

## Check before you say it is done

"Done" means a check ran and passed **in this session**. Run the narrowest check that
covers the change, then paste the real command and output — failures included.

- Any code change: `uv run <the script you touched>` runs clean, or the test suite.
- New dependency: run something that imports it, so the import is proven not assumed.
- Docs only (`README.md`, `PLAN.md`, `RUNS.md`): re-read what you wrote. No command.
- A check you could not run: say so, and say why. Never predict that it would pass.

## Log it

Append one entry per substantive turn to `RUNS.md` (newest first): model, what was
asked, what changed, how it was checked. Don't rewrite older entries.

## Keep it small

Small commits with messages that say *why*. No speculative abstractions, no
unrequested refactors. Match the style the existing code already has.
