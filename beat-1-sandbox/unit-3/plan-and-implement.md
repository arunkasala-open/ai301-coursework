# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

arunkasala-open

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5984869234

> My plan for this, built from my own repro above (commit `f89c06f`, passlib 1.7.4, bcrypt 4.3.0). This is my first time in this codebase, so I've said below what I haven't checked yet.
> 
> **Cause, from my repro:** `verify_password` in `core/security.py` returns `bool(pwd_context.verify(plain_password, hashed_password))` with no exception handling. My control run with a real bcrypt hash returns `True`/`False`; the same call with `'not_a_valid_bcrypt_hash'` raises `passlib.exc.UnknownHashError: hash could not be identified`, and the traceback passes through line 37, the `pwd_context.verify(...)` call. Only the stored hash differs between the two runs, so that's what I'm treating as the cause.
> 
> **What I haven't verified:** other commenters report that bcrypt-prefixed but broken strings (e.g. `$2b$notarealhash`) raise a bare `ValueError` instead, and that `UnknownHashError` subclasses `ValueError`. Those are their findings; my repro only covered the one string above. I'll run those shapes myself before choosing what to catch, and I won't use a bare `except Exception`. I also haven't run the login route; from reading `api/routes/auth.py` a malformed hash currently reaches its `except Exception` (500), but I haven't observed that.
> 
> **Change (two files):**
> - `core/security.py`: in `verify_password` only, return `False` when the stored hash can't be parsed or identified.
> - `tests/unit/test_security.py`: remove the `xfail` marker on `test_verify_with_wrong_hash_format`, as the issue asks (it's `strict=True`, so it has to go in the same change). If the extra shapes reproduce for me, add one small parametrized test in the same file.
> 
> **Not touching:** `api/routes/auth.py`, `hash_password`, the token functions, the `pwd_context` config, or dependencies.
> 
> **Test plan:** re-run my repro commands after the fix. Expected: the control still prints `verify correct: True` / `verify wrong: False`; the malformed-hash call prints `result: False` with no traceback; `-k test_verify_with_wrong_hash_format` goes from `1 xfailed` to `1 passed`; the rest of `test_security.py` still passes. I'll also check the other malformed shapes return `False`.
> 
> **Risks:** PR #75 already touches these same two files, so this may overlap. I'm tested only on one passlib/bcrypt version. Returning `False` for a corrupt hash means login gives a 401 with no log line, so I'd like to know if you'd want one added. I didn't see a selective-review rule in `docs/CONTRIBUTING.md`, but I'm not assuming a PR will be accepted, and I'm glad to hold off if you'd rather someone else take it.
> 
> I used Claude Code to help draft this plan; I read the code and ran the repro commands myself.

---

## Your branch

**Branch**

fix/72-verify-password-malformed-hash

**Evidence**

Commands and output, before (unmodified `main`, `f89c06f`) and after (my branch), from the repo's venv:

```
=== BEFORE (main, f89c06f) ===
$ python -c "from core.security import hash_password, verify_password; h = hash_password(...); print(verify correct / wrong)"
verify correct: True
verify wrong: False
$ python -c "result = verify_password(\"password\", \"not_a_valid_bcrypt_hash\"); print(\"result:\", result)"
           ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^
  File "/Users/arunkasala/Documents/AI301/pathreview-ai301-fa26-s1/.venv/lib/python3.13/site-packages/passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
$ python -m pytest tests/unit/test_security.py -v -k test_verify_with_wrong_hash_format
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
================= 24 deselected, 1 xfailed, 1 warning in 0.15s =================
$ python -m pytest tests/unit/test_security.py -q
24 passed, 1 xfailed, 1 warning in 5.77s

=== AFTER (fix/72-verify-password-malformed-hash) ===
$ python -c "from core.security import hash_password, verify_password; h = hash_password(...); print(verify correct / wrong)"
verify correct: True
verify wrong: False
$ python -c "result = verify_password(\"password\", \"not_a_valid_bcrypt_hash\"); print(\"result:\", result)"
result: False
$ python -m pytest tests/unit/test_security.py -v -k test_verify_with_wrong_hash_format
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
================= 1 passed, 27 deselected, 1 warning in 0.11s ==================
$ python -m pytest tests/unit/test_security.py -q
28 passed, 1 warning in 5.70s
```

The "before" traceback is trimmed to its last lines and a harmless passlib/bcrypt version warning (`error reading bcrypt version`) is filtered out.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run: 18/20 (bar 18/20, PASS). It matches the agreement line in `eval-run.txt`.

**Package analysis**

pkg-20 (ghostty-org/ghostty#11261, category thread-convention). Gold label: reject. My rubric: accept. This is one of my two misses (the other is pkg-14).

The gold note and the package show why it should be a hold: the repo's `CONTRIBUTING.md` + `AI_POLICY.md` say "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance", and the plan comment contains no disclosure, even though the thread's first comment says "the AI-proposed solution was to recompute `prev` at every point it is used". The plan itself is good: the cause is grounded in the repro ("the same first test with the hyperlink start removed ... passes, isolating the growth-during-print as the trigger"), it follows the maintainer's direction, and it names its scope and test.

My rubric read it as accept because all three required checks pass. The only check that reads the repo's policy is `repo-fit`, and I made it preferred, so a missing AI disclosure is graded but cannot change the verdict.

**Check rationale**

`| diagnosis-backed | The plan's Diagnosis/Cause, read against the repro evidence (its numbered steps, timings, Expected and Actual). | The plan states a specific cause, and that cause explains the repro observation that isolates a variable (a step with a component removed, a flag toggled, or a re-entry that changes the result), with no repro step contradicting it. The plan targets the cause the evidence points at, not just the symptom, and does not present an unverified guess as settled. **Fail** if any repro step or timing contradicts the cause, or the cause rests only on a thread claim with no repro observation behind it. **Unclear** if a cause is stated but no repro step isolates the variable it names. | required |`

It reads that way because of the Phase 1 calibration. The sample rubric's diagnosis check only asked whether the plan "says what causes the bug", which let calib-03 through even though its repro contradicted the cause (26 s with no pager in the loop, instant with `--color=never`). I rewrote the check to require that the cause explain the repro observation that isolates a variable, to fail on any contradicting step or on a cause that rests only on a thread claim, and to grade `unclear` when no step isolates the variable. After testing against calib-01 I also stopped counting "fits some observation" as enough, since almost any cause fits some step.

**Trade-offs**

`diagnosis-backed` gives up plans whose cause may be right but that I cannot verify from the package. Because the cause must explain the repro observation that isolates a variable, a plan with no isolating step in its repro grades `unclear`, and `unclear` on a required check is a hold. A correct but untested cause is rejected for that reason. It also accepts a cause that fits the isolating observation but is wrong in a way the repro never tests, since the check can only read the repro evidence in the package.

How I know what it changed: wrong-cause matched 4/4 in `eval-run.txt` (pkg-01, pkg-07, pkg-11, pkg-16), and neither of my two misses came from this check. pkg-14 was rejected by `scope-matches-cause`, and pkg-20 was accepted because `repo-fit` is only preferred. I did not re-run any canary with `--only`, so I have not tested what loosening or tightening this check would change.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
