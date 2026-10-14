# mlr-agent-lab

A small lab for experimenting with coding agents: how they read a project, follow
working rules, run checks, and log what they did. The repo is deliberately tiny so
the process is the subject, not the code.

Layout:

| File | What it is |
| --- | --- |
| `AGENTS.md` | Working rules for any agent (or human) working here. Source of truth. |
| `CLAUDE.md` | One line that loads `AGENTS.md` into Claude Code. |
| `PLAN.md` | Where the project stands and what the next step is. |
| `RUNS.md` | Running log: model used, what was asked, how it was verified. |
| `hello.py` | Placeholder script, used as the smoke test. |

## Setup

[uv](https://docs.astral.sh/uv/) is the only thing you need installed. Nothing else —
no global pip installs, no system Python changes.

```bash
uv --version        # confirm uv is available
uv run hello.py     # works straight away; uv manages the environment
```

Dependencies come in with `uv add <package>`, which creates and updates
`pyproject.toml` + `uv.lock`. Commit both files; never commit `.venv/`.

## Running things

| Command | What it does |
| --- | --- |
| `uv run hello.py` | Runs the placeholder script — the smoke test for any change. |
| `uv add <package>` | Adds a dependency and syncs the lockfile. |
| `uvx <tool>` | Runs a tool in an ephemeral env without installing it. |

## Working rules

Start every session with `README.md` and `PLAN.md`, then follow `AGENTS.md`.
The short version: read before writing, use `uv`, and don't call something done
until a check has actually run.
