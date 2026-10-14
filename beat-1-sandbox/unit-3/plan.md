# Plan: issue #60, `FaithfulnessChecker.check()` crashes on `text: None`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60
Reproduction this plan builds on: my Unit 2 repro comment,
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5905342853
(commit `2f4e82f`, macOS 26.6.2 arm64, Python 3.11.16, structlog 26.1.0, pytest 9.1.1).

## Repro evidence relied on

Failing run, the issue's own snippet:

```text
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
  File "rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
TypeError: sequence item 0: expected str instance, NoneType found
```

Controls and tests from the same report:

| Check | Recorded result |
|---|---|
| Missing-key snippet `[{'content': 'Python skills'}]` | `score = 0.0`, exit 0 |
| String-text snippet `[{'text': 'Python skills'}]` | `score = 0.0`, exit 0 |
| `test_none_context_chunk_text` | `1 xfailed`, exit 0 |
| `test_none_context_chunk_text` with `--runxfail` | `1 failed`, same `TypeError`, exit 1 |
| `test_missing_text_key_in_chunk` | `1 passed`, exit 0 |
| `tests/unit/test_faithfulness_checker.py` | `18 passed, 4 xfailed`, exit 0 |

## Diagnosis

`check()` builds the context at `rag/evaluator/faithfulness_checker.py:38`
with `chunk.get("text", "")`. The `""` default is used only when the
`text` key is missing. When the key is present with value `None`,
`.get()` returns `None`, and `" ".join(...)` raises because it only
accepts strings. The controls fit this: the missing-key chunk gets the
default and scores `0.0`; the string chunk joins normally. Only the
present-but-`None` value fails, so the cause is the read of the chunk
value, not the join or the scoring.

## Thread and prior work

No maintainer has commented on #60, so there is no maintainer direction
to follow. Three classmates (sid-pandya, pkmaster21, riyan42069) have
posted plans for the same `chunk.get("text") or ""` fix, and four open
PRs implement it: #80 (pakmultilinks-dot), #94 (sid-pandya), #216
(Aryankn29) and #219 (pkmaster21). Under the house rules in
`scope.md`, a classmate's plan does not block mine, and each student
builds on their own fork. My plan reaches the same fix from my own
controls, and my comment says so rather than presenting it as new. If
a maintainer merges one of those PRs first, I will compare my branch
with it and close mine rather than duplicate it.

## Scope

In scope:

- Line 38 of `rag/evaluator/faithfulness_checker.py`: treat a `None`
  `text` value the same as a missing key (empty string).
- `tests/unit/test_faithfulness_checker.py`: delete the
  `@pytest.mark.xfail(strict=True, reason="issue #60: ...")` marker on
  `test_none_context_chunk_text`. `docs/CONTRIBUTING.md` requires this:
  the marker is strict, so once the fix works the test would report
  `XPASS(strict)` and fail CI.

Not in scope:

- The three `xfail` tests that cite issue #59 (claim-support scoring in
  `_is_supported()`). They stay marked and must still report `xfailed`.
- Changes to `_extract_claims()`, `_is_supported()`, the score
  formula, or `rag/evaluator/eval_suite.py` (the only caller).
- Validating other non-string `text` values (numbers, lists). The
  issue and my repro cover only `None`.

## Files

- `rag/evaluator/faithfulness_checker.py` (one line, line 38)
- `tests/unit/test_faithfulness_checker.py` (remove one decorator, 4 lines)

## Approach

1. Branch from `main` at `2f4e82f` as `fix/60-null-chunk-text`.
2. Change line 38 to
   `context_text = " ".join([chunk.get("text") or "" for chunk in context_chunks])`.
   `or ""` maps both a missing key and an explicit `None` to `""`;
   any non-empty string is unchanged, and an empty string stays empty.
3. Delete the issue-60 `xfail` decorator from
   `test_none_context_chunk_text`. Leave the test body unchanged; its
   assertions (`isinstance(score, float)` and `0.0 <= score <= 1.0`)
   are the acceptance criterion from my repro report.
4. Commit as `fix(rag): handle None text in faithfulness context chunks`
   with `Fixes #60`, per the repo's Conventional Commits rule.

## Test plan

Re-run the Unit 2 repro steps against the branch (same venv, same
commands):

1. The issue snippet, printed:
   `.venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'text': None}]))"`
   Before: `TypeError` at line 38, exit 1. Expected after: no
   traceback, `score = 0.0`, exit 0 (an empty context supports no
   claims).
2. Both controls: expected unchanged, `score = 0.0`, exit 0.
3. `pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text`:
   before `XFAIL`; expected after `PASSED` with no marker and no
   `--runxfail`.
4. `pytest tests/unit/test_faithfulness_checker.py -q -m unit`: before
   `18 passed, 4 xfailed`; expected after `19 passed, 3 xfailed` (the
   three #59 tests).
5. `make lint`, `make typecheck` and `make test-unit`, to match the
   repo's PR checklist. I will report which of these I actually ran
   and their results; integration and frontend jobs are not touched
   by this change and are left to CI.

## Risks and unknowns

- I have not traced where `eval_suite.py` gets chunks with
  `text: None`. This fix makes the checker tolerate them; it does not
  change whatever produces them. If a reviewer wants the producer
  fixed too, that is a separate issue.
- `or ""` also turns other falsy values (`0`, `[]`, `False`) into
  `""`. Non-falsy non-string values such as a number would still raise.
  I am treating both as out of scope because the issue is about `None`.
- A `None` chunk now counts as empty context, so a claim cannot be
  supported by it. I believe that matches the test's intent ("handle
  gracefully", score in `[0, 1]`); it is not a behavior choice the
  issue states outright.

## Deviations

Nothing changed; the plan held. The build on `fix/60-null-chunk-text`
(commit `93bf3ea`) is the two edits listed above and nothing else: line
38 now reads `chunk.get("text") or ""`, and the issue-60 `xfail`
decorator is gone from `test_none_context_chunk_text`. Every expected
result in the test plan came out as written: the snippet returns
`score = 0.0` with exit 0, both controls are unchanged, the named test
reports `PASSED`, and the evaluator file shows `19 passed, 3 xfailed`.

One thing to note that is not a change to the plan: the fork's Unit 2
venv only has `structlog` and `pytest`, so `make lint`, `make typecheck`
and `make test-unit` ran against a separate venv with `pip install -e
".[dev]"`. Results: ruff "All checks passed!", mypy "no issues found in
76 source files", and `376 passed, 52 xfailed`. Integration and frontend
checks were not run; they are left to CI.

Before posting, I changed the branch name from `fix/60-none-chunk-text`
to `fix/60-null-chunk-text`, because sid-pandya's PR #94 already uses
the first name. This happened before the comment went up, so the posted
plan and the build match.
