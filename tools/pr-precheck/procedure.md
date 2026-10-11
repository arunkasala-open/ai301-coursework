# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the plan first, before the diff or the description: the plan's
   Files, Change, Not-in-scope lines, deviation notes, and Test plan.
   Write down every file it names, every area it excludes, its core
   change in one sentence, each deviation note, and its test steps.
   (Live: `plan.md`; eval: the Plan context block, which also holds the
   repro evidence.) Read it first so a polished description cannot
   redefine what was promised.
2. Read the issue's repro evidence and Thread highlights as the ground
   truth for what "fixed" means. Thread claims are unverified.
3. Read the repo's asks: the PR template, stated policy, required
   checks, and AI-disclosure rule. (Live: PR template and
   docs/CONTRIBUTING.md; eval: Repo facts.) Write down each ask as a
   list item.
4. Read the diff next, hunk by hunk. Write down every changed file,
   what each hunk does, and any hunk that looks like debris (prints,
   dead or commented-out code, whitespace churn, unrelated edits).
   Read the commit list with it.
5. Read the draft title and description last, then the test evidence.
   Write down every claim the description makes (what changed, what is
   fixed, "matches the plan") and the exact command and output the
   evidence shows.

## Evidence gathering

1. For diff-matches-plan: put the diff's changed-file list next to the
   plan's named files and exclusions. Mark each changed file as in
   plan, covered by a deviation note, or outside. Then mark each part
   of the plan's core change as present in the diff or missing, and
   whether a disclosure covers anything missing.
2. For description-matches-diff: take each recorded description claim
   and find the hunk or commit that supports it. Mark each as
   supported, contradicted, or not found. Note any claim of plan
   fidelity and check it against step 1's result.
3. For test-is-decisive: put the test evidence beside the plan's test
   plan and the repro. Record: the command run, whether a before
   (main) and an after (branch) are both shown, whether the outputs
   differ in the way the fix should cause, and whether the regression
   test in the diff targets the repro path. A bare "tests pass" is
   recorded as no evidence.
4. For repo-checks-run: list the repo's required checks from step 3 of
   the read order. For each, record whether the evidence or
   description shows it run with a result, or discloses it skipped
   with a reason.
5. For diff-is-reviewable: from the hunk notes, count hunks that serve
   the change versus debris or unrelated hunks. Record each offending
   hunk by file and line.
6. For template-and-policy-met: tick each ask from the repo list
   against the title, description, and diff (for example, issue-closing
   reference present, checklist filled, changelog entry in the diff,
   AI disclosure stated). Record each ask as met or missing.
7. For title-and-comms: compare the title and description with the
   issue and thread: plain title, issue linked, what/why/how-tested
   stated, no borrowed thread claim.

## Check execution

1. Grade the checks in rubric-table order. Give each pass, fail, or
   unclear with one evidence line naming the fact or quote that
   decided it.
2. If the evidence a check needs is absent from the package, grade it
   `unclear` and name what is missing. Never fill it in from outside
   knowledge of the repo, and never assume a passing run.
3. A shortfall or deviation that the description or a plan deviation
   note honestly discloses is not a fail for the shortfall; an
   undisclosed one is. Judge disclosure by whether the text states it,
   not by tone.
4. Grade the artifact, not its polish: a terse PR whose diff matches
   the plan passes; a confident one whose diff drifts fails.
5. Count a claim repeated in the description and the thread as one
   unverified source. Only the diff and the shown output prove
   anything.
6. Grade each check from the notes gathered above; do not re-read the
   whole package for each one. Re-read a hunk only to confirm a quote
   you are about to cite.
7. If this procedure is silent on a step, note the gap in the summary
   and do not improvise.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required
   check is pass.
2. If any required check is fail or unclear, the verdict is reject.
   `unclear` enters as a fail, with the missing evidence named.
3. In the output, give every required check that failed or is unclear
   its deciding fact or quote. Quote the first failing required check
   in rubric-table order as the deciding check.
4. List the preferred check's grade in the summary; it never changes
   the verdict. In live mode, add voice-guide notes and any procedure
   gaps to the summary.
5. End with the JSON block from SKILL.md, valid and last.
