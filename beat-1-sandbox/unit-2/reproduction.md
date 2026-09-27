# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

arunkasala-open

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5860030595

> Hi! This is my first open-source contribution — I'm working through Path
> Review as a course assignment. I'd like to claim this one: `verify_password`
> in `core/security.py` lets passlib's `UnknownHashError` escape when the
> stored hash isn't a recognizable format, instead of failing closed and
> returning `False`.
>
> Plan: confirm the crash against the covering test
> (`test_verify_with_wrong_hash_format`, manifest H-05), then wrap the hash
> lookup in `verify_password` so an unrecognized format returns `False`
> instead of raising, and drop the `xfail` marker once it passes. I'll
> follow up with a full reproduction report before opening a PR.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5860037433

> **Environment:** macOS 25.6.0 (arm64), Python 3.13.2, repo at commit
> `f89c06f`. Confirmed with the maintainers' own comment on this issue that
> no `.env`/Docker stack is needed to trigger this — a venv with the
> security-module deps is enough:
>
> ```
> $ python3.13 -m venv .venv && source .venv/bin/activate
> $ pip install "passlib[bcrypt]>=1.7.4" "bcrypt>=4.0.1,<5.0.0" \
>     "python-jose[cryptography]>=3.3.0" "pydantic[email]>=2.5.0" \
>     "pydantic-settings>=2.1.0" pytest
> ```
>
> Installed: passlib 1.7.4, bcrypt 4.3.0.
>
> **Control run** (a real bcrypt hash, to confirm `verify_password` works
> normally first):
>
> ```
> $ python -c "
> from core.security import hash_password, verify_password
> h = hash_password('correct horse battery staple')
> print('verify correct:', verify_password('correct horse battery staple', h))
> print('verify wrong:', verify_password('wrong password', h))
> "
> verify correct: True
> verify wrong: False
> ```
>
> **Reproduction** (a malformed, non-bcrypt hash — the case the issue
> describes):
>
> ```
> $ python -c "
> from core.security import verify_password
> result = verify_password('password', 'not_a_valid_bcrypt_hash')
> print('result:', result)
> "
> Traceback (most recent call last):
>   File "<string>", line 3, in <module>
>     result = verify_password('password', 'not_a_valid_bcrypt_hash')
>   File ".../core/security.py", line 37, in verify_password
>     return bool(pwd_context.verify(plain_password, hashed_password))
>   File ".../passlib/context.py", line 2343, in verify
>     record = self._get_or_identify_record(hash, scheme, category)
>   File ".../passlib/context.py", line 2031, in _get_or_identify_record
>     return self._identify_record(hash, category)
>   File ".../passlib/context.py", line 1132, in identify_record
>     raise exc.UnknownHashError("hash could not be identified")
> passlib.exc.UnknownHashError: hash could not be identified
> ```
>
> **Covering test** (`test_verify_with_wrong_hash_format`, manifest H-05),
> run as-is against `main`:
>
> ```
> $ python -m pytest tests/unit/test_security.py -v -k test_verify_with_wrong_hash_format
> tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
> 1 xfailed in 0.16s
> ```
>
> Expected: `verify_password` returns `False` for an unrecognized hash
> format, same as it does for a wrong-but-well-formed hash (control run,
> above).
>
> Actual: `verify_password` raises `passlib.exc.UnknownHashError` instead
> of returning `False`, exactly as the issue describes — the exception
> escapes uncaught from the `pwd_context.verify(...)` call on line 37. The
> covering test currently passes only because it's marked `xfail`
> (manifest H-05); it will need that marker removed once `verify_password`
> is fixed to fail closed.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3`: 3/3 agreement (partial; used only to confirm the
   skill executes against the harness before spending on a full run).
2. Full run #1: 18/20 agreement (bar: 18/20, passing, but barely). Two
   scored packages disagreed with gold: `pkg-09` and `pkg-10`, both false
   rejects, both failing on `behavior-matches-issue`.
3. Targeted `--only` reruns while revising `behavior-matches-issue` and
   `claims-match-evidence` (the two flipped packages, plus canaries
   `pkg-02`, `pkg-08`, `pkg-16`, `pkg-17` for `wrong-target` and `pkg-04`
   for `no-evidence`, since the revision loosened a check those categories
   depend on): first pass 6/7 (one more flip surfaced: `pkg-09` moved from
   failing `behavior-matches-issue` to failing `claims-match-evidence`
   instead), second pass 8/8 (including a `disclosure`-category canary,
   `pkg-20`) after a second, narrower wording fix.
4. Full confirming run (`--save-run eval-run.txt`): **19/20 agreement**
   (bar: 18/20 — PASS), every category matched. This is the run recorded
   in the committed `eval-run.txt`.

**Package analysis**

`pkg-20` (Ghostty, the `disclosure` category). My rubric decided
`reject`; the gold label is `reject`; they agree. Every other check on
this package passes cleanly — the environment is recorded, the steps
are exactly reproducible, the artifact matches the issue's described
symptom precisely, and the conclusion doesn't overclaim. The package
still holds because `ghostty-org/ghostty`'s `AI_POLICY.md` requires
disclosing "the tool used and the extent of the assistance" for any
AI-assisted comment, and neither the candidate claim comment nor the
candidate repro report mentions AI use anywhere. My rubric's
`ai-disclosure-honored` check reads the repo-facts block's policy
against the comment text as its own fact, independent of how faithful
the reproduction itself is — so a technically perfect repro still
holds if the repo's own disclosure rule is silently ignored.

**Check rationale**

From `rubric.md` as currently uploaded:

> `claim-comment-restrained` | The claim comment's own wording. | Pass if
> the claim comment does not make guarantees, timelines, or reservation
> demands beyond what the shown evidence supports (e.g., "I will fix
> this within N days, guaranteed," insisting the issue be reserved,
> excessive flattery or begging standing in for substance). Fail if it
> does. | required

This check didn't exist in my original four-check rubric. I added it
after noticing `pkg-19` (Vue), whose repro report is genuinely solid —
environment, steps, and behavior all check out — but whose *claim
comment* begs "Kindly assign it to me, I will fix it within 2 days
guaranteed... keep this issue reserved for me." None of my original
four checks touch the claim comment's own promises at all;
`claims-match-evidence` only grades whether the *reported results* are
overclaimed, not whether the *ask* is. Without this check, the rubric
would have accepted a boilerplate over-promising comment as long as the
repro itself was clean, which is exactly the `unfollowable-comms`
failure mode the eval set is built to catch (its category description
names "a boilerplate over-promising comment" as one of three causes,
alongside missing environment and unfollowable steps — the other two
were already covered).

**Trade-offs**

Loosening `behavior-matches-issue` and `claims-match-evidence` to stop
penalizing honest "I could not reproduce this" reports (see run history,
step 3) is a package whose result it changed twice, and it is a
trade-off I accept deliberately. The original wording failed any report
whose artifact didn't show the issue's exact behavior, full stop — which
correctly holds a dishonest report but also wrongly held `pkg-09` and
`pkg-10`, both honest non-reproductions that mention more than one
attempt (a base run plus a variant) without pasting a transcript for
each one. The fix makes the check read *what a repeat-count claim is
being used to support* — propping up a false "confirmed" (fail) versus
narrating real effort before an honest negative result (pass) — rather
than a bright-line "every claimed run needs its own pasted output." I
re-ran `pkg-02`, `pkg-08`, `pkg-16`, `pkg-17` (`wrong-target`) and
`pkg-04` (`no-evidence`) as canaries after this change specifically
because they were the packages most likely to slip through a loosened
version of this check, and all four still correctly reject. The
trade-off I'm accepting: this check now asks a grader to judge intent
behind a claim rather than apply a mechanical rule, which is a harder
thing to apply consistently than a bright line — I chose it anyway
because the bright line's false-reject rate on honest reports was worse
for a check whose whole job is to protect honesty.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
