# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `env-recorded` | The repro report's environment record, read against the issue's own stated environment (or the repo's bug-report template's asked-for fields). | Pass if the report states the relevant tool version and OS/platform it was run on, and any meaningful difference from the issue's stated environment is explicitly acknowledged rather than left silent. Fail if no version/OS is stated at all, even if everything else about the report is strong. | required |
| `steps-rerunnable` | The repro report's steps, read against its stated starting state. | Pass if a stranger could recreate the same starting state and run the exact commands/actions shown without guessing. Fail if any input, repo, or config the steps depend on is stated as private, unshared, or otherwise unavailable to a stranger — a repro someone else cannot run is not evidence they can check. | required |
| `behavior-matches-issue` | The shown output/log/error/screenshot, read against the specific behavior the issue describes (the exact symptom, not just "a failure"). | Pass if the shown artifact demonstrates the same behavior the issue describes, OR the report honestly documents a genuine, specific attempt to trigger that behavior and states plainly that it did not occur, naming what was tried and what differed from the issue's stated conditions. Fail if the shown artifact demonstrates a different error, a different code path, or an adjacent symptom presented as if it were the issue's behavior — especially when the reproduction steps quietly typo or substitute the issue's own trigger input. | required |
| `claims-match-evidence` | The claim comment's and repro report's stated conclusions, read against the artifacts actually shown. | Pass if the report claims only what the shown evidence supports about whether the issue's behavior occurred. An honest "could not reproduce" backed by a real, described attempt passes, including a plain mention of how many attempts were made. Fail if the report asserts confirmation, consistency, or certainty ("confirmed", "exactly as described") about the issue's behavior when the shown artifact demonstrates something else, or leans on unshown claims (e.g., "ran this ten times") specifically to prop up a conclusion the shown evidence does not support. | required |
| `ai-disclosure-honored` | The repo-facts block's contribution/AI policy, read against the claim comment and repro report's own text. | Pass if the repo's stated policy does not require AI-use disclosure, or if it does and the comment(s) include an explicit disclosure (naming the tool and the extent of assistance). Fail if the policy requires disclosure and no disclosure appears anywhere in the comments. | required |
| `claim-comment-restrained` | The claim comment's own wording. | Pass if the claim comment does not make guarantees, timelines, or reservation demands beyond what the shown evidence supports (e.g., "I will fix this within N days, guaranteed," insisting the issue be reserved, excessive flattery or begging standing in for substance). Fail if it does. | required |

## Verdict rule

Ready (`accept`) only if every required check passes. `unclear` counts
as a fail, so any fail or `unclear` on a required check means `reject`
(hold). There is no partial credit: one failing required check holds
the whole package regardless of how strong the others are.
