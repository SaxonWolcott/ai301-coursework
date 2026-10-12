# Evidence guide: where evidence lives in a PR package

An eval bundle has these sections, in order: a header (source,
captured date), `## Repo facts` (the "pull requests" line holds the
template and contributing asks; the "contribution policy" line holds
any AI policy), `## Issue`, `## Thread highlights`, `## Plan context`
(the repro evidence the plan built on, then the plan itself with its
scope, "Not in scope", files, deferral notes, and test plan), and
`## Candidate PR` with `### Title`, `### Description`, `### Commits`,
`### Diff`, and `### Test evidence`.

In live mode the same evidence lives in the top folder of the
student's fork clone, on their branch: `plan.md` (with
`## Deviations`), `git diff main...HEAD` and `git log main..HEAD`,
`pr_draft.md` (first line is the title), `test_evidence.md`, the issue
thread on GitHub, and Path Review's `.github/PULL_REQUEST_TEMPLATE.md`
and `docs/CONTRIBUTING.md`.

## Plan fidelity (harness category: silent-drift)

Where it lives:

- Eval bundle: the plan's promise is in `## Plan context`, after
  "Plan:": the one-line change, "Scope, one bounded change: ...",
  "Not in scope: ...", "Files: ...", and any "Deliberately deferred"
  or "Known limit" note. What actually changed is the `### Diff`
  block: read each `--- a/` / `+++ b/` file header and each `@@` hunk.
  The fidelity claims are in `### Description` (phrases like
  "implements the posted plan exactly", "no changes beyond the plan",
  "no functional changes outside", "docs now document").
- Live mode: the promise is `plan.md`, including everything under
  `## Deviations`. The change is `git diff main...HEAD` (use
  `git diff main...HEAD --stat` for the file list first). The claims
  are the Summary and Changes sections of `pr_draft.md`.

What good looks like: every hunk is the planned change, its test, an
artifact the repo requires for any change (changelog, i18n string), or
work named in a deviation note; every deliverable the plan promises is
in the diff or its absence is disclosed; and every claim in the
description is true of the diff. Silent drift in the "more" direction
is an unplanned hunk (pkg-03's `truncate()` rewrite and new config
option, calib-03's second hunk in a file the plan never names); in the
"less" direction it is a promised deliverable missing with no note
(pkg-17's docs update). Either is worse when the description claims
fidelity. An honest deferral (pkg-13's `%VAR%` note, pkg-16's
start-path note, stated in the plan and restated in the description)
re-ties the mismatch and passes.

## Test evidence (harness category: not-tested)

Where it lives:

- Eval bundle: the plan's test plan is the "Test plan:" sentence at
  the end of `## Plan context` (the repro to re-run, its
  expected-after, the controls, and the suite command). The issue's
  failing input is in `## Issue` and the plan's "Repro evidence"
  paragraph. The proof is `### Test evidence`, plus any Before/After
  blocks in `### Description` (pkg-16 puts its before there).
- Live mode: the test plan is in `plan.md`. The proof is
  `test_evidence.md` and the Testing section of `pr_draft.md`: the
  unit 2 repro re-run on `main` (before) and on the branch (after),
  and the pasted results of `make test-unit`, `make test-integration`
  ("no tests ran" is expected), `make lint`, and `make typecheck`.

What good looks like: the plan's repro is re-run on the issue's own
failing input, with observable output both before and after (pasted
text, exit code, error message, or a concrete UI step result like
pkg-08's prompt-vs-toast flow), every repro the test plan names is
covered, and the repo's suite is reported as a named command with its
result. Not tested looks like prose instead of output (pkg-04 "colors
work now", pkg-10 "verified working on my machine"), a run on the path
the fix does not change (pkg-07's single-file control, pkg-14's GET
instead of the issue's POST), or one of two named repros silently
skipped (calib-04). An honestly reported failing check, with its
reason, is still evidence.

## Diff quality (harness category: unreviewable)

Where it lives:

- Eval bundle: every `+` and `-` line in `### Diff`, and the messages
  in `### Commits`.
- Live mode: every line of `git diff main...HEAD` and the messages in
  `git log main..HEAD --oneline`. `git diff main...HEAD --stat` should
  list only the files the fix needs.

What good looks like: every changed line is the fix, its test, or a
comment explaining the fix, so a reviewer sees the change and nothing
else. The debris tells: debug output left in (pkg-15's `DEBUG`
`eprintln!`, a stray `print(`), commented-out code or a commented-out
first attempt (pkg-12), dead functions (pkg-12's `_unused_buffer_probe`,
pkg-15's `count_open_fds` behind `allow(dead_code)`), new TODO notes,
import restructuring, re-indentation of untouched code, lines removed
and re-added identically (pkg-18), and hunks in files unrelated to the
fix. Commits named "wip", "fmt", or "misc cleanups while debugging"
point at debris but are not debris themselves. Path Review specific:
do not remove a `# noqa: <RULE>` annotation as a drive-by cleanup;
`docs/CONTRIBUTING.md` says those are deliberate and lint fixes go in
their own PR.

## Standards and comms (harness category: standards-wall)

Where it lives:

- Eval bundle: the "pull requests:" line of `## Repo facts` lists the
  template's sections, checklist, issue-link line, and required file
  entries (changelog, whatsnew); the "contribution policy" line states
  any AI policy. Compliance is in `### Description` (sections, links,
  checklist answers, AI disclosure) and in `### Diff` for required
  file entries.
- Live mode: `.github/PULL_REQUEST_TEMPLATE.md` asks for Summary,
  Issue (`Closes #N`), Changes (one bullet per change), Testing (a
  checklist: tick only true boxes; leave "CI is green" unticked,
  since CI has not run; tick the xfail box only if the diff removes
  the seeded bug's `@pytest.mark.xfail` marker), Screenshots / Demo
  (content or "N/A" with a reason), and Notes for Reviewers.
  `docs/CONTRIBUTING.md` says incomplete templates are sent back. The
  course requires an own-words AI-use disclosure in Notes for
  Reviewers, plus any deviation from the plan.

What good looks like: every unconditional ask is met with real
content (an inline "Problem:" / "Solution:" label counts as a
section), every required file entry is actually in the diff, and any
AI policy's requirement is met (pkg-11 states human review in its own
words, which is what its policy asks). A repo with no template and no
AI policy asks for nothing here, so a terse description passes
(pkg-19). A wall looks like a required checklist visibly ignored
(pkg-01: no `closes #`, no checklist, and a whatsnew entry the
template requires missing from the diff) or a required disclosure
absent (pkg-20: the policy requires naming the tool and extent of all
AI use, and the description says nothing).
