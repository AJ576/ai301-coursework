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

## Things I never post

- I never claim to have reproduced a bug before testing it.
- I never present a hypothesis as a confirmed cause.
- I never promise a fix before I understand the failure.
- I never use generic filler instead of issue-specific information.
