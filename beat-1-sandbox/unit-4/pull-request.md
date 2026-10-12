# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/161

**Branch**

`fix/58-widen-bias-patterns`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run (20 packages, saved with `--save-run eval-run.txt`): **18/20**, bar PASS. Categories: clear-accept 5/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3.

That one full run is the run recorded in `eval-run.txt` ("agreement: 18/20 scored items (bar: 18/20: PASS)"). It met the bar and every category had at least one match, so I made no revisions and did no `--only` re-runs after it.

**Package analysis**

**pkg-02** (Textualize/rich#4208, clear-accept). My rubric's verdict: **reject** (failed: diff-clean). Gold label: **accept**.

The diff makes the planned change in all three `save_*` methods (export with `clear=False`, write, then clear only after the write succeeds). It also adds a regression test and the CHANGELOG entry that rich's template asks for. My diff-clean check fails a diff that has "lines deleted and re-added identically". In the `save_html` and `save_svg` hunks, the diff removes and re-adds `html = self.export_html(` / `theme=theme,` and `svg = self.export_svg(` / `title=title,` / `theme=theme,` with no visible change. Only the `clear=` line in each call actually differs. Read literally, that is the "pure churn" pattern I wrote the clause for (pkg-18's re-printed lines), so the grader tagged those lines as debris and failed the package.

The gold label is right that this isn't debris. Those lines are the context of a multi-line call where one argument changed. A reviewer sees the fix immediately, and nothing unrelated rides along. My rubric can't tell a few unchanged lines around a real one-argument edit from whole blocks re-printed for no reason, and that is the gap this package exposes.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`:

> | template-asks-met | The repo-facts "pull requests" line (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`), each stated ask read against the description and diff | Pass if the repo states no PR template or asks, or every unconditional ask is met: the issue link line it asks for ("closes #N", "fixes #N"), each section it names has real content (inline labels like "Problem:" count; exact headings do not matter), each checklist item is addressed, and every file entry it requires (changelog, whatsnew, docs) is actually in the diff, not just claimed. Conditional asks ("screenshots if appropriate", "for UI changes") pass when the condition does not apply. Fail if any unconditional ask is visibly ignored, or a required file entry is absent from the diff | required |

Why it reads this way: the standards-wall category has only two packages, and the category floor means missing both sinks the run no matter how the other 18 go. pkg-01 is a technically excellent fix with decisive ASAN evidence. Every check about the change itself passes, so the only thing that can hold it is the repo's stated asks. That is why this check is `required`, not `preferred`. I made it compare against the diff, not just the description ("every file entry it requires ... is actually in the diff, not just claimed"), because pkg-01's template requires a whatsnew entry and none appears in the diff.

I rejected a stricter version that required the template's exact section headings. Seven accept packages come from repos with different templates or none: pkg-13 uses inline "Problem:" / "Solution:" labels, and pkg-19 and pkg-11 have no template at all. A heading-shaped check would have failed those terse but complete PRs, which is the "grade the thing, not the polish" mistake. The "pass if the repo states no PR template" clause and the conditional-ask clause are there so this check only bites when a repo actually asks for something and the PR visibly ignores it. The AI-disclosure half of the standards wall (pkg-20) is a separate check, `ai-disclosure`, because that policy lives on the contribution-policy line rather than the PR-template line.

**Trade-offs**

The `diff-clean` check gives up pkg-02 to catch pkg-18. pkg-18's debris is mechanical: an import restructure and two blocks of identical re-printed lines around a correct fix, which the gold note calls arguable. To make sure the grader rejected it, I spelled out "lines deleted and re-added identically" as debris. That clause is exactly what flipped pkg-02, a clear accept, to reject: its multi-line `export_html(` / `export_svg(` calls show unchanged argument lines removed and re-added around the one `clear=False` edit. I accept that miss in exchange for a check that holds every unreviewable package (3/3).

The other miss, pkg-05, comes from the strictness of `decisive-evidence`. Its condition requires observable before/after output for everything the plan's test plan names. pkg-05 shows the keybinding table before and after, but its second behavior, the one-time replace warning, appears only as a prose sentence ("Same-key redefine prints the one-time warning") with no output. The same strictness is what catches calib-04 and pkg-07/pkg-14, where a repro the plan names is skipped or run on the wrong path. I kept it, because loosening it to "the main repro is shown" would risk the 4/4 not-tested category to win back one accept.

I made no revision after the run, so no canary re-runs were needed. Both misses are in clear-accept, which still has 5/7 matches, and the run passes the bar.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
