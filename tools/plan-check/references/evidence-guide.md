# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

Where it lives:

- Eval bundle: the plan's cause sits in the `## Candidate plan`
  section, under a heading or label like "Diagnosis", "Cause", or
  "Root cause", or in the first sentences of the plan when there is no
  label (calib-01 uses a bare "Cause:" line). The behavior it must
  explain lives in the `## Repro evidence` block: the numbered steps,
  the artifact (output, stack trace, timings), the `Control:` line or
  lines, and the `Expected:` / `Actual:` lines. The issue body's stack
  trace or reduced case and the thread highlights add context, but
  thread claims about the cause are opinions; the repro block is the
  observation.
- Live mode: the cause is in the draft `plan.md`. The repro evidence
  is the student's posted repro comment on the issue (or the house
  repro pack as quoted in the drafts).

What good looks like: the stated cause names a mechanism (a specific
code path, value, or missing step) that would produce every step's
artifact, and no control contradicts it. A control that shows the bug
without the blamed component, or the blamed component working
correctly, rules the cause out. Step output
that shows the damage already done before the blamed code runs also
rules it out. The planned change should alter the code that
causes the bug. A docs note or a workaround is not a fix when the
evidence or thread has already isolated a fixable defect.

## Scope

Where it lives:

- Eval bundle: in `## Candidate plan`, the scope statement ("Scope",
  "In scope", "Change", "In:") and the not-in-scope line ("Not in
  scope", "Out:", "Deferred", "follow-up"), plus every numbered item
  in the approach. Items tucked into the approach or the comment
  ("while I'm in there", "also") count as scope too. Read them against
  the issue's title and body: that is what the fix was asked for.
- Live mode: the same parts of `plan.md` and the draft comment.

What good looks like: every committed item is needed to fix the
reproduced behavior or to test the fix, and anything bigger is named
as out of scope or a follow-up. A plan that honestly scopes down and
defers related work still passes. Scope creep is a committed migration, library
upgrade, component rewrite, new option or feature, CI change, or port
to another component, even when the core fix inside it is right.

## Executability

Where it lives:

- Eval bundle: in `## Candidate plan`, the approach or steps, any
  backticked file paths and function names, and the in-scope line,
  which often names the code site (calib-01: "the push completion
  callback in `pkg/gui/controllers/sync_controller.go`"). The thread
  highlights may name a culprit file the plan can point to.
- Live mode: the same parts of `plan.md`.

What good looks like: the plan has picked one approach and says where
it lands (a file, function, or named code site), so a stranger could
open that file and make the first edit without choosing anything. It
is not executable if the layer or location is open ("library A or
library B, not sure", "somewhere", "whichever is easier"), the
first step is open-ended investigation or profiling, or no code site
is named at all.

## Test plan

Where it lives:

- Eval bundle: in `## Candidate plan`, the "Test plan", "Test", or
  "Verification" part, or test-related approach steps ("add the
  issue's cases as regression tests"). Hold it against the repro
  block's steps and its `Expected:` / `Actual:` lines.
- Live mode: the test plan in `plan.md`, held against the student's
  Unit 2 repro steps.

What good looks like: it names at least one result you could watch
fail today and pass after the fix: the repro re-run with its expected
output ("at step 3 the color must flip", "exit 0"), or a regression
test built from the issue's input. "Run the full test suite and make
sure nothing regresses" (calib-04) and a feeling ("it should be
quicker", "nothing seems broken") name nothing observable for this
fix and are not decisive.

## Honesty

Where it lives:

- Eval bundle: in `## Candidate plan`, a "Risks", "Unknowns", or
  "Open questions" part, or risk sentences inside the approach
  ("I have not yet measured the cost of X"). Compare
  against what the repro block did not cover (other platforms,
  performance, other code paths).
- Live mode: the risks and unknowns section of `plan.md`, and its
  `## Deviations` section after the build. A deviation is honest only
  if it is written in `plan.md`; a change that only shows in the diff
  is not recorded.

What good looks like: the plan says what it has not verified and what
it will do if that goes wrong, instead of presenting untested parts as
certain. A long, confident plan with no stated unknowns is not more
ready than a short one that names them.

## Comms

Where it lives:

- Eval bundle: the `## Candidate plan comment` section is the words
  being graded. Hold it against `## Thread highlights` (comments marked
  OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR carry maintainer
  direction) and against the `## Repo facts` block's
  "contribution policy" line (the AI policy) and "bug reports" line.
- Live mode: the draft comment, held against the live issue thread
  and the repo's CONTRIBUTING.md, AI_POLICY.md (or similar), and issue
  templates.

What good looks like:

- Thread: the comment engages each piece of maintainer direction, by
  following it or by naming it and saying why the plan differs. This
  covers a culprit file the maintainer named, a preferred or rejected
  approach, a patch they asked people to test, and open or prior PRs
  (saying it won't race an open PR, or will build on it, counts). It
  fails if the comment heads somewhere else without a word about that
  direction, such as planning a docs workaround after the owner has
  isolated the culprit and posted a patch.
- AI policy: treat every package as AI-assisted. Match the comment to
  the policy's exact ask. "All AI usage in any form must be disclosed"
  means the comment itself must name the tool and the extent; an
  excellent plan still fails without it. A policy asking for disclosure
  only in the pull request does not require it in the comment.
  A policy asking for comments in the contributor's own words needs a
  first-person comment specific to this issue. No stated
  policy needs nothing.
