# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/131

**Branch**

`fix/72-verify-password-malformed-hash`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run, in order: 19/20 (bar 18/20, PASS). `eval-run.txt` records: "agreement: 19/20 scored items  (bar: 18/20: PASS)". Categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. I did not revise after it.

**Package analysis**

pkg-16 (kubernetes/minikube#23471). Gold label: accept ("honestly disclosed shortfall: image-load path fails loudly with before/after transcripts; the start-path behavior deliberately deferred ... ready as scoped"). My tool rejected it, failing repo-checks-run. My rubric reads that check as: "The evidence names or shows the repo's own required checks run on the branch (and their result), or honestly states which could not be run and why. **Fail** if the repo requires checks and nothing shows them run or disclosed as skipped." pkg-16's test evidence shows `go test ./pkg/minikube/machine/... ./cmd/...` passing, and its repo facts name no required checks. The harness output only says "failed: repo-checks-run" and I did not read the per-check evidence line, so I infer the grader read the narrow `go test` run as the repo's checks not being fully shown, or read the no-stated-checks case as unclear. Either way, my rule "**Unclear** if the repo states no checks" counts as fail, so the miss comes from my rule, not from the run.

**Check rationale**

> | repo-checks-run | The test evidence and description, read against the repo's stated checks in the repo facts or CONTRIBUTING (test suite, linter, formatter, type check). | The evidence names or shows the repo's own required checks run on the branch (and their result), or honestly states which could not be run and why. **Fail** if the repo requires checks and nothing shows them run or disclosed as skipped. **Unclear** if the repo states no checks. | required |

It reads this way because I rewrote my Unit 3 rubric for a PR without the activity worksheet, and it needed a check for the "not tested" family beyond the before/after: a PR can show a decisive repro and still never run the repo's own suite, linter or type check. I kept it required because Path Review's CONTRIBUTING.md demands all five CI jobs. I accepted the "honestly states which could not be run and why" clause rather than demanding every check run, so a disclosed skip is not failed, which is the honest-outcome rule.

**Trade-offs**

One case this check misses or gets wrong: pkg-16, a gold accept my tool rejects, because "Unclear if the repo states no checks" counts as fail under my verdict rule. I accept that cost: a repo with no stated checks is rare, and loosening it risks flipping not-tested packages that agree today (4/4). I did not re-run with `--only` or change anything after the full run, since 19/20 cleared the bar and every category had a match. Nothing else changed, and I know because there was only the one full run in `eval-run.txt`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
