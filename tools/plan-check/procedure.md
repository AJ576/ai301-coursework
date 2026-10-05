# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

Read the package in this order, every time, and keep a short note per
step. The order exists so the plan is read against evidence already in
hand: a confident plan read first becomes the lens you judge the
evidence through, which is exactly how a wrong cause gets accepted.

1. **Repo facts** (eval: the `## Repo facts` block; live: CONTRIBUTING,
   the AI policy file, the issue template). Note two things verbatim:
   what the bug-report template asks for, and what the contribution
   policy requires of a comment -- especially whether AI use must be
   disclosed and whether comments must be in the contributor's own
   words. Note "no stated AI policy" just as explicitly when that is
   what it says.
2. **The issue** (title, body excerpt). Note in one line what behavior
   is reported and what the issue asks for. This line is the yardstick
   for the Bounded change check.
3. **Thread highlights** (live: the thread). Note every explicit
   maintainer or owner move: a culprit isolated (with file and lines),
   an approach proposed, an approach rejected, a test build posted, a
   question or request to the reporter. Mark each one as direction the
   plan will be measured against. If there is none, write "no explicit
   direction" -- that note is what makes Thread direction a P later.
4. **Repro evidence.** Note the steps, the artifact (output, error,
   panic, timing matrix), and every control run and what each control
   rules out. Write the controls down as exclusions ("tokenizer
   cleared", "bindings cleared", "growth-during-print is the
   trigger"): these are what a wrong cause collides with.
5. **The candidate plan**, read last of the two drafts. Note the
   stated cause, the files and areas named, each approach step, the
   test plan's observable outcome, and any risk or unknown.
6. **The candidate plan comment.** Note what it commits to, what
   thread move it engages, and whether it carries a disclosure.

Do not grade anything while reading. Steps 1-4 produce the evidence;
steps 5-6 produce the claims measured against it.

## Evidence gathering

One gathering move per evidence family in the rubric. Record the quote
or fact, not an impression: every check's output line must name the
fact that decided it, so capture it while reading.

- **Grounded diagnosis.** Pull the plan's stated cause (one sentence,
  quoted) and set it beside the exclusion list from read-order step 4.
  Record: the cause, and for each control run whether it supports the
  cause, is neutral, or rules it out. If the thread named a culprit
  too, record that separately -- thread agreement is not evidence of
  cause.
- **Bounded change.** List every distinct unit of work the plan
  commits to, taken from the scope statement *and* each numbered
  approach step. Tag each unit "needed for the reported behavior" or
  "extra". Record any deferral and whether a reason is given.
- **Executable by a stranger.** From the approach steps, record the
  first concrete action and the file, function, or layer it names.
  Record any unresolved fork verbatim ("X or Y, whichever is easier").
- **Decisive test plan.** Record the command or step to run, the
  stated before state, and the stated after outcome. Then ask the one
  question that decides it: would this produce a different result
  before and after the fix? Record the answer.
- **Thread direction.** For each direction noted in read-order step 3,
  record how the plan and the comment treat it: follows, departs with
  a stated reason, departs silently, or never mentions it.
- **Stated policy asks.** From read-order step 1, record each policy
  ask that reaches a comment like this one. For each, record the
  comment text that satisfies it, or "absent". If no ask reaches a
  comment, record "no applicable ask".
- **Honest unknowns.** Record each risk, unknown, or open question the
  plan states, and each assertion made with no support in the repro
  evidence.
- **Comment matches the plan.** Record any cause, scope, approach, or
  promise in the comment that is not in the plan.

Gather only from the package in eval mode. Live, gather from the
locations named in `references/evidence-guide.md`; a fact you cannot
locate there is an absence, which is a grade, not a reason to keep
hunting elsewhere.

## Check execution

Execute the checks in rubric-table order: Grounded diagnosis, Bounded
change, Executable by a stranger, Decisive test plan, Thread
direction, Stated policy asks, then the two preferred checks. The
order is deliberate -- the cause informs whether the scope and the
test plan follow from anything -- but an earlier F never short-circuits
a later check. Grade all of them, every run: the summary is feedback,
not just a verdict, and a package held on one check still tells the
student what else is wrong.

For each check:

1. Restate the rubric's pass condition in your own words before
   looking at the grade you expect. Grade the condition, not your
   overall feeling about the plan.
2. Apply it to the facts gathered for that check only. No re-reading
   the whole package; if the gathering step for this check recorded a
   fact, that is the evidence, and if it recorded nothing, that
   absence is the evidence.
3. Grade P, F, or ?:
   - **P** when the recorded facts satisfy the condition, *or* when the
     condition does not apply to this package (no explicit maintainer
     direction; no policy ask that reaches a comment). Say which in
     the evidence line.
   - **F** when the recorded facts violate the condition -- including
     when something required by a stated policy or by the thread is
     recorded as absent.
   - **?** when the evidence the check names exists but is too vague to
     decide, or cannot be matched against the repro evidence at all.
     Reach for `?` only after rereading the one section the check
     names; a plan that simply does not say the thing is an F against
     a condition that requires it, not a `?`.
4. Write one evidence line: the quote or fact that decided it, in the
   package's words where possible. "Looks fine" is not a grade.

Where the rubric and your reaction disagree, the rubric wins; note the
tension in the summary. Where this procedure is silent on a package
you hit, note the gap in the summary rather than inventing a step.

## Verdict assembly

1. Collect the grades for the six `required` checks. Ignore the
   `preferred` grades entirely for verdict purposes -- they are
   reported, never counted.
2. Convert every `?` on a required check to a fail before counting.
   An unverifiable claim is not a claim you can build from.
3. If all six required checks are P, the verdict is `accept`. If one
   or more is F (or a converted `?`), the verdict is `reject`. No other
   outcome exists; do not soften a reject into a conditional accept or
   round a single F up because the rest of the plan is strong.
4. Name the deciding check: the first required F in rubric-table
   order. Quote its evidence line in the readable summary as the
   reason the package is held, and list any other required F beneath
   it, then the preferred-check notes as feedback.
5. Emit the summary, then the fenced JSON block last, with every check
   (required and preferred) in the `checks` array, each with its grade
   and its one-line evidence, and the binary `verdict`. Nothing follows
   the JSON block.

Same grades in, same verdict out: the only input to step 3 is the six
required grades.
