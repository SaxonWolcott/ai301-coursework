# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

\*\*# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile — no `@`, no profile URL. Your
comments upstream are identified by this name.]

SaxonWolcott

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

`https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58#issuecomment-5898912972`

Hello, I'd like to work on issue #58. I will test the current implementation of the detect_bias function with the existing 9 tests and report my setup and results.

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

agreement: 13/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in disclosure)

agreement: 14/20 scored items (bar: 18/20: below the bar)

agreement: 20/20 scored items (bar: 18/20: PASS)

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

`pkg-02` was rejected by the gold label and my rubric because the type of error in the issue and the type of error in the repro report didn't match. The issue shows an exit 101 error while the repro report shows an exit 1 error. This failed the output-match check of my rubric which states "Fail if the output shows a different or adjacent symptom but the report presents it as confirming the issue." The repro report says it "aborts the run with a non-zero exit code, exactly as the issue describes," but the issue describes specifically an exit 101, not a "non-zero exit code."

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/repro-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

For my `version-pinned` check, the second half reads "FPass if the report names the version it tested and that version is the issue's affected version, the latest release, or main. Any other version passes only if the report points out the difference from the issue's version." Originally I had it failing any repro report that used a version different from the one in the issue report. However, after testing on the packages I realized that was too strict since there are cases like running the latest release where exact version matching isn't necessary. I also gave room to explain any differences since their could be any number of legitimate reasons to use a different version.

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

I changed `input-match` to compare the issue's trigger instead of requiring identical input, and that changed the result for `pkg-05`. In Run 2, under "exactly the same" input, it was rejected because the report used a hand-written minimal env.yml instead of the issue's full environment file from a URL. That file still contained the trigger, that being a `category:` section, which conda (the package manager the issue is about) doesn't recognize. Under the new wording, pkg-05 is accepted in Run 4, which matches the gold label. What I gave up is when an input differs from the issue's, the grader now has to judge whether it still contains the trigger instead of just comparing the two.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
