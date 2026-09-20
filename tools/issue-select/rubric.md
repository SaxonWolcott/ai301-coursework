# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check             | Evidence                                                                   | Pass condition                                                                                                                                      | Weight    |
| ----------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| active-repo       | dates of the 5 most recent default-branch commits, issues, merges, and PRs | There is significant activity in the repo within the last month                                                                                     | required  |
| maintainer-active | maintainer comments, commit log reviews, labels, and merges                | There must be a maintainer comment, review, label, merge, or commit on the main branch within 90 days                                               | required  |
| not-taken         | asignees, Linked PRs, comments                                             | fail if there are current asignees, PRs, or comments indicating someone wants to or has taken the issue already in the past 90 days; pass otherwise | required  |
| reasonable-scope  | repo description, issue desciription, PR description                       | reject if it requires significant product decisions; otherwise pass                                                                                 | required  |
| AI-policy         | repo description, contributing.md or other contributing guidelines         | fail if there is an explicit ban on AI-generated code; pass otherwise                                                                               | required  |
| good-first-issue  | issue tags                                                                 | pass if the tags include the good-first-issue tag                                                                                                   | preferred |
| contributing.md   | files in repo root, contributing.md contents                               | Has a contributing.md file that is clear and well-written                                                                                           | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every required check passes. Treat unclear results the same as failures. If any of the required checks
fails or is unclear, reject the issue. Preferred checks do not affect the verdict, but will be used to rank accepted issues
along with my fit profile.
