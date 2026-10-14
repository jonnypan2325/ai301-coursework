# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md`. Stop if the `Repo:` line is a
   placeholder or the issue is outside the scoped repo. Note the
   house rules.
2. Read `rubric.md` and `references/evidence-guide.md`. List the
   check names, their weights, and the verdict rule.
3. Read the package in this order. Read the plan before the PR so
   the description's framing cannot set your expectations of the
   diff:
   a. Repo facts (eval) or `.github/PULL_REQUEST_TEMPLATE.md`,
      `docs/CONTRIBUTING.md`, and the house rules (live). Write down
      every required section, checklist item, required artifact, and
      the AI-use policy, word for word.
   b. The issue and thread highlights (eval) or the live thread.
      Write down the failing input and any maintainer direction.
   c. The plan ("Plan context" or `plan.md`). Write down each planned
      change, the files it names, the not-in-scope list, every case
      in the test plan, and every deviation note.
   d. The diff and commits ("Diff" and "Commits", or
      `git diff main...HEAD`, `--stat`, and
      `git log --oneline main..HEAD`). List every changed file and
      summarize each hunk in one line.
   e. The title, description, and test evidence ("Candidate PR", or
      `pr_draft.md` and `test_evidence.md`).
4. Live mode only: read `voice-guide.md` last.

## Evidence gathering

1. diff-within-plan: put the hunk list from 3d next to the plan list
   from 3c. Mark each hunk "planned", "required artifact", "explained
   by deviation note", or "unplanned". Mark each planned change
   "present" or "missing", and for each missing one quote the note
   that discloses it, or record "no note".
2. description-matches-diff: list every factual claim in the title,
   description, and checklist about what the change does or
   contains. For each, quote the hunk that proves it, or record
   "not in diff".
3. repro-before-after: list the test plan's failure cases and its
   controls from 3c. For each failure case, quote the before and
   after result for that same input from the evidence (printed output
   or a specific note), or record what is missing: no result, a
   generic result ("works now"), a different input, or a control path
   only. For each control, record whether the evidence states it.
4. repo-checks-shown: list the repo's own test or check commands from
   3a and 3c. For each one the evidence mentions, quote its stated
   result (printed line, summary, or specific count). Record "never
   run" if none is mentioned, and record any checklist box ticked for
   a check the evidence says was not run.
5. diff-clean: scan every `+` and `-` line for the debris tells in the
   evidence guide. Quote each one found with its file. Before
   counting churn, check whether the lines sit inside the statement
   the fix edits; if so, they are diff noise, not debris.
6. template-and-policy: for each ask from 3a, quote where the
   description or diff meets it, or record "missing". Quote the AI
   disclosure sentence if the policy requires one, or record "no
   disclosure".
7. thread-direction: for each maintainer direction from 3b, quote
   where the description engages it, or record "not engaged".
8. Eval mode: gather only from the bundle; run nothing. Live mode:
   gather the candidate side only from `plan.md`, the committed diff,
   `pr_draft.md`, and `test_evidence.md`; gather the repo side with
   `gh` and the files named in 3a.

## Check execution

1. Grade the checks in rubric order, one at a time, from the evidence
   gathered for that check only.
2. Apply each pass condition literally and grade `pass` or `fail`,
   with one line quoting the fact that decided it (for a fail, the
   first offending hunk, claim, or missing item).
3. Grade `unclear` only when the evidence a check needs is absent
   from every place the evidence guide names. Say what was missing.
4. A check with nothing to apply to (no maintainer direction, no
   required disclosure) passes, with the evidence saying so.
5. Before failing for a shortfall, check the deviation notes and the
   description: a shortfall disclosed in both passes.
6. Do not let one check's result decide another's: a correct fix does
   not excuse debris, and decisive evidence does not excuse a
   missing template section.

## Verdict assembly

1. If every required check is `pass`, the verdict is `accept`. If any
   required check is `fail` or `unclear`, the verdict is `reject`.
   Preferred checks never change the verdict.
2. In the summary, name the deciding check: the first required check
   in rubric order that failed or was unclear, with its quoted
   evidence. For an accept, say all required checks passed.
3. Live mode: list each voice-guide rule the title or description
   breaks, quoting the rule. These never change the verdict.
4. Note any step of this procedure that did not fit the package.
5. End with the JSON block from SKILL.md, every rubric check in
   rubric order, and nothing after it.
