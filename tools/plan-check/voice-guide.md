# Voice guide: how I talk upstream

## Who I am in threads

I'm a student contributor working on PathReview issues. I state what I tested, what I observed, and what I plan to investigate without overstating certainty.

Readers should expect concise, issue-specific comments based on what I actually know.

## Rules I write by

### Rule: State evidence, not assumptions

I distinguish what I observed from what I think might be causing it.

- Wrong: "This is definitely caused by the chunker ignoring documents without headings."
- Right: "I'll reproduce the heading-less document case first, then trace how the chunker handles it."

### Rule: Don't claim work I haven't done

I don't say I reproduced or confirmed something before I actually have.

- Wrong: "I've confirmed the bug and know what needs to be fixed."
- Right: "I'll first reproduce the behavior described in the issue."

### Rule: Be specific to the issue

I mention the actual behavior or task instead of using generic contribution language.

- Wrong: "I'd like to work on this issue and contribute a fix."
- Right: "I'd like to work on issue #56 and reproduce the case where a document without headings produces no chunks."

### Rule: Keep comments concise

I give enough context to make my intent clear without adding unrelated background.

- Wrong: "I've been looking through the repository and there are several interesting areas I could potentially investigate..."
- Right: "I'll reproduce the reported case, trace the chunker behavior, and then work on the relevant test and fix."

### Rule: Commit to an approach without overselling my certainty

A plan comment commits me to a direction in front of the people who
maintain the code, so I name the approach I chose and keep the parts I
have not measured visible as open questions.

- Wrong: "This will fix it with no performance impact."
- Right: "I plan to recompute the pointer only when the page capacity
  changed, so the hot path stays one comparison. I have not measured
  that comparison yet; if it shows up in the print benchmark I will
  move the check to the two growth-adjacent sites instead."

### Rule: Answer the direction a maintainer already gave

If someone with context has isolated a culprit, proposed an approach,
rejected one, or asked me to test something, my comment takes that up
first, and says why if I am going somewhere else.

- Wrong: "Since there's a workaround, I plan to document it." (posted
  under a thread where the owner isolated the culprit in a source file
  and asked the reporter to test a patched build)
- Right: "Thanks for the patched build from 8916cbc -- it fixes
  everything but the first key press for me. I plan to work on the
  console input handling in src/tui/light_windows.go rather than
  documenting the /dev/tty workaround."

### Rule: Disclose AI assistance when the repo asks for it

Where CONTRIBUTING or an AI policy asks contributors to disclose AI
use, I state the tool and how much it did, in the comment, in my own
words.

- Wrong: (no mention, in a repo whose policy requires disclosing all
  AI usage)
- Right: "Per the repo's AI policy: the implementation will be
  AI-assisted with Claude Code, with me reviewing and testing every
  change before it goes up."

## Things I never post

- I never claim to have reproduced a bug before testing it.
- I never present a hypothesis as a confirmed cause.
- I never promise a fix before I understand the failure.
- I never use generic filler instead of issue-specific information.
- I never promise a date or a turnaround ("PR up this week") that I
  have not already done the work to back.
- I never write "same approach as above" instead of my own plan.
- I never leave an AI-use disclosure out of a comment in a repo whose
  stated policy asks for one.
