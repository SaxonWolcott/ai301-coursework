# Procedure: how this tool grades a PR package

These steps grade a pull request someone else wrote. They do not fix
the PR or suggest a better change. Follow them in order; where a step
says "record", write the fact down (with a short quote, file name, or
hunk location) before moving on, because later checks read those
notes and nothing else.

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL is in
   the scoped repo; if it is not, or the `Repo:` line is still a
   placeholder, stop and say so. Note the house rules. Eval mode: skip
   this step.
2. Read `rubric.md` and list its checks in table order, their weights,
   and the verdict rule. Read `references/evidence-guide.md` so you
   know where each check's evidence lives.
3. Read the issue (title, body, repro). Record in one line what
   behavior is wrong, and the exact failing input (the command, file,
   request method, header, or UI step the bug needs).
4. Read the plan next, before any part of the candidate PR (eval: the
   plan-context block including its repro evidence; live: `plan.md`
   with `## Deviations`). Record:
   (a) the planned change in one line;
   (b) every file, function, or code site the plan names;
   (c) every deliverable the plan commits to (the fix, each test, each
       docs change, each warning or message);
   (d) the "not in scope" list, word for word;
   (e) every deferral or deviation note, word for word;
   (f) the test plan: each repro or failure mode it says to re-run,
       its expected-after, and the suite command it names.
   The plan is read before the PR so that the diff is judged against
   what was promised, not the other way round. A PR read first makes
   its own description the yardstick, which is exactly how drift
   hides.
5. Read the repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`
   and `docs/CONTRIBUTING.md` from `main`). Record:
   (a) every unconditional PR ask as a list (issue link line, named
       sections, checklist items, required file entries such as a
       changelog or whatsnew), marking conditional asks ("if
       appropriate", "for UI changes") as conditional;
   (b) the AI policy's exact requirement: none, disclose all usage
       with tool and extent, human-in-the-loop / own-words text, or a
       ban. Live: also record the course rule that Notes for Reviewers
       must disclose AI use in the student's own words.
6. Read the thread highlights. Record any maintainer direction
   (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR) about the approach or
   about what the PR should include; it is context for the plan, and
   a plan deferral that cites it is still a disclosed deferral.
7. Read the diff (eval: the `### Diff` block; live: `git diff
   main...HEAD`), then the commit list. Do not read the description
   yet; the diff is read on its own so its contents are recorded
   before any claim about them.
8. Read the PR title and description last, then the test evidence
   (eval: `### Test evidence`; live: `test_evidence.md` plus any
   output pasted into the description). Do not grade while reading.
   Length, polish, and confidence are not evidence.

## Evidence gathering

Build each record below with quotes. Every check reads from exactly
these records.

1. **Hunk table (diff-within-plan, diff-clean)**: list every hunk in
   the diff as one row: file, function or area, and a few words on
   what it does. For each row write one tag:
   - `planned`: the planned change from Read order 4(a)/(b);
   - `test`: a test for the planned change;
   - `required-artifact`: a changelog/whatsnew/i18n/docs entry the
     repo's asks from Read order 5(a) require;
   - `deviation`: work named in a deviation note (plan or
     description);
   - `extra`: anything else (another function rewritten, a new
     flag/option/setting, a rename pass, a dependency bump, anything
     on the "not in scope" list);
   - `debris`: a debug print/log, commented-out code or earlier
     attempt, dead or unused function, new TODO/FIXME, import
     reordering, re-indentation or reformatting of lines the fix does
     not change, lines removed and re-added identically, or a hunk in
     an unrelated file.
   A single hunk can hold both a planned line and a debris line; tag
   the debris line separately. Comments that explain the fix are not
   debris. Note commit messages such as "wip" or "misc cleanups".
2. **Deliverable checklist (plan-delivered)**: for each deliverable
   from Read order 4(c), write "in diff" (with the hunk), "disclosed
   as deferred/dropped" (quote the note from the plan or description),
   or "missing". Also record whether the diff contains the plan's
   actual code change at all.
3. **Claim list (description-faithful)**: list every factual claim
   the title and description make about the change ("adds X",
   "exactly as planned", "no changes beyond the plan", "docs now
   say", "no functional changes outside Y", "CHANGELOG entry added").
   Next to each, write "true of the diff" with the hunk, or "false"
   with the contradicting hunk or absence. Then list every `planned`,
   `deviation`, and `extra` hunk from record 1 that changes behavior,
   and mark whether the description mentions it.
4. **Evidence pairing (decisive-evidence)**: for each repro or
   failure mode in the plan's test plan (Read order 4(f)), write:
   - the input it was run with, and whether that is the issue's
     failing input from Read order 3 or a control / unchanged path;
   - the before output (or "issue's captured failure", or "none");
   - the after output (or "none");
   - whether it is observable output (pasted text, exit code, error
     message, concrete UI step result) or prose ("works now",
     "tested locally", "verified on my machine").
   If the plan names a repro the evidence never runs, write "not run"
   and quote any note that discloses the skip.
5. **Suite record (repo-checks-run)**: quote each suite or check
   command the evidence reports and its result. Write "none" if no
   command is named. Live: record each of `make test-unit`,
   `make test-integration`, `make lint`, `make typecheck` with its
   pasted result; "no tests ran" for `make test-integration` is the
   expected Path Review result and counts as reported.
6. **Asks table (template-asks-met)**: for each unconditional ask from
   Read order 5(a), quote where the description or diff meets it, or
   write "unmet". For a required file entry, point to the hunk in the
   diff; a description that only claims the entry does not meet it.
   Inline labels ("Problem:", "Requirements:") meet a named section.
   Live: also check that the CI box is left unticked (ticking it
   before CI runs is a false claim) and, if the issue is a seeded bug,
   that the diff removes its `@pytest.mark.xfail` marker.
7. **Disclosure record (ai-disclosure)**: put the policy requirement
   from Read order 5(b) next to any disclosure in the description.
   Quote it, and note whether it names the tool, states the extent,
   and is in the contributor's own words. Write "no disclosure" if
   there is none.
8. **Preferred records**: note whether a test hunk exercises the
   issue's failing input (regression-test), and whether the title
   names the behavior fixed (title-clear).

Live mode: if a source file is missing, record the absence ("no
`test_evidence.md` in the working copy") as the evidence. Eval mode:
use only the bundle text; fetch nothing.

## Check execution

1. Run the checks in `rubric.md` table order, one at a time, using
   only that check's record from Evidence gathering. Re-open the
   package only to confirm a quote, not to hunt for new reasons.
2. Apply each pass condition literally to its record:
   - **diff-within-plan** fails if any hunk in record 1 is tagged
     `extra`, even when the planned fix is also present and correct.
   - **plan-delivered** fails if any deliverable in record 2 is
     "missing", or if the diff never contains the plan's code change.
     "Disclosed as deferred/dropped" passes.
   - **description-faithful** fails if any claim in record 3 is
     "false", or any behavior-changing hunk goes unmentioned.
   - **decisive-evidence** fails if any planned repro in record 4 is
     "not run" without a disclosure, ran only a control or unchanged
     path, has no before or no after, or is prose rather than
     observable output.
   - **repo-checks-run** fails if record 5 is "none" or names no
     command. An honestly reported failing check with its reason
     passes.
   - **diff-clean** fails if any line in record 1 is tagged `debris`.
     Commit messages alone never fail it.
   - **template-asks-met** passes if the repo states no asks; fails if
     any unconditional ask in record 6 is "unmet".
   - **ai-disclosure** passes if the repo states no AI policy (eval);
     fails if record 7 does not meet the policy's requirement, or the
     repo bans AI-assisted work. Live: fails if Notes for Reviewers has
     no own-words AI disclosure.
3. Grade `unclear` only when the evidence a check needs is truly
   absent and its absence is not itself the failure the check
   describes (for example, the plan-context block gives no test plan
   to pair against). A missing before/after, missing suite command, or
   missing disclosure is a `fail`, not `unclear`. A condition written
   as "pass if there is no X" passes when the package has no X.
4. Write one line of evidence per check: the quote, hunk, or absence
   that decided it. A fail names the failing item (the `extra` hunk,
   the false claim, the unmet ask).
5. Grade each check independently. A decisive test never offsets
   drift, and a clean diff never offsets a missing disclosure. The
   same hunk may fail more than one check (an unannounced new flag is
   `extra` for diff-within-plan and an unmentioned change for
   description-faithful); grade both.

## Verdict assembly

1. Take the required checks only. If every one is `pass`, the verdict
   is `accept`. If any is `fail` or `unclear`, the verdict is
   `reject`.
2. Preferred checks (regression-test, title-clear) are reported but
   never change the verdict.
3. The deciding check is the first failing or unclear required check
   in `rubric.md` table order. In the summary, quote its evidence
   first, then list every other failing required check with its
   evidence. For an accept, quote the evidence for diff-within-plan
   and decisive-evidence.
4. Live mode: after the verdict, give a fix list (one concrete action
   per failed check, such as "remove the `print(` on line N of
   `core/x.py` and commit" or "add a `## Deviations` note for the
   change to `y.py`"), then hold `pr_draft.md`'s title and description
   against `voice-guide.md` and quote any rule it breaks. Neither
   changes the verdict.
5. End with the JSON block from SKILL.md as the last thing in the
   output: every rubric check in table order with its grade and
   one-line evidence, and the verdict.
