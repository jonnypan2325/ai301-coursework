---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this ready to submit?**
A PR package is a candidate pull request (its title, description,
commits, diff, and test evidence) read against the plan it claims to
implement (including that plan's deviation notes) and the issue the
plan belongs to, with the repo's stated standards (PR template,
contributing guide, AI-use policy) and the issue thread as context.

Do not grade the plan itself, the issue, or the code's general
quality outside this change, and do not grade more than one package
per run. Do not answer from gut feel: execute `procedure.md`, which
applies the checks in `rubric.md` to evidence located with
`references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Read
these, and nothing else, as the candidate side:

- `plan.md` in the working directory, including its `## Deviations`
  section. A house-chain student reads the house plan instead.
- The branch's diff: run `git diff main...HEAD` (three dots) from the
  working copy, plus `git diff main...HEAD --stat` and
  `git log --oneline main..HEAD` for the file list and commits. Only
  committed changes count; uncommitted work is not part of the PR.
- `pr_draft.md`: the PR title on its first line, then the filled PR
  template as the description.
- `test_evidence.md`: the before and after re-runs and the repo's
  check outputs.
- The issue, from the URL the student gives: read its body and thread
  with `gh issue view <n> --comments` (or the GitHub API), plus the
  student's posted repro and plan comments there. A house-chain
  student reads the house repro pack instead.
- The repo's stated standards: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy.

Read the drafts the way a maintainer will read the opened PR: the
package is what the drafts and the diff contain, not other files in
the working directory.

**Eval mode** grades a package bundle (a markdown file with repo
facts, the issue, thread highlights, plan context, and the candidate
PR's title, description, commits, diff, and test evidence). The
bundle is the whole world: take every fact from the bundle text,
fetch nothing, run no commands, and read no other file. Eval mode
always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If its `Repo:`
line still holds the `<ORG>/<PATH-REVIEW-REPO>` placeholder, stop
without grading and tell the student to fill the `Repo:` line in
`scope.md` with their section's Path Review repo. If the issue URL is
not in the scoped repo, refuse to grade and say why. Never guess a
scope. Apply the house rules `scope.md` lists (branch naming, one PR
per issue per student, the template always used, classmates' PRs not
blocking) as stated standards when grading. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the PR
title and description in `pr_draft.md` against each rule. Report every
rule the draft breaks in the summary, quoting the rule and the
offending text. The voice guide is personal and carries no gold
labels, so it never changes the verdict on its own; only a rubric
check that names it can. In eval mode, ignore `voice-guide.md`
entirely.

## Component reads

- `rubric.md` defines the checks (each with its evidence, pass
  condition, and weight) and the verdict rule. Grade every check it
  lists, by name, and nothing else.
- `references/evidence-guide.md` says where each evidence family lives
  in a bundle and in a live PR, and what good looks like there.
- `procedure.md` is the operating procedure. Execute it as written, in
  order: read order, evidence gathering, check execution, verdict
  assembly.

Where the procedure is silent on a step you need, do not improvise
around it: take the most literal reading of the rubric, and report
the gap in the summary so the student can close it.

If `rubric.md` has no checks or no verdict rule, or `procedure.md` has
no steps, refuse to grade: say which file is empty and stop. A tool
that invents checks at runtime produces noise that looks like
judgment.

## Verdict and output

The verdict is binary: `accept` means the PR is ready to submit;
`reject` means hold it. There is no third verdict and no score;
reservations go in check evidence lines.

Before the JSON you may print a short readable summary: one line per
check, the deciding check for the verdict, any procedure gaps, and in
live mode any voice-guide notes. Then end the reply with this fenced
JSON block, valid, listing every rubric check in rubric order, with
nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff itself against the
  plan, and the evidence itself against the test plan. A terse
  complete PR can be ready; a polished confident description can hide
  drift.
- The rubric decides: if a check passes by its stated condition but
  feels wrong, it still passes. Note the tension in the summary.
- The procedure decides how: follow `procedure.md` as written and
  report its gaps instead of inventing steps.
- Unclear: apply the rubric's verdict rule. If the rule is silent,
  treat `unclear` as `fail`, because a claim you cannot verify from
  the package is not ready to submit.
- Honest shortfalls are not drift: a PR that does less than the plan
  and says so in the plan's deviation notes and the description can
  be ready.
