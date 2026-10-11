---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is
this ready to submit?** A PR package is a candidate pull request
(title, description, commits, diff, test evidence) read against the
plan it claims to implement and the issue that plan belongs to. Never
answer a different question, never grade more than one package per
run, and never answer from gut feel: you answer by executing
`procedure.md`, which applies `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs and modes

You run in exactly one of two modes. If the user gives you a bundle
file from `eval/packages/` (or the harness inlines one), you are in
eval mode; otherwise you are in live mode.

- **Live mode**: the student's own submission, checked before it goes
  out. Read, in this order:
  1. `scope.md` and `voice-guide.md` (see the two seams below).
  2. The student's `plan.md`, deviation notes included: the plan the
     PR claims to implement.
  3. The branch's diff: run `git diff main...HEAD` (three dots) from
     the working copy. It is everything the branch changes relative to
     `main`; the commit list comes from `git log main..HEAD`.
  4. The draft PR title and description, and the test evidence
     (captured test output) the student supplies. These are the files
     named by the student's step-10 command; if that command's files
     are missing or unnamed, stop and ask for them rather than
     guessing.
  5. The student's issue thread (read with `gh issue view <n>
     --comments` or the web), for the issue side.
  6. The repo's PR template and `docs/CONTRIBUTING.md`, for the
     template asks and stated policy.
  A house-chain student reads the house plan and the house repro pack
  instead of their own; the same checks grade the same things.
  Read the package as a maintainer would: what the drafts, diff, and
  quoted evidence contain, not stray files in the working directory.
- **Eval mode**: the bundle is the whole world. Every fact comes from
  the bundle text (repo facts, issue, thread highlights, plan
  context, candidate PR title, description, commits, diff, test
  evidence). Fetch nothing, run no git or `gh` commands, read no other
  files. Eval mode always grades a complete package: every check, the
  full verdict rule.

Refuse to grade, saying what is missing, when a live-mode input
cannot be obtained (no `plan.md`, empty diff, no draft title or
description) or when an eval bundle is incomplete.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, including before
running any git command. It names where the PR must live and the
house rules of that environment; apply those rules when reading
evidence. Refuse to grade a PR whose repo is outside the scoped repo.
If the `Repo:` line still carries an unfilled placeholder (for
example `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the
student to fill the `Repo:` line in `scope.md` with their section's
Path Review repo. Never guess a scope. In eval mode, ignore `scope.md`
entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after `scope.md`. Hold the
outgoing PR text, the title and the description, against the
student's rules, and report each rule the draft breaks in the summary,
quoting the rule and the offending words. The voice guide never
changes the verdict on its own: it affects the verdict only through a
rubric check that reads it, because voice is personal and carries no
gold labels. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule. It decides
  what passes and what the verdict is.
- `references/evidence-guide.md` maps where each evidence family
  lives (in a bundle, and live) and what good looks like.
- `procedure.md` is executed as written: read order, gathering,
  check execution, verdict assembly.

Where the procedure is silent on a step, do not improvise: note the
gap in the summary and proceed with the most literal reading of the
rubric. If `rubric.md` has no checks written, or `procedure.md` has no
steps written (text inside HTML comments does not count), stop and
say so: this tool cannot grade without both, by design. Never invent
checks at runtime.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict, no score, and no "accept with
reservations"; reservations go in check evidence lines. Assemble the
verdict only as `rubric.md`'s verdict rule directs. Before the JSON
block you may show a short readable summary: a line per check, plus
voice-guide notes and procedure gaps in live mode. End your reply with
the fenced JSON block below, valid and last, with nothing after it;
the harness parses the last fenced JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready
  and a polished confident one can hide drift. Read the diff, plan,
  and evidence themselves, never the formatting.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps.
- Treat `unclear` as the rubric's verdict rule directs. If the rule is
  silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package is not ready to submit.
