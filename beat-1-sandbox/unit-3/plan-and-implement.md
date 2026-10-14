# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of my plan, the branch I built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

jonnypan2325

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-6178704601

Plan for #60, built from my reproduction above (commit `2f4e82f`, Python 3.11.16).

**Cause:** `rag/evaluator/faithfulness_checker.py:38` reads each chunk with `chunk.get("text", "")`. The default only covers a missing key, so `{"text": None}` passes `None` into `" ".join(...)`, which raises the `TypeError`. My controls fit this: the missing-key chunk and the string chunk both return `score = 0.0`; only the present-but-`None` value fails.

**Relation to earlier work here:** sid-pandya, pkmaster21 and riyan42069 have already posted plans for this same `or ""` fix, and #80, #94, #216 and #219 are open PRs that implement it. I reached the same diagnosis from my own controls. My plan isn't a different approach; it's my own build of that fix, per the course's one-branch-per-student setup. If a maintainer merges one of those first, I'll check my branch against it and close mine rather than duplicate it.

**Change (branch `fix/60-null-chunk-text` on my fork):**

1. Line 38 becomes `chunk.get("text") or ""`, so a `None` value is treated like a missing key.
2. Remove the issue-60 `@pytest.mark.xfail(strict=True)` marker from `test_none_context_chunk_text`, as `docs/CONTRIBUTING.md` asks for seeded bugs.

**Out of scope:** the three #59 xfail tests and the claim-support scoring in `_is_supported()`, the score formula, and the caller in `eval_suite.py`.

**How I'll check it:** re-run my repro steps on the branch.

- The issue snippet should return `score = 0.0` with exit 0 instead of the traceback.
- Both controls should be unchanged.
- `test_none_context_chunk_text` should report `PASSED` with no marker.
- The evaluator test file should go from `18 passed, 4 xfailed` to `19 passed, 3 xfailed`.

I'll also run `make lint`, `make typecheck` and `make test-unit`, and report which ones I actually ran.

**Open questions:**

- Should the producer of `text: None` chunks be fixed as well? I haven't traced where they come from. This change makes the checker tolerate them but doesn't touch the source, so I'd treat that as a separate issue unless a reviewer wants it here.
- Should a non-string `text` value (e.g. a number) be handled too? `or ""` maps falsy values to `""`, but a non-falsy non-string would still raise. I've left it out because the issue is about `None`.

**AI assistance:** Claude Code helped draft this plan and will make the edits. I review every diff before keeping it and run the checks myself.

---

## Your branch

**Branch**

fix/60-null-chunk-text

(On my fork: https://github.com/jonnypan2325/pathreview-ai301-fa26-s3/tree/fix/60-null-chunk-text, commit `93bf3ea`, built on `main` at `2f4e82f`.)

**Evidence**

These are my Unit 2 reproduction steps re-run against the built change, in the same environment as the Unit 2 report: macOS 26.6.2 (arm64), Python 3.11.16, structlog 26.1.0, pytest 9.1.1, in the fork's `.venv`. Local path prefixes are replaced with `<repo>`. Claude Code ran these commands in my clone; the output below is copied from the saved run logs.

*Before: unchanged `main` at `2f4e82f`*

```text
$ git rev-parse --short HEAD
2f4e82f

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
exit=1

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text -rx
============================= test session starts ==============================
platform darwin -- Python 3.11.16, pytest-9.1.1, pluggy-1.6.0 -- <repo>/.venv/bin/python
rootdir: <repo>
configfile: pyproject.toml
collecting ... collected 22 items / 21 deselected / 1 selected

tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text XFAIL [100%]

=========================== short test summary info ============================
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text - issue #60: faithfulness checker crashes when a context chunk has text: None
====================== 21 deselected, 1 xfailed in 0.04s =======================
exit=0

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text --runxfail
            Faithfulness score 0.0-1.0 (ratio of supported claims)
        """
        if not feedback or not context_chunks:
            logger.info(
                "faithfulness_empty_input",
                has_feedback=bool(feedback),
                has_chunks=bool(context_chunks),
            )
            return 0.0
    
        # Extract key claims from feedback (sentences)
        claims = self._extract_claims(feedback)
        if not claims:
            logger.info("faithfulness_no_claims_extracted")
            return 0.5  # Default to neutral if no extractable claims
    
        # Concatenate context text
>       context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E       TypeError: sequence item 0: expected str instance, NoneType found

rag/evaluator/faithfulness_checker.py:38: TypeError
=========================== short test summary info ============================
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
======================= 1 failed, 21 deselected in 0.04s =======================
exit=1

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -q -m unit
..x...x......x....x...                                                   [100%]
18 passed, 4 xfailed in 0.05s
exit=0
```

*After: branch `fix/60-null-chunk-text` at `93bf3ea`*

```text
$ git rev-parse --abbrev-ref HEAD
fix/60-null-chunk-text

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'text': None}]))"
2026-10-14 14:17:59 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit=0

$ # Control: missing text key
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'content': 'Python skills'}]))"
2026-10-14 14:17:59 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit=0

$ # Control: string text
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'text': 'Python skills'}]))"
2026-10-14 14:17:59 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit=0

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text -rx
============================= test session starts ==============================
platform darwin -- Python 3.11.16, pytest-9.1.1, pluggy-1.6.0 -- <repo>/.venv/bin/python
rootdir: <repo>
configfile: pyproject.toml
collecting ... collected 22 items / 21 deselected / 1 selected

tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text PASSED [100%]

======================= 1 passed, 21 deselected in 0.04s =======================
exit=0

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -q -m unit
..x...x......x........                                                   [100%]
19 passed, 3 xfailed in 0.06s
exit=0
```

The repo's own checks, run on the branch. The repro venv has only `structlog` and `pytest`, so these used a separate venv built with `pip install -e ".[dev]"`. `make test-unit` output is cut to its summary line.

```text
$ make lint VENV_BIN=.venv-full/bin
.venv-full/bin/ruff check .
All checks passed!
exit=0

$ make typecheck VENV_BIN=.venv-full/bin
.venv-full/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
exit=0

$ make test-unit VENV_BIN=.venv-full/bin
[warnings trimmed]
================= 376 passed, 52 xfailed, 5 warnings in 13.35s =================
exit=0

$ .venv-full/bin/black --check rag/evaluator/faithfulness_checker.py tests/unit/test_faithfulness_checker.py
All done! ✨ 🍰 ✨
2 files would be left unchanged.
exit=0
```

Result: the issue snippet goes from a `TypeError` with exit 1 to `score = 0.0` with exit 0. Both controls are unchanged. `test_none_context_chunk_text` goes from `XFAIL` to `PASSED` with its marker removed. The evaluator file goes from `18 passed, 4 xfailed` to `19 passed, 3 xfailed`; the three remaining xfails are issue #59. Integration and frontend checks were not run.

## Eval iterations

**Run history**

1. Full run, first rubric: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. pkg-03, pkg-09 and pkg-14, all `clear-accept`, were rejected.
2. Partial, `--only pkg-03,pkg-09,pkg-14,pkg-16,pkg-17,pkg-18`, after loosening `executable-by-stranger` and `uncertainty-honest`: `agreement: 5/6 scored items`. pkg-14 was still rejected.
3. Partial, `--only pkg-14,pkg-03,pkg-09,pkg-04`, after narrowing `uncertainty-honest`: `agreement: 4/4 scored items`.
4. Full confirming run, saved with `--save-run` as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4.

Every run graded with Sonnet through the supplied harness. Claude Code wrote and revised the rubric, procedure and evidence guide and ran the harness. No classroom worksheet or group feedback is claimed; I wrote these from the "missed the activity" path.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174, `clear-accept`). Gold label: `accept`. My rubric said `reject` in run 1 and run 2, and `accept` in runs 3 and 4.

The plan fixes the reattach handshake. It names the code path ("the client attach/reattach path in `zellij-server`'s client connection handling") and commits to one change: drain pending OSC query responses before pane input is wired. It defers the Windows variant with a reason, and its test plan is five SSH reattach cycles with no rgb strings.

In run 1, `executable-by-stranger` failed it:

> "exact functions to be pinned in the PR after tracing the query issuance with debug logs"; only crate-level areas are named, so the location is left to the build.

My check asked for named files or functions. The grader read "pin the function later" as leaving the location undecided, which made it look like the real unbuildable plans (pkg-17's "gocui? tcell? not sure", pkg-18's "somewhere ... whichever is easier"). The difference is that pkg-14 has already decided both the place and the change; only the exact line is left for the build.

After I fixed that, run 2 failed it on `uncertainty-honest` instead:

> States 'the reattach-path change in 0.44.2' and fresh-attach query behavior as fact; neither is shown in the package and the plan does not flag the mechanism as unknown

That was my honesty check grading the diagnosis a second time. Whether the cause is grounded is `diagnosis-grounded`'s job, and that check had passed pkg-14 because every control fits the cause. In run 4 both checks pass, with the evidence "Names client attach/reattach path ... commits to draining pending OSC responses before pane input is wired" and "Flags Windows as untestable and the keystroke-drain risk; no guarantees, dates, or contradicted claims of work done".

**Check rationale**

Exactly as it reads in `tools/plan-check/rubric.md`:

```text
| uncertainty-honest | Claims in the plan and the plan comment that work was done or will succeed (tested, verified, confirmed, will fix, dates and timelines), read against what the repro evidence and thread show; the plan's risks or unknowns if any; Honesty in the evidence guide. | Fail only when (a) the plan or comment states a result, test, or verification that the package shows did not happen or that the package contradicts; (b) it promises a guaranteed outcome or a delivery date; or (c) a risk the package itself exposes (for example a case the repro shows behaving differently, or a platform the author says they cannot test) is presented as settled rather than flagged. Whether the stated cause is supported by the evidence is graded by diagnosis-grounded, not here: explanatory background in the diagnosis that the package neither shows nor contradicts does not fail this check. The author's own report of extra checks they ran counts as reported work unless the package contradicts it. Minor wording or attribution slips do not fail this check. Stated unknowns, risks, and conditional language pass, and a plan with no risks section passes when none of (a)-(c) applies. | required |
```

My first version said a plan failed when any claim was "not shown by the package". That rejected three good plans in run 1. pkg-03 was rejected for the author's "I have verified gzip, xz, and zstd locally", which is the author's own extra work, not a contradiction. pkg-09 was rejected for saying a collaborator called an option "simpler" when a different commenter had, which is a misattribution that has no bearing on the fix.

The second version still failed pkg-14, because the check was re-grading whether the diagnosis's background facts were shown. That is why the check now names exactly three ways to fail: a claimed result the package refutes, a guarantee or date, and a risk the package exposes being treated as settled. It also says outright that grounding the cause belongs to `diagnosis-grounded`. I rejected dropping the check entirely, because overclaiming ("I'll have the PR up this week", "tested" with no test) is one of the lecture's failure families, and pkg-04's comment still fails it.

**Trade-offs**

Narrowing the honesty check means it no longer catches some confident plans. In runs 1 and 2, pkg-16 (pandas, `wrong-cause`, a "long and confident" plan) failed both `diagnosis-grounded` and `uncertainty-honest`. In run 4 it failed only `diagnosis-grounded` and `thread-direction-engaged`. Its verdict is still `reject`, but the overconfident framing is no longer flagged on its own.

I checked for flips with canaries in the partial runs. pkg-16, pkg-17 and pkg-18 were in run 2, and pkg-04, the thread-convention canary, was in run 3. All stayed `reject`. Before narrowing the check I also confirmed from run 1 and run 2 results that no reject depended on `uncertainty-honest` alone: every one also failed a diagnosis, scope, executability, test-plan or thread check.

The case I accept this will miss is a plan whose cause is grounded, scope is bounded and test is decisive, but which states unshown background as fact. My rubric now accepts that plan. The full run is 20/20, but that only covers this set.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; my skill's files in `tools/plan-check/`.
