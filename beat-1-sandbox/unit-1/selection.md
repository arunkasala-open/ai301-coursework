# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

````
## Summary

All three issues clear every required check — they're all solid first-issue candidates from `codepath/pathreview-ai301-fa26-s1`.

**Repo-level facts (shared across all three):** Last 5 commits are all by a human (Aburke225), most recent Sept 16, 2026 — well within 120 days of today (Sept 19, 2026) → `maintainer-alive` pass. Repo is not archived and was pushed to Sept 16, 2026 → `repo-in-use` pass. `docs/CONTRIBUTING.md` says nothing about AI-assisted contributions → `policy-allows-ai` passes (silence passes).

**Ranked (accepted, in fit order):**

1. **#72 — `verify_password` raises `UnknownHashError` instead of returning False** — Best fit: smallest, most tightly bounded (1–2h), single named file (`core/security.py`) plus a paired test file (`tests/unit/test_security.py`) with an `xfail` marker to clear — exactly the "read unfamiliar code, make an existing test pass, follow conventions" rep you're after.
2. **#68 — Keyword search raises `ZeroDivisionError` on empty index** — Same shape (named fix file + named test file + xfail marker to clear) but larger (2–4h) and touches a RAG retriever, so a bit more codebase to navigate.
3. **#62 — Health check references `settings.redis_host`, which doesn't exist** — Also bounded and well-specified, but its body names no test file or xfail marker, so it doesn't hit your "practice writing tests" goal as directly as the other two.

None of these are frontend/CSS work, so all three respect your stated preference.

**Rejected:** none — all three passed every required check.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 commits by human Aburke225, most recent 2026-09-16, within 120 days of 2026-09-19"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48:27Z, within 120 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded fix in core/security.py to fail closed on UnknownHashError; no design debate, no umbrella, estimated 1-2 hours"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; comments: 0; PR search for issue 72 returned total_count:0"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI-related restrictions (silence passes)"},
      {"name": "friendly-signal", "grade": "pass", "evidence": "Labeled 'good first issue'; names exact file (core/security.py) and test file (tests/unit/test_security.py)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 commits by human Aburke225, most recent 2026-09-16, within 120 days of 2026-09-19"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48:27Z, within 120 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded fix in rag/retriever/keyword_search.py for empty-corpus ZeroDivisionError; no design debate, estimated 2-4 hours"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; comments: 0; PR search for issue 68 returned total_count:0"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI-related restrictions (silence passes)"},
      {"name": "friendly-signal", "grade": "pass", "evidence": "Labeled 'good first issue'; names exact file (rag/retriever/keyword_search.py) and test file (tests/unit/test_keyword_search.py)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 commits by human Aburke225, most recent 2026-09-16, within 120 days of 2026-09-19"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48:27Z, within 120 days"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Single bounded fix reconciling settings.redis_host/redis_port with actual redis_url field in core/config.py; no design debate, no umbrella"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; comments: 0; PR search for issue 62 returned total_count:0"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI-related restrictions (silence passes)"},
      {"name": "friendly-signal", "grade": "pass", "evidence": "Labeled 'good first issue'; names exact files (api/routes/health.py, core/config.py) but no test file or effort estimate given"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

One run: agreement 20/20 scored items (bar: 18/20: PASS). This matches the
`agreement: 20/20 scored items  (bar: 18/20: PASS)` line in `eval-run.txt`.
My rubric passed on the first full run, so no revision cycle was needed.

**Issue analysis**

`issue-08` (source: zulip/zulip#39794, category `claimed`). My rubric's
decision: reject. Gold label: reject. Reasoning: the bundle's repo-facts line
reads "this issue: assignees: piyushagarwal-55; linked PRs:
zulip/zulip#39811 (open)". That alone fails `unclaimed` twice over — an
assignee is set, and the linked PR is open, not closed-unmerged. The thread
also shows a `zulipbot` comment ten days later warning "We noticed that you
have not made any updates to this issue or linked PRs for 10 days... you
will be automatically unassigned in 4 days" — but that is a warning of a
future un-assignment, not an actual one, so per my rubric's wording ("no bot
un-assignment") the claim is still live and the check still fails.

**Check rationale**

The `unclaimed` check's pass condition, as currently written in `rubric.md`:

> Fail if assignees names anyone, OR any linked PR is open, OR the thread
> has an "I'll take this / working on this" claim dated within 180 days of
> the capture date with no follow-up sign it was abandoned (no bot
> un-assignment, no long silence after).

I wrote it with three separate fail triggers because a Path Review issue (or
its real-world sibling) can already be "someone's" in three different ways
— formally assigned, an open PR already racing to close it, or just a
comment claim with no PR yet — and I wanted each to be checked from a
concrete repo fact rather than a feeling. I added the 180-day staleness
carve-out to the comment-claim clause specifically because a year-old "I'll
take this" with total silence afterward isn't a real signal the issue is
spoken for; I didn't extend that same carve-out to open PRs, since an open
PR means someone has already produced actual work no matter how old.

**Trade-offs**

The assignee clause ("Fail if assignees names anyone") has no staleness
carve-out, unlike the comment-claim clause right next to it. So an issue
where a maintainer assigned someone a long time ago, that person went
completely silent, and no PR was ever opened would still fail `unclaimed`
under my rubric as written — even though the same 180-day staleness logic
that lets an old comment claim pass through would, applied consistently,
argue for letting this pass too. I accept missing these genuinely-abandoned
but still-assigned issues as a trade-off for keeping the assignee clause a
single fact check ("is the field empty or not") instead of one that also
has to infer how long ago the assignment happened from data the bundle may
not carry.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit: I write mainly Python and wanted backend practice, and this issue is
   a one-file fix in `core/security.py` — no frontend/CSS involved. All 3
   candidates I graded were accepted, so fit is what actually chose between
   them: `issues/72` is the only one where the body names a real covering
   test (`tests/unit/test_security.py`, currently marked
   `@pytest.mark.xfail`) to un-mark once fixed, which is direct practice at
   the thing I said I wanted — writing/updating tests and following an
   existing project's conventions rather than my own. Its own 1-2 hour
   estimate also fits the time I have this week better than `issues/68`'s
   2-4 hours.

2. What the verdict identified correctly: every required check came back
   clean from concrete facts for all three candidates — no assignee, no
   linked PR, no comments at all, an active repo, and no AI-contribution ban
   anywhere in the repo. What the rubric couldn't weigh, because
   `friendly-signal` only checks for a label or a named location: which
   candidate actually taught me the skill I wanted. All three carry "good
   first issue" and a named fix location, so the rubric alone left me with a
   3-way tie; reading each body myself and noticing only `issues/72` and
   `issues/68` name a specific pre-existing test to un-xfail (rather than
   just describing the bug) is what actually broke the tie toward `issues/72`.

3. Anticipated difficulty in claiming it: low-to-moderate. The described fix
   is small — catch `UnknownHashError` in `verify_password` and return
   `False` instead of letting it escape — but before opening a PR I still
   need to read `core/security.py` to see what other exceptions it already
   handles, run the existing `tests/unit/test_security.py::test_...` case
   that's marked `xfail` under manifest id H-05 to see what it currently
   asserts, and confirm removing the marker doesn't reveal a second, unrelated
   failure in that test.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
