# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

Profile: "I want my first contribution to be in a TypeScript or Python project" + "I am most interested in backend bugs involving APIs, data flow, business logic, integrations, and server-side reliability" + "I prefer issues already labeled as bugs"
gh search issues --language typescript --label "good first issue" --label bug --sort updated --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

Profile: same three sentences, for the Python half of "TypeScript or Python project"
gh search issues --language python --label "good first issue" --label bug --sort updated --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

Profile: "I am also interested in agentic-system and LLM-tooling bugs, especially problems involving tool use, workflow orchestration, retrieval, state handling, evaluation, and unexpected agent behavior"
gh search issues llm agent --language python --label bug --sort updated --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

## Ranking filters

Apply in this order to the combined, de-duplicated results. Every rule
reads only fields the queries return.

1. Drop any result with one or more assignees.
2. Drop any result whose `updatedAt` is more than 90 days before the scan date.
3. Drop any result with more than 10 comments (`commentsCount` > 10): a
   crowded thread usually means a contested or stalled issue.
4. Drop any result carrying any of these labels (case-insensitive):
   `question`, `discussion`, `RFC`, `epic`, `tracking`, `duplicate`,
   `wontfix`, `invalid`.
5. Order the survivors by comment count in this sequence: 1 to 5 comments
   first (someone has engaged, but the thread is not crowded), then 0
   comments, then 6 to 10 comments. Within each band, fewer comments
   first; break ties by most recent `updatedAt` first.
6. Walk that order and keep only the first result from each repository
   (`repository.nameWithOwner`), so the graded candidates come from
   different projects instead of one repo's batch of fresh issues.

## How many to grade

Grade the top **5** ranked results per scan.
