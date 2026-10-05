# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

# Rubric

Each check is graded P (pass), F (fail), or ? (unclear: the evidence a
check names is missing, too vague to decide, or cannot be matched
against the repro evidence).

A check whose condition does not apply to this package (no maintainer
direction in the thread, no stated policy ask) is graded P and the
evidence line says why it does not apply. Judge the plan and the
comment themselves, never their shape: a terse plan with no headings
can pass every check, and a long sectioned one can fail them all.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Grounded diagnosis | The plan's stated cause, read against the repro-evidence block's steps, artifact, and control runs (live: the student's posted repro comment) | The stated cause names a specific mechanism (a file, function, call site, or the operation that misbehaves) AND nothing in the repro evidence rules that cause out. A control run or artifact in the package that the cause contradicts, or that already exonerates the named culprit, is F even when the cause sounds plausible and the thread agrees with it. A cause restated as the symptom ("it crashes", "it is slow") is ?. | required |
| Bounded change | The plan's scope statement, the files or areas named, and the steps of its approach, read against what the issue actually asks for | Every change the plan commits to is needed to fix the reported behavior. Rewrites, migrations, new options, abstractions, or cleanups the issue did not ask for make it F, even when the core fix inside them is correct. Deferring adjacent work with a reason is a pass, not a fail. An explicit not-in-scope line is useful evidence but is not required: a plan whose named work is already one bounded change passes without one. | required |
| Executable by a stranger | The plan's approach: the files or areas it names, the direction it chose, and the order of work | Someone who has not read the issue could start the first step today without asking the author a question. The plan has picked its approach: unresolved forks left to build time ("gocui or tcell, not sure", "upstream or vendored, whichever is easier", "profile and then optimize") are F, and so is work described only as an area to investigate with no file, function, or layer named. Naming a flagged unknown alongside a chosen approach is not a fork. | required |
| Decisive test plan | The plan's test plan, read against the repro evidence's steps and artifacts | The test plan names something observable that distinguishes fixed from broken: a command or repro step to re-run, plus the outcome expected after the fix (an exit code, an output value, a passing named test case, a rendered result). "Run the test suite", "test it manually", "should feel fast", or "nothing else should feel broken" is F: those pass identically before and after the fix. An expected outcome that does not follow from the diagnosis is F. | required |
| Thread direction | The plan and the plan comment, read against the thread highlights (live: the issue thread) | Where a maintainer has already given explicit direction (an isolated culprit, a proposed approach, a rejected approach, a posted test build, a request to test something), the plan follows it or says plainly why it departs, and the comment engages it rather than talking past it. A plan that proposes something the thread already rejected, or a comment that answers a different question than the one the maintainer asked, is F. If the thread contains no explicit maintainer direction, P. | required |
| Stated policy asks | The plan comment, read against the repo-facts block's bug-report template asks and contribution policy, including any AI-use policy | Whatever the repo's stated policy asks of a comment like this, the comment does it. Specifically: if the policy requires disclosing AI use, the comment discloses it (tool and extent); if the policy requires the comment be in the contributor's own words, the comment reads as written rather than generated. Every package here is AI-assisted work, so a required disclosure that is absent is F, however good the plan is. If the repo facts state no policy ask that reaches a comment, P. | required |
| Honest unknowns | The plan's risks, unknowns, and (live) its `## Deviations` section | Claims the evidence does not support are marked as unknown, open question, or risk rather than asserted flatly, and anything deferred says so. False confidence in an otherwise sound plan lands here. | preferred |
| Comment matches the plan | The plan comment, read against the plan it belongs to | The comment commits to the same cause, scope, and approach the plan does, and promises nothing the plan does not contain (no timelines the plan has not justified). | preferred |

## Verdict rule

- **accept** (ready to post and build from) if every `required` check
  is P.
- **reject** (hold) if any `required` check is F or ?. An `?` counts
  exactly as an F: a claim that cannot be verified from the package is
  not a claim that can be built from.
- `preferred` checks never move the verdict in either direction. Report
  an F or ? on one in the summary as feedback, then apply the two rules
  above unchanged.
- There is no third verdict. One required F is enough to reject, and
  the output names that check as the deciding one.
