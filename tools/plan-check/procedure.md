# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md`. Confirm the issue URL is in the
   scoped repo; if not, or if the Repo line is a placeholder, stop
   and say so. Note the house rules.
2. Read `rubric.md` and `references/evidence-guide.md`. List the
   seven check names and the verdict rule before reading the package.
3. Read the package in this order, and do not read the candidate plan
   until steps a-c are done, so the plan's framing cannot steer how
   the evidence is read:
   a. Repo facts (eval) or the repo's CONTRIBUTING and AI policy
      (live). Write down any AI-disclosure requirement and any
      human-written-comment rule, word for word.
   b. The issue and the thread highlights (eval) or the live thread.
      Write down every maintainer, owner, member, or collaborator
      direction, rejected approach, patch, prior art, and open PR.
   c. The repro evidence. Write down the failing command and its
      artifact, and each control run with its result. Note what each
      control rules out.
   d. The candidate plan. Write down its stated cause, its in-scope
      and not-in-scope items, files, approach steps, test plan, and
      risks.
   e. The candidate plan comment.
4. Live mode only: read `voice-guide.md` last.

## Evidence gathering

1. For diagnosis-grounded: put the plan's cause next to the notes
   from step 3c. For each control run, record one line: "fits" or
   "rules out", with the quoted result.
2. For scope-bounded: list every distinct change the plan proposes.
   Mark each one "fixes or tests the reproduced bug" or "not asked
   for by the issue", quoting the plan's words.
3. For executable-by-stranger: quote the named file or function and
   the chosen approach. If either is missing or hedged, quote the
   hedge.
4. For test-plan-decisive: quote the test plan, then record the
   expected outcome it names and whether that outcome would differ
   before and after the fix.
5. For uncertainty-honest: quote every certainty claim, date, or
   guarantee in the plan and comment, and record whether the package
   supports it.
6. For thread-direction-engaged: take the direction list from step
   3b. For each item, quote where the comment or plan engages it, or
   record "not engaged".
7. For repo-conventions-disclosure: take the policy notes from step
   3a, then quote the comment's disclosure sentence, or record "no
   disclosure".
8. Eval mode: gather only from the bundle text; fetch nothing. Live
   mode: gather issue-side facts with `gh` or the web, and the
   candidate side only from the drafts.

## Check execution

1. Grade the checks in rubric order, one at a time, using only the
   evidence gathered for that check.
2. Apply the rubric's pass condition literally. Grade `pass` or
   `fail`, with one line naming the fact or quote that decided it.
3. Grade `unclear` only when the evidence the check needs is truly
   absent from the package after looking in every place the evidence
   guide names. Write what was missing.
4. A check whose pass condition says "when the thread contains no
   such direction, this check passes" (or that has no applicable
   policy) is graded `pass`, with the evidence "no direction in
   thread" or "no disclosure requirement in repo facts".
5. Do not let one check's result change another's: a correct core
   fix does not rescue scope, and a polished comment does not rescue
   a contradicted diagnosis.
6. Only re-read the package when a check's gathered evidence is
   missing a quote; otherwise grade from the notes.

## Verdict assembly

1. Count the grades. If every check is `pass`, the verdict is
   `accept`. If any check is `fail` or `unclear`, the verdict is
   `reject`.
2. In the readable summary, name the deciding check: the first failed
   or unclear check in rubric order, with its quoted evidence. For an
   accept, say that all checks passed.
3. Live mode: list voice-guide rules the draft comment breaks,
   quoting each rule. They do not change the verdict.
4. Note any step of this procedure that did not fit the package.
5. Emit the JSON block in the format SKILL.md gives, with all seven
   checks in rubric order, as the last thing in the output.
