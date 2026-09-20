# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `<ORG>/<PATH-REVIEW-REPO>` <!-- paste your section's repo from the Unit 1 Check-In page -->

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

<!-- YOU write this part: a few sentences about you. What languages and
tools you have actually used, what you want to get better at, anything
you want to avoid. The skill uses this only to RANK the issues your
rubric accepts, never to change a verdict: fit cannot rescue an issue
your rubric rejects, and cannot sink one it accepts. -->

TypeScript is my strongest language. I've built with React, NestJS, and
Supabase, plus D3.js for data viz. I've used Python for AI/agent work
(LangGraph agents, MCP servers, an eval harness with multi-seed validation),
but I'm not fluent in it yet. I'd like to take Python issues and pick it up
as I go. I'm strongest on AI/agent tooling, dev tooling, and anything
touching LLM workflows. My main goal is learning to write tests: I've done
almost none, so I'd prefer issues where adding or fixing tests is part of
the work, ideally in a repo with an existing test suite I can learn from.
I also want practice navigating a large existing codebase and going through
real code review. I'd rather avoid issues that need deep low-level systems
work (C/C++, kernels, compilers) or heavy frontend CSS polish, and I'd skip
anything that needs a big GPU budget to reproduce.
