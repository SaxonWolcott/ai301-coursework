---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, description, commits, diff, and test evidence),
read against the plan it claims to implement (including that plan's
deviation and deferral notes) and the issue that plan belongs to,
under the repo's stated PR template and contribution policy.

Do not answer any other question. Do not review code style for its own
sake, suggest a better fix, re-grade the plan, or grade more than one
package per run. You answer by executing `procedure.md`, which applies
the checks in `rubric.md` to evidence located per
`references/evidence-guide.md`, never from gut feel.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (an eval
package markdown file or its text), you are in eval mode. If you were
asked to grade the student's own draft PR in a working copy, you are in
live mode.

**Live mode** reads exactly these inputs, from the top folder of the
student's fork clone, on their branch:

- **Plan**: `plan.md`, including any `## Deviations` section. Deviations
  recorded there are part of the plan and are graded as such.
- **Diff**: the output of `git diff main...HEAD` (three dots), run in
  the working copy. This is every committed change the branch makes
  relative to `main`. Also read `git log main..HEAD --oneline` for the
  commit messages. Uncommitted or unstaged changes are not part of the
  PR: do not grade them, but note in the summary if `git status` shows
  modified tracked files outside `plan.md`, `comment.md`,
  `pr_draft.md`, and `test_evidence.md`.
- **Title and description**: `pr_draft.md`. The first line is the PR
  title; everything after it is the description.
- **Test evidence**: `test_evidence.md`, plus any before/after or check
  output pasted into the description.
- **Issue**: the issue URL the student gives. Fetch the issue body and
  its thread (via `gh issue view <URL> --comments`, the GitHub API, or
  the web).
- **Repo standards**: the repo's PR template
  (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, read
  from the working copy's `main` (`git show main:<path>`) or from the
  repo on GitHub. These are the live equivalent of the bundle's
  repo-facts block. Path Review's course rule also applies: every PR
  discloses AI use, in its own words, under Notes for Reviewers.

A house-chain student (one routed to the instructor's house issue)
reads the house plan and house repro pack in place of their own plan
and repro; the diff, draft, and evidence are still their own, and the
same checks grade the same things.

If a live input is missing (no `plan.md`, no `pr_draft.md`, no
`test_evidence.md`, or an empty diff), do not refuse: grade it, and
the checks that read that input fail with the absence as their
evidence. The one exception is the issue URL: without it you cannot
apply the scope seam, so stop and ask for it.

**Eval mode**: the bundle is the whole world. Every fact comes from
the bundle text: its repo-facts block, issue, thread highlights, plan
context, and candidate PR (title, description, commits, diff, test
evidence). Fetch nothing, run no commands against any repo, and read
no other files except this skill's own components. Eval mode always
grades a complete package: every check in `rubric.md`, then the full
verdict rule. The bundle id (for example `pkg-07`) is the `item`.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else.

- If its `Repo:` line still holds a bracketed placeholder such as
  `<ORG>/<PATH-REVIEW-REPO>`, stop without grading. Say that the scope
  file has not been finalized and the student should get their
  cohort's Path Review repo from the instructor. Emit no verdict.
- Refuse to grade a PR whose issue is not in the scoped repo, or whose
  branch would not target that repo. Say which repo is in scope and
  stop. Never guess or widen the scope.
- Apply its house rules while reading evidence: the PR comes from the
  student's own fork branch, delivers one plan for one issue, and uses
  the repo's template in every section. A classmate's PR on the same
  issue is not a problem and is not grounds to fail anything.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading the checks, read `voice-guide.md` in this
skill directory and hold the outgoing PR text (the title and the
description in `pr_draft.md`) against each rule and each "never post"
item. For every rule the draft breaks, report it in the summary under a
"Voice" heading: name the rule and quote the offending line. If none
are broken, say so in one line.

The voice guide never changes the verdict and never adds a row to the
JSON `checks` array, because no check in `rubric.md` reads it. It is
the student's own style, reported for them to act on.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, then work:

1. `rubric.md`: the checks (name, evidence, pass condition, weight) and
   the verdict rule. It is the only source of checks. Do not add,
   drop, rename, or merge checks.
2. `references/evidence-guide.md`: where each evidence family (plan
   fidelity, test evidence, diff quality, standards and comms) lives in
   a bundle and in a live working copy, and what good looks like. Use
   it to locate evidence for each check.
3. `procedure.md`: the operating steps (read order, evidence
   gathering, check execution, verdict assembly). Execute it as
   written, step by step.

Where the procedure is silent on something you need to do, do not
improvise a step around it: take the most literal reading of the
rubric's pass condition, and record the gap in the summary under a
"Procedure gaps" heading so the student can fix it.

Refusal rule: if `rubric.md` has no filled check rows or no verdict
rule, or `procedure.md` has no steps beyond its template comments,
stop and say which file is empty. Do not invent checks or steps, and
do not emit a verdict. An empty tool that grades anyway produces noise
that looks like judgment.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject` means
hold. There is no third verdict, no "accept with reservations", and no
score. Reservations go in a check's evidence line, not the verdict.

Before the JSON, print a short readable summary: one line per check
(name, grade, the deciding fact), then in live mode the Voice notes and
a concrete fix list for every failed check, then any Procedure gaps.

End your reply with this fenced JSON block, with exactly one entry per
`rubric.md` check in table order, each `evidence` a single line that
quotes or names the deciding fact. The block must be valid JSON, and
nothing may follow it. In live mode `item` is the issue URL (or the PR
URL if one exists); in eval mode it is the bundle id.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact,
  quote, hunk, or absence that decided it. "Looks fine" is not
  evidence.
- **Read the diff itself.** Check every hunk of the diff against the
  plan; never take the description's word for what the diff contains.
  A confident "implements the plan exactly" is a claim to verify, not
  evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready, and a beautiful confident one can hide drift or untested
  claims. Length, formatting, and enthusiasm never move a grade.
- **Honest shortfalls are not failures.** A gap the plan or
  description discloses in plain words is graded as disclosed; the
  same gap left silent is a fail.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes; mention the tension in
  the summary. The fix belongs in the rubric, not the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Apply the rubric's verdict rule to
  `unclear` grades. Where it is silent, a claim you cannot verify from
  the package counts as failing: a PR you cannot verify is not ready
  to submit.
