# Contribution: Make the latency flush scheduler testable and cover its error-isolation arms

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Saxon Wolcott
**GitHub Username:** SaxonWolcott
**Issue:** https://github.com/nimbus-agent/Nimbus/issues/976
**Pull Request:** not opened yet

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

**Unit 5: turned my issue-select skill into issue-scout and wrote its search strategy**

I wrote three `gh search issues` queries from my fit profile (Python
and TypeScript good-first-issues in AI and dev-tooling orgs, plus a
test-focused query) and eight ranking filters. My first scan graded
five issues and my rubric rejected all five on `not-taken`: each one
already had open PRs, which my filters couldn't see because search
results don't include linked PRs (three of the five even had zero
comments). I added `-linked:pr` to every query, which showed that
almost every good-first-issue in those AI orgs gets a PR within days
(results dropped from 38 to 5 and from 30 to 0). So I moved to the
Issue Selection site and checked its 893 Python/TypeScript issues for
hidden claims before grading any of them.

**Unit 6: installed the voice guard and wrote a repo-memory file for Nimbus**

On Windows the voice guard broke three ways. Git Bash couldn't create the
symlinks, so I used the pointer-file fallback. The hook command's `$HOME`
never expanded, so the hook failed without logging anything until I gave it
an absolute path. And Claude Code sometimes ran commands through PowerShell,
which the Bash-only hook never saw, so I extended both the matcher and
`check_voice.py` to cover PowerShell. After that, my Nimbus#976 claim went
through the hook and logged a `pass` in `guard-log.md` on Nov 7. For
`CLAUDE.local.md` I dropped the `@AGENTS.md` import, because Nimbus ships its
own CLAUDE.md instead. I also skipped the orientation agent, since the
subsystem is only four files, read them directly instead, and kept the notes
to 46 lines.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My scout's own GitHub search came up empty: all five issues it graded
already had PRs, and after I filtered out linked PRs the AI orgs I'd
targeted had almost nothing left. So I pulled the Python and TypeScript
good-first-issues from the Issue Selection site, filtered out the ones
with linked PRs or claim comments, and graded three survivors:
yamada-ui#7988, nimbus-agent/Nimbus#976, and graphql-hive#1601. All
three were accepted. I first claimed yamada-ui#7988 on Nov 4, but on
Nov 6 the maintainer folded it into his own PR (#8006) with a related
issue and merged it the same day, so I had to re-pick. I re-graded
Nimbus#976 with my patched rubric on Nov 6 and again checked the
thread on Nov 7: still accepted, with the difficulty note at
**moderate**. That means one source file and two test files, a design
choice I have to make and explain (how to make the 30-second timer
tick reachable from a test), and a per-file branch-coverage floor of
80% that the file only clears by 2.4 points. It fits me because it's
TypeScript, my strongest language, in a large codebase, and the whole
issue is about testing well, which is the thing I most want to learn.
I want to come out of it knowing how to add a test seam without
changing behavior, how Istanbul measures line and branch coverage, and
how to "red-prove" a test so I know it can actually fail.

The scout held up on the issues it graded, but a real search showed
its gaps. The search step couldn't see linked PRs, so I added
`-linked:pr` to every query. The old `reasonable-scope` check only
looked at product decisions, so I replaced it with `scope-feasible`
plus a difficulty note. Its `not-taken` check still depends on reading
the whole thread, because claims hide in comments that search and
regex miss. And nothing in it could have predicted a maintainer taking
over the issue himself. It also has no check for review quality:
Nimbus has one maintainer, who merges almost all of his own PRs, so
the review I get will come from one person. If I patch it again, I'd
add a preferred check for at least two distinct human mergers in the
last ten merged PRs. Nimbus also requires signing a CLA that lets them
relicense contributions, which my rubric doesn't look at either.

Claim comment: https://github.com/nimbus-agent/Nimbus/issues/976#issuecomment-6714038487

**Toolkit**

- Tool: issue-scout (my search strategy, rubric, and fit profile).
  I ran its three `gh search` queries first, then used it to grade
  the issues I pulled from the Issue Selection site, and re-graded my
  picks after patching the rubric.
- What it produced: scan 1 rejected all five issues on `not-taken`;
  the site sweep graded yamada-ui#7988, Nimbus#976, and
  graphql-hive#1601 as accept; the Nov 6 re-pick accepted Nimbus#976
  (difficulty: moderate) and rejected yamada-ui#6069 on `not-taken`
  because it was assigned to someone else.
- Difficulties: search results don't show linked PRs, so taken issues
  looked free; `reasonable-scope` let big, vague issues through and
  would have rejected anything with an open design question; and the
  scout can't see claims that sit in comments, review quality, CLAs,
  or a maintainer about to do the work himself (which is how I lost
  yamada-ui#7988).
- What changed: added `-linked:pr` to every query; applied the unit-6
  scope patch, replacing `reasonable-scope` with `scope-feasible` and
  adding the straightforward / moderate / ambitious difficulty note
  (logged in scan-notes.md on Nov 4). I'd still add a preferred
  check for at least two human mergers in the last ten merged PRs.

---

## Understanding the Issue

**Problem Description**

Nimbus buffers query-latency samples in memory and flushes them to
SQLite from `startLatencyFlushScheduler`, which has three flush
triggers: a 30-second timer, the SIGTERM/SIGINT handlers, and a final
flush inside `stop()` at shutdown. Each one wraps the flush in a
`try`/`catch`, because losing telemetry must never crash the gateway or
abort shutdown. The issue says no test proves any of those catches
works. Nothing is broken in the product; what's missing is a test that
would fail if someone made one of those catches rethrow.

**Expected Behavior**

Each error arm has a test that makes the flush genuinely throw (a
sample in the buffer and a database write that fails) and checks the
error is swallowed. Removing any one `try`/`catch` should make a test
fail. For `stop()`, the test should also check that its SIGTERM and
SIGINT listeners are still removed when the flush throws.

**Current Behavior**

On `main` (`7350b29e`), two of the three arms already have that test:
#1603 added `latency-ring-buffer.coverage.test.ts`, which proves the
tick and the signal handler. The `stop()` arm doesn't: if I delete its
`try`/`catch`, all 37 latency-ring-buffer tests still pass, because no
test makes the flush inside `stop()` throw.

**Affected Components**

- `packages/gateway/src/db/latency-ring-buffer.ts`:
  `startLatencyFlushScheduler` and its `stop()`, plus
  `flushLatencyBuffer`, whose two early returns (table missing, empty
  buffer) don't throw.
- `packages/gateway/src/db/latency-ring-buffer.coverage.test.ts`: the
  file that already proves the tick and signal arms, with the
  `breakInserts()` trigger helper.
- `packages/gateway/src/db/latency-ring-buffer.test.ts`: the older
  `startLatencyFlushScheduler` tests, which call `stop()` only against
  a healthy database.
- `packages/gateway/src/platform/assemble.ts:522`: the one production
  call site, which I won't change.

**Toolkit**

- Tool: Claude Code in my Nimbus clone, with `CLAUDE.local.md` loaded,
  reading the four files above and the lcov output from my
  reproduction.
- What it produced: the four answers above, and the finding that the
  issue's problem is two-thirds solved on main.
- Difficulties: the issue text was written before #1603, so its line
  numbers, coverage numbers and "never runs in a test" statements were
  all out of date. Taken at face value, it would have had me build a
  seam and tests that already exist.
- What changed: I wrote these answers from what main does today, not
  from the issue text, and narrowed the work to the `stop()` arm.

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 Home 10.0.26200 on an ARM64 laptop, using Git Bash.
- Runtime: Bun 1.4.4 (Nimbus needs Bun 1.2+, and its CI uses 1.4).
- Project: my fork SaxonWolcott/Nimbus, cloned on `main` at
  `7350b29e` (2026-11-07), then `bun install` (1034 packages, no
  errors).

Bun wasn't installed, so I installed it with
`winget install Oven-sh.Bun`. The installer only updates PATH for new
shells, so my open terminal couldn't find `bun` until I pointed PATH at
the winget package folder. The test run also logs a warning that
`sqlite-vec` is unsupported on win32-arm64; these tests don't use vector
search, so it didn't matter.

**Steps to Reproduce**

1. From the clone root, run the issue's own Istanbul coverage command:
   `rm -rf coverage`, then in `packages/gateway`
   `bun test src/db test/unit/db/latency-ring-buffer.test.ts --preload ../../scripts/coverage/istanbul-register.ts --preload ../../scripts/coverage/report-coverage.ts`,
   then back at the root `bun scripts/coverage/merge-coverage.ts`.
2. Read the `SF:packages/gateway/src/db/latency-ring-buffer.ts` record
   in `coverage/lcov.info` (`LF`/`LH`, `BRF`/`BRH`, and any
   `DA:<line>,0`).
3. Red-prove each error arm the way the issue asks: delete one
   `try`/`catch` in `startLatencyFlushScheduler` (keeping the
   `flushLatencyBuffer` call), run `bun test src/db/latency-ring-buffer`
   from `packages/gateway`, then restore the file with `git checkout`.
4. Observed result: the issue says the file is at 94.2% line (97/103)
   and 82.4% branch (28/34), with the tick and signal-handler bodies
   never run. I got 100% line (91/91) and 91.7% branch (22/24), with
   both bodies run 3 times each. Only one of the three arms is still
   unproven.

**Reproduction Evidence**

- **Commit showing reproduction:** no commit needed. The reproduction
  is a coverage measurement on unchanged upstream `main`, at
  https://github.com/SaxonWolcott/Nimbus/commit/7350b29e28c6142015e20b330a619e49038ac276
- **Screenshots/logs:**

  ```text
  318 pass / 1 skip / 0 fail, 644 expect() calls (319 tests across 22 files)
  SF:packages/gateway/src/db/latency-ring-buffer.ts
  LF:91  LH:91  BRF:24  BRH:22   (no DA:<line>,0 lines)
  ```

  | Arm removed | Result |
  |---|---|
  | interval tick | 36 pass, 1 fail ("a tick whose flush throws is swallowed...") |
  | signal handler | 36 pass, 1 fail ("a flush that throws inside the signal handler is swallowed...") |
  | flush in `stop()` | 37 pass, 0 fail |

- **My findings:** the issue only partly reproduces. A third test file
  the issue doesn't mention, `src/db/latency-ring-buffer.coverage.test.ts`,
  was added by the maintainer in #1603 (merged 2026-10-04), two months
  after he wrote the issue. It drives the tick with fake timers, calls
  the signal handler directly, and breaks inserts with a
  `RAISE(ABORT)` trigger, so those two arms are already covered and
  red-proven. The file has also shifted (the issue's lines 211-225 are
  now 194-208). The one gap left is the flush inside `stop()`: removing
  its `try`/`catch` fails no test, so the issue's third acceptance item
  ("A test proves `stop()` swallows a flush error (and still removes
  its listeners)") is still unmet. My claim had promised all three
  arms, so I'm narrowing my contribution to that one arm and asking the
  maintainer whether he still wants it.

**Toolkit**

- Tool: repro-check with the wild scope, run as
  `claude "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue <URL>"`.
- What it produced: first a reject, because `claim-specific` failed:
  my posted claim promised to confirm a baseline and cover three arms
  that my own report showed were mostly done already. After I added a
  correction and one question to the top of the report, it accepted,
  with every required check passing.
- Difficulties: the claim was already posted, so the grader was
  grading a comment I couldn't change; the only fix was to correct it
  in the next one. It also said up front that it didn't re-run my
  coverage numbers, so my Actual output is the only evidence for them.
- What changed: I put the correction at the top of the report, noted
  the line shift so readers don't think the issue or I got the lines
  wrong, and reworded one flat statement about sqlite-vec to say it
  came from the warning.

---

## Solution Approach

**Analysis**

In `packages/gateway/src/db/latency-ring-buffer.ts`, `startLatencyFlushScheduler(db)` returns a
`stop()` that clears the 30-second timer, removes its SIGTERM and SIGINT listeners, then calls
`flushLatencyBuffer(db, latencyRingBuffer)` inside a `try`/`catch` (`/* ignore */`). That catch has
never caught anything in a test. The existing `stop()` calls in `latency-ring-buffer.test.ts` run
against a healthy in-memory database, and the `afterEach` in `latency-ring-buffer.coverage.test.ts`
calls `stop()` only after its tests have emptied the buffer or dropped the failing trigger.
`flushLatencyBuffer` returns early, without touching the database, when the buffer is empty or
the `query_latency_log` table is missing, so none of those calls reaches the `catch`. I confirmed
this on main (`7350b29e`): deleting the `try`/`catch` in `stop()` leaves all 37 latency-ring-buffer
tests passing, while deleting the tick's or the signal handler's catch fails one test each (those
two arms were covered by #1603).

**Proposed Solution**

Add one test to `latency-ring-buffer.coverage.test.ts`, next to the tick and signal tests, using
that file's existing `breakInserts()` trigger and `push()` helper: start the scheduler, push a
sample, make inserts fail, then call `stop()` and assert that it does not throw, that the failed
sample was not written, and that the SIGTERM and SIGINT listener counts are back to what they were
before the scheduler started. Update the file's header comment to say it covers the third arm too.

What I won't change: `latency-ring-buffer.ts` itself (no new options or seams are needed, since
`stop()` is already reachable from a test), the `assemble.ts` call site, the older
`latency-ring-buffer.test.ts`, and the two arms #1603 already covers. Branch coverage should stay
at 22/24, because I add no branches.

**Implementation Plan**

1. Create a branch `test/976-stop-flush-error` from `main` in my fork's clone.
2. In `packages/gateway/src/db/latency-ring-buffer.coverage.test.ts`, add a `describe` for the
   flush inside `stop()` with one test: record the SIGTERM/SIGINT listener counts, start the
   scheduler, `breakInserts()`, `push()` one sample, then expect `stop()` not to throw, expect
   `logged()` to be empty, and expect both listener counts to equal the recorded ones. Keep the
   scheduler in a local variable, not the file-level `scheduler`, so the `afterEach` doesn't stop it
   twice.
3. Update the file's header comment to name the third trigger.
4. Red-prove: delete the `try`/`catch` in `stop()`, confirm the new test fails, restore it, confirm
   it passes.
5. Run the manifest checks (`bun run typecheck`, `bun run lint`, the db tests,
   `bun run preflight:fast`) and the issue's Istanbul coverage command, and record before and
   after numbers in test_evidence.md.

**Unknowns**

- The maintainer hasn't answered whether he wants the narrowed scope or would rather close the
  issue. CONTRIBUTING makes assignment a hard gate only in high-traffic periods, so I'm building
  now and will say in the PR that the tick and signal items were already covered by #1603.
- The 22/24 branch figure is my own Istanbul run on Windows ARM64 at `7350b29e`, not the issue's
  older 28/34. CONTRIBUTING says the coverage floor is CI-Linux-authoritative, so CI's number is
  the one that counts. Since the change adds no branches, I expect no change there, but I haven't
  confirmed it on Linux.
- The test reads `process.listenerCount`, which other test files could change if they ran in the
  same process. The count is taken right before `startLatencyFlushScheduler` and compared right
  after `stop()`, so only a listener added between those two lines would affect it.

Deviation (2026-11-07): step 2 originally said to clear the file-level
`scheduler` variable after the test. While writing it, I kept the
scheduler in a local variable instead, which never touches the
`afterEach` and is simpler. Same behavior; plan.md is updated to match.

**Toolkit**

- Tool: plan-check with the wild scope, run from my clone as
  `claude "plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue <URL>"`.
- What it produced: accept on the first run, with all six required
  checks passing. Before grading, it checked the plan's claims against
  main: #1603's helpers exist, and the `afterEach` really does call
  `stop()` again. The one preferred check it failed was
  `unknowns-stated`, because the plan had no risks section and stated
  "22/24 branches" without saying where that number came from.
- Difficulties: it advised me not to build until the maintainer
  answers or assigns me, which conflicts with my plan to keep moving.
  It also pointed out a gap in my own procedure: it doesn't say what to
  do when the posted repro has no expected/actual lines, so it fell
  back on the red-prove result in my plan.
- What changed: I added an Unknowns section covering the unanswered
  maintainer question, the 22/24 figure being my own Windows run and
  not CI's, and the listener-count risk. I kept building, because
  CONTRIBUTING makes assignment a hard gate only in high-traffic
  periods; I'll say in the PR that the tick and signal arms were
  already covered.

---

## Testing Strategy

The fix is a test, so the proof is that the test can fail: my
reproduction's red-prove step, re-run on the branch.

1. **Red-prove the `stop()` arm, before and after.** I deleted the
   `try`/`catch` around the flush in `stop()` and ran
   `bun test src/db/latency-ring-buffer` from `packages/gateway`.
   - Before (main `7350b29e`): 37 pass, 0 fail. Nothing notices.
   - After (branch `8bdcb3b9`): 37 pass, 1 fail. The new test fails with
     `expect(received).not.toThrow()`.
   - With the catch restored on the branch: 38 pass, 0 fail.
2. **Coverage with the issue's Istanbul command.** Before and after,
   the file stays at 91/91 lines and 22/24 branches. The flush line in
   `stop()` (line 218) goes from 8 hits to 9. I expected the numbers not
   to move: the line already ran, and Istanbul doesn't count a
   `try`/`catch` as a branch. So this shows the change adds no branches
   to risk the 80% floor, not that it raises coverage.
3. **The repo's own checks from my ship manifest,** run on the branch in
   a fresh `cmd` shell:
   - `bun run typecheck`: exit 0
   - `bun run lint`: exit 0 (4529 files checked, no fixes)
   - `bun test packages/gateway/src/db packages/gateway/test/unit/db`:
     394 pass, 3 skip, 0 fail (main: 393 pass)
   - `bun run preflight:fast`: "preflight PASSED", all 37 gates green

These are Windows ARM64 runs. CONTRIBUTING says the coverage floor is
decided only by CI on Linux, so the PR's CI run is the final word on
coverage.

**Toolkit**

- Tool: Bun's test runner and the issue's Istanbul preloads, driven
  from Claude Code, plus a small Python script that deletes exactly one
  `try`/`catch` for the red-prove and `git checkout` to restore it.
- What it produced: the before and after table above, saved with the
  full outputs in `test_evidence.md` in my clone.
- Difficulties: the coverage numbers can't show this fix, since line
  and branch coverage were already as high as they'll go for this
  file. Only the red-prove shows the difference. Running the checks
  "in a fresh shell" on Windows also meant rebuilding PATH from the
  registry, because my session's PATH didn't have Bun.
- What changed: I made the red-prove the main evidence and reported the
  coverage numbers as unchanged instead of trying to show a gain.

---

## Implementation Notes

**2026-11-07: reproduced, and the scope shrank**

Installed Bun 1.4.4 with winget and ran the issue's coverage command on
main. Got 100% line and 91.7% branch, not the issue's 94.2% and 82.4%.
Found `latency-ring-buffer.coverage.test.ts` from #1603, which already
covers the tick and signal arms. Red-proving all three arms showed only
the `stop()` catch is unproven. Posted a correction on #976 asking
whether the maintainer wants the narrower scope, and decided to keep
building rather than wait, since CONTRIBUTING makes assignment a hard
gate only in busy periods.

**2026-11-07: ship-gate setup and two Windows fixes**

Installed the ship-gate and patched its hook and `check_gate.py` to
also watch PowerShell, as I did for the voice guard. Wrote the ship
manifest from CONTRIBUTING's "Before Opening a PR" list. Testing it in
a fresh shell, `preflight:fast` failed with "Executable not found:
bunx" because the winget package ships only `bun.exe`; I added
`bunx.exe` as a hardlink to it, the way Bun's own installer does.

**2026-11-07: plan, then the test**

plan-check accepted the plan. Created `test/976-stop-flush-error` and
added one test to `latency-ring-buffer.coverage.test.ts`, reusing its
`breakInserts()` trigger and `push()` helper, and updated its header
comment. Decision: I kept the scheduler in a local variable instead of
the file-level one, so the file's `afterEach` never stops it a second
time (recorded as a deviation in Solution Approach). Red-proved it: with
the `stop()` catch removed it fails, with it restored all 38 pass.
Committed as `8bdcb3b9`, "test(gateway): prove stop() swallows a
failing latency flush".

**Toolkit**

- Tool: Claude Code in my clone, with `CLAUDE.local.md` loaded, writing
  the test against the existing helpers in the #1603 test file.
- What it produced: one 15-line test and a two-line comment update in
  one commit, with no change to the source file.
- Difficulties: the first draft of my plan reused the file-level
  `scheduler` variable, which the file's `afterEach` stops again. I
  also had to remember that `flushLatencyBuffer` returns early on an
  empty buffer, so the test had to push a sample after
  `breakInserts()` or it would never reach the catch.
- What changed: I switched to a local variable, recorded that as a
  deviation, and updated `plan.md` to match.

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
