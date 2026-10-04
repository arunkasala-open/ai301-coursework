# Plan: issue #72, `verify_password` raises on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
Repro comment (mine, Unit 2): https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5860037433
Branch (on my fork): `fix/72-verify-password-malformed-hash`

This is my first time in this codebase. I read `core/security.py`,
`tests/unit/test_security.py` and the one caller, `api/routes/auth.py`. I have
not run anything beyond what my repro comment shows, and I say below where a
claim is not yet verified.

## Evidence I rely on (quoted from my repro comment)

Environment: "macOS 25.6.0 (arm64), Python 3.13.2, repo at commit `f89c06f`",
"Installed: passlib 1.7.4, bcrypt 4.3.0."

Control run, a real bcrypt hash:

```
$ python -c "
from core.security import hash_password, verify_password
h = hash_password('correct horse battery staple')
print('verify correct:', verify_password('correct horse battery staple', h))
print('verify wrong:', verify_password('wrong password', h))
"
verify correct: True
verify wrong: False
```

Repro, a malformed non-bcrypt hash:

```
$ python -c "
from core.security import verify_password
result = verify_password('password', 'not_a_valid_bcrypt_hash')
print('result:', result)
"
Traceback (most recent call last):
  File "<string>", line 3, in <module>
    result = verify_password('password', 'not_a_valid_bcrypt_hash')
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  ...
passlib.exc.UnknownHashError: hash could not be identified
```

Covering test, run as-is against `main`:

```
$ python -m pytest tests/unit/test_security.py -v -k test_verify_with_wrong_hash_format
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
1 xfailed in 0.16s
```

Repro **Expected**: "`verify_password` returns `False` for an unrecognized hash
format, same as it does for a wrong-but-well-formed hash (control run, above)."
Repro **Actual**: "`verify_password` raises `passlib.exc.UnknownHashError`
instead of returning `False`... the exception escapes uncaught from the
`pwd_context.verify(...)` call on line 37."

## Diagnosis

`verify_password` in `core/security.py` returns
`bool(pwd_context.verify(plain_password, hashed_password))` with no exception
handling. When passlib cannot identify the stored hash's scheme,
`pwd_context.verify` raises `UnknownHashError`, and nothing in
`verify_password` catches it, so the exception reaches the caller.

What in my repro backs this: the control and the repro call the same function
with the same plain password style and differ only in the stored hash. A valid
bcrypt hash returns `True`/`False`; the string `not_a_valid_bcrypt_hash`
raises. So the stored hash is the variable that flips the behavior. The
traceback names `core/security.py` line 37 as the frame that called
`pwd_context.verify` and let the exception out. I am treating that as the
cause because the traceback shows the raise passing straight through that line.

What I have **not** verified:

- Other malformed shapes. Several commenters on the thread report that a string
  with a bcrypt prefix but a broken body (for example `$2b$notarealhash`) raises
  a bare `ValueError` instead of `UnknownHashError`. That is their claim, not my
  finding: my repro only exercised `not_a_valid_bcrypt_hash`. I will run those
  shapes myself during the build before relying on it.
- That `UnknownHashError` is a subclass of `ValueError`. SaiChen22 reports
  `issubclass(UnknownHashError, ValueError): True`. I have not run that check
  yet; it is step 1 of the build.
- The login path. `api/routes/auth.py` calls `verify_password` inside a
  `try` whose `except Exception` returns a 500 "Login failed". From reading
  the code, a malformed hash on a user row would produce a 500 today and a 401
  after the fix. I have not run the login route (it needs the database), so
  this is from reading, not observation.

## Scope

Will change (two files):

- `core/security.py`: `verify_password` returns `False` when the stored hash
  cannot be parsed or identified, instead of letting the exception escape.
- `tests/unit/test_security.py`: remove the
  `@pytest.mark.xfail(strict=True, reason="issue #72 (manifest H-05)...")`
  decorator on `test_verify_with_wrong_hash_format`, as the issue asks. If the
  extra malformed shapes reproduce on my machine, add one small parametrized
  test for them in the same file (no new test file).

Will not change:

- `api/routes/auth.py`. The login route already treats a `False` from
  `verify_password` as a 401; the fix belongs in `verify_password`, where the
  exception originates. I will not edit the route's `except Exception`.
- `hash_password`, `create_access_token`, `decode_access_token`, `pwd_context`
  configuration (schemes, `deprecated="auto"`), or any other module.
- Any other test in `test_security.py`, and no dependency or version changes.
- No logging added and no new exception types or return types; the signature
  stays `(str, str) -> bool`.

## Files I'll touch

1. `core/security.py` (the `verify_password` function only)
2. `tests/unit/test_security.py` (remove the xfail marker; possibly add one
   parametrized test)

## Approach

1. Before editing, check `issubclass(UnknownHashError, ValueError)` and run the
   other malformed shapes from the thread (`''`, `'$2b$notarealhash'`,
   `'$2b$12$tooshort'`) on unmodified `main`, to see which exception each raises.
2. Wrap the `pwd_context.verify(...)` call in `verify_password` in a
   `try`/`except` that returns `False`. The exception to catch depends on step 1:
   if `UnknownHashError` is a `ValueError` and the prefixed shapes raise
   `ValueError`, catch `ValueError` (this covers both). If step 1 shows
   something different, catch exactly the types observed. I will not use a bare
   `except Exception`, because that would also hide real bugs such as a
   misconfigured context.
3. Keep the successful path unchanged: a valid hash still returns `True` or
   `False` from `pwd_context.verify`.
4. Remove the xfail marker. This has to happen in the same change: the marker is
   `strict=True`, so once `verify_password` returns `False` the test would
   report XPASS(strict) and fail the suite.
5. Update the docstring's "Returns" to say `False` is also returned when the
   stored hash is not a recognizable format.

## Test plan

Re-run my Unit 2 repro steps against the built change. Same venv, same
commands as in my repro comment.

| Step (from my repro) | Before the fix (my posted result) | Expected after the fix |
|---|---|---|
| Control: `verify_password('correct horse battery staple', h)` and `verify_password('wrong password', h)` with `h = hash_password(...)` | `verify correct: True` / `verify wrong: False` | Same: `True` / `False`. The valid-hash path must not change. |
| Repro: `verify_password('password', 'not_a_valid_bcrypt_hash')` | Traceback ending `passlib.exc.UnknownHashError: hash could not be identified` | Prints `result: False` with no traceback. |
| Covering test: `python -m pytest tests/unit/test_security.py -v -k test_verify_with_wrong_hash_format` | `1 xfailed` | `1 passed` (marker removed). Before removing the marker, running with `--runxfail` should show the pre-fix failure and the post-fix pass, so I know the test fails without the fix. |
| Whole file: `python -m pytest tests/unit/test_security.py -v` | passes with that one test xfailed | All tests pass, none xfailed or xpassed. |

Additional checks beyond the repro (they do not replace it): the other
malformed shapes from the thread (`''`, `'$2b$notarealhash'`,
`'$2b$12$tooshort'`) each return `False` after the fix.

I will paste both the before and the after output into the PR/evidence, with
the commands as run.

## Risks and unknowns

- **Overlap with other work.** The issue has many classmates on it, and PR #75
  by kragent66-glitch already touches the same two files (`core/security.py`,
  `tests/unit/test_security.py`). My plan is my own, built from my own repro,
  but a maintainer may take that PR instead, or merge conflicts could occur. I
  will not promise a PR; see the repo-policy point below.
- **Catch too narrow or too broad.** `UnknownHashError` alone may miss the
  `ValueError` shapes; `Exception` would swallow genuine errors. I will pick
  based on what step 1 actually shows. Unknown until I run it.
- **Which `ValueError` sources exist.** I have not traced where the prefixed
  shapes raise in passlib's stack. A `ValueError` raised for a different reason
  (for example an over-long password under some bcrypt versions) would also be
  turned into `False`. I will check that the existing
  long-password test in `test_security.py` still returns `True` as written.
- **Version coverage.** I tested only passlib 1.7.4 with bcrypt 4.3.0 on
  Python 3.13.2. Other versions could raise different exception types; I
  haven't tested them.
- **Login 500 to 401 is unverified.** See Diagnosis: reasoned from reading
  `auth.py`, not run.
- **Silent failure.** Returning `False` for a corrupt stored hash makes login
  fail with 401 and no signal that the row is corrupt. The issue asks for
  fail-closed and I am not adding logging; whether the maintainers want a log
  line is a question for them.
- **Repo policy.** `docs/CONTRIBUTING.md` says to branch and open a PR from a
  fork. I did not find a selective-review rule or an AI-disclosure rule there,
  but I read it only for those terms. The course scope asks me to branch as
  `fix/<issue-number>-<slug>` on my own fork.

## Deviations

Nothing changed; I built what I posted. The diff touches only
`core/security.py` (`verify_password`) and `tests/unit/test_security.py`
(xfail marker removed, one parametrized test added). Step 1 of the approach
confirmed what I had left unverified: `UnknownHashError` is a `ValueError`, and
`''`, `$2b$notarealhash` and `$2b$12$tooshort` each raise a `ValueError`, so I
caught `ValueError` and not a bare `Exception`, as the plan said I would. The
existing long-password test still passes. I did not run the `--runxfail` check
or the login route, so the login 500-to-401 claim is still from reading the code.
