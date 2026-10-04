# Procedure: how this skill grades a plan package

## Read order

1. Read the Repro evidence first: the numbered steps, Expected and
   Actual, and any timings. Write down which step isolates a variable
   (a component removed, a flag toggled, a re-entry that changes the
   result). This is the ground truth the plan is judged against. Read
   it before the plan, so a confident plan cannot make its cause look
   established.
2. Read the candidate plan: Diagnosis/Cause, Scope (or Change/In/Out),
   Changes, Test plan. Write down the plan's cause in one sentence,
   every file it names, every area it excludes, and its test steps.
3. Read Thread highlights as unverified claims, never as evidence. If
   there are none, skip this step and treat the absence as neutral.
4. Read the candidate plan comment, then the Repo facts (contribution
   policy).

## Evidence gathering

1. For diagnosis-backed: if the plan says "the repro steps above",
   copy those steps from the Repro evidence first. Mark each repro
   observation as supports, contradicts or neutral toward the plan's
   cause. Record whether the plan cites the repro or only a thread
   comment.
2. For scope-matches-cause: list every file the plan changes (test
   files too) and every area it excludes. Compare both lists with where
   the plan's diagnosis and the repro evidence point.
3. For test-reproduces-bug: put the plan's test steps next to the repro
   steps. Compare the test's expected result with the repro's Actual.
4. For repo-fit: compare the contribution policy in Repo facts with the
   plan's size and with what the plan comment says about the policy.
5. For comment-matches-plan: compare the comment's cause and change
   with the plan's Diagnosis and Changes, and with Thread highlights.

## Check execution

1. Grade the checks in the order of the rubric table. Give each one
   pass, fail or unclear, with one evidence line naming the fact or
   quote that decided it.
2. If the evidence a check needs is missing from the package, grade it
   `unclear` and name what is missing. Do not guess, and do not fill it
   in from outside knowledge of the repo.
3. If the plan states a cause but no repro step isolates the variable
   it names, grade diagnosis-backed `unclear`, not pass, and name the
   missing observation.
4. Count a claim repeated in the thread and in the plan as one
   unverified source, not two. Only the Repro evidence can support a
   cause. A confident tone or a quoted maintainer is not support.
5. Grade each check from the notes gathered above; do not re-read the
   whole package for each one.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all three required
   checks are pass.
2. If any required check is fail or unclear, the verdict is reject.
3. In the output, quote the deciding fact for every required check that
   is fail or unclear, and name any missing evidence.
4. List the preferred check grades in the summary. They never change
   the verdict.
