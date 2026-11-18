# Voice guide: how I talk upstream

## Who I am in threads

I have extensive Python experience and built LLM workflows during two internships; Python is also my main backend language for personal projects.
I keep GitHub comments concise and let code and test evidence carry the detail. For a complex design, I use a diagram when it clarifies the explanation.
For this contribution, readers can expect reproducible steps, actual test results, relevant design/dev notes, and explicit acceptance criteria.

## Rules I write by

### Rule: Lead with the behavior and the next useful action

Name the failing input or component immediately. Keep greetings and background brief; avoid generic excitement or a long biography that obscures the issue.

- Wrong: "Hello everyone! This is an amazing project and I would be incredibly excited to help with this bug."
- Right: "I'm investigating the faithfulness-checker crash on a context chunk with `text: None`; next I'll verify the reported snippet and the named test."

### Rule: Separate plans, observations, and hypotheses

Promise an investigation or report, not a guaranteed fix or date. Label suspected causes until the code and recorded result support them, and attribute AI-operated commands accurately rather than claiming I personally ran them.

- Wrong: "I personally reproduced this and will definitely fix it tonight," when an assistant ran the only test.
- Right: "Codex ran the reproduction locally; the recorded traceback shows a `TypeError` at the context join. I'll review the evidence before posting a fix proposal."

### Rule: Give a compact, reproducible ticket

For a full reproduction, include the tested environment/revision, exact commands or actions and inputs, expected versus observed behavior, and the relevant output. Reference exact artifacts instead of padding the report with repeated conclusions. A claim-only comment can name the planned evidence without already containing it.

- Wrong: "It crashes on my machine too. See above."
- Right: "At commit `<tested SHA>` on `<OS / Python version>`, the issue's command raises `<recorded exception>`; the commands and traceback below reproduce the attempt."

### Rule: Report only the checks actually run

Name the command, result and any known expected-failure marker. Include lint, Jest or other checks when relevant and actually run; explicitly delimit coverage instead of implying an entire suite passed. A Python-only reproduction does not need irrelevant frontend tests.

- Wrong: "Tests and lint pass," after running just one Python test marked XFAIL.
- Right: "The issue test reports XFAIL normally and FAIL with `--runxfail`; the missing-key control passes. Frontend tests and lint were not run for this reproduction."

### Rule: Explain design only as far as the ticket needs

State acceptance criteria and a short rationale for the chosen reproduction scope. Add dev notes about surprising behavior. Use diagrams for complex flows and screenshots/videos for visual behavior when helpful; do not manufacture a design section or media for a simple traceback.

- Wrong: "The whole architecture needs a redesign; here's a diagram," on a single null-input crash with no supporting investigation.
- Right: "Acceptance criterion: a `text: None` chunk returns a float in `[0, 1]` without raising. This report uses an isolated evaluator test because the trigger does not involve the database or UI."

### Rule: Commit to one approach, and name what is still open

A plan comment commits me to an approach in front of maintainers. Name the change, where it goes, and how I will show it works; say what is out of scope; flag an open question as a question. If someone has already suggested a direction or opened a PR, say how my plan relates to it.

- Wrong: "I'll fix this properly and clean up the evaluator while I'm in there. PR coming tomorrow."
- Right: "Plan: normalize `None` to `""` at the context join in `faithfulness_checker.py:38` and drop the issue-60 `xfail` marker. Out of scope: the #59 scoring tests. Open question: whether non-string `text` values need handling too."

### Rule: Make the PR description a true account of the diff

A PR title and description are what a maintainer reads before the diff. Every sentence must be true of the committed diff: name each change, paste the before/after and check output instead of summarizing them, tick only the boxes that are true, and say what I left out or changed from the plan. State AI use plainly in Notes for Reviewers.

- Wrong: "Implements the plan exactly. All tests pass. Ready to merge!"
- Right: "Changes: line 38 now uses `chunk.get("text") or ""`; removed the issue-60 `xfail` marker. `make test-unit`: `376 passed, 52 xfailed`. Nothing changed from the posted plan."

## Things I never post

- Invented test runs, logs, screenshots, personal verification or classroom feedback.
- A setup/configuration failure presented as proof of a different reported bug.
- A plan that silently changes after posting; a changed build gets a Deviations note and, if the posted plan is no longer true, a follow-up comment.
- Guaranteed delivery dates, guaranteed fixes, or requests that classmates stop parallel work.
- Claims that all tests passed when checks were skipped, selected narrowly, or marked XFAIL.
- A ticked checklist box for a check I did not run, or a CI box ticked before CI has run.
- Generic "same as above" confirmations in place of my own evidence.
- A statement that a comment was graded by repro-check when only a manual review occurred.

## Upstream: how I talk to maintainers

### Claims I post

#### Rule: Ask to be assigned, then wait

A claim states my interest, one concrete first step, and when I will report back, then asks a maintainer to assign me. I do not start the fix or open a PR before the project's claim process allows it, and I never claim an issue someone else has already claimed or been assigned.

- Wrong: "I'm taking this one, PR coming tonight."
- Right: "I'd like to work on this. My first step is to reproduce it locally on `main`, and I can report back within two days. Could a maintainer assign it to me?"

#### Rule: Raise the open edge in the claim

If the issue leaves a choice open (behavior, wording, where the change lives), I ask about it in the claim with my default, rather than picking silently and surprising the reviewer later.

- Wrong: "I'll handle the details however makes sense."
- Right: "One question before I start: should the empty state show the old label or nothing at all? I'll default to nothing unless you'd prefer otherwise."

### Questions I ask

#### Rule: One answerable question, with what I already tried

A question names what I ran or read, what I saw, and asks one thing a maintainer can answer without opening my machine. I check the README, docs, and existing issues first.

- Wrong: "How does the backend work? Where should I start? Is there a setup doc?"
- Right: "On a clean `main`, `pnpm test` fails one suite with `ECONNREFUSED 127.0.0.1:5432` after I started Postgres with the README's Docker command. Is there a seed step the README doesn't list?"

### Pings I send

#### Rule: One nudge, after the repo's own rhythm

I nudge only after the repo's usual wait for a first response (measured from its recently merged outside PRs) plus a few days. The nudge is one short message with the current state, it tags only a reviewer already on the PR, and I send it once.

- Wrong: "Any update?? It's been a day."
- Right: "Checking in on this: the branch is rebased on `main` and all checks pass. Happy to change anything whenever someone has time to review."
