# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | The diff's changed files and hunks, read against the plan's stated scope (Files, Change, Not in scope) plus any deviation notes. | Every changed file falls inside the plan's named files or boundary, or is covered by a recorded deviation note, and the plan's core change is actually present in the diff (nothing promised is silently missing). A shortfall or extra that the description or a deviation note honestly discloses still passes. **Fail** if the diff touches a file or area the plan excludes or never names with no note, or omits part of the plan's change with no note. **Unclear** if the plan names no scope to compare against. | required |
| description-matches-diff | The PR description's claims (what changed, what is fixed, "implements the plan as posted"), read against the diff contents and commits. | Every claim in the description is borne out by the diff: no claimed file, behavior, test, or fidelity-to-plan that the diff contradicts or lacks. A terse description passes if it claims nothing false. **Fail** if the description claims a change the diff does not contain, or claims plan fidelity the diff contradicts. **Unclear** if a material claim cannot be located in the diff. | required |
| test-is-decisive | The test evidence, read against the plan's test plan and the issue's repro: the named command or script, its output, and the regression test in the diff. | The evidence shows an observable before/after on the repro path the bug lives on (output differing between main and the branch, or a test that fails before and passes after), and it exercises the changed code, not an unchanged path. "Tests pass" with no named test or output does not count. **Fail** if the evidence is a bare claim, shows only the after state, exercises an unchanged path, or shows no difference that the fix could cause. **Unclear** if output is shown but the before/after cannot be tied to the plan's test plan. | required |
| repo-checks-run | The test evidence and description, read against the repo's stated checks in the repo facts or CONTRIBUTING (test suite, linter, formatter, type check). | The evidence names or shows the repo's own required checks run on the branch (and their result), or honestly states which could not be run and why. **Fail** if the repo requires checks and nothing shows them run or disclosed as skipped. **Unclear** if the repo states no checks. | required |
| diff-is-reviewable | The diff hunks and commit list, read as a reviewer would. | Every hunk serves the change: no debug prints, commented-out or dead code, whitespace or formatting churn, or unrelated hunks that bury the real change. A larger diff passes if every hunk is purposeful. **Fail** if any such debris or unrelated hunk is present, even if the core fix is correct. **Unclear** if the diff is truncated so hunks cannot be judged. | required |
| template-and-policy-met | The repo-facts block (live: the PR template and docs/CONTRIBUTING.md), read against the title, description, and diff. | Every ask the template or stated policy makes of this PR is met in the draft or the diff: required template sections and checklist items filled (including an issue-closing reference), required companion changes present (such as a changelog/whatsnew entry), and the stated AI-disclosure policy satisfied. **Fail** if a stated required ask is visibly ignored or missing. If the repo states no such asks, this passes. **Unclear** if the policy is referenced but its text is not in the package. | required |
| title-and-comms | The PR title and description, read against the issue and the thread. | The title states the change in plain terms, the description links the issue and says what, why, and how it was tested, and nothing presents a thread claim as the author's own finding. | preferred |

## Verdict rule

Accept (ready to submit) only if every required check passes. Any
required `fail` means reject (hold). A required `unclear` also means
reject: treat it as a fail and name the missing evidence in that
check's evidence line, so the hold reads as "hold pending X". Preferred
checks are graded and noted but never change the verdict. Honest
disclosure counts: a PR that openly states a shortfall (a skipped
check with its reason, a plan deviation with its note) is not failed
for the shortfall itself, only for hiding it. When several required
checks fail, quote the first failing one in table order as the
deciding check.
