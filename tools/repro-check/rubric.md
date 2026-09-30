# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check           | Evidence                                                                                                                                                            | Pass condition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Weight    |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| version-pinned  | Repro report: environment/setup section or first lines. Issue: body, version field, and thread for the affected/confirmed version. Repo-facts block: latest release | Pass if the report names the version it tested and that version is the issue's affected version, the latest release, or main. Any other version passes only if the report points out the difference from the issue's version. Fail if no version is named, or if the tested version differs from what the issue targets (especially an older one) and the report doesn't say so                                                                                                                                                                                                                                                                                                                                               | required  |
| runnable-steps  | Repro report: steps section and code blocks, read against the issue's own reproduction                                                                              | Pass if a stranger could re-run every step using only the report, the issue, and public resources. Every file, config, and input the steps depend on is shown inline, taken from the issue (e.g. "the issue's script verbatim"), publicly available, or described precisely enough to recreate without guessing: the contents that trigger the bug are named (e.g. "a minimal env.yml with a valid dependencies list plus a category: section"). Fail if there are no steps, if a stranger would have to guess what goes into a file or input, or if the reproduction depends on private or unshared code, config, or data, including when the report says it can't share them or that the bug did not reproduce outside them | required  |
| input-match     | Repro report: steps and code blocks, read against the issue's reproduction and any maintainer comment naming the trigger                                            | Pass if the report's input keeps every condition the issue (or a maintainer) names as the trigger. Incidental differences are fine: file names, flags that only print or stay offline, a smaller input that keeps the trigger. Fail if the trigger itself changed or was dropped (different operator or syntax, missing flag, different code path), unless the report says so and presents the run as a cannot-reproduce, not a confirmation                                                                                                                                                                                                                                                                                  | required  |
| output-match    | Repro report: output excerpts and code blocks, read against the error or behavior the issue describes                                                               | Pass if the output shows the same failure the issue describes (same error type or message, same wrong value), or if the report is an honest cannot-reproduce that shows what happened instead and says what differed. Fail if the output shows a different or adjacent symptom but the report presents it as confirming the issue, or if the output is trimmed or differs from the issue's and the report doesn't say so                                                                                                                                                                                                                                                                                                      | required  |
| claim-specific  | Claim comment, read against the issue and the repro report                                                                                                          | Pass if the comment names something specific to this issue (what was reproduced, where the investigation will start) and promises nothing the evidence can't back. Fail on text that could be pasted onto any issue, guaranteed timelines, or requests to reserve or assign the issue                                                                                                                                                                                                                                                                                                                                                                                                                                         | required  |
| AI-policy       | Repo-facts block (live: CONTRIBUTING.md, AI_POLICY.md, templates) for the policy. Claim comment and repro report for the disclosure                                 | Treat the underlying work as AI-assisted. Fail if the repo bans AI-assisted contributions outright, or requires disclosure and the comments don't disclose in the form the policy asks for (e.g. tool + extent). If the policy only requires human-written comments, pass when the comments are specific and first-person rather than boilerplate. Pass if no policy exists. Strong proof elsewhere doesn't offset a fail here                                                                                                                                                                                                                                                                                                | required  |
| environment     | Repro report: environment/setup section                                                                                                                             | OS, runtime version, and relevant config are listed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | preferred |
| expected-actual | Repro report: summary or expected/actual lines, usually near the top or right after the output                                                                      | States what should have happened and what actually did happen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict. In live claim-only mode, checks marked "not yet applicable" are left out of the verdict.
