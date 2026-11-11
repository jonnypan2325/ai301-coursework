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
