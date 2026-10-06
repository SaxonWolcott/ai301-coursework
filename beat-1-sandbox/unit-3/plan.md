# Plan for #58: Bias detector patterns are too narrow to match common phrasings

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58
My repro: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58#issuecomment-5901508457

## Diagnosis

The detector itself runs. The problem is that every regex in
`BiasDetector.DISMISSIVE_PATTERNS` and `BiasDetector.DEMOGRAPHIC_PATTERNS`
(`safety/bias_detector.py`) only matches one fixed word order with a
short, closed list of nouns and verbs. Any wording outside that list
falls through to `return False, ""`.

From my repro (Python 3.12.10, fork at `f89c06f`, same as `main`):

```
'The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education' -> (False, '')
'Given their age, they likely cannot keep up with modern frameworks' -> (False, '')
'bootcamp graduates lack rigor' -> (True, 'Dismissive language about educational background')
```

The third line is the control. The exact phrase the current pattern was
written for is still flagged, so detection isn't broken; the patterns
are just too narrow. With `--runxfail`, all 9 `issue #58` tests fail on
`assert False is True`:

```
9 failed, 23 passed in 0.14s
```

In the repro I had only traced one test to its pattern. Since then I've
checked each of the 9 test strings, plus the issue's two sentences,
against the patterns. Each one misses for one of these reasons:

| Missed text                                                                                                                                           | Why the current pattern misses                                                                                            |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| "bootcamp graduates can't write production code", "coding bootcamp graduates can't…", "bootcamp developers can't…", "young bootcamp graduates can't…" | the graduates/developers pattern only allows the verbs `lack` or `missing`                                                |
| "bootcamp programmers lack proper training"                                                                                                           | the noun list is `graduates?/developers?`; `programmers` isn't in it                                                      |
| "bootcamp education lacks fundamentals"                                                                                                               | pattern 1 requires `is` before every verb, so it can only match "is lacks"                                                |
| "self-taught developers are not equal to university graduates"                                                                                        | pattern 4 requires `self-taught is …`, with no noun in between and no `are`                                               |
| "bootcamp attendance means inadequate training"                                                                                                       | no pattern covers "X means inadequate"                                                                                    |
| "young developers can't handle complex systems"                                                                                                       | the age pattern only allows the singular `person/developer/programmer`                                                    |
| "developers from poor backgrounds can't afford proper tools"                                                                                          | the background pattern only allows `person from` or `coming from`                                                         |
| issue sentence 1 (bootcamp … lacks the rigor)                                                                                                         | the bootcamp mention and the negative claim are separated by other words, and every pattern needs them next to each other |
| issue sentence 2 ("Given their age, they … cannot")                                                                                                   | no pattern covers age stated as a reason                                                                                  |

## Scope

One bounded change: widen the existing regex lists so these phrasings
match, while every currently-passing "not flagged" test stays unflagged.

In scope:

- the pattern lists in `safety/bias_detector.py`
- removing the `@pytest.mark.xfail(strict=True, reason="issue #58: …")`
  marker from the 9 tests, as `docs/CONTRIBUTING.md` requires for seeded bugs
- adding regression tests for the issue's two example sentences, plus a
  near-miss control that must stay unflagged

Not in scope:

- replacing the regex approach with an NLP or ML classifier
- changing the reason strings, the `(bool, str)` return value, or
  `detect_bias`'s signature
- covering every possible biased phrasing; the target is the issue's
  examples and its 9 tests
- other `safety/` modules, or lint and type baseline cleanup

## Files

- `safety/bias_detector.py`: the `DISMISSIVE_PATTERNS` and `DEMOGRAPHIC_PATTERNS` lists only
- `tests/unit/test_bias_detector.py`: remove 9 xfail markers, add 3 tests

## Approach

1. Dismissive (education) patterns:
   - pattern 1: change `is\s+(?:insufficient|inadequate|lacks)` to
     `(?:is\s+(?:insufficient|inadequate)|lacks)`
   - pattern 2: replace it with
     `(?:bootcamp|self-taught)\s+(?:graduates?|developers?|programmers?)\s+(?:lack|missing|can't|cannot)`.
     This adds `programmers?` and `can't|cannot`, and drops the required
     object (`rigor|fundamentals|proper training`) after the verb. "can't
     write production code" and "lack fundamentals" both need that, and
     the group noun plus a negative verb is already the biased claim.
   - pattern 4: allow an optional `graduates?|developers?|programmers?`
     noun, and `is|are`
   - add `(?:bootcamp|self-taught|online\s+course)\s+\w+\s+means\s+(?:inadequate|insufficient|poor|weak)`
   - add a same-sentence pattern:
     `bootcamp\b[^.]{0,60}?\black(?:s|ing)?\s+(?:the\s+)?(?:rigor|fundamentals|proper\s+training)`.
     `[^.]` keeps the match inside one sentence, and the 60-character
     cap keeps it from reaching across a paragraph.
2. Demographic patterns:
   - age pattern: allow `persons?|people|developers?|programmers?`
   - background pattern: allow `(?:person|people|developers?|programmers?)\s+from`
   - add an age-as-a-reason pattern:
     `(?:given|because\s+of|due\s+to)\s+(?:their|his|her)\s+age\b[^.]{0,40}?\b(?:can't|cannot|won't|will\s+not|unable)`
3. Delete the 9 `issue #58` xfail markers.
4. Add 3 tests: `test_issue_bootcamp_rigor_sentence_detected` and
   `test_issue_age_reason_sentence_detected`, using the issue's sentences
   verbatim, and `test_bootcamp_factual_lacks_not_flagged` for "The
   bootcamp didn't cover testing, so this project lacks test coverage",
   which must stay unflagged.

Before writing this plan, I ran these exact patterns in a scratch script
outside the repo. All 36 strings came out as expected: the 32 test
strings, the issue's 2 sentences, the control, and the near-miss. I'll
still confirm this against the real file during the build.

## Test plan

Re-run my repro steps after the change:

1. `.venv/Scripts/python -m pytest tests/unit/test_bias_detector.py -v`
   - before: `23 passed, 9 xfailed`
   - expected after: `35 passed` (the 32 existing tests plus 3 new), no xfailed, no XPASS
2. The repro's three-sentence script:
   - expected after:
     ```
     'The candidate only attended a bootcamp, ...' -> (True, 'Dismissive language about educational background')
     'Given their age, they likely cannot keep up with modern frameworks' -> (True, 'Demographic assumptions detected')
     'bootcamp graduates lack rigor' -> (True, 'Dismissive language about educational background')
     ```
     The third line is the control and must be unchanged.
3. The negative controls already in the file must still pass:
   `test_positive_bootcamp_mention_not_flagged`,
   `test_neutral_bootcamp_mention_not_flagged`,
   `test_educational_background_positive_not_flagged`,
   `test_comparative_without_bias`, and the observation half of
   `test_assumption_vs_observation`.
4. `make lint`, `make typecheck`, and `make test-unit`, per CONTRIBUTING's PR checklist.

## Risks and unknowns

- Overmatching. A wider pattern could flag factual sentences that happen
  to mention a bootcamp. The near-miss test and the existing negative
  tests guard against this, but regex can't tell intent, so some
  phrasings will still be missed and some edge cases may be flagged.
  I'm not claiming full coverage.
- First match wins. Text with both kinds of bias returns only the
  education reason, because those patterns are checked first. That
  happens today too, and no test asserts otherwise, so I'm leaving it alone.
- I've only run this on Windows. CI runs on Linux; I don't expect any
  difference for pure regex, but I haven't checked.
- I haven't yet checked whether ruff's line-length rule flags the longer
  pattern strings. If it does, I'll split them with implicit string
  concatenation rather than adding a `noqa`.

## Deviations

1. **Long patterns split across lines.** As the line-length risk above
   expected, several patterns ran over ruff's 100-character limit. I
   split them with implicit string concatenation, as planned, with no
   `noqa`. The regexes themselves are unchanged.
2. **Curly apostrophes (added scope).** The plan's patterns only
   accepted a straight `'` in `can't`/`won't`/`doesn't`. The issue text
   itself uses curly quotes (`’`), and feedback pasted from docs or
   generated by an LLM often does too, so "Bootcamp graduates can’t…"
   was missed. Every contraction in both pattern lists now accepts
   either apostrophe (`can[’']t`, `won[’']t`, `doesn[’']t`). That
   includes the existing `doesn't prepare` and immigrant/international
   patterns, which the plan didn't otherwise touch.
3. **One more test than planned.** I added
   `test_curly_apostrophe_detected` for item 2, so the test file ends at
   `36 passed` instead of the planned `35 passed`. There are still no
   xfails and no XPASS.

Results against the test plan:

- Steps 1–3 hold. The test file shows `36 passed`. The issue's two
  sentences return `True` with the education and demographic reasons
  respectively. The `bootcamp graduates lack rigor` control is
  unchanged. The existing not-flagged tests still pass.
- Step 4 holds for everything my change touches:
  - Lint, format, types: `ruff check` and `black --check` pass on both
    changed files, and `mypy` passes on `safety/bias_detector.py`. I ran
    these tools on my two files directly instead of `make lint` and
    `make typecheck`, because those cover the whole repo, which has
    pre-existing findings unrelated to this change.
  - Full unit suite (`pytest tests/unit -m unit`): `388 passed, 44
xfailed`, 0 failed. All 44 xfails are other issues' seeded bugs, and
    none are marked `issue #58`.
  - Frontend (`npm ci && npm test -- --run`): 17 of 18 passed. The one
    failure, `ProfileForm > validates portfolio URL character limit`,
    timed out at 5010ms against vitest's 5000ms default on my ARM
    Windows laptop. This change touches no frontend code.

Known limits I found by trying sentences outside the tests, left
unfixed because they're beyond the plan's scope:

- False positive: "Given their age, these dependencies cannot be
  upgraded safely" is flagged, because "their age" can refer to things,
  not people.
- False positive: "Critics claim bootcamp grads are lacking
  fundamentals, but your work shows otherwise" is flagged. The
  same-sentence pattern can't tell when a claim is being argued against.
- Arguable: "Your bootcamp capstone lacks the rigor of your later
  projects" is flagged, though it criticizes the project, not the person.
- Miss: "Older developers can't…" isn't caught, because the age list is
  only `young|old|aged`.
- Miss: "They can't complete this project because they're a girl" isn't
  caught, before or after this change. The detector has no gender
  patterns at all, so this is a missing category rather than a narrow
  pattern, and a separate issue from #58.
