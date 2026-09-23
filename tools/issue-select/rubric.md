# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks


| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Issue fresh | The archived flag, last push to any branch, latest release, issue creation date, and recent issue activity in the repo-facts block and comment thread. | Pass if the repository is not archived and either its last push was within 180 days or its latest release was within 365 days of the capture date. An old issue may still pass when recent comments or maintainer activity show that it remains relevant. | required |
| Maintainer alive | The last 5 default-branch commit dates and authors, the maintainer first-response sample in the repo-facts block, and comments from an Owner, Member, or Collaborator in the issue thread. | Pass if there is at least one non-bot human commit within 90 days of the capture date or a maintainer responded to an issue within 30 days. Bot activity alone does not pass this check. | required |
| Active contribution | The issue's assignees and linked pull requests in the repo-facts block, along with claim comments, work-in-progress statements, and pull-request references in the comment thread. | Pass if there is no current assignee, no open linked pull request, and no contributor who stated within the previous 14 days that they are actively working on the issue. Closed unmerged pull requests and abandoned claims older than 14 days do not fail the check. Apply any Path Review house rules from scope.md in live mode. | required |
| Bounded non-breaking scope | The issue body, labels, opening date, author association, comment thread, requested deliverables, affected components, maintainer scope statements, and linked pull-request history. | Pass if the issue describes one coherent outcome whose completion can be observed or tested. Also pass a terse issue when it was opened by an Owner, Member, or Collaborator or carries a good-first-issue label and names a specific missing or incorrect behavior with finite examples, unless later evidence contradicts that scope. The work may touch multiple related files, list multiple implementation approaches, or include optional follow-up work. Fail if it is a usage question, an umbrella or tracking issue, or an open-ended codebase-wide change. Fail if a required product decision such as behavior, semantics, interface design, or an asset remains unsettled or marked TBD. Fail when an issue has been open for more than two years with at least two closed unmerged pull requests or repeated abandoned attempts, unless a maintainer clarified a current implementation path within the last 180 days. Also fail if it requires a large architectural rewrite or explicitly requires a breaking change to a public API, schema, configuration format, or other established interface. | required |
| AI-assisted contribution allowed | The contribution-policy line in the repo-facts block, including CONTRIBUTING.md, dedicated AI-policy files, and relevant issue or pull-request templates. | Pass unless the project explicitly bans AI-generated or AI-assisted contributions. Silence passes. Requirements for disclosure, testing, personal understanding, or human review pass because they are conditions rather than bans. | required |
| Reproduction and completion clarity | Reproduction steps, error output, acceptance criteria, expected behavior, labels, and maintainer clarification in the issue body and comment thread. | Pass if the evidence provides either a reproducible failure or at least one concrete, testable expected outcome. | preferred |
| Backend or agentic fit | The issue title, body, labels, affected components, and maintainer comments. | Pass if the work concerns APIs, server-side logic, business logic, data flow, integrations, reliability, tool use, agent workflows, orchestration, state handling, evaluation, or unexpected agent behavior. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every required check passes. Reject if any required check fails.

For Issue fresh, Maintainer alive, Bounded non-breaking scope, and AI-assisted contribution allowed, an unclear result counts as a failure because the issue cannot be verified as a safe first contribution.

For Active contribution, lack of an assignee, open linked pull request, or recent claim is a pass; affirmative evidence that somebody is currently working on the issue is a failure. If the available evidence is genuinely incomplete, unclear counts as a failure.

Preferred checks never change the accept or reject verdict. Use them only to rank issues that have already been accepted.
