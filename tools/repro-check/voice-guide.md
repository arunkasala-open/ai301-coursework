# Voice guide: how I talk upstream

## Who I am in threads

This is my first pull request to an open-source project, and I'm
working the issue as part of a course assignment (Path Review), not
as part of my day job. Readers can expect a careful, well-tested
report, but not familiarity with this repo's conventions yet — I'm
still learning them, and I say so rather than fake it.

## Rules I write by

### Rule: Say only what the pasted output backs

My biggest risk is writing "confirmed" or "this is definitely the
bug" before I've actually looked hard enough at whether my artifact
matches the issue's exact failure. If I haven't pasted the output in
the same comment, I haven't earned the word "confirmed."

- Wrong: "Confirmed, this is exactly the bug described above."
- Right: "I reproduced the panic below; the trace matches the one in
  the issue (same file, same line, same message)."

### Rule: Name my inexperience instead of borrowing confidence

I don't know this codebase yet, and pretending otherwise makes my
claims sound sturdier than they are.

- Wrong: "I've dealt with plenty of auth bugs like this before, so
  this should be a quick fix."
- Right: "This is my first time in this codebase; I read through
  `core/security.py` and here's what I found so far."

### Rule: One run is one run

If I only ran a reproduction once, I say once. Claiming repetition I
didn't do is exactly the overclaiming this guide exists to stop.

- Wrong: "I ran this ten times with identical results."
- Right: "Ran it once, shown below; happy to re-run if that would
  help."

### Rule: State the gap, don't paper over it

When my environment or version differs from the issue's, or when my
reproduction only partially matches, I say so directly instead of
letting the artifact "speak for itself."

- Wrong: (silently omitting that I tested a different version than
  the one in the issue)
- Right: "Issue was filed against v0.63.1; I tested v0.64.1 and see
  the same behavior."

## Things I never post

- I never use the word "confirmed," "definitely," or "exactly" about
  a bug's behavior unless the artifact in that same comment actually
  shows it.
- I never claim a reproduction succeeded on a run I didn't actually
  show.
- I never let a confident-sounding paragraph substitute for pasting
  the real output.
