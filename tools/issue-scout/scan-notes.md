# issue-scout scan notes

Dated log of what each scan got wrong and what changed because of it.

- 2026-11-11 (scan 1): all three rejections (welfare-time#100, repowise#5478, pycord#3437) ranked in the 1-5 comment band and the single comment on each was a same-day claim. Ranking filter 5 had put 1-5 comments ahead of 0 on the theory that a comment meant an engaged maintainer. Changed search-strategy.md filter 5 to order fewest comments first (0, then 1-5, then 6-10).
- 2026-11-11 (scan 1): Q3 (Python `llm agent` bugs) never reached the top 5. Its results carry no good-first-issue label and lost every ordering tie to Q1/Q2, so the agentic half of the fit profile never got graded. Named the queries Q1-Q3 in search-strategy.md and added ranking filter 7, which reserves the fifth grading slot for the best surviving Q3 result.
- 2026-11-11 (scan 1): the AI-assisted contribution check passed pycord#3437 although its CONTRIBUTING.md says AI use by new contributors is "highly discouraged, and will likely result in the direct closure". The scout passed it by the letter (not an explicit ban) and flagged the tension itself. Added a fail condition to that check in rubric.md for newcomer AI warnings that threaten closure, and kept warnings tied only to breaking a stated condition (such as undisclosed AI use) as passes.
