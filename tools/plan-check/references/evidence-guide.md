# Evidence guide: where evidence lives in a plan package

In an eval package the sections are: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Repro evidence`, `## Candidate plan`,
`## Candidate plan comment`. Plans vary in shape: a long one has
Summary / Diagnosis / Scope / Changes / Test plan subsections (calib-03);
a short one is a few labelled lines, Cause / Change (with In and Out) /
Test (calib-01). Read the labels, not the headings count. In live mode
the same evidence is in the draft plan, the draft comment, the issue
thread, the student's posted repro comment, and the repo's docs.

## Diagnosis and grounding

Where it lives: the plan's cause is in its Diagnosis (or "Cause")
text. The behavior that cause must explain is in `## Repro evidence`:
the numbered steps (look for the one that isolates a variable: a flag
toggled, a component removed, a re-entry), the timings, and the
Expected / Actual lines. Live: the student's posted repro comment.

What good looks like: the stated cause explains the isolating
observation and no repro step or timing contradicts it. It cites
behavior the repro actually shows, not a mechanism asserted from a
thread comment. In calib-01, "the view's model is not refreshed after
a push" explains step 4 (re-entering fixes the color). In calib-03,
"bindings are dropped" cannot explain step 3 (26 s with no pager at
all) or step 2 (instant with `--color=never`).

## Scope

Where it lives: the plan's Scope statement, or its Change line with In
and Out; the Changes list for the files it names; the test item, if it
adds a test file.

What good looks like: every file to be changed is named, the plan
says what it will not touch, the change location would address the
stated cause, and no excluded area is where the repro points. One
bounded change (one file, one callback) is good; a plan that rules out
the area the evidence implicates ("highlighting is unrelated") or
names no files is not.

## Executability

Where it lives: the Changes list, or the Change line: files or areas,
the approach, and the order of work.

What good looks like: a stranger could open the named file and start
without asking the author anything: it says what to call or add, where,
and in what order. "Add the commits context to the post-push refresh
scope in the push callback in `sync_controller.go`" is executable;
"fix the refresh logic" is not.

## Test plan

Where it lives: the plan's Test plan (or Test line), and any test item
in Changes. Compare it with the numbered steps and Actual in
`## Repro evidence`.

What good looks like: it re-runs the repro steps (or says "the repro
steps above") and states an observable expected result that differs from
the repro's Actual, so it fails before the fix and passes after. A
manual test is fine. A test that checks only internal state, or that
passes whether or not the bug is fixed (a binding-set smoke test for a
timing bug), proves nothing.

## Honesty

Where it lives: the plan's stated risks, unknowns and assumptions; the
way the Diagnosis is phrased; in live mode, any recorded deviation in
`plan.md`.

What good looks like: unknowns are stated as unknowns, and a cause the
repro does not isolate is flagged as unverified instead of asserted.
False confidence looks like a thread claim adopted as "as identified in
this thread" with no repro behind it. In live mode, a mid-build
deviation is recorded in the plan, not only in the diff.

## Comms

Where it lives: `## Candidate plan comment`, read against `## Thread
highlights` (maintainer comments, claims by others) and `## Repo facts`
(bug-report template, contribution policy, any AI-use requirement).

What good looks like: the comment states the same cause and change as
the plan, claims no more than the repro supports, does not pass off a
thread claim as the author's own finding, and acknowledges any policy
that affects the PR (calib-01's comment mentions the
review-bandwidth note in CONTRIBUTING). A boilerplate comment that
ignores the thread or the policy is not thread-aware.
