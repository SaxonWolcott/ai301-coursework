# Procedure: how this skill grades a plan package

These steps grade a plan someone else wrote. They do not write or fix
the plan. Follow them in order; where a step says "record", write the
fact down (with a short quote) before moving on, because later checks
read those notes.

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL is in
   the scoped repo; if it is not, or the `Repo:` line is still a
   placeholder, stop and say so. Note the house rules. Eval mode: skip
   this step.
2. Read `rubric.md` and list its checks, their weights, and the
   verdict rule. Read `references/evidence-guide.md` so you know where
   each check's evidence lives.
3. Read the issue (title, body, any stack trace or reduced case).
   Record in one line what behavior is wrong.
4. Read the repro-evidence block next, before the plan. Record:
   (a) the steps and what each one showed, (b) every control run and
   what it rules in or out, (c) the expected and actual lines. The
   evidence is read before the plan so the plan's diagnosis is judged
   against what was observed, not the other way round.
5. Read the thread highlights. Record every comment from an OWNER,
   MEMBER, COLLABORATOR, or CONTRIBUTOR that gives direction: a named
   culprit file or line, a preferred approach, a rejected approach, a
   patch or test build to try, or an open or prior PR. Record claims
   about the cause separately; they are context, and step 4's evidence
   outranks them.
6. Read the repo-facts block. Record the AI policy's exact
   requirement: none, disclosure of all AI usage, disclosure only in
   the pull request, comments in the contributor's own words, or a ban.
7. Read the candidate plan, then the candidate plan comment, in full.
   Do not grade while reading. Length, polish, and confidence are not
   evidence: a short plan can be complete and a long one can be wrong.

## Evidence gathering

For each check, pull exactly this and record it with a quote:

1. **cause-grounded**: quote the plan's stated cause (the sentence
   that says why the bug happens). Then, for each control and step
   recorded in Read order step 4, write one line: "consistent" or
   "rules it out", with the reason. Also record what the planned
   change actually alters: the defect's code, or only docs, a
   workaround, or the symptom.
2. **change-bounded**: list every item the plan commits to doing
   (approach steps, in-scope lines, "also" items). For each, write
   "needed for the fix or its test", "deferred/out of scope", or
   "extra" (a migration, rewrite, new option or feature, CI change,
   port to another component, or cleanup the issue didn't ask for).
   Items under "not in scope" or "follow-up" are deferred, not extra.
3. **executable**: quote the files, functions, or code sites the plan
   names, and the single approach it picks. Quote any phrase that
   leaves a decision open ("somewhere", "or", "not sure", "whichever",
   "investigate first", "profile").
4. **decisive-test**: quote the test plan. Write down the specific
   observable result it expects after the fix (an exit code, an output
   line, a test that passes) and whether that same check fails today
   per the repro's actual line.
5. **thread-direction**: for each piece of maintainer direction
   recorded in Read order step 5, quote the part of the comment (or
   plan) that engages it, or write "not engaged".
6. **ai-policy**: put the policy requirement from Read order step 6
   next to the comment. Quote any disclosure in the comment (tool and
   extent), or write "no disclosure". Note whether the comment is
   first-person and specific to this issue.
7. **unknowns-stated**: quote the plan's risks or unknowns section,
   or write "none stated".

Live mode: the drafts (`plan.md`, the draft comment) are the candidate
side. Take the issue, thread, and repo policy from GitHub as the
evidence guide says, and take the repro evidence from the student's
posted repro comment (or the house repro pack as quoted in the drafts).
Eval mode: use only the bundle text; fetch nothing.

## Check execution

1. Run the checks in rubric order, one at a time, using only the
   notes recorded for that check in Evidence gathering. Re-open the
   package only to confirm a quote, not to look for new reasons.
2. Apply the check's pass condition literally to the notes:
   - cause-grounded fails if any control or step was marked "rules it
     out", if the stated cause only restates the symptom, or if the
     change only works around or documents the symptom while the
     evidence or thread isolates a fixable defect (unless the plan says
     why the real fix is out of reach).
   - change-bounded fails if any item was marked "extra", even when
     the core fix is right. Deferred items never fail it.
   - executable fails if any open-decision phrase covers where or how
     the main change lands, or no code site is named.
   - decisive-test fails if no observable before/after result was
     recorded ("suite passes", "nothing regresses", or a feeling do not
     count).
   - thread-direction fails if any piece of maintainer direction is
     "not engaged", or the plan does something the maintainer rejected.
     No direction recorded means pass.
   - ai-policy fails if the policy requires disclosing all AI usage
     and the comment has no disclosure of tool and extent, if the
     policy requires own-words comments and the comment is generic
     boilerplate, or if the repo bans AI-assisted work. No policy, or a
     policy that only asks for disclosure in the pull request, means
     pass on disclosure.
3. Grade `unclear` only when the evidence the check needs is truly
   absent from the package (for example, the repro block has no
   expected/actual line to compare a test against). If a condition is
   written as "pass if there is no X" and the package has no X, grade
   pass, not unclear.
4. Write one line of evidence per check: the quote or fact that
   decided the grade. A check graded fail names the failing item.
5. Grade each check independently. A strong result on one check never
   offsets a fail on another.

## Verdict assembly

1. Take the required checks only. If every one is `pass`, the verdict
   is `accept`. If any is `fail` or `unclear`, the verdict is
   `reject`.
2. Preferred checks (unknowns-stated) are reported but never change
   the verdict.
3. In the summary, list each check's grade. For a reject, name every
   failing or unclear required check and quote the evidence that
   decided it. For an accept, quote the evidence for cause-grounded and
   decisive-test.
4. Live mode: after the verdict, compare the draft comment with
   `voice-guide.md` and quote any rule it breaks. This does not change
   the verdict.
5. End with the JSON block from SKILL.md as the last thing in the
   output: every check with its grade and one-line evidence, and the
   verdict.
