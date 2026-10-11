# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- *Eval bundle:* the plan's scope pair, deviation notes, and core change
  sit in the `## Plan context` block (the Files, Change, and Not in
  scope lines). What actually changed is the `### Diff` section under
  `## Candidate PR`, with `### Commits` beside it. The fidelity claims
  to test are in `### Description`.
- *Live:* `plan.md` (its Files, Change, Not-in-scope lines and any
  deviation notes); the branch's diff from `git diff main...HEAD` and
  `git log main..HEAD`; the claims in `pr_draft.md`.

**What good looks like.** Every changed file in the diff is named in
the plan or covered by a deviation note, the plan's core change is
visibly present in the hunks, and each description claim ("fixes X",
"implements the plan") has a hunk behind it. An honest deviation
re-ties a mismatch: a note in the plan (or a stated shortfall in the
description) saying what changed and why makes the PR faithful. Silent
drift is the mismatch with no note, in either direction: the diff
touches more than the plan (an excluded or unnamed file, extra
behavior), or does less (a promised piece missing), or the description
claims fidelity the diff contradicts. Judge by files and hunks, not
by how the description reads.

## Test evidence (harness category: not-tested)

**Where it lives.**
- *Eval bundle:* `### Test evidence` under `## Candidate PR`, read
  against the plan's Test plan (inside `## Plan context`) and the
  repro evidence in the same block. The regression test, if any, is in
  the `### Diff`. The repo's required checks are listed in `## Repo
  facts`.
- *Live:* `test_evidence.md` (commands and captured output), read
  against `plan.md`'s Test plan and the issue's repro; any new test in
  the branch diff; the checks named in the repo's `docs/CONTRIBUTING.md`
  and PR template.

**What good looks like.** The evidence names the command, shows the
output on both main (before) and the branch (after), and the two
differ in exactly the way the fix should cause, on the repro path the
bug lives on. A regression test in the diff fails before and passes
after. The outcome of the repo's own checks (suite, linter, type
check) is visible, or the PR honestly says which were skipped and
why. "Tests pass" with no named test or output, an after-only run,
output from an unchanged path, or checks never mentioned is not
evidence.

## Diff quality (harness category: unreviewable)

**Where it lives.**
- *Eval bundle:* the unified `### Diff` and the `### Commits` list.
- *Live:* `git diff main...HEAD` and `git log main..HEAD` from the
  working copy, not the files as they sit in the editor.

**What good looks like.** The fix is visible and every hunk serves it:
the change, its test, and any required companion edit. Debris tells:
debug prints or leftover logging, dead or commented-out code,
whitespace or formatting churn on lines the fix did not need,
renamed or reordered code unrelated to the change, and drive-by edits
in other files. A large diff is fine if every hunk is purposeful; one
buried unrelated hunk is enough to fail.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval bundle:* the repo's asks are in `## Repo facts` (PR template
  sections, contributing instructions, stated policy including AI-use
  disclosure, required companion files such as a changelog). They are
  met or ignored in `### Title`, `### Description`, and the `### Diff`.
  Maintainer direction is in `## Thread highlights`.
- *Live:* the repo's PR template (usually `.github/PULL_REQUEST_TEMPLATE`)
  and `docs/CONTRIBUTING.md`, read against `pr_draft.md` and the
  branch's diff; the issue thread for any explicit maintainer ask.

**What good looks like.** Each ask is met with real content: template
sections filled (not left as boilerplate or deleted), an
issue-closing reference present, a required companion change such as a
changelog or whatsnew entry visible in the diff, the AI-disclosure
statement present where the policy demands it, and any explicit
maintainer direction in the thread addressed. A visibly ignored
required ask fails even when the code is excellent; a repo that states
no asks passes. Title and description quality (plain title, issue
linked, what/why/how-tested) is the preferred check here. Whether
description claims match the diff belongs to plan fidelity, above.
