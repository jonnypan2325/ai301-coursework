# Evidence guide: where proof lives in a reproduction package

In eval mode, use only the frozen package's Issue, Repo facts, Thread highlights, Candidate claim comment, and Candidate repro report. The source issue may have changed since capture, so do not replace package facts with live GitHub facts. In live mode, use the actual issue, repository instructions, and supplied draft comments. Files merely sitting elsewhere in the working directory do not count as evidence shown to a reader of the comments.

## Environment

**Where it lives:** In an eval bundle, find the repro report's environment statements and compare them with the Issue and Repo facts, especially the bug template's requested fields. In live mode, compare the draft with the issue's environment and the repository's bug template, README and setup instructions.

**What good looks like:** A reader can identify the tested release or source revision, OS, and the runtime, installation method or configuration needed for this particular trigger. Explicitly requested environment information is supplied. An explained test on a newer release or different OS is legitimate when the report describes only what was tested. Do not infer that the candidate used the issue author's environment just because the issue provides it. Do not demand irrelevant machine detail.

## Steps

**Where it lives:** Read the candidate report's commands, UI actions, input documents, setup references, flags and configuration. Compare them with the trigger in the Issue and relevant Thread highlights. Live mode uses the same comparison against the GitHub issue and the draft; inspect referenced setup documentation when necessary.

**What good looks like:** Starting state, essential inputs and ordered actions are supplied or explicitly referenced so a stranger can repeat the attempt. Preserve semantically important punctuation, flags, file content, and state: a changed input that fails parsing before reaching the bug is not equivalent. An explicit reference to a complete unchanged issue recipe can avoid repeating it; vague "did the same thing" wording cannot replace missing essential steps. Grade repeatability, not formatting or length.

## Behavior shown

**Where it lives:** Find the report's actual terminal output, traceback, log excerpt, screenshot content, before/after UI observation or named test result. In eval mode, the artifact available inside the bundle is the evidence; an unsupported "attached" claim is not an unseen artifact. In live mode, inspect linked accessible artifacts or content included in the draft. Compare the result with the issue's specific failure, including exception, exit status or visual change when those distinguish it.

**What good looks like:** The artifact is connected to the documented command/action and shows the relevant outcome. A reproduced result matches the reported failure, rather than merely showing an error. A cannot-reproduce result documents a faithful attempt and the actual output or state that did occur; a useful control can distinguish successful execution from failure to reach the trigger, but a fixed number of controls is not mandatory. For known expected-failure tests, state whether pytest reported XFAIL or whether the marker was bypassed to expose the failure; XFAIL does not mean the bug is fixed.

## Honesty

**Where it lives:** Compare the claim's promises and the report's headline, expected/actual statements, diagnosis and conclusion with the artifacts and environment. In live mode, evaluate the text a maintainer will actually see; do not substitute private notes for missing evidence in a posted claim.

**What good looks like:** Clearly distinguishes observed facts, hypotheses and intended next work. Does not call a different failure a confirmation, generalize a local run to all versions or users, or claim personal verification that did not occur. Evidence-backed inability to reproduce is useful and can be ready to post. If the attempt never reaches the reported trigger, describe the setup blocker honestly and continue gathering evidence before treating it as a completed repro package. A claim written before the run describes the plan, not invented results.

## Comms

**Where it lives:** In eval mode, read the Candidate claim comment against the Issue and read both comments against the Repo facts' template and contribution policy, including AI requirements. In live mode, read the issue/thread, the repository's README, contribution guide and templates, the installed scope and voice guide, and the actual drafts.

**What good looks like:** The claim names a distinguishing behavior/component/input and a useful next action; comments are specific and constructive rather than generic requests, pressure or blame. Follow explicit repository information/disclosure requirements; do not invent an AI disclosure policy where none is stated. Course evaluation candidates are treated as AI-assisted work, as stated by the instructor gold-label notes: a missing disclosure cannot establish that AI was not used. Compare the candidate comments directly with any policy requiring disclosure (including tool/extent details when required). A permissive policy asking contributors to take responsibility is not automatically a disclosure rule. This course premise is about the supplied evaluation candidates, not a claim that every public GitHub contributor uses AI. Distinguish new-issue administrative checkboxes from requirements that actually apply to a reproduction comment on an existing issue. Evaluate the personal voice guide only in live mode. In the scoped Path Review classroom repository, classmates' claims do not block another claim, but every contributor supplies their own evidence and words.
