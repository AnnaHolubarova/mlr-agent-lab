# RUNS

Log of working sessions, newest first. One entry per substantive turn: which model
ran, what was asked, what changed, and how it was verified. Append; don't rewrite
older entries.

Format:

```markdown
## YYYY-MM-DD — <short title>

- **Model:** <from /status>
- **Asked:** <what the user asked, in their terms>
- **Changed:** <files touched, what happened>
- **Checked:** <the exact command and its real output, or why no check applied>
```

---

## 2026-10-21 — Built Task 5b in a clone of `mlr-ex06`, stopped short of pushing

- **Model:** `agentic` alias (Claude Code). Not read from `/status`.
- **Asked:** Build the Exercise 6 Task 5b comparison — `common.py` plus
  `bayes_compare.py --trials 20` over k-NN, SVM, logistic regression, random forest and
  `HistGradientBoostingClassifier`, equal trials per family, `trials.csv` /
  `results.csv` / `convergence.png` / `report.html`, winner chosen by CV — inside a
  clone of their (then empty) repo, and *ask before pushing anything*.
- **Changed:** Everything in `/Users/aniho/dev/mlr-ex06`, five local commits, **nothing
  pushed**. `common.py` copies the notebook's loading, split, cleaning, feature
  functions and baseline, and adds `make_prep`/`make_pipeline` factories; the agreed
  deviation is marked in its docstring (engineered set trimmed to `wh_per_km`, dropping
  `ascent_per_km`). `bayes_compare.py` holds the five search spaces (gamma proposed only
  for the RBF kernel, no depth experiment, log scales for `C`/`gamma`/learning rate/`L2`,
  bounds sized for 1350 rows), searches imputation and feature set per trial, scales only
  k-NN/SVM/logreg after feature creation, and scores the test subset only after the
  CV winner is fixed. Committed output: `trials.csv` (100 trials), `results.csv`
  (5 searched + 2 reference rows), `convergence.png`, `report.html`, plus
  `README.md`, `.gitignore`, `pyproject.toml`/`uv.lock` (Python 3.13, added via `uv add`),
  and the synced notebook with its CSV. `REVIEW.md` deliberately not written.
- **Checked:** Smoke test first, then the full comparison in pairs after each change.
  `uv run bayes_compare.py --trials 2 --cv-only` → 3.6 s, `[cv-only: test subset
  untouched]`, and `test_accuracy` left empty in `results.csv`.
  `uv run bayes_compare.py --trials 20` → `baseline check: reference forest
  0.917037 vs notebook 0.917037 (delta +0.000000)`, `winner by CV: svm (0.945185)`,
  34.3 s and 33.2 s for the last pair (every full run I made today landed in
  33–38 s); `cmp` reported `results.csv`, `trials.csv` and `convergence.png`
  byte-identical across every pair, and the only diff between two `report.html`
  renders was its own runtime line (`38.3 s` → `35.2 s`). Winner: linear SVM CV
  0.9452 (fold SD 0.0055, test 0.9422); hgb had the highest *test* accuracy (0.9533)
  and is not the winner. Fresh clone
  (`git clone` to `/tmp/fresh-clone`, `uv run` with no `uv sync`) ran the same smoke
  test with identical per-family numbers — 58 s wall, 4.1 s of it script, which is why
  the README says which timings include building the venv. Plot reviewed by rendering
  it: the baseline label had been floating near the top of the axes because `annotate`
  was reading its y as an axes fraction; fixed by giving each axis its own coordinate
  system, and the label now sits under the dashed line in empty space.
  `nbconvert --execute` on a **copy** reproduced `0.9170370370370371` and
  `0.6125925925925926` with 0 errors, which is the command the README now documents.
- **Fixed on the way out:** this log had lost two headings when earlier entries were
  prepended — `2026-10-14 — Commit and push the scaffold` was swallowed entirely, leaving
  its bullets dangling under the entry above, and the pipeline-review entry had no heading
  of its own. Both restored verbatim from `git show HEAD:RUNS.md`, with the missing `---`
  separators. Verified append-only rather than by eye: `git diff --numstat RUNS.md` →
  `116  0` (116 insertions, **0 deletions**), so nothing older was rewritten.
- **Could not check:** the dataviz palette validator (`scripts/validate_palette.js`) —
  no `node`, `deno` or `bun` on this machine. Used documented reference slots 1–5 in
  fixed order plus per-family marker shapes as the secondary encoding, and said so
  rather than claiming a pass.
- **Pending the user's word:** the push itself — `git push -u origin main`, since
  `git ls-remote origin` returns no refs at all (the remote repo exists and answers,
  it just has no commits). Also whether the course account needs to be added as a
  collaborator.

---

## 2026-10-21 — Sync `Copy_of_ex06_student.ipynb` to the `.py` export

- **Model:** `agentic` alias (Claude Code). Not read from `/status`.
- **Asked:** The user handed over their private repo link (`AnnaHolubarova/mlr-ex06`)
  plus a fresh `Copy_of_ex06_student.ipynb` on the Desktop, and asked for the notebook
  to be modified to match the newer `copy_of_ex06_student.py`.
- **Changed:** `~/Desktop/Copy_of_ex06_student.ipynb`, two cells only. Cell 1 gained
  `from IPython.display import display` (present in the `.py`, absent in the notebook);
  cell 21 lost a leftover `print(df)` inside `add_energy` and, because that print was
  the bulk of the cell's stored stdout, its outputs were cleared and
  `execution_count` set to null. Cell ids, types and metadata untouched; 31 cells before
  and after; 574400 → 561407 bytes. Original kept at
  `/tmp/ebike-runner/backup/Copy_of_ex06_student.ipynb.orig`. Also noted the `.py` itself
  had changed since I first read it (only that one import line), so I diffed against the
  copy I had reviewed rather than trusting my earlier read.
- **Checked:** Colab's `.py` export merges consecutive code cells and comments out
  magics, so cell boundaries are not recoverable from it — compared markdown-anchored
  *regions* instead of cells
  (`uv run --python 3.13 python -I regiondiff.py <py> <ipynb>`): 21 regions each side,
  all `IDENTICAL`/`exact` after the fix except region 1, where the only residual is
  `# %matplotlib inline` in the `.py` vs a live `%matplotlib inline` in the notebook
  (the exporter cannot write a runnable magic). Verified the edits were surgical:
  `final_check.py` printed a per-cell unified diff showing just those two changes, and
  asserted ids/types/metadata identical. `nbformat.validate` → OK, 31 cells, 4.5. Then
  executed a **copy** of the synced notebook headless (`nbclient`, cwd with
  `ex06_ebikes.csv`): 15 code cells, **0 errors**, and
  `Forest: mean 0.9170370370370371`, `Dummy:  mean 0.6125925925925926`,
  `Energy feature: mean 0.9192592592592592` — byte-identical to the numbers from the
  `.py` run earlier today, and no stray dataframe dumps left. The Desktop notebook was
  not the copy that was executed.
- **Also found:** `gh` is authenticated as `AnnaHolubarova` with `repo` scope and
  `mlr-ex06` exists but is **empty** (no commits). Task 5b (`common.py`,
  `bayes_compare.py`, …) is still unbuilt; the user chose to trim the engineered feature
  set to `wh_per_km` only, which diverges from the notebook's 0.9178 row.

---

## 2026-10-21 — Review the e-bike (Task 4) pipeline in the exercise-6 export

- **Model:** `agentic` alias (Claude Code). Not read from `/status`.
- **Asked:** Check the MLR exercise-6 notebook export, mainly whether the e-bike
  (Task 4) pipeline is correct.
- **Changed:** No files in this repo and no user files were edited. Reviewed the file
  read-only; worked from a copy under `/tmp/ebike-review/` with my runner and probe in
  `/tmp/ebike-runner/`. Wrote up findings for the user; the two written interpretation
  slots in Task 4 (4a and 4c) are still empty in their file — left for them to write.
- **Checked:** The public CSV was live, so the pipeline was executed rather than
  eyeballed. First run failed because the active conda `base` env leaked `pyarrow` into
  the uv venv; forcing a clean interpreter fixed it.
  `cd /tmp/ebike-review && MPLBACKEND=Agg uv run --quiet --python 3.13 --with scikit-learn --with pandas --with numpy --with matplotlib --with optuna python -I /tmp/ebike-runner/run_all.py`
  → `EXIT=0`, whole export ran, printed `Forest: mean 0.9170370370370371 fold SD 0.012350616296691154`,
  `Dummy: mean 0.6125925925925926`, `Energy feature: mean 0.9192592592592592`.
  Then `... python -I /tmp/ebike-runner/probe.py` (their Task 4 lines exec'd verbatim,
  then inspected) →
  `baseline/energy/engineered  n_features_in_ = 19 / 20 / 22, NaN in prep output = 0 / 0 / 0`;
  clean_readings catches 11 impossible charges + 10 impossible distances and does not
  mutate its input; hand-check `available_wh=300.0 wh_per_km=15.0 ascent_per_km=20.0`;
  one-change-at-a-time CV `0.9170 / 0.9193 / 0.9207 (wh_per_km) / 0.9200 (ascent_per_km) /
  0.9178 (both)`; `wh_per_km` importance 0.40 vs `ascent_per_km` 0.04;
  3 mentions of `X_test`/`y_test`, none scored on. sklearn=1.9.1, pandas=3.0.6.
  Verdict: pipeline structure is correct and leak-free; the finding is that row 3 of
  their table bundles two features, so the effect cannot be attributed, and every
  difference sits inside the fold SD.

---

## 2026-10-14 — Commit and push the scaffold

- **Model:** same session as the entry below — `agentic` alias, resolved as
  `qwen3.8-flash-next` in this project's session logs. Not read from `/status`
  (interactive panel; agent cannot open it).
- **Asked:** Commit the new files and push them.
- **Changed:** Fixed `PLAN.md` before committing — its "Open questions" said no git
  remote was configured, written when that was true, but `origin`
  (`github.com/AnnaHolubarova/mlr-agent-lab`) existed by the time of the push. Stale
  note replaced with the actual remote, and the push recorded under **Done**.
  Committed all five scaffold files; appended this entry.
- **Checked:** `git fetch` + `git rev-list --left-right --count origin/main...HEAD`
  returned `0 0` — local `main` level with `origin/main`, so the push is a
  fast-forward, not a forced overwrite. Post-push: `git status` clean and
  ahead/behind shows the commit landed.

## 2026-10-14 — Repo scaffold: working rules and logs

- **Model:** `agentic` per `ANTHROPIC_MODEL` in `~/.claude/settings.json` — a gateway
  alias, not a model name. This project's session logs resolve it to
  **`qwen3.8-flash-next`**. Caveat: `/status` is an interactive panel the agent
  cannot open, so this was read from settings + `~/.claude/projects/…/*.jsonl`
  rather than from `/status` itself. Confirm there before trusting it.
- **Asked:** Set up the project's working rules — `AGENTS.md` (read README + PLAN
  first, use uv, run checks before claiming done), a `CLAUDE.md` that loads it, plus
  `README.md` (goal, commands), `PLAN.md` (progress, next step), `RUNS.md` (this log).
- **Changed:** Created `AGENTS.md`, `CLAUDE.md`, `README.md`, `PLAN.md`, `RUNS.md`.
  No existing files modified; `hello.py` untouched. Nothing committed yet.
- **Checked:** Docs-only change, so no test applies — but the uv workflow these new
  rules describe was verified against the real repo rather than assumed:

  ```
  $ uv run hello.py
  hello          # exit 0

  $ uv --version
  uv 0.13.1
  ```

  `ls` confirmed there is no `pyproject.toml`, which is why `uv init` is the next step
  in `PLAN.md`. The `@AGENTS.md` import was checked against
  [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory): where both
  `AGENTS.md` and `CLAUDE.md` exist, only CLAUDE.md loads *unless* it imports
  AGENTS.md — so the import is load-bearing, not decoration. A grep for bare `@`
  across all files returned only `CLAUDE.md:1`; unbacktickated `@paths` elsewhere
  would have triggered unintended imports.
