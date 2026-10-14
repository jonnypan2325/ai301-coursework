# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives (eval bundle): the cause is in the candidate plan's
  "Diagnosis" line or first paragraph. The behavior it must explain is
  in the "Repro evidence" block: the numbered steps, the artifact
  (output, traceback, timing), and especially every "Control" run.
  Maintainer analysis is in "Thread highlights".
- Where it lives (live): the cause is in the draft `plan.md`'s
  diagnosis section. The evidence is the student's posted repro
  comment on the issue (or the house repro pack quoted in the drafts),
  read from the issue thread with `gh issue view <n> --comments`.
- What good looks like: the cause names a mechanism (a line, a call,
  a value) and every control run fits it. If a control shows the
  blamed component working, or shows the failure before the blamed
  step runs, the diagnosis is contradicted, however confident the
  plan or the thread sounds.

## Scope

- Where it lives: the plan's "Change", "Scope", "In scope" and
  "Not in scope" lines, and its numbered approach steps. Compare them
  with the issue title and body: that is what the fix was asked for.
- What good looks like: one change that removes the reproduced
  failure, plus its tests, plus the same defect at sibling sites on
  the same code path. A stated deferral with a reason is fine. A
  drive-by rewrite looks like extra fronts the issue never mentions:
  migrations, dependency upgrades, new options or settings, UI or
  module restructures, CI or harness changes.

## Executability

- Where it lives: the plan's "Files" line, file paths or function
  names inside the change or approach, and the numbered approach
  steps.
- What good looks like: a stranger can go to the named file, function,
  or code path and start the chosen change without asking which layer,
  which library, or which of two options. Naming the code path and the
  change, then pinning the exact line during the build, is enough.
  Hedges such as "somewhere", "not sure", "whichever is easier", or
  "profile first" mean the decision has not been made.

## Test plan

- Where it lives: the plan's "Test plan" section, read against the
  repro evidence's command, inputs, and failure artifact.
- What good looks like: a check that fails today and passes after
  the fix, with the expected observable result stated: the repro
  command re-run with the new expected output or exit code, a named
  test that flips, or a regression test built from the repro inputs.
  "Run the whole suite" or "nothing should break" proves no fix.

## Honesty

- Where it lives: "Risk", "Unknowns", or "Open question" lines in the
  plan; words like "verified", "confirmed", "will", dates and
  timelines in the plan and the comment. Live: the `## Deviations`
  heading at the end of `plan.md` is where a mid-build change gets
  recorded.
- What good looks like: anything the plan says was tested, verified,
  or will work is either shown by the package or phrased as a plan,
  an unknown, or a risk. False confidence is a result the package
  shows did not happen, a guaranteed fix, a delivery date, or a risk
  the package exposes being treated as settled. Whether the cause
  itself is grounded belongs to Diagnosis and grounding, not here. An
  author's report of their own extra checks counts as reported work
  unless the package contradicts it. A plan without a risks section
  is honest if it overclaims nothing.

## Comms

- Where it lives (eval bundle): the "Candidate plan comment", read
  against "Thread highlights" (look for OWNER, MEMBER, COLLABORATOR,
  or CONTRIBUTOR comments naming a culprit, approach, rejected idea,
  patch, PR, or request) and against the "Repo facts" block's
  "contribution policy" line (AI-use disclosure, human-written
  comment rules) and bug-report asks.
- Where it lives (live): the draft `comment.md` read against the live
  thread on the issue, the repo's CONTRIBUTING and any AI policy, and
  the house rules in `scope.md`.
- What good looks like: thread-aware means the comment names the
  maintainer's direction or the open PR and says how the plan follows
  it or why it departs. Boilerplate restates the plan without the
  thread. When the policy requires AI disclosure, the comment says
  which tool was used and how much it did.
