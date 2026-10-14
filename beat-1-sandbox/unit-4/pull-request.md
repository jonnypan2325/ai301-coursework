# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request I opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/239

**Branch**

fix/60-null-chunk-text

## Eval iterations

**Run history**

1. Full run, first version of my files: `agreement: 13/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`. Every reject agreed, but all seven `clear-accept` packages were rejected.
2. Partial, `--only pkg-02,pkg-05,pkg-08,pkg-11,pkg-13,pkg-16,pkg-19,pkg-04,pkg-10,pkg-17,pkg-18`, after loosening four checks: `agreement: 11/11 scored items`. The seven accepts plus four canaries.
3. Full confirming run, saved with `--save-run` as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3.

Every run graded with Sonnet through the supplied harness. Claude Code wrote and revised my SKILL.md, rubric, procedure and evidence guide from my group's Unit 4 worksheet, and ran the harness.

**Package analysis**

`pkg-11` (BurntSushi/ripgrep#3477, `clear-accept`). Gold label: `accept`. My rubric said `reject` in run 1 and `accept` in runs 2 and 3.

The PR stops the gitignore trailing-space trim at an escaped space. Its test evidence is a re-run of the issue's input with the results written as `#` notes, not printed output:

```text
$ rg hi                      # before: rg: ./.gitignore: line 1: error
                             # parsing glob 'foo\': dangling '\'
$ rg hi                      # after: searches normally, no error
```

followed by "`cargo test -p ignore` passes (214 tests)".

In run 1, my rubric failed it on three checks. `repro-before-after` said:

> Before/after results are '#' comment descriptions ('after: searches normally, no error'), not printed command output.

`repo-checks-shown` said:

> 'cargo test -p ignore passes (214 tests)' is a claim with no printed command output or summary line.

`description-matches-diff` said the added test had "no plain unescaped trailing-space case".

My first rubric followed our worksheet's rule for calib-03, where "a note after # saying what it printed doesn't count". Applied to this set, though, that rule failed four of the seven clear accepts on this check alone. Those gold accepts report a specific result for the exact failing input, even when it is written as a note or a summary. What the not-tested rejects lack is specificity: pkg-04 says "colors work now", pkg-10 says "verified working on my machine for a full day", and pkg-07 and pkg-14 run a control or an unchanged path.

After the revision, run 3 passes pkg-11 with:

> Same input printf 'foo\\   \n': before 'dangling \'' error, after searches normally and 'foo ' not listed.

The unescaped case is asserted inside the same added test, so the description's claim holds.

**Check rationale**

Exactly as it reads in `tools/pr-precheck/rubric.md`:

```text
| repro-before-after | The test evidence, read against the plan's test plan and the issue's reproduction (its failing input, command, and symptom); Test evidence in the evidence guide. | For the issue's failing input, the evidence states what happened before the fix and what happens after it, as printed output or as a specific observation of that same input (for example "before: parse error on line 3; after: matches"). Results written as notes or a summary table count when they are specific to the input. Every distinct failure case the plan's test plan names is re-run the same way, unless the plan's deviation notes and the description say it was deferred and why; controls the plan expects to stay unchanged may simply be stated as unchanged. Fail when the evidence is generic ("tested locally", "works now", "verified on my machine for a day", "colors work now"), re-runs only a control or a path the fix does not change, or leaves out a failure case the plan named with no disclosure. | required |
```

The first version required "printed output (command plus what it printed)" and failed anything "described in a `#` note". That made the check reject four of the seven accepts (pkg-05, pkg-08, pkg-11 and pkg-13).

I kept two things from it, because they are what separate the four not-tested packages. First, the result has to be about the same input the issue fails on: pkg-07 and pkg-14 re-run a control or an unchanged path. Second, every failure case the plan names has to be covered: calib-04 re-runs only the first of two crashes. What I changed is what counts as a result. A specific observation of that input now counts, and a generic one ("works now", "verified on my machine") does not.

I rejected moving this check to `preferred`. pkg-04 and pkg-07 fail only this check in run 3, so without it both would be accepted.

**Trade-offs**

The check now trusts a specific written result without seeing the terminal. A PR that writes a plausible but invented "before: error X; after: works" note for the right input would pass `repro-before-after`. I accept that miss, because the eval package can't show whether a note was really observed.

To make sure loosening didn't flip the rejects, I re-ran canaries in run 2: pkg-04 and pkg-10 (not-tested), pkg-17 (silent drift, where the docs claim matters) and pkg-18 (unreviewable, the arguable churn package). All four stayed `reject`. In run 3, all four not-tested packages still fail `repro-before-after` for the right reason. For example, pkg-07 got: "Evidence only runs the single-file control ... the two-file a.go/b.go failing repro from the plan's test plan has no before/after and no disclosed deferral."

---

Related paths: `eval-run.txt` in this directory; my skill's files in `tools/pr-precheck/`.
