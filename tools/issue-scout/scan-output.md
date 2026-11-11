# issue-scout scan output

## Scan 1: 2026-11-11 (run a scan)

I ran the scan: 2 of the 5 graded candidates are accepted, and 3 are rejected because someone else had already claimed them.

The searches returned 300 results: 100 from each query. After removing duplicates and applying your four drop rules, 215 were left. Keeping one issue per repo left 94, and I graded the top 5 in your ranking order.

## Accepted (best fit first)

**1. aws-samples/sample-spec-driven-presentation-maker#135**, "Token discipline: check inline directives"
- **Surfaced by:** Q2 (Python, good first issue + bug).
- **Why it ranked top 5:** no assignee, 1 comment, updated today, not a label your filters drop.
- **Fit:** it's a Python validation check in an LLM/agent presentation tool, tested with pytest and run with uv, so it matches the profile closely.
- **Ready to pick up:** the maintainer (`ShotaroKataoka`, the only committer recently) narrowed it 7 hours before the scan to "inline font sizes only." That comment names the function, the location format and the test file. I confirmed `sdpm/sdpm/engine/checks/font_size.py` and `tests/test_checks_font_size.py` exist, and the comment ends "Comment here if you'd like to take it."
- **AI policy:** no ban.

**2. maniator/verticopolis#679**, "rentals get no cleanliness cell"
- **Surfaced by:** Q1 (TypeScript, good first issue + bug).
- **Why it ranked top 5:** no assignee, 1 comment, updated within the last 90 days.
- **Ready to pick up:** the owner opened it and rechecked it on 2026-11-04. The fix is a single concrete change, and the owner commits daily.
- **Weaker fit:** the fix is now in the Rust engine (`engine-rs/src/panels/heatmap.rs`), not TypeScript. It's also a game-overlay rendering fix, not backend work. It fails the "backend or agentic fit" preferred check, which only affects ranking, not the verdict.

## Rejected

| Issue | Surfaced by / why it was top 5 | Check that rejected it |
|---|---|---|
| tamadalab/welfare-time#100 | Q2 · 1 comment, updated today | **Active contribution:** `kana112` wrote "取り組みます" ("I'll work on it") today. The issue is also reserved for a university course. |
| repowise-dev/repowise#5478 | Q2 · 1 comment, updated today | **Active contribution:** two linked pull requests are open (#5491, #5494), both opened today, plus a claim comment. |
| Pycord-Development/pycord#3437 | Q2 · 1 comment, updated today | **Active contribution:** `shivam-modi` wrote "I'd like to work on this… will open a PR shortly" today. |

## Problems with the strategy and rubric

I couldn't write `scan-notes.md` because permission was denied, so here is what I would have logged:
- **Comment ordering:** ranking 1–5 comments first mostly finds issues that were just claimed. On fresh good-first-issue bugs, one comment is usually "I'll take this." Consider putting the 0-comment group first.
- **Language filter:** `--language` matches the repo's main language, not the language of the fix. welfare-time is tagged Python but the fix is in templates and CSS; verticopolis is tagged TypeScript but the fix is in Rust.
- **Q3 (llm agent) never reached the top 5:** its results lack the good-first-issue label, and the comment ordering pushed them down. If agentic bugs matter to you, consider guaranteeing a Q3 issue a slot in the top 5.
- **AI-policy check:** pycord passes it by the letter because it only says AI use by new contributors is "highly discouraged" and "will likely result in the direct closure." For a newcomer that works like a ban.
- **No check for reserved issues:** the rubric has no check for issues reserved for a closed group, like welfare-time's course reservation.
- **repowise rule for later:** repowise allows AI help but asks contributors to remove `Co-Authored-By: Claude` lines from commits before merge. Worth remembering if it comes up again.

Per your scope's claim rule, if you commit to #135, comment on it right away.

```json
[
  {
    "item": "https://github.com/aws-samples/sample-spec-driven-presentation-maker/issues/135",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Not archived; last push 2026-11-11, latest release v0.14.0 on 2026-11-10."},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last 5 commits by human ShotaroKataoka, newest 2026-11-11; he re-scoped this issue 2026-11-11."},
      {"name": "Active contribution", "grade": "pass", "evidence": "No assignee, no linked PRs, no open PR referencing #135; only comment is the maintainer's 'Comment here if you'd like to take it.'"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Maintainer comment narrows to 'Good first slice: inline font sizes only' with function, location format, and test file named; hex colours deferred to a follow-up."},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "CONTRIBUTING.md points contributors to AGENTS.md; no AI ban in CONTRIBUTING or PR template."},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Example JSON '{{22pt:big}} → not checked'; done = report NNpt values not in --fs-* tokens, with cases in tests/test_checks_font_size.py."},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "Python validation logic in an agent/MCP slide-generation engine (sdpm engine checks)."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/maniator/verticopolis/issues/679",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Not archived; last push 2026-11-11, release v3.0.1 on 2026-11-11; owner rechecked issue 2026-11-04 'Still true.'"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last 5 commits by owner maniator (Naftali Lubin), all 2026-11-11."},
      {"name": "Active contribution", "grade": "pass", "evidence": "No assignee, no linked PRs; only comment is the owner's recheck; no open PR references #679."},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Owner-opened good-first-issue: 'give both rental kinds the same tint: \"na\" treatment as the condo' in cleanliness_cells."},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "Repo ships AGENTS.md/CLAUDE.md and owner posts Claude Code-generated comments; no ban in CONTRIBUTING.md."},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Expected outcome: Studio and Apartment get an 'na' cleanliness cell like condos."},
      {"name": "Backend or agentic fit", "grade": "fail", "evidence": "Labeled 'render'; game heatmap overlay fix in engine-rs (Rust), not backend/API or agentic work."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/tamadalab/welfare-time/issues/100",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Not archived; last push 2026-11-11."},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Owner tamada merged PR #150 on 2026-11-11."},
      {"name": "Active contribution", "grade": "fail", "evidence": "kana112 commented '取り組みます' (I'll work on it) on 2026-11-11, within 14 days."},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Owner-opened; fix stated: add animation-duration: 200ms to .animate-in and drop 3 duration-200 classes."},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "CONTRIBUTING.md: 'AI の利用は歓迎します' (AI use is welcome), with disclosure required."},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "確認方法: run 'just serve', modal animates, DevTools shows 200ms."},
      {"name": "Backend or agentic fit", "grade": "fail", "evidence": "CSS/Hugo template animation fix, frontend only."}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/repowise-dev/repowise/issues/5478",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Not archived; last push 2026-11-11, release v0.60.1 on 2026-11-02."},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Five human-authored commits on 2026-11-11 (e.g. mercael91, ANO3457)."},
      {"name": "Active contribution", "grade": "fail", "evidence": "Open linked PRs #5491 (amalsaidov) and #5494 (laozhej), both 2026-11-11, plus laozhej's 'I'm taking this issue.'"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "'First issue: one branch with a fixture'; must not change other languages or the <anonymous> fallback."},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": ".github/CONTRIBUTING.md only asks that coding-agent co-author trailers be removed; a condition, not a ban."},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Repro output \"class names: ['<anonymous>']\", expected 'CharList'; tests listed for record/class/union/enum."},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "Python analysis-engine logic in a code-intelligence tool for agents."}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/Pycord-Development/pycord/issues/3437",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "Not archived; last push 2026-11-11; issue opened 2026-11-10."},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Human commits by ToothyDev 2026-11-10 and Paillat-dev 2026-11-08."},
      {"name": "Active contribution", "grade": "fail", "evidence": "shivam-modi on 2026-11-11: 'I'd like to work on this... will open a PR with a fix and regression tests shortly.'"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "Single behavior: _to_payload() should not invent key 1 when the API payload only has key 0."},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "CONTRIBUTING: AI use must be disclosed; new-contributor use 'highly discouraged, and will likely result in the direct closure' (not an explicit ban; tension noted)."},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "Minimal repro code: prints {0: None, 1: None}; expected only key 0."},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "Python API-wrapper data round-trip bug (Discord API payload serialization)."}
    ],
    "verdict": "reject"
  }
]
```


## Grade 1: 2026-11-11 (direct: two AntAlmanac picks, two Issue Selection site picks)

I graded all four issues against `rubric.md`. Only one passes: **icssc/AntAlmanac#2101**. The other three each fail a required check, and two of those have someone already working on them.

## Accepted

**1. icssc/AntAlmanac#2101: Show saved courses as DepartmentCourseGroup** (found by: direct link)
- **Issue fresh, pass:** the repo isn't archived and was last pushed on 2026-11-11.
- **Maintainer alive, pass:** two people committed by hand on 2026-11-09 (anthonyj33, who wrote this issue, and SahilKhanna123).
- **Active contribution, pass:** nobody is assigned, there are no comments, no pull request is linked, and searching open pull requests found nothing related.
- **Bounded non-breaking scope, pass:** a Collaborator opened it. It asks for one visible change: show saved courses using the `DepartmentCourseGroup` display instead of `GroupRequirement`, with a screenshot of the target look.
- **AI-assisted contribution allowed, pass:** ICSSC's linked contributor guidelines say "AI assistance in development and debugging is encouraged." The conditions are that work must be started by a human, written comments must be mostly your own, any AI-written text must be disclosed, and you must review your own code.
- **Reproduction and completion clarity (preferred), pass:** the screenshot shows the expected result.
- **Backend or agentic fit (preferred), fail:** it's labeled `frontend` and `Planner`, and the work is swapping how something is displayed.

It's the only accepted candidate, so there was nothing to rank it against. It's TypeScript, which suits you, but it's UI work, not the backend work your profile prefers.

**Before you take it:**
- **You can't just start.** AntAlmanac's README and ICSSC's guidelines both say to comment that you're interested and wait for a team member to assign you. They also say "Our team members have the final say on whether a task is reserved for internal development."
- **It might be internal work.** It was opened yesterday by a team member as part of Planner work, with no `good first task` label, so there's a real chance the team means to do it themselves. The rubric doesn't check for this.
- **Your claim comment must be your own writing.** ICSSC says contributions must be "human-initiated" and bans "PRs, especially those generated by agents scraping GitHub."

## Rejected

**2. icssc/AntAlmanac#1980: Optimize performance of course-rename lookups** (direct)
**Failed Active contribution:** victorialee13 assigned themselves on 2026-10-16. Every other required check passed. Both preferred checks passed too, since it's lookup logic, and it has the `good first task` label. It would have ranked above #2101 if it were free.

**3. AcademySoftwareFoundation/dna#255: Add email provider** (direct)
**Failed Active contribution:** pull request #264 by KrishnaAdityaSrivastava is linked and open, last updated 2026-11-08. Everything else passed, and it's Python backend integration work that fits your profile well.

**4. ProjectX-VJTI/Xplore-workshop#27: Python Bugs - Exercise 1** (direct)
**Failed Maintainer alive:** the last human commit to the main branch was on 2026-06-05, 159 days ago. Pull request #169 has been open since 2026-08-05 with no response, and a 2026-03-02 request to be assigned also went unanswered.

**Where the rubric fell short:** #27 passed the scope check, but it isn't a real issue to fix. It's a homework exercise, and the repo's CONTRIBUTING file tells students to copy the files into their own `SOLUTIONS/<githubid>_solutions/` folder and not touch the originals, so no fix ever closes it. Two lines worth adding to `scan-notes.md`:
1. Fail the scope check when an issue is a course or workshop exercise whose CONTRIBUTING sends solutions to per-student folders.
2. Note when a repo makes you wait for a team member to assign you, or keeps some tasks for its own team. The rubric can't see that today, and it matters for #2101.

```json
[
  {
    "item": "https://github.com/icssc/AntAlmanac/issues/2101",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "archived: false; pushedAt 2026-11-11T06:10:59Z"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "human commits 2026-11-09 by anthonyj33 (#2069) and SahilKhanna123 (#2092)"},
      {"name": "Active contribution", "grade": "pass", "evidence": "assignees: []; no linked PRs; 0 comments; no related open PRs found"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "COLLABORATOR-opened: 'Instead of showing groups of saved courses as GroupRequirements, use DepartmentCourseGroups' with target screenshot"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "ICSSC guidelines: 'AI assistance in development and debugging is encouraged' (conditions: human-initiated, disclose AI writing, self-review)"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "screenshot of DepartmentCourseGroup shows the concrete expected rendering"},
      {"name": "Backend or agentic fit", "grade": "fail", "evidence": "labels: Planner, frontend; UI component swap"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/icssc/AntAlmanac/issues/1980",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "archived: false; pushedAt 2026-11-11T06:10:59Z"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "human commits 2026-11-09 by anthonyj33 and SahilKhanna123"},
      {"name": "Active contribution", "grade": "fail", "evidence": "assignees: [victorialee13], assigned 2026-10-16"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "COLLABORATOR-opened, 'good first task' label; replace O(n) COURSE_RENAMES array searches with faster lookups"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "ICSSC guidelines: 'AI assistance in development and debugging is encouraged'"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "'every use of COURSE_RENAMES searches the array with O(n) time' gives a testable target"},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "data lookup / business logic in Scheduler course-rename handling"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/AcademySoftwareFoundation/dna/issues/255",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "archived: false; pushedAt 2026-11-11T01:10:37Z"},
      {"name": "Maintainer alive", "grade": "pass", "evidence": "human commits 2026-11-10 camerontarget14, 2026-11-06 jspada200"},
      {"name": "Active contribution", "grade": "fail", "evidence": "linked PR #264 OPEN by KrishnaAdityaSrivastava, updated 2026-11-08"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "'good first issue'; acceptance criteria: provider class, settings, send basic email; Gmail flavor marked 'Bonus'"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI policy (silence); requires CLA + DCO only"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "explicit acceptance criteria incl. 'Able to send a basic email'"},
      {"name": "Backend or agentic fit", "grade": "pass", "evidence": "label Backend; new email provider integration"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/ProjectX-VJTI/Xplore-workshop/issues/27",
    "checks": [
      {"name": "Issue fresh", "grade": "pass", "evidence": "archived: false; pushedAt 2026-11-04T05:27:06Z"},
      {"name": "Maintainer alive", "grade": "fail", "evidence": "last human default-branch commit 2026-06-05 (159 days); PR #169 open since 2026-08-05 unanswered"},
      {"name": "Active contribution", "grade": "pass", "evidence": "no assignee; claim by Asmi768 2026-03-02 (>14 days); PR #43 CLOSED unmerged"},
      {"name": "Bounded non-breaking scope", "grade": "pass", "evidence": "'good first issue'; fix scripts in test_playground/basics per expected behavior in file comments"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI policy (silence)"},
      {"name": "Reproduction and completion clarity", "grade": "pass", "evidence": "repro steps given; 'Expected behavior ... Mentioned in comments of said py files'"},
      {"name": "Backend or agentic fit", "grade": "fail", "evidence": "Python-basics tutorial exercise, labels bug/documentation/problem level 1 - starter"}
    ],
    "verdict": "reject"
  }
]
```
