# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `maintainer-alive` | Repo facts: "last 5 default-branch commits" (dates and authors) and "maintainer first-response sample" (days to first owner/member/collaborator comment) | Pass if at least one of the last 5 default-branch commits is by a human (or a bot merging a human's PR) dated within 120 days of the capture date, OR at least one entry in the maintainer first-response sample shows a numeric days-to-response value (not "no maintainer comment in thread"). | required |
| `repo-in-use` | Repo facts: "archived:" flag, "last push to any branch", "latest release" | Fail immediately if archived is "yes". Otherwise pass if last push to any branch is within 120 days of the capture date, OR the latest release is within 365 days of the capture date. | required |
| `scope-fits` | Issue title, body, and comment thread; the "linked PRs:" line for abandoned-attempt history | Fail if the issue is an umbrella/tracking issue whose sub-items are meant to become separate issues or PRs assigned to different people; if the thread shows unresolved design debate with no maintainer decision settling the approach; if a maintainer states the required fix needs a rework/redesign of the architecture (naming the technical cause of a bug, even a performance bug with multiple contributing causes, is NOT this — that is normal diagnosis, not a redesign call); if the issue is a pure usage/support question rather than a change to make; if two or more linked PRs against it are closed unmerged (abandoned attempts) while the issue has stayed open for a long time, showing the work is harder than the label suggests; or if it is a feature request whose own text leaves the concrete design undecided (hedges like "TBD", "not sure yet", "possibly ... if needed", no alternatives considered) and no maintainer has endorsed the request or settled those specifics in the thread. One bounded change described as several sub-steps toward that single change (e.g. "add this page, then update these three pages to point to it") is NOT an umbrella; it still counts as one piece of work if one contributor could reasonably do it all. A terse body or missing repro steps does not by itself fail this check. Otherwise pass. | required |
| `unclaimed` | Repo facts: "this issue: assignees:" and "linked PRs:" lines; the Comments section | Fail if assignees names anyone, OR any linked PR is open, OR the thread has an "I'll take this / working on this" claim dated within 180 days of the capture date with no follow-up sign it was abandoned (no bot un-assignment, no long silence after). A closed/unmerged linked PR does not by itself fail this check. A claim older than 180 days with no PR or further activity from the claimant since is stale, not a live claim — it does not fail this check even if a maintainer's reply to it merely said "sure, go ahead" rather than explicitly reopening it to others. Otherwise pass. | required |
| `policy-allows-ai` | Repo facts: "contribution policy" line | Fail only on an outright ban on AI-generated/AI-assisted contributions. Disclosure, human-review, or testing requirements are conditions, not bans, and pass. No statement at all passes. | required |
| `friendly-signal` | Issue labels (e.g. "good first issue") and how precisely the issue names the files/functions/behavior to change | Pass if the issue carries a beginner-friendly label or names the specific location/behavior to fix. Ranks accepted issues; never gates the verdict. | preferred |

## Verdict rule

Accept if every required check (`maintainer-alive`, `repo-in-use`, `scope-fits`,
`unclaimed`, `policy-allows-ai`) grades `pass`. Reject if any required check
grades `fail`. `unclear` on a required check counts as `fail`: a first issue
whose liveness, scope, claim status, or policy cannot be verified from the
evidence is not one to take. `friendly-signal` never changes the verdict; use
it only to rank accepted issues (and, in live mode, alongside the fit profile
in `scope.md`).
