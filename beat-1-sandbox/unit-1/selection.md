# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.


All three are in the scoped repo (`codepath/pathreview-ai301-fa26-s3`). Repo facts gathered live on 2026-09-23: not archived, last push 2026-09-16 (7 days), last 5 default-branch commits all by human collaborator `Aburke225`, no AI ban in `docs/CONTRIBUTING.md`, root, or the PR template. Per the Path Review house rule, classmate claim comments (all `author_association: NONE`) do not block.

**Accepted, in fit order:**

**1. #60 — Faithfulness checker crashes on `text: None`** — the only one shipping a runnable reproduction snippet with the observed `TypeError`, and it sits in the RAG *evaluator*, matching the evaluation/agent-behavior interest most directly.
- Issue fresh: pass — repo not archived, pushed 2026-09-16
- Maintainer alive: pass — `Aburke225` (COLLABORATOR) committed 2026-09-16, 7 days ago
- Active contribution: pass — no assignee, no linked PR; lone claim by `pkmaster21` is a classmate (house rule)
- Bounded non-breaking scope: pass — one `.get()` null-handling fix in `faithfulness_checker.py`, `good first issue` + `tier-1`, opened by a COLLABORATOR
- AI-assisted allowed: pass — no AI restriction anywhere in the contribution docs
- Reproduction clarity: pass (preferred) — copy-pasteable repro plus named failing test
- Backend/agentic fit: pass (preferred) — RAG evaluation logic

**2. #69 — Output parser crashes on top-level JSON array** — unexpected LLM output shape in a fallback path; hits the "unexpected agent behavior" interest, with a named xfail test as the definition of done.
- Issue fresh: pass · Maintainer alive: pass · AI-assisted allowed: pass (same repo evidence)
- Active contribution: pass — no assignee, no linked PR; two claims (`Yina-Mu`, `tonybuii2003`) are classmates
- Bounded non-breaking scope: pass — two named files, "Estimated effort: 2–4 hours", xfail marker H-02 to remove
- Reproduction clarity: pass (preferred) — exact `AttributeError` and covering test named
- Backend/agentic fit: pass (preferred) — LLM output parsing / data flow

**3. #68 — Keyword search `ZeroDivisionError` on empty index** — same clean shape, but retrieval plumbing is the least agentic of the three.
- Issue fresh: pass · Maintainer alive: pass · AI-assisted allowed: pass
- Active contribution: pass — no assignee, no linked PR; `acordero4852`'s claim is a classmate
- Bounded non-breaking scope: pass — guard `index()` on empty corpus, mirroring existing `search()` behavior; xfail H-01
- Reproduction clarity: pass (preferred) — expected behavior stated explicitly
- Backend/agentic fit: pass (preferred) — server-side retrieval reliability

No rejections. One note on the rubric, not on these runs: every check here passed on repo-level facts shared by all three candidates, so the rubric did no discriminating between them — the ranking came entirely from the fit profile.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Repo archived:false, pushedAt 2026-09-16 (7 days before 2026-09-23 capture)"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot commit by Aburke225 (COLLABORATOR) on 2026-09-16, within 90 days"},
      {"name": "Active contribution", "grade": "pass", "evidence": "assignees: [], no linked PR in timeline; only claim is pkmaster21 (author_association NONE = classmate, house rule)"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Single null-handling fix in rag/evaluator/faithfulness_checker.py; labels bug/good first issue/tier-1; opened by COLLABORATOR"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "No AI ban in docs/CONTRIBUTING.md, repo root, or .github/PULL_REQUEST_TEMPLATE.md — silence passes"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Body has runnable repro: FaithfulnessChecker().check('Knows Python.', [{'text': None}]) -> observed TypeError, plus test_none_context_chunk_text"},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "RAG faithfulness evaluator — evaluation logic in the agentic system"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Repo archived:false, pushedAt 2026-09-16 (7 days before 2026-09-23 capture)"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot commit by Aburke225 (COLLABORATOR) on 2026-09-16, within 90 days"},
      {"name": "Active contribution", "grade": "pass", "evidence": "assignees: [], no linked PR in timeline; claims by Yina-Mu and tonybuii2003 both author_association NONE (classmates, house rule)"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Fallback path fix in rag/generator/output_parser.py, 'Estimated effort: 2-4 hours', xfail H-02 to remove"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "No AI ban in docs/CONTRIBUTING.md, repo root, or .github/PULL_REQUEST_TEMPLATE.md — silence passes"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Names exact failure \"AttributeError: 'list' object has no attribute 'items'\" and covering test in tests/unit/test_output_parser.py"},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "LLM output parsing / data flow — unexpected agent output handling"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Repo archived:false, pushedAt 2026-09-16 (7 days before 2026-09-23 capture)"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot commit by Aburke225 (COLLABORATOR) on 2026-09-16, within 90 days"},
      {"name": "Active contribution", "grade": "pass", "evidence": "assignees: [], no linked PR in timeline; only claim is acordero4852 (author_association NONE = classmate, house rule)"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Guard index() on empty corpus in rag/retriever/keyword_search.py, 'Estimated effort: 2-4 hours', xfail H-01 to remove"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "No AI ban in docs/CONTRIBUTING.md, repo root, or .github/PULL_REQUEST_TEMPLATE.md — silence passes"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "States index([]) raises ZeroDivisionError inside rank-bm25 and that index() should mirror search()'s empty-case handling"},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "Server-side retrieval reliability in the RAG retriever"}
    ],
    "verdict": "accept"
  }
]
```




---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

agreement: 2/3 scored items
agreement: 1/1 scored items
agreement: 3/5 scored items
agreement: 2/2 scored items
agreement: 19/20 scored items  (bar: 18/20: PASS)

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

For issue-01, the final run recorded:

`issue-01  accept  reject   NO     failed: Bounded non-breaking scope, Backend or agentic fit (preferred)`

My rubric decided reject, while the gold label was accept. The required Bounded non-breaking scope check caused the rejection; Backend or agentic fit was preferred and therefore could not change the verdict. The issue requested a new permanent documentation page plus updates to several related pages and an optional troubleshooting entry. The rubric interpreted that breadth as insufficiently bounded, while the gold label treated those related documentation changes as one coherent task with a stated destination and outcome.

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

My current rubric contains this check:

`| Bounded non-breaking scope | The issue body, labels, opening date, author association, comment thread, requested deliverables, affected components, maintainer scope statements, and linked pull-request history. | Pass if the issue describes one coherent outcome whose completion can be observed or tested. Also pass a terse issue when it was opened by an Owner, Member, or Collaborator or carries a good-first-issue label and names a specific missing or incorrect behavior with finite examples, unless later evidence contradicts that scope. The work may touch multiple related files, list multiple implementation approaches, or include optional follow-up work. Fail if it is a usage question, an umbrella or tracking issue, or an open-ended codebase-wide change. Fail if a required product decision such as behavior, semantics, interface design, or an asset remains unsettled or marked TBD. Fail when an issue has been open for more than two years with at least two closed unmerged pull requests or repeated abandoned attempts, unless a maintainer clarified a current implementation path within the last 180 days. Also fail if it requires a large architectural rewrite or explicitly requires a breaking change to a public API, schema, configuration format, or other established interface. | required |`

I wrote the check this way because my early version rejected terse but valid maintainer-filed bugs and treated every multi-file task as too broad. The current wording allows finite maintainer-defined bugs, related file changes, and alternative implementation approaches while still rejecting umbrella issues, unresolved product decisions, repeated abandoned attempts, architectural rewrites, and breaking changes.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

The canary runs included these results:

`issue-04  accept  accept   yes`
`issue-15  reject  reject   yes`

The revised check allows issue-04, even though its description is very short, because a collaborator opened it, applied a good first issue label, and named finite examples of the missing behavior. The trade-off is that I accept some risk that a terse maintainer-filed issue may contain unstated complexity. Conversely, the age-and-abandoned-attempt threshold rejects issue-15; that can exclude an old issue that may eventually become feasible, but I prefer that false rejection over choosing a first contribution with years of unresolved history.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]


1. Issue #60 fits my interest in backend and agentic-system bugs because it affects a RAG faithfulness evaluator. It has a runnable Python reproduction, a named failing test, and a small null-handling fix, so it appears manageable within the available time without requiring a breaking interface change.

2. The verdict correctly identified the active repository, permitted AI-assisted workflow, absence of an assignee or linked pull request, reproducible TypeError, bounded fix, and direct fit with evaluation work. Beyond the rubric, I weighed the quality of the reproduction and my interest in evaluation more heavily than the presence of parallel classmate activity.

3. I expect claiming it to be moderately easy. Other classmates have shown interest, but the Path Review house rule permits parallel work and ties course credit to the pull request. The main challenge will be writing a clear claim comment that states my intended reproduction and test plan without implying that other students must stop working on it.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
