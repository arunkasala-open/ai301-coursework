# Contribution: Refactor/rearrange tests (qim3d #193)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Arun Kasala
**GitHub Username:** arunkasala-open
**Issue:** https://github.com/qim-center/qim3d/issues/193
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout with my own search strategy, rubric fix, and a graded scan**

I wrote a search strategy of four `gh search issues` queries (Python and JavaScript, good first issue and help wanted, plus a tests query) with ranking filters that drop assigned, stale, heavily discussed and styling issues, then ordered the rest by fewest comments. The first scan fell short: all five graded issues passed, but three came from `unmess`, a 9-day-old solo repo that has never reviewed a PR, and fewest-comments-first favored issues posted minutes earlier. In response I tightened `maintainer-alive` so a single-author repo needs evidence of outside interaction, and I added strategy rules to drop issues under 3 days old and titles containing "UI" or "CSS". I have not re-scanned to confirm those fixes work.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My first pick was not this issue. On 2026-10-18 my scout graded three picks directly (FluentFlyout #540, jellyfin #17666, Listenarr #582) and accepted only FluentFlyout #540, so I claimed it. When I re-graded those three on 2026-10-31 after this week's scope patch, all three came back rejected on `unclaimed`. Jellyfin and Listenarr each have an open PR from someone else. FluentFlyout was rejected because of my own claim, which led me to add an exception, after which #540 passed as "moderate". Then I looked at the code and found that FluentFlyout is a Windows-only WPF app (`net10.0-windows`), and I work on a Mac, so I could not reproduce or test a fix. I posted an unclaim comment on #540 and searched again. The two Python picks from my first scan (pr-agent #4161 and #4162) had both been closed. I re-ran my four strategy queries with my ranking filters, which left 71 results. Most were tiny single-author repos my `maintainer-alive` check rejects, so I read the full threads of the two established Python projects near the top (both had zero comments) and had the scout grade them. It rejected glass #839 because it is a container for 13 sub-issues, and it accepted qim3d #193 ("Refactor/rearrange tests") with a **moderate** difficulty note: the two files the maintainer named are still undone, anything beyond them is my call to agree with him, and the test-naming rule from PR #194 is enforced by a pre-commit hook.

It fits me because it is all Python and all tests, and the job is reading a test suite I have never seen and reorganising it the way the project already works. That is exactly the practice my fit profile asks for. qim3d's CI also runs on macOS, so I can build and test it on my own machine. I hope to learn how a real scientific Python project lays out its pytest suite, to work with its `uv`/`just`/`prek` tooling, and to get a PR through its two-step review. My honest read of the scout: its grading held up, but my setup has gaps. The rubric had no idea which claims were mine, and nothing in my fit profile says I need code that runs on a Mac, so the scout accepted a Windows-only app and I lost a pick because of it. The scope patch did its job (Listenarr #582 now passes the scope check and fails only on its open PR). The scout caught glass #839 as a container from its sub-issue list, but my scope check only names the issue body as evidence, so I would add sub-issues there. I would also add "runs on macOS" to my fit profile. My claim comment: https://github.com/qim-center/qim3d/issues/193#issuecomment-6548255139

**Toolkit**

- Tool: issue-scout, with my search strategy and my rubric. I ran it in direct-grade mode (`issue-scout: grade <URLs>`) three times on 2026-10-31: on my three old picks, on #540 again after a rubric fix, and on two new candidates I found by running my strategy queries and filters again.
- What it produced: verdicts with check-by-check evidence. The two new candidates got: qim3d #193 accept, difficulty moderate, with the open edge named ("agree the list with the maintainer"); glass #839 reject on `scope-feasible`, a container with 13 sub-issues. All three old picks came back rejected on `unclaimed`.
- Difficulties: the old `scope-fits` check rejected issues with any open design edge, which real issues almost always have. `unclaimed` counted my own claim on #540 as a competing one. The scout had no way to know I need code that runs on macOS, so it accepted a Windows-only app.
- What changed: I applied the unit-6 scope patch, replacing `scope-fits` with `scope-feasible` and adding the difficulty note, so open edges are flagged instead of rejected. I added an exception to `unclaimed` so my own account's claim reads as "already yours". I would also add "runs on macOS" to my fit profile and add sub-issues to the scope check's evidence.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
