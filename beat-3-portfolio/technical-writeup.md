# Building an AI assisted Github Open Source issue scout



## Overview
Currently, I have all the skills built, just waiting to be assigned an issue that I have identified.
The scout is able to help me scour GitHub for open issues in Open Source Repos that are relevant to my skillset and preference.
I was able to accomplish that by having Claude analyze my resume to produce a tailored fit profile and search strategy to look for valid issues.


## What happened
At the very start, the Claude Skills that I have created for the issues selection was too restrictive, and only gave me very limited issues. When I made the rules more lenient, I ended up getting too many issues.

Scan 1 read-out (2026-11-11):

```text
I ran the scan: 2 of the 5 graded candidates are accepted, and 3 are rejected because someone else had already claimed them.
```

| Rejected issue | Its one comment, posted 2026-11-11 |
|---|---|
| tamadalab/welfare-time#100 | "取り組みます" ("I'll work on it") |
| repowise-dev/repowise#5478 | a claim comment, plus two PRs opened the same day |
| Pycord-Development/pycord#3437 | "I'd like to work on this... will open a PR with a fix and regression tests shortly." |

One change that I made on 2026-11-11 was instead of ranking issues with 1-5 comments first, I ranked untouched issues (zero comments) first, as more often than not, an issue with a recent comment actually means someone else claimed that issue before me.

Ranking rule 5 in `search-strategy.md`, before:

```text
5. Order the survivors by comment count in this sequence: 1 to 5 comments
   first (someone has engaged, but the thread is not crowded), then 0
   comments, then 6 to 10 comments.
```

After:

```text
5. Order the survivors by comment count, fewest first: 0 comments, then
   1 to 5, then 6 to 10. Break ties by most recent `updatedAt` first.
   On fresh good-first-issue bugs the first comment is usually a claim,
   so an untouched thread is the best cheap sign the issue is still free.
```

I ensured that my AI usage complied with all open source repo's AI disclosure and agreement.

The same scan passed pycord#3437 on my AI-policy check, with this evidence:

```text
"CONTRIBUTING: AI use must be disclosed; new-contributor use 'highly discouraged, and will likely result in the direct closure' (not an explicit ban; tension noted)."
```

Added to that check in `rubric.md` on 2026-11-11:

```text
Also fail when the policy warns that AI use by new or first-time contributors will or will likely lead to closure or rejection (for example "highly discouraged, and will likely result in the direct closure"), because for a first contribution that works as a ban.
```

In the end, the scout accepted a frontend issue I gave it on AntAlmanac (#2101), which is a course planner for students.
When grading my picks, I saw that it had two misses. One of the issues was a homework exercise that wasn't meant to be fixed, and another one needed a maintainer assignment.
Re-grade of the same four picks after my rubric changes (2026-11-17):

```text
{"name": "scope-feasible", "grade": "fail", "evidence": "\"This issue is expected as a Standard Exercise for students to do\"; CONTRIBUTING: \"Make all changes only inside your <githubid>_solutions folder\""}
{"name": "Open to outside claim", "grade": "fail", "evidence": "README: \"leave a comment expressing your interest and wait for a member of our team to assign it to you\"; no good first task or help wanted label"}
```


## Limitations and what's next
The limitations of my skills include bot pushes count as activity in the repo, which is inaccurate, since a repo's health should only be based on maintainers and contributors, not bots. This should be a simple fix in my claude skill workflow, adding more parameters and limitations. Also, I need to ensure the skills change as my preferences and experiences change, and that it's not static. Next steps would be adding more skills and automating the skills via hooks + workflow so I'm able to be notified about new issues without me having to run the skills manually.
