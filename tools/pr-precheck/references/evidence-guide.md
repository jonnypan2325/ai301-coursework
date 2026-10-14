# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- In a bundle: the plan lives in "Plan context" (its change, files,
  in-scope and not-in-scope lines, test plan, and any deviation
  notes). What changed lives in "Diff" (every `--- a/` / `+++ b/`
  file header and `@@` hunk) and "Commits". The fidelity claims live
  in the candidate PR's "Title" and "Description", including any
  checklist lines.
- In my own PR: the plan is `plan.md` (its Scope, Files, Approach,
  and `## Deviations`); the diff is `git diff main...HEAD` and
  `git diff main...HEAD --stat`; the claims are in `pr_draft.md`.
- What good looks like: every file in the diff is one the plan names
  (or its folder, its test, or an artifact the template requires),
  every hunk in that file is the planned change, and every planned
  change shows up. A mismatch is fine only when a deviation note
  explains it and the description repeats it. Silent drift goes both
  ways: more than the plan (a rewrite, a new option or flag, a rename
  pass) or less than the plan (a promised docs change or test that
  is not in the diff), especially under a description that says
  "exactly as planned" or "no changes beyond".

## Test evidence (harness category: not-tested)

- In a bundle: the "Test evidence" section (often inside the
  description), read against the plan's "Test plan" and the issue's
  reproduction steps; any test files in the "Diff".
- In my own PR: `test_evidence.md` (the before on `main`, the after
  on the branch, and the `make test-unit`, `make test-integration`,
  `make lint`, `make typecheck` output), read against `plan.md`'s Test
  plan and my Unit 2 repro comment; the Testing section of
  `pr_draft.md`.
- What good looks like: the same input or command that failed in the
  issue is run before and after, and the result of each is stated
  specifically: printed output, or a precise note about that input
  ("before: dangling backslash parse error; after: matches"). Every
  failure case the test plan names is re-run, or its deferral is
  disclosed; controls may just be stated as unchanged. The repo's own
  test command appears with its result (a printed summary line such
  as `19 passed, 3 xfailed`, or a specific count). "Works now",
  "verified on my machine", or a run of a control or unchanged path
  proves nothing about the fix.

## Diff quality (harness category: unreviewable)

- In a bundle: every `+` and `-` line in "Diff", and the "Commits"
  list.
- In my own PR: `git diff main...HEAD` line by line, and
  `git log --oneline main..HEAD`.
- What good looks like: a reviewer sees only the fix, its test, and
  required artifacts. Debris tells: `print`/`eprintln`/`console.log`
  or debug logging, commented-out code or an earlier attempt left in,
  an unused helper (often behind `allow(dead_code)` or `noqa`), a new
  `TODO`, and whole hunks or blocks of re-indenting, import
  restructuring, or identical removed-and-re-added lines away from
  the fix. A few unchanged lines re-added inside the statement the fix
  edits are diff noise, not debris.

## Standards and comms (harness category: standards-wall)

- In a bundle: the "Repo facts" block's "pull requests" line (the
  template's required sections and checklist, required changelog or
  whatsnew entries) and "contribution policy" line (AI-use
  disclosure rules); "Thread highlights" for maintainer direction;
  the description for whether each ask is met; the diff for whether
  required artifacts exist.
- In my own PR: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue
  with `Closes #`, Changes, Testing checklist, Screenshots / Demo,
  Notes for Reviewers), `docs/CONTRIBUTING.md` (branch naming,
  Conventional Commits, removing the issue's `xfail` marker), the
  house rules in `scope.md`, the course's AI-use disclosure in Notes
  for Reviewers, and the live issue thread.
- What good looks like: every section the template asks for has real
  content, checklist boxes are ticked only when true, the issue is
  linked the way the template asks, any required entry file is in the
  diff, and AI use is disclosed where the policy requires it, with
  the tool and extent if the policy asks. Boilerplate left
  unfilled, a missing checklist, or a missing required disclosure is
  the wall. (Whether description claims match the diff belongs to
  plan fidelity, above.)
