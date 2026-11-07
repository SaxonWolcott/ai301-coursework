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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
