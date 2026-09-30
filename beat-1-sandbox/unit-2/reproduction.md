# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

> The skill passed the course evaluation 20/20. Both comments are posted, and their permalinks and exact text are recorded below. The reproduction was confirmed from the fork after the claim; the pre-publication report received an accept verdict from the live skill check.

## Your identity upstream

**GitHub username**

jonnypan2325

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5905217025

I'm investigating the crash in `FaithfulnessChecker.check()` when a context chunk contains `text: None`.

I've already completed a local preparation run. Next, I'll do the following:

1. Verify the issue's reproduction snippet and `test_none_context_chunk_text` from my fork.
2. Compare the behavior when the `text` key is missing.
3. Post my environment, commands, and results before proposing a change.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5905342853

Confirmed from my fork after posting my claim: the reported `TypeError` occurs on unchanged commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.

**Environment:** macOS 26.6.2 (arm64), Python 3.11.16, structlog 26.1.0, pytest 9.1.1. Recorded September 29, 2026, at 23:10 PDT.

**Reproduce from a fresh checkout:**

```bash
git clone https://github.com/jonnypan2325/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
python3.11 -m venv .venv
.venv/bin/python -m pip install 'structlog==26.1.0' 'pytest==9.1.1'
.venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

**Expected / acceptance criterion:** Handle a `text: None` chunk without raising and return a float in `[0, 1]`, as asserted by the existing `test_none_context_chunk_text`.

**Observed:** The issue's exact snippet exits with status 1. Relevant traceback (local path prefix omitted):

```text
  File "rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
```

**Checks run:**

```bash
# Control: missing text key
.venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'content': 'Python skills'}]))"
# Control: string text
.venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print('score =', FaithfulnessChecker().check('Knows Python.', [{'text': 'Python skills'}]))"
# Existing issue test: normal marker, then bypass it to expose the failure
.venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text -rx
.venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text --runxfail
# Existing missing-key test and the complete evaluator test file
.venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_missing_text_key_in_chunk
.venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -q -m unit
```

| Check | Recorded result |
|---|---|
| Missing-key snippet | `score = 0.0`, exit 0 |
| String-text snippet | `score = 0.0`, exit 0 |
| Named issue test | `21 deselected, 1 xfailed`, exit 0 |
| Named issue test with `--runxfail` | `1 failed, 21 deselected`, same `TypeError`, exit 1 |
| Existing missing-key test | `1 passed, 21 deselected`, exit 0 |
| Complete evaluator test file | `18 passed, 4 xfailed`, exit 0 |

**Dev notes / scope:** `.get("text", "")` supplies the default for an absent key, but a present `None` reaches the string join. The controls distinguish those inputs. XFAIL is the existing expected-failure marker, not a successful fix. No application code or test markers were changed. The other three XFAILs in this file refer to issue #59 and are outside this report.

The setup is intentionally the isolated evaluator portion of the repository's Python environment: its declared `structlog` dependency and `pytest` dev dependency. The module imports only `re` and `structlog`, and its test fixtures do not require services. The full `make setup` sequence, database/migrations, API, frontend/Jest, integration tests, lint and type checks were not run. This is a nonvisual Python failure, so terminal evidence is used instead of screenshots/video.

**AI assistance:** I ran the reproduction commands and tests locally and verified the results. Codex assisted with the reproduction setup and drafting this report.

## Eval iterations

**Run history**

The actual runs, in order (agreement lines copied from the recorded outputs):

1. Initial targeted run: `pkg-01,pkg-02,pkg-04,pkg-06,pkg-09,pkg-20`.

   `agreement: 5/6 scored items`

2. After clarifying the course AI-assistance premise, targeted disclosure/policy checks: `pkg-05,pkg-07,pkg-20`.

   `agreement: 3/3 scored items`

3. Confirming full run of all 20 scored packages with the final rubric and evidence guide, saved by the original harness in `eval-run.txt`.

   `agreement: 20/20 scored items  (bar: 18/20: PASS)`

The first two runs were partial and did not establish the category floor. The final complete run matched at least one item in every category. Claude Sonnet performed the grading through the supplied harness; Codex operated the commands and helped draft/revise the components. No classroom rubric-swap feedback is claimed.

**Package analysis**

My rubric initially returned `accept` for `pkg-20` (Ghostty). The instructor's expected answer (gold label) was `reject`. The reproduction had the required technical details, but the comments were missing the AI disclosure required by the repo. Claude accepted them with this reasoning:

> Provides version, configuration, and platform as the bug template requests; no AI use is claimed, so no disclosure is owed.

The course notes say these samples are AI-assisted. The updated rule makes that explicit and requires the tool name and how it was used when the repo asks for both. In the final run, `pkg-20` returned `reject`, matching the gold label:

> repo facts require AI usage disclosed with tool and extent; neither the claim comment nor the repro report contains any such disclosure

**Check rationale**

Exact check from my final rubric:

```text
| repository-conventions | Repo-facts template/contribution/AI-use policy against both comments; live mode also the actual contribution instructions and personal voice guide. | Follows explicit applicable repository requirements, including AI-use disclosure when required. In this course eval, treat candidate comments/reports as AI-assisted work (the instructor gold-label notes establish this premise); do not infer that missing disclosure means no AI was used. If the captured policy requires disclosure, require an explicit disclosure that supplies the tool and extent when that policy asks for them; missing disclosure fails even when the technical report is excellent. An explicit compliant disclosure satisfies this part. Permissive-with-responsibility policies and silence about AI create no additional disclosure requirement. Read new-issue administrative requests (such as confirming a duplicate search) in context: a reproduction comment on an existing issue need not repeat new-issue checkboxes unless the policy explicitly requires them for comments. Required reproduction information may appear in any readable format unless the repository explicitly mandates a format. In live mode, also follows the personal voice rules; in eval mode do not consult that guide. Apply Path Review's parallel-claim rules only in live mode for the scoped classroom repo. | required |
```

The original rule required disclosure when the repo asked for it, but didn't explicitly say the course samples were AI-assisted. That gap let Claude excuse the missing disclosure.

The updated check closes that gap. It requires disclosure only when the repo requires it, and applies issue-template requirements to comments only when those requirements cover comments. Since this check is `required`, missing a required disclosure rejects the report even if the reproduction is complete.

**Trade-offs**

A useful reproduction can still be rejected for missing a required disclosure. That's intentional, but the check also needs to accept reports that follow the repo's policy. The targeted re-run checked three cases:

- `pkg-07`: includes AI disclosure. Stayed `accept`.
- `pkg-05`: repo policy doesn't require disclosure. Stayed `accept`.
- `pkg-20`: missing required disclosure. Changed to `reject`.

The final full run matched all 20 expected answers. That supports the change for this test set; it doesn't cover every repo policy. Treating the samples as AI-assisted is specific to this course evaluation and shouldn't be assumed for unrelated GitHub comments.

---

Full evaluation output: [eval-run.txt](eval-run.txt), generated by the supplied harness. Both posted comments and their links are recorded above. Reproduction from my fork is complete.
