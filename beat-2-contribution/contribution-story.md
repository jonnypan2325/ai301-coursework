# Contribution: Users should not be able to save a "Loading..." course

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jonathan Pan
**GitHub Username:** jonnypan2325
**Issue:** https://github.com/icssc/AntAlmanac/issues/2008
**Pull Request:** Not opened yet. Anthony assigned #2008 to me on 2026-11-22. The fix is built and tested on a local branch, and I'm reviewing the code myself before I open the PR, as ICSSC's guidelines require.

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5 (2026-11-11): Built the issue-scout and patched its strategy and rubric after its first runs**

I installed the issue-scout on top of my Unit 1 issue-select skill. Using my resume, Claude helped me update my fit profile to name TypeScript
and Python and draft a three-query search strategy with six ranking filters.
The first scan cut 300 results to 94 and graded five, but three of the five were rejected because someone had already commented that they were taking the issue earlier that day.
My ranking put issues with 1-5 comments ahead of untouched ones. The problem is that on fresh issues, that one comment usually means
someone has claimed it. The same scan passed pycord#3437 on AI policy despite a "will likely result in the direct
closure" warning for new contributors, and grading my own picks later
passed a homework exercise (Xplore-workshop#27) on scope.

Because of that, I moved zero-comment issues to the top of the ranking, tightened the AI-policy and scope checks, and added a check that flags repos where a maintainer must assign the issue first.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

**Toolkit**

- Tool: issue-scout (my search strategy and rubric), run headless with Claude Code: one wild scan on 2026-11-11, then direct grades of the issues I picked (2026-11-11, re-graded 2026-11-17, and a third grade on 2026-11-18).
- What it produced: the third grade accepted #2008 as "straightforward", with every required check passing; #762 and #2012 were accepted as "ambitious". It flagged that the repo wants a maintainer to assign the issue first, and that the December comment said the crash was probably gone, so I should confirm it still reproduces.
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

Clicking Save (or pressing Cmd/Ctrl+S) while a dragged course is still loading should save the real course once it loads. The roadmap should never store a `"Loading..."` placeholder, and the "Roadmap saved" toast should only appear when what was saved matches what the user sees.

**Current Behavior**

There is no Application Error any more (the issue's original symptom). Instead:

- Logged in, live site, 2026-11-21: the `roadmaps.save` payload contained `courses: [{courseId: "Loading..."}]`. After a reload, the server's response still returned `"courseId": "Loading..."` while the UI showed the quarter with 0 courses.
- Logged in, live site, 2026-11-18: the save request was an empty diff, so the course was simply never saved. Why the two runs differ is not established yet.
- Logged out (local): `Loading...` is written to localStorage key `roadmap` and the course is gone from the UI after a reload.
- In every case the success toast appears.

The window is one `courses.get` request: about 268-401 ms unthrottled and about 2 s on DevTools 3G, so it needs a slow load and a quick Save.

**Affected Components**

All under `apps/antalmanac-planner/site/src`:

- `app/roadmap/catalog/ProgramRequirementsList.tsx` (`CourseTile`): starting a drag dispatches `setActiveCourse(LOADING_COURSE_PLACEHOLDER)` and `setActiveCourseLoading(true)`, then calls `courses.get`.
- `app/roadmap/planner/Quarter.tsx` (`addCourse`): if the active course is still loading, it dispatches `createQuarterCourseLoadingPlaceholder`.
- `store/slices/roadmapSlice.ts` (`createQuarterCourseLoadingPlaceholder`): splices the placeholder (id `Loading...`) into `state.plans` without recording a revision.
- `hooks/planner.ts` (`useSaveRoadmap`): saves `state.plans` with no check for a loading course.
- `helpers/planner.ts` (`collapsePlanner`, `saveRoadmap`): turns each course into `{courseId: course.id}`, so the placeholder becomes `courseId: "Loading..."`.
- `component/AppHeader/SaveButton.tsx`: Save button and Cmd/Ctrl+S shortcut; already disabled with a spinner while `saveInProgress`.

**Toolkit**

- Tool: Claude Code, used to trace the save path through the planner source (drag, placeholder, save hook, payload) after I described the issue's steps.
- What it produced: the file-and-function chain listed under Affected Components, ending at `useSaveRoadmap` with no loading check.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Reproduction Process

**Environment Setup**

- macOS 26, Chrome 157.0.8103.52.
- Node 22.23.5 (via `fnm`), pnpm 10.22.0 (the repo's `packageManager`).
- Fork `jonnypan2325/AntAlmanac`, cloned and tested at upstream `main` `cea852b4a`; `pnpm install`, then `pnpm dev` and open `/planner`.
- An Anteater API key goes in the gitignored `apps/antalmanac/.env`.
- Live site: antalmanac.com/planner with my own account, on a throwaway roadmap ("test") so my real roadmap stayed untouched.

Challenges:
- Logging in locally needs OAuth secrets I don't have, so the logged-in path was tested on the live site instead of locally.
- The bug only shows while `courses.get` is in flight. Locally, Claude Code wrapped the page's `fetch` to hold that request for 8 s. On the live site I used DevTools network throttling (3G) with no code changes.
- Hovering a course chip opens its popover, which fetches the course early and closes the window. The repro only works if you drag without hovering first.

**Steps to Reproduce**

1. Open `/planner`, go to Catalog > Major (e.g. "Computer Science, B.S.") and expand a requirement group.
2. DevTools > Network > throttling: 3G (or delay `courses.get` locally). Filter for `roadmaps.save`.
3. Without hovering the chip, drag a course into a quarter, and within about 1-2 s click Save or press Cmd+S.
4. Observed: the "Roadmap saved" toast appears. The `roadmaps.save` payload contains `courseId: "Loading..."` for that quarter.
5. Reload. Observed: the quarter shows 0 courses, but the server's response for the roadmap still contains `"courseId": "Loading..."`.

**Reproduction Evidence**

- **Commit showing reproduction:** None. The reproduction needed no code change: it used a browser-side request delay locally and DevTools throttling on the live site, so there is nothing to commit.
- **Screenshots/logs:** Posted in my issue comment: https://github.com/icssc/AntAlmanac/issues/2008#issuecomment-7067955608 (save payload with `Loading...`, server response after reload, `courses.get` timings on 3G and unthrottled). Logged-out localStorage after Save: `{"courses":[{"courseId":"Loading..."}]}`. Logged-in empty diff from 2026-11-18: `{"deletedPlanners":[],"newPlanners":[],"deletedQuarters":[],"deletedYears":[],"updatedQuarters":[],"updatedYears":[],"updatedPlanners":[],"newYears":[],"newQuarters":[],"overwrite":false,"currentPlanIndex":1}`.
- **My findings:** The Application Error is gone, but the bug is still real. The maintainer's December question was whether `Loading...` still gets saved: my 2026-11-21 live run shows it does, server-side. My 2026-11-18 run had concluded the opposite (empty diff), and the later run overturned that.

**Toolkit**

- Tool: repro-check (my Unit 2 rubric with the wild scope) and the voice guard, plus Claude Code driving Chrome for the delayed runs.
- What it produced: repro-check held my draft twice (environment missing, then evidence missing), then accepted it, with all seven required checks passing. The voice guard stopped two drafts (no next step and report-back time, then no environment or steps) before passing.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Solution Approach

**Analysis**

- Dragging a chip starts `courses.get` in `CourseTile` (`ProgramRequirementsList.tsx`) and marks the active course as loading.
- Dropping it before the response arrives makes `Quarter.tsx` `addCourse` dispatch `createQuarterCourseLoadingPlaceholder` (`roadmapSlice.ts`), which splices `LOADING_COURSE_PLACEHOLDER` (id `Loading...`) into `state.plans`. It records no revision.
- `useSaveRoadmap` (`hooks/planner.ts`) has no loading check. It rebuilds the last-saved state (`restoreRevision` over a copy of `state.plans`), then runs `collapseAllPlanners` on both, which writes each course as `{courseId: course.id}`, so the placeholder becomes `courseId: "Loading..."`. Then `saveRoadmap` (`helpers/planner.ts`) sends the diff (logged in) or writes the whole roadmap to localStorage (logged out).
- The toast only reflects whether the request succeeded, not what was saved.
- Open point (hypothesis): the placeholder has no revision, so it can cancel out of the diff (the 2026-11-18 empty save) or be sent when that quarter is in the diff for another reason (the 2026-11-21 run). I'll confirm this while building.

**Proposed Solution**

My preferred fix, pending the maintainers' answer in the thread, is the one the issue itself describes: keep Save clickable, and make `useSaveRoadmap` wait for a loading course to finish before it builds the payload. While waiting it keeps the existing `saveInProgress` spinner on.

Deliberately not changing:
- no server-side validation;
- no clean-up of `Loading...` entries already stored (raised as a separate question);
- no new UI;
- no change to the drag, prefetch or autosave behaviour.

If the maintainers prefer it, the fallback is the simpler fix: disable Save while a course is loading.

**Implementation Plan**

1. `hooks/planner.ts` `useSaveRoadmap`: when a course is loading, set `saveInProgress`, wait until loading clears, then build the before/after from the current store state. The handler currently closes over `planners` from the render, so it must read fresh state after waiting.
2. Confirm in `helpers/planner.ts` that the collapsed payload can no longer contain `Loading...`.
3. Add a Jest test (planner site, jsdom) that puts a loading placeholder in a quarter, triggers save, and asserts the payload carries the real course and never `Loading...`.
4. Run `pnpm lint` and `pnpm test run` (the repo's CI checks).

Deviations, recorded 2026-11-22 when I built it:
- The helpers went into a new file, `helpers/loadingCourses.ts` (`hasLoadingCourse`, `removeLoadingCourses`, `waitForLoadedCourses`), rather than into `helpers/planner.ts`, so they can be tested without the save code.
- `useSaveRoadmap` now reads the latest store state through `useStore` instead of values captured at render time.
- I added a 10 s timeout: if the course never loads (e.g. `courses.get` fails), the save goes ahead with the placeholder stripped instead of spinning forever. Step 2 ("confirm the payload can't contain `Loading...`") became that strip.
- Step 3's Jest test targets the new helpers rather than rendering the hook.
- The root cause I'd marked as a hypothesis was confirmed in `helpers/roadmapEdits.ts` (`modifyQuarterCourse`): when the course loads, its revision's "before" snapshot leaves the placeholder out. Rolling back to the last save can therefore make that quarter differ (the placeholder gets sent, as in the 2026-11-21 run) or match (an empty diff, as in the 2026-11-18 run).

**Toolkit**

- Tool: Claude Code, reading `useSaveRoadmap`, `SaveButton` and `roadmapSlice` to weigh the two fixes from the thread. plan-check has not been run yet, because the plan waits on the maintainers' choice of approach.
- What it produced: the finding that a saving state already exists (`saveInProgress`, with a spinner on the Save button), so the "wait" fix needs no new UI.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Testing Strategy

Run on 2026-11-22 against branch `fix/2008-wait-for-loading-course` (commit `b7663c5a5`). Results are below each check.

1. **Unit test (new):** a Jest test that saves while a course is loading. Before the fix, it should show `Loading...` in the payload (or the course missing). After the fix: the real course id, and no `Loading...`. It is independent of network speed.
2. **Repro re-run, logged out, local:** the same steps with 3G throttling. Before: localStorage `roadmap` holds `courseId: "Loading..."` and the course is gone after reload. After: localStorage holds the real course id and it survives a reload.
3. **Repro re-run, Network tab:** the `roadmaps.save` payload must never contain `Loading...`. Logged-in local testing needs OAuth secrets I don't have, so I will ask the maintainers to confirm the logged-in path, or rely on the unit test.
4. **Repo checks:** `pnpm lint` (oxfmt and oxlint) and `pnpm test run`, with output pasted into the PR.

Results:
- **New tests:** `helpers/loadingCourses.test.ts`, 5 passed. They cover:
  - detecting a placeholder;
  - stripping it without mutating the plans;
  - resolving at once when nothing is loading;
  - waiting until the real course replaces the placeholder;
  - giving up after the timeout.
- **Repro re-run, logged out, local** (Claude Code drove Chrome with the same 8 s delay):
  - While loading, the Save button showed its spinner and made 0 localStorage writes.
  - After the load, one write: Fall = `{"courseId":"I&CSCI45C"}`.
  - After a reload, I&C SCI 45C was still there, 4 units.
  - Before the fix, the same steps stored `{"courseId":"Loading..."}`, and the quarter was empty after reload.
- **`pnpm lint`:** exit 0. The first run flagged formatting in `hooks/planner.ts`; I fixed it with oxfmt.
- **`pnpm test run`:** exit 0. Planner Jest: 46 passed, 47 skipped (already skipped in the repo). Vitest: 15 files, 82 tests passed.
- **Not tested:** the logged-in save end to end locally (no OAuth secrets). It goes through the same wait in `useSaveRoadmap`.

**Toolkit**

- Tool: ship-gate (installed, with `ship-manifest.md` in my clone). Not run yet: there is no fix branch to gate.
- What it produced: nothing yet. The checks it will run are listed above.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Implementation Notes

**2026-11-11**

Chose icssc/AntAlmanac. The scout accepted #2101 as my first pick.

**2026-11-17**

Applied the Unit 6 scope patch to the scout and re-graded. #2101 stayed an accept, rated "moderate", with a warning that the repo assigns issues itself.

**2026-11-18**

A maintainer told me on Discord that #2101 was already assigned internally (GitHub hadn't been updated), so I re-picked. The scout graded #2008 (accept, straightforward), #762 and #2012. Reproduced #2008:
- locally, logged out, with an 8 s delay: `Loading...` written to localStorage;
- live, logged in, with a 15 s delay: an empty save diff.

**2026-11-20**

Anthony okayed #2008 on Discord. I reproduced it myself on the live site with only DevTools 3G, with both the Save button and Cmd+S, saving within about 1.5 s of the drag. Found that hovering a chip prefetches the course and hides the bug.

**2026-11-21**

My own live run captured `Loading...` in the save payload, and the server returned it after reload. This overturned the 2026-11-18 conclusion that nothing invalid reaches the server. repro-check held my comment twice (missing environment, then missing evidence), then accepted it. I posted the claim plus repro on the issue.

**2026-11-22**

Anthony assigned #2008 to me on GitHub, so I started the fix. I went with the "wait" approach from my comment:
- `useSaveRoadmap` waits for loading placeholders to clear, reads fresh store state, and never sends a placeholder.
- A 10 s timeout keeps a failed load from hanging the save.

Tracing it confirmed why my two live runs differed (see Deviations). Lint and the full test command pass, and the logged-out repro now saves the real course.

**Toolkit**

- Tool: Claude Code, keeping a facts-only evidence file (`docs/issue-2008-repro-evidence.md`) and project memory up to date as findings changed.
- What it produced: a dated record that let me catch the 2026-11-18 "nothing reaches the server" conclusion being overturned by my 2026-11-21 run.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Pull Request

**PR Link:** Not opened yet. The fix is on local branch `fix/2008-wait-for-loading-course` (commit `b7663c5a5`, not pushed), waiting on my own review of the code.

**PR Description:** Not written yet.

**Maintainer Feedback:**

- 2026-11-18: On Discord, Anthony Jenanian said my first pick (#2101) was already assigned internally and suggested checking back after he updated GitHub.
- 2026-11-20: Anthony okayed me taking #2008 on Discord.
- 2026-11-21: Posted my claim and reproduction on #2008 (https://github.com/icssc/AntAlmanac/issues/2008#issuecomment-7067955608).
- 2026-11-22: Anthony assigned #2008 to me on GitHub. No reply yet to my question about which fix approach they prefer.

**Status:** Not opened. Assigned to me on 2026-11-22; the fix is built and tested locally, and my review comes before the PR.

**Toolkit**

- Tool: PR-monitor. Not installed or run yet, because no PR is open.
- What it produced: nothing yet.
- Difficulties: TODO(you)
- What changed: TODO(you)

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

- AntAlmanac README, "Contributing": the comment-then-wait-for-assignment rule and the local setup.
- ICSSC contributor guidelines (https://docs.icssc.club/docs/contributor/common/guidelines): the AI-use and disclosure rules I followed in my comment.
- Awesome-E's 2025-12-17 comment on #2008 (https://github.com/icssc/AntAlmanac/issues/2008): it said the Application Error was probably gone, suggested checking the Network tab for `Loading...`, and named the two fixes.
- ICSSC Discord, where Caden Lee and Anthony Jenanian explained that issues are assigned internally before GitHub shows it, and okayed #2008.
