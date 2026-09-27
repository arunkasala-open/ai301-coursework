# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: the repro report's own "Environment" line(s) (eval
bundle: the `## Candidate repro report` section; live mode: the
draft's environment line, or the issue thread if the student is
answering a maintainer's follow-up question about it). Compare it
against the issue's stated environment (eval bundle: the `## Issue`
section's own version/OS line; live: the issue body) and, when the
repo has one, the bug-report template's asked-for fields in the
repo-facts block.

What good looks like: a named tool version and OS/platform, stated as
plainly as "bat 0.26.1 (cargo), Fedora 44." When the reporter's
environment differs from the one the reproducer used, a good report
says so in one line ("filed against v0.63.1 on Termux; same behavior
here on 0.64.1 Linux") rather than leaving the reader to notice the
mismatch themselves. A report that shows a perfect artifact match to
the issue but never states what it ran on is still a fail here — the
match without an environment record is not evidence someone else can
place.

## Steps

Where it lives: the repro report's numbered steps or command
transcript, read against its own stated starting point (a fresh repo,
a specific file, a specific config).

What makes steps followable: every input the commands touch is either
shown inline or trivially recreatable from what's shown (a one-line
file, a `git init` from empty). A stranger should be able to copy the
starting state and the commands and land on the same fork in the
road, with no invented shorthand ("using our internal config," "our
company monorepo") standing in for something that cannot leave the
report. If a report says a file, repo, or config is private and
therefore not included, that is an automatic fail here regardless of
how convincing the pasted output looks — nobody else can check it.

## Behavior shown

Where it lives: the artifact block(s) in the repro report (command
output, log excerpt, panic/stack trace, screenshot description),
read against the issue's own description of the failure — not the
issue's title or one-line summary, but the specific mechanism named
in its body (which function panics, which flag path is taken, which
message appears).

What it means for an artifact to show the issue's behavior: the same
failure class, at the same point, for the same reason as the issue
describes. A `capacity overflow` panic is not the same behavior as an
"invalid value" parse error, even on the same command family and even
if the reporter calls both "a crash." The single most common way a
report fails this check while looking clean: the reproducer silently
changes the trigger input (a `:` where the issue used `=`, an
offset-from-end range typo'd as offset-from-start) and the artifact
that results is a different, adjacent failure — read the exact input
shown against the exact input the issue gave, character for
character, before trusting a paragraph that says "confirmed."

## Honesty

Where it lives: the claim comment's and repro report's own
conclusion language ("Expected"/"Actual" lines, an "Analysis" or
summary paragraph), read against what the artifact block actually
shows.

What separates a specific-and-honest report from an overclaiming one:
the conclusion says only what the shown run demonstrates about
whether the issue's behavior occurred. A confident "this confirms the
bug" or "exactly as described" resting on an artifact that actually
shows a different failure fails, no matter how rigorous or thorough
the write-up sounds — and watch for reports that cite an unshown
repeat count ("I ran this ten times, identical results") specifically
to prop up that kind of mismatched confirmation.

That is different from an honest cannot-reproduce report, which
often *also* mentions more than one attempt (a base run plus a
variant tried to provoke the behavior) without pasting a transcript
for each one. That is normal methodology narration, not overclaiming:
the report isn't using the attempt count to assert the issue's
behavior occurred, it's using it to show real effort was made before
concluding it didn't. Judge what the repeat-count claim is being used
to support, not just whether a count is present without a matching
transcript.

## Comms

Two distinct things live under this family; check both.

**AI-use disclosure.** Where it lives: the repo-facts block's
contribution policy line (eval bundle) or the repo's CONTRIBUTING.md /
AI-policy doc (live mode), read against the claim comment and repro
report text. What good looks like: if the policy is silent on AI use,
there is nothing to check here and the package passes by default. If
the policy states AI assistance must be disclosed, a passing comment
names the tool and the extent of the assistance somewhere in what was
posted; a comment that says nothing about AI use when the policy
requires it fails, even if the reproduction itself is flawless —
disclosure is checked as its own fact, never inferred from how
polished or unpolished the prose looks.

**Restraint in the claim comment.** Where it lives: the claim comment
alone (not the repro report). What good looks like: a comment that
states interest, cites the reproduction below it, and says what the
student plans to look at next — nothing more. Fails here: guarantees
about a timeline or outcome the evidence cannot back ("I will fix
this within 2 days, guaranteed"), demands to have the issue reserved,
or flattery/urgency standing in for substance ("please, I really need
this," excess exclamation points and emoji in place of technical
content). This is separate from `claims-match-evidence`: that check
grades whether the *reported results* are overclaimed; this one
grades whether the *ask* is overclaimed.
