# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58

**Verdict output**

```
1. #58 — Bias detector patterns are too narrow · accept
Best fit by a distance: Python, in the safety/LLM layer you're strongest in, and it comes with 9 failing unit tests in tests/unit/test_bias_detector.py that spell out the intended behavior against a 52-line detector. That's the largest existing-test surface of the three, which is exactly the "learn to write tests from a suite I can read" goal. No good first issue label, but the rubric doesn't require one.
{
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/58",
    "checks": [
      {"name": "active-repo", "grade": "pass", "evidence": "Last main commits 2026-09-16 (x3) by Aburke225; issues updated through 2026-09-19; repo not archived — all within the last month."},
      {"name": "maintainer-active", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) applied labels bug/safety/tier-1 on 2026-09-10 and pushed to main 2026-09-16, both inside 90 days."},
      {"name": "not-taken", "grade": "pass", "evidence": "assignees: [], comments: 0, timeline shows only three labeled events; repo has 0 PRs in any state."},
      {"name": "reasonable-scope", "grade": "pass", "evidence": "Broaden regexes in a 52-line safety/bias_detector.py; 9 xfail tests in tests/unit/test_bias_detector.py already specify the intended behavior, so no product decision is left open."},
      {"name": "AI-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; no AI_POLICY.md, no ban in README."},
      {"name": "good-first-issue", "grade": "fail", "evidence": "labels are ['bug', 'safety', 'tier-1'] — no good first issue label."},
      {"name": "contributing.md", "grade": "pass", "evidence": "docs/CONTRIBUTING.md covers branch naming, Conventional Commits, the five CI jobs, and how to remove xfail markers for seeded bugs."}
    ],
    "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

agreement: 11/20 scored items (bar: 18/20: below the bar)

agreement: 16/20 scored items (bar: 18/20: below the bar)

agreement: 5/6 scored items

agreement: 1/1 scored items

agreement: 16/20 scored items (bar: 18/20: below the bar)

agreement: 0/4 scored items

agreement: 2/4 scored items

agreement: 1/2 scored items

agreement: 19/20 scored items (bar: 18/20: PASS)

**Issue analysis**

`issue-02` produced a rejection which is in agreement with the gold labels. My rubric produced a rejection because it asks for "significant \[repo\] activity within the last month" and "a maintainer comment, review, label, merge, or commit on the main branch within 90 days." Both of these requirements are unfulfilled as maintainer first-response sample has no maintainer comments and the lasst default-branch commit was in 2023.

**Check rationale**

"| not-taken | asignees, Linked PRs, comments | fail if there are current asignees, PRs, or comments indicating someone wants to or has taken the issue already in the past 90 days; pass otherwise | required |"

This check sites all the possible places (assignees, Linked PRs, and comments) where someone could indicate they want to or have already taken the issue. This ensures it looks in all possible places and not just for official opened PRs. The pass condition sets a reasonable 90 limit so that if there is an old comment or PR claiming the issue, it doesn't get rejected since the issue is likely not currently being addressed.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

This check might reject some issues where someone has expressed interest in an issue but hasn't actually claimed it or worked on it at all. Without an actual PR, it can be a little unclear if someone has claimed an issue, so many good issues might be rejected when they aren't actually claimed.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

Answer all three:

1. **The issue's fit to your interests and to the time available.**

   I'm interested in this issue as I like language processing problems. It's in Python which I'm trying to practice more of and only revolves around a single function that already has tests made for it so it seems like my work is cut out for me.

2. **What the verdict identified correctly, and what you weighed that the rubric could not.**

   All the rubric parts were correct and it identified the desire for Python and tests practice, and my proficiency in the safety/LLM layer based on my fit profile. The only thing it didn't really weigh is if I think the issue itself is interesting or seems fun.

3. **The anticipated difficulty in claiming it.**

   This seems like a very reasonable challenge since the existing function that has a bug isn't that long and this bug is in an area I feel confident in.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
