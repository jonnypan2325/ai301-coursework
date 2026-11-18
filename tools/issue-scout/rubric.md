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
| scope-feasible | Issue body: the work it asks for; for exercises, the repo's CONTRIBUTING.md. | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. Also fail if the issue is a course, workshop, or tutorial exercise that no single fix can close (for example when CONTRIBUTING.md sends solutions to per-student folders), or if it explicitly requires a breaking change to a public API, schema, configuration format, or other established interface. | required |
| AI-assisted contribution allowed | The contribution-policy line in the repo-facts block, including CONTRIBUTING.md, dedicated AI-policy files, and relevant issue or pull-request templates. | Pass unless the project explicitly bans AI-generated or AI-assisted contributions. Silence passes. Requirements for disclosure, testing, personal understanding, or human review pass because they are conditions rather than bans. Also fail when the policy warns that AI use by new or first-time contributors will or will likely lead to closure or rejection (for example "highly discouraged, and will likely result in the direct closure"), because for a first contribution that works as a ban. A warning that only applies to contributions breaking a stated condition, such as undisclosed AI use, is still a condition and passes. | required |
| Reproduction and completion clarity | Reproduction steps, error output, acceptance criteria, expected behavior, labels, and maintainer clarification in the issue body and comment thread. | Pass if the evidence provides either a reproducible failure or at least one concrete, testable expected outcome. | preferred |
| Backend or agentic fit | The issue title, body, labels, affected components, and maintainer comments. | Pass if the work concerns APIs, server-side logic, business logic, data flow, integrations, reliability, tool use, agent workflows, orchestration, state handling, evaluation, or unexpected agent behavior. | preferred |
| Open to outside claim | The contribution steps in CONTRIBUTING.md or the README, any contributor guidelines they link, and the issue's labels and author association. | Pass if an outside contributor can start after posting a claim comment, or if the issue carries a label inviting newcomers (such as good first issue, good first task, or help wanted). Fail if the project requires a maintainer to assign the issue before work starts, or says some tasks are reserved for its own team, and the issue has no such label: the claim may be declined or left unanswered. On an accepted issue that fails this check, say so in the summary so the student asks before planning around it. | preferred |

## The difficulty note

With each accepted candidate, report a one-line difficulty note in
the read-out summary:

- **straightforward**: the issue names where the change lives and the
  change is bounded.
- **moderate**: the location is yours to find, or several files are
  involved.
- **ambitious**: an open design edge, or an unfamiliar subsystem;
  name the edge, because claiming this issue means raising it with
  the maintainer early.

The note is information for the chooser, never a verdict input: it
does not appear in the fenced JSON block, it never changes accept or
reject, and in eval mode it is skipped entirely.

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every required check passes. Reject if any required check fails.

For Issue fresh, Maintainer alive, scope-feasible, and AI-assisted contribution allowed, an unclear result counts as a failure because the issue cannot be verified as a safe first contribution.

For Active contribution, lack of an assignee, open linked pull request, or recent claim is a pass; affirmative evidence that somebody is currently working on the issue is a failure. If the available evidence is genuinely incomplete, unclear counts as a failure.

Preferred checks never change the accept or reject verdict. Use them only to rank issues that have already been accepted.
