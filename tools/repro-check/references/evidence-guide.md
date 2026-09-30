# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- Where it lives:
  - Eval: The environment/setup section of the repro report. Compare against the issue context (the version or platform the issue reports) and the repo-facts block (supported versions, current release).
  - Live: The environment section of my draft comment. Compare against the issue body and the repo's README or install docs for supported versions.
- What good looks like: The report names the exact package version or commit SHA, the runtime version, and the OS. Those match what the issue targets, or the report calls out the difference ("issue is on 2.3.0, I tested on 2.4.1 and it still happens").

## Steps

- Where it lives:
  - Eval: The steps section of the repro report, plus any code blocks it references.
  - Live: The steps in my draft comment.
- What good looks like: A stranger with only the report could go from a clean setup (fresh clone or fresh install) to the bug with no guessing. Every command, file, and input value needed is written out, and the last step is the one that triggers the bug.

## Behavior shown

- Where it lives:
  - Eval: The output/results section of the repro report (pasted terminal output, logs, tracebacks, screenshots). Compare against the symptom described in the issue context.
  - Live: The output in my draft, compared against the error or behavior in the original issue and any later comments in the thread.
- What good looks like: The pasted output shows the same failure the issue describes: same error type or message, same wrong value, same code path. A different error or a nearby symptom doesn't count, even if something did break. Output is pasted verbatim, or any trimming is marked.

## Honesty

- Where it lives:
  - Eval: The summary or conclusion lines of the repro report and the claim comment, checked against the report's own output section.
  - Live: Any sentence in my draft that states a cause, a fix, or a result, checked against what my output actually shows.
- What good looks like: Every claim has a matching piece of evidence in the report. Guesses are labeled as guesses ("I think", "not confirmed yet"). A report that says "could not reproduce on X" and shows what happened instead counts as honest and passes. A report that says "reproduced" when the output shows something different fails.

## Comms

- Where it lives:
  - Eval: The claim comment, compared against the issue context and the repo-facts block (contribution guidelines, issue/PR templates, AI-use policy).
  - Live: My draft comment, compared against the issue thread (is anyone already assigned or working on it?), CONTRIBUTING.md, the repo's templates, and any AI-use disclosure rules.
- What good looks like: The comment is about this specific issue, meaning it mentions what I actually reproduced rather than generic "I'd like to work on this" text that could go on any issue. It follows the repo's template and claiming process, discloses AI use if the repo requires it, and doesn't claim an issue someone else is already working on.
