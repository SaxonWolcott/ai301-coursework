# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.]

SaxonWolcott

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58#issuecomment-5984659040

## Plan for #58

Following up on my reproduction above, here is my plan. I see another plan has also been posted here. Per the course setup, I'm posting my own, built from my repro.

**Diagnosis:** The detector runs (my control `bootcamp graduates lack rigor` still returns `(True, 'Dismissive language about educational background')`), but each regex in `DISMISSIVE_PATTERNS` and `DEMOGRAPHIC_PATTERNS` in `safety/bias_detector.py` only matches one fixed word order with a short list of nouns and verbs. Since my repro, I've checked all 9 failing test strings against the patterns. They miss for a few concrete reasons:

- the `bootcamp graduates/developers` pattern only accepts `lack`/`missing`, so "can't" phrasings miss (4 tests)
- `programmers` isn't in the noun list
- pattern 1 requires `is` before `lacks`, so "bootcamp education lacks fundamentals" can't match
- the self-taught comparison doesn't allow a noun or `are`
- the age pattern is singular-only ("young developers" misses)
- the background pattern needs "person from"
- nothing covers "X means inadequate"

The issue's two sentences miss because the bootcamp mention and the negative claim are separated by other words, and because age stated as a reason ("Given their age…") has no pattern.

**Approach:** Edit the existing patterns rather than replace them:

- the graduates/developers pattern becomes `(?:bootcamp|self-taught)\s+(?:graduates?|developers?|programmers?)\s+(?:lack|missing|can't|cannot)`
- pattern 1 accepts `lacks` without `is`
- the self-taught comparison allows a noun and `is|are`
- the age and background patterns accept plural and `developers from`

I'm also adding three patterns:

- "X means inadequate"
- a same-sentence `bootcamp … lacks the rigor/fundamentals` (limited by `[^.]{0,60}` so it can't reach across sentences)
- "given/because of their age … can't/cannot"

**Scope:** Widen those two pattern lists only, remove the 9 `issue #58` xfail markers (per CONTRIBUTING), and add regression tests for the issue's two sentences plus a near-miss that must stay unflagged ("The bootcamp didn't cover testing, so this project lacks test coverage"). Not in scope: replacing regex with an ML/NLP approach, changing the reason strings or return type, or claiming coverage of every biased phrasing.

**Test plan:** Re-run my repro steps.

- The test file should go from `23 passed, 9 xfailed` to `35 passed` (the 32 existing tests plus 3 new) with no xfails.
- Both of the issue's sentences should return `True`, with the education and demographic reasons respectively.
- The `bootcamp graduates lack rigor` control should stay unchanged.
- The existing positive and neutral bootcamp tests should still pass.

I'll also run `make lint`, `make typecheck`, and `make test-unit`.

**Risks:** Wider patterns could flag factual sentences that mention a bootcamp. The existing negative tests and the near-miss test are there to catch that, but regex can't read intent, so some phrasings will still slip through. I've also only run this on Windows so far.

I tried the proposed patterns in a scratch script against all of the test strings and the issue's sentences, and they behaved as expected. I'll confirm against the real file during the build. If anyone sees a problem with this approach, I'd appreciate hearing it before I open a PR. I used Claude Code to help analyze the patterns and draft this plan, and I've checked and run everything described here myself.

---

## Your branch

**Branch**

[The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field.]

fix/58-widen-bias-patterns

**Evidence**

[Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output.]

### Result before changes

```
python -m venv .venv
.venv/Scripts/python -m pip install pytest structlog
.venv/Scripts/python -m pytest tests/unit/test_bias_detector.py -v
```

Result: 23 passed, 9 xfailed. The 9 xfails are the tests marked issue #58.

The xfail marker hides the actual assertion failures, so I re-ran with --runxfail to see them:

```
.venv/Scripts/python -m pytest tests/unit/test_bias_detector.py --runxfail -q --tb=line
```

```
FAILED ...::test_dismissive_bootcamp_language_detected
FAILED ...::test_bootcamp_lacks_rigor_detected
FAILED ...::test_demographic_assumption_age_detected
FAILED ...::test_coding_bootcamp_variant
FAILED ...::test_developer_vs_programmer_distinction
FAILED ...::test_multiple_bias_indicators
FAILED ...::test_negative_educational_claim
FAILED ...::test_rich_poor_assumption
FAILED ...::test_assumption_vs_observation
9 failed, 23 passed in 0.14s
```

(Trimmed: test paths shortened to ...::; removed the progress line, the FAILURES and short test summary info header lines, and the one-line assertion printed for each failure.) Each of those 9 lines is assert False is True: the detector returned "not biased" for text the test says should be flagged.

Then I ran the issue's two example sentences, plus the exact phrase the issue says the patterns do match as a control:

```
.venv/Scripts/python -c "from safety.bias_detector import BiasDetector as B
for t in ['The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education','Given their age, they likely cannot keep up with modern frameworks','bootcamp graduates lack rigor']: print(repr(t), '->', B.detect_bias(t))"
```

```
'The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education' -> (False, '')
'Given their age, they likely cannot keep up with modern frameworks' -> (False, '')
'bootcamp graduates lack rigor' -> (True, 'Dismissive language about educational background')
```

(One structlog bias_detected warning line, logged just before the third result, removed.)

### Result after changes

```
python -m venv .venv
.venv/Scripts/python -m pip install pytest structlog
.venv/Scripts/python -m pytest tests/unit/test_bias_detector.py -v
```

Result: All 36 tests passed.

Then I ran the issue's two example sentences, plus the exact phrase the issue says the patterns do match as a control:

```
.venv/Scripts/python -c "from safety.bias_detector import BiasDetector as B
for t in ['The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education','Given their age, they likely cannot keep up with modern frameworks','bootcamp graduates lack rigor']: print(repr(t), '->', B.detect_bias(t))"
```

```
2026-10-05 16:47:27 [warning  ] bias_detected                  reason='Dismissive language about educational background'
'The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education' -> (True, 'Dismissive language about educational background')
2026-10-05 16:47:27 [warning  ] bias_detected                  reason='Demographic assumptions detected'
'Given their age, they likely cannot keep up with modern frameworks' -> (True, 'Demographic assumptions detected')
2026-10-05 16:47:27 [warning  ] bias_detected                  reason='Dismissive language about educational background'
'bootcamp graduates lack rigor' -> (True, 'Dismissive language about educational background')
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

1. agreement: 19/20 scored items (bar: 18/20: PASS)

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

`pkg-04` was rejected by both the gold labels and by my rubric. This package failed three checks in my rubric. The first was `thread-direction` because the fzf owner had already found the code causing the bug and posted a patched build for testing. This plan comment doesn't reference that fix and doesn't give a reason for not following the owner's lead, saying "I plan to document it properly." This is the same reason the gold label gives. This package also fails the `cause-grounded` check because instead of fixing the code, the plan only documents a workaround with `> /dev/tty`. Finally, it fails `decisive-test` because a plan that only involves documentation cannot have a decisive test.

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

| cause-grounded | The plan's diagnosis (stated cause) and planned change, read against every step, artifact, and control run in the repro-evidence block. Thread claims about the cause are context only; the repro evidence outranks them | Pass if the plan names a cause and that cause is consistent with every repro step and control: nothing in the evidence shows the bug happening without the blamed component, before the blamed code runs, or with the blamed thing already working. Fail if any control or step rules the named cause out, if the diagnosis only restates the symptom, or if the planned change works around or documents the symptom while the evidence or thread isolates a fixable defect, unless the plan says why the real fix is out of reach | required |

This check does a few things. First it ensures that the repo evidence outranks the thread. I didn't want to pass a plan just because it matches what someone guesses in the commments. The check tests the cause against the repro's steps and controls. This check also fails plans that just restate the symptom without naming a real cause. Lastly, it makes workarounds (like seen in pkg-04) fail so that a plan can't get away with not providing a real fix. This was the first iteration of this check.

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

My `executable` check fails a plan if "the first step is open-ended investigation" or the location is left open. This keep out vague plans like "I'll look around and fix it" but it costs plans like pkg-14 that name the right area and have a strong repro, but say somethinbg like "exact functions [are] to be pinned in the PR after tracing the query issuance with debug logs." Although gold labels have that as a clear accept, my check rejected it saying it was an unfinished investigation. Personally, I'd rather accept that miss than loosen the check since I don't want to allows any vagueness.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
