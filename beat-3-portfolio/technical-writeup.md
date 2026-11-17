# Improving Test File Organization in the qim3d Open-Source Project

## Overview

This fall, I made my first open-source contribution to **qim3d**, a free tool scientists use to analyze 3D images, such as material scans. I found that some of the project's test files were not organized properly, so I split a large test file into two smaller files, moved four tests into two new files beside the related code, and removed an empty folder. Alongside this work, I also created **my own plan-check process and its rubric** to review my plans, as well as **my own voice guide and its rules** to help make sure my writing followed the right guidelines. The plan-check process rejected my first plan, and the voice guide stopped my initial draft because it did not follow one of its rules, so I made changes based on that feedback. I then verified that the same 43 tests were collected and passed, confirmed that the full test suite had 318 passing tests, and got all lint checks to pass. I submitted the code changes in [pull request #298](https://github.com/qim-center/qim3d/pull/298), which is waiting for maintainer review.

## What happened

First, I explored the project structure and looked at the existing test files to understand how they were connected to the source code. After reviewing the files, I separated the large test file into two smaller files, with each one focusing on a particular feature. I also moved the four tests from the incorrectly placed test file into two new files next to the related code they test. Once everything was moved, I removed the unnecessary empty folder.

**Pull Request:** https://github.com/qim-center/qim3d/pull/298

### The problem

I used a few terminal commands to understand the existing folder structure and check where the files were located.

```bash
$ ls -d qim3d/data_handling
ls: qim3d/data_handling: No such file or directory

$ grep -n '^#' tests/generate/test_generate.py
14:# Tests for _generate_twisted_thread
83:# Tests for _generate_berry_positions
119:# Tests for _overlapping_placement
160:# Tests for berry function
262:# Tests for rope function

$ wc -l tests/generate/test_generate.py
     379 tests/generate/test_generate.py
```

From these results, I understood that the test file had 379 lines and included five different sections. I wanted to organize them in a way that would make more sense based on the functions they were testing.

### How I decided to organize the files

Before moving anything, I spent some time looking at the functions and understanding how they were connected to each other. I did not want to simply create a separate file for every section because that could make the project unnecessarily complicated.

Instead, I decided to group the tests based on the main functions they belonged to. I looked through the source code to understand which internal functions were used by the public functions and how they were connected.

Here is how I understood the existing test structure:

| Test section | Number of tests | Related source |
|---|---:|---|
| `_generate_twisted_thread` | 4 | `_shapes.py:298` |
| `_generate_berry_positions` | 2 | `_shapes.py:21` |
| `_overlapping_placement` | 3 | `_shapes.py:110` |
| `berry` | 6 | `_shapes.py:159` |
| `rope` | 8 | `_shapes.py:443` |

After looking at these connections, I decided to organize the tests into two files based on the main features, rather than creating five separate files.

One thing I also had to consider was the naming convention used by the project. The `tests/` directory does not contain an `__init__.py` file, so I needed to make sure the test file names were unique across the test suite. The project also uses a pre-commit check called `name-tests-test`, which requires test files to follow the `test_*.py` naming format.

I wanted my changes to follow the existing project rules instead of introducing a new structure that might cause problems later.

### My first approach did not work as expected

One thing I learned during this process was that not every test is useful for proving that a change is correct.

During my first attempt, the plan-check tool rejected my approach in its second run. The problem was that I was using a test that only counted the collected tests. This check could still pass even before making any changes, so it was not really proving that my reorganization was correct.

Here is part of plan-check's output from that run (2026-10-31):

```text
**Verdict: reject (hold).**
...
- **test-reproduces-bug: unclear.** The test is the collect-only check (same 43 names)
  plus `just test`, `just lint` and `just pc`. That's the right check for a refactor.
  But there are no repro steps or Actual to compare it with, and it passes before the
  change too, so the rubric can't score it as a pass.
```

After looking at the feedback, I understood that I needed a better way to verify the actual changes. A good test for a refactoring task should be able to identify a problem if the intended changes are not made. Otherwise, it does not provide enough confidence that the work is correct.

After plan-check rejected my first plan, I added a step to save the test names before making any changes, and I updated the test so it repeats the reproduction steps and checks that the old folder is gone and all five expected files exist. This means the test would fail on the original code if the problem had not been fixed.

The test step in my plan, after the change:

```text
2. Before moving anything, save the "before" test list:
   `just test tests/generate tests/data_handling --collect-only -q`
7. Test: re-run the repro steps against the change. Expected after the change,
   which would fail on today's `main`:
   - `ls tests/data_handling` gives "No such file or directory".
   - `ls tests/generate` lists exactly `test_background.py`, `test_berry.py`,
     `test_noise.py`, `test_rope.py` and `test_volume_collection.py`.
   - `just test tests/generate --collect-only -q` lists 43 tests, and their
     function names match the step-2 list exactly.
```

This helped me understand why verification is especially important when reorganizing existing code.

### How I checked that no tests were lost

This was probably the most important part of my contribution. Since I was moving existing tests into different files, I wanted to make sure that all the original tests were still available afterwards.

Before making the changes, I collected the list of tests from the original branch. After reorganizing the files, I collected the tests again from my branch and compared the results.

I used the following commands:

```bash
$ just test tests/generate tests/data_handling --collect-only -q > before.txt

$ just test tests/generate --collect-only -q > after.txt

$ grep '::' after.txt | cut -d: -f1 | sort | uniq -c
   2 tests/generate/test_background.py
  11 tests/generate/test_berry.py
  16 tests/generate/test_noise.py
  12 tests/generate/test_rope.py
   2 tests/generate/test_volume_collection.py

$ diff <(grep '::' before.txt | sed 's/.*:://' | sort) \
       <(grep '::' after.txt  | sed 's/.*:://' | sort) && echo identical

identical
```

I compared the actual test names rather than comparing their file paths because the purpose of my contribution was to move tests into different files. Their locations were expected to change, but the tests themselves should remain the same.

The comparison returned `identical`, which gave me confidence that the original test names had been preserved.

I also checked the test results on the original branch and my updated branch.

| Check | Original branch | My branch |
|---|---|---|
| Test files in `tests/data_handling` | `test_generate_data_handling.py` | Folder not present |
| Tests for the affected areas | 43 passed | 43 passed |
| Complete project test suite | 318 passed | 318 passed |
| Lint checks | — | All checks passed |

These results helped me confirm that the reorganization did not affect the existing tests and that the complete project test suite was still passing. I then submitted my changes through pull request #298, which is currently waiting for review from the project maintainers.

### Some unexpected problems I came across

While working on this contribution, I also faced a few small problems that I had to investigate instead of assuming everything was working correctly.

One example was finding an old `__pycache__` folder inside `tests/data_handling` after switching branches. At first, this made the folder structure look different from what I expected. I had to understand that these were leftover ignored files and not necessarily part of the current branch.

Another issue happened when I was running a shell loop in zsh. I expected a variable to be split into separate values, but zsh handled it differently, which resulted in a command-not-found error. I had to adjust how I was passing the values.

I also experienced a problem with the pre-commit hook because it was built using Python 3.8, which prevented my first commit attempt from completing.

These situations taught me that command output does not always mean what I initially think it means. It is important to investigate errors and understand what is happening before making further changes.

### Tools and checks I used during the contribution

During this work, I also used Claude Code tools to help review the changes and check whether the work followed the original plan.

One of the checks stopped my draft because it identified a rule that I had not followed: I should only make claims that are supported by the actual code changes and command output. After reviewing the draft, I made corrections and ran the check again.

From my voice guard's log:

```text
2026-10-31 16:36 [manual] stop: rule broken: "Claim only what the diff and pasted output show" | target: pr_draft.md
2026-10-31 16:36 [manual] stop: rule broken: "I never let a confident-sounding paragraph substitute for pasting the real output." | target: pr_draft.md
2026-10-31 16:37 [manual] pass | target: pr_draft.md
```

### After I submitted

After I posted my work, maintainer JeppeKlitgaard thanked me on issue #193 on November 9, 2026, and said that @delestro or another team member would review it. I followed up with @delestro on November 12 to ask for a review.

> Hi @arunkasala-open,
>
> Thank you for working on this contribution! I am working a bit less on `qim3d` at the moment due to being allocated to another project, I am sure @delestro or another team member will be happy to review your contribution.
>
> (JeppeKlitgaard on qim3d issue #193, 2026-11-09)

## Limitations and what's next

One limitation of my contribution is that I mainly focused on organizing the existing test files rather than adding new features or changing the functionality of the software. I also tested the changes on macOS, so I have not personally verified the same results on other operating systems. Although all 318 tests passed locally, the final acceptance of my contribution depends on the project maintainers reviewing and approving the pull request.

My next step is to wait for feedback from the maintainers and make any additional changes if they request them. I would also like to learn more about the project's code structure and take part in other improvements in the future. This experience helped me understand that even small changes can be useful in open-source development, especially when they make a project easier for other developers to work with.
