# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's diagnosis (stated cause) and planned change, read against every step, artifact, and control run in the repro-evidence block. Thread claims about the cause are context only; the repro evidence outranks them | Pass if the plan names a cause and that cause is consistent with every repro step and control: nothing in the evidence shows the bug happening without the blamed component, before the blamed code runs, or with the blamed thing already working. Fail if any control or step rules the named cause out, if the diagnosis only restates the symptom, or if the planned change works around or documents the symptom while the evidence or thread isolates a fixable defect, unless the plan says why the real fix is out of reach | required |
| change-bounded | The plan's scope (in-scope and not-in-scope statements) and every item in its approach, read against what the issue reports and the repro reproduces | Pass if every planned change is needed to fix the reproduced behavior or to test that fix (small, directly related docs updates count). Explicitly deferring related work to a follow-up is a pass, not a fail. Fail if the plan also commits to work the issue never asked for: dependency or library migrations, rewrites or redesigns of the surrounding component, new user-facing options or features, CI changes, porting to other components, or "while I'm in there" cleanups, even when the core fix inside it is correct | required |
| executable | The plan's approach and named files/functions, read as a stranger starting tomorrow | Pass if a stranger could start the first edit without making a design decision: the plan picks one approach and names where it lands (a file, function, or code site the evidence or thread points to). Fail if the location or layer is left open ("somewhere", "X or Y, not sure", "whichever is easier"), if the first step is open-ended investigation or profiling with no chosen change, or if no files or code site are named at all | required |
| decisive-test | The plan's test plan, read against the repro evidence's steps and expected/actual lines | Pass if the test plan names at least one observable outcome that fails before the fix and passes after it: the repro re-run with its expected result, or a regression test built from the issue's input. Fail if the only test is "run the existing suite / nothing regresses", a feeling ("it should be quicker", "nothing seems broken"), or no test at all | required |
| thread-direction | The candidate plan comment and plan, read against the thread highlights, especially OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR comments | Pass if there is no explicit maintainer direction in the thread, or if the comment engages each piece of it: a named culprit location, a preferred or rejected approach, a patch or test build to try, or an open PR. Engaging means following it, or naming it and saying why the plan differs. Fail if the comment goes in a different direction without acknowledging it, or proposes something the maintainer already rejected | required |
| ai-policy | Repo-facts block's contribution policy (live: CONTRIBUTING.md, AI_POLICY.md), read against the candidate plan comment. Treat every package as AI-assisted work | Pass if the repo states no AI policy, or the policy's requirements for issue comments are met. If the policy requires disclosing all AI usage (not only in pull requests), the comment must disclose the tool and the extent of the help. If the policy asks only for disclosure in the pull request, the comment does not need it. If the policy requires comments in the contributor's own words, the comment must be first-person and specific to this issue, not boilerplate. Fail if a required disclosure is missing from the comment, or if the repo bans AI-assisted contributions | required |
| unknowns-stated | The plan's risks/unknowns, read against what the repro evidence leaves untested | Pass if the plan names what it has not verified (a platform, a performance cost, another code path) instead of presenting untested parts as certain | preferred |

## Verdict rule

Accept (ready) if every required check passes. Reject (hold) if any
required check fails or is graded unclear: a plan whose required check
cannot be confirmed from the package is not ready to build from.
Preferred checks never change the verdict. A check whose condition
says "pass if there is no X" (no maintainer direction, no AI policy)
passes when the package shows no X; that is a pass, not an unclear.
