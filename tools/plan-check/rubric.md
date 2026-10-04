# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-backed | The plan's Diagnosis/Cause, read against the repro evidence (its numbered steps, timings, Expected and Actual). | The plan states a specific cause, and that cause explains the repro observation that isolates a variable (a step with a component removed, a flag toggled, or a re-entry that changes the result), with no repro step contradicting it. The plan targets the cause the evidence points at, not just the symptom, and does not present an unverified guess as settled. **Fail** if any repro step or timing contradicts the cause, or the cause rests only on a thread claim with no repro observation behind it. **Unclear** if a cause is stated but no repro step isolates the variable it names. | required |
| scope-matches-cause | The plan's Scope and Changes (or its Change/In/Out lines), read against the plan's diagnosis and the repro evidence. | Every file the plan will change is named (test files too, if it adds any; a manual test needs no file). The plan says what it will not touch. Fixing the change location would address the stated cause, and no excluded area is where the repro evidence points. The change is one bounded piece of work a stranger could start on. **Fail** if the plan excludes the area the evidence implicates, or leaves a file it must change unnamed. **Unclear** if the plan does not say what it will not touch. | required |
| test-reproduces-bug | The plan's Test plan, plus any test item in Changes, read against the repro steps and the repro's Actual. | The test re-runs the repro steps (or says "the repro steps above") and states an expected result that differs from the repro's Actual, so it would fail before the fix and pass after. A manual test is fine; extra checks beyond the repro do not hurt. **Fail** if the test would not have caught the reported symptom (for example, it checks internal state when the symptom is a timing). **Unclear** if there is a test plan but no expected result. | required |
| repo-fit | Repo facts (contribution policy), read against the candidate plan comment and the plan's size. | The plan does not contradict the stated contribution policy, and the comment acknowledges any policy that affects the PR (selective review, AI disclosure, size limits). If the policy says nothing relevant, this passes. | preferred |
| comment-matches-plan | The candidate plan comment, read against the plan and the Thread highlights. | The comment's cause and change are the same as the plan's, it claims no more than the repro supports, and it does not present a thread claim as the author's own finding or ignore what the thread asks. | preferred |

## Verdict rule

Accept (ready) only if all three required checks pass. A required
`fail` means reject (hold). A required `unclear` also means reject:
treat it as a fail, and name the missing evidence in that check's
evidence line so the hold reads as "hold pending X". Preferred checks
are graded and noted but never change the verdict.
