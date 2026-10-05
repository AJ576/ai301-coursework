# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives.** In an eval bundle: the plan's opening diagnosis
line or paragraph under `## Candidate plan`, read against the
`## Repro evidence` block, whose three parts carry the weight -- the
numbered steps, the artifact (the pasted output, error, panic, or
timing matrix), and the control runs ("Control:", "Control runs:",
"the same test with X removed"). The `## Thread highlights` may also
name a culprit; it is context, not proof. Live: the diagnosis section
of the draft `plan.md`, read against the student's own posted repro
comment on the issue thread (or, on the house issue, the house repro
pack as quoted in the draft).

**What good looks like.** The cause names a mechanism you could point
at in the code (a file, a function, a call site, an operation that runs
at the wrong time) and the repro evidence's artifact is what that
mechanism would produce. Read the control runs last and hardest: a
control exists to exonerate something, so a cause the control already
cleared is contradicted no matter how confident the plan or the thread
sounds. Nothing at all is pinned down when the stated cause only
repeats the symptom the issue reported.

## Scope

**Where it lives.** In an eval bundle: the plan's `Scope:` statement,
any `In scope:` / `Not in scope:` text, and -- just as important --
the numbered `Approach:` steps, which are where unannounced extra work
actually shows up. Read those against the issue title and body excerpt
under `## Issue`. Live: the Scope and Files sections of `plan.md`
against the issue body.

**What good looks like.** One bounded change: the files or areas named
are the ones the diagnosis implicates, and each approach step exists
because the reported behavior needs it. A drive-by rewrite announces
itself in the approach list -- a migration, a new option, a
restructure, a unifying abstraction, a CI matrix, or a framework
appearing next to a two-line fix. A plan that narrows itself (fixes
one platform, defers the general case, gives a reason) is bounded, not
incomplete. An explicit not-in-scope line is good evidence of bounding
but its absence proves nothing on its own: check the approach steps
instead.

## Executability

**Where it lives.** In an eval bundle: the plan's `Approach:` steps
and whatever files, functions, or layers they name. Live: the Files
and Approach sections of `plan.md`.

**What good looks like.** A stranger can open the first named file and
start. The approach has already made its decisions: which layer, which
of the competing fixes, which direction. Hedged forks are the tell --
"gocui? tcell? not sure", "upstream or vendored, whichever is easier",
"add recover() somewhere", "profile first and then optimize" -- each
one is a decision the plan pushed to build time, which means the plan
is not the thing being graded, the build is. A chosen approach that
flags one open question beside it is still executable; a plan made of
open questions is not.

## Test plan

**Where it lives.** In an eval bundle: the plan's `Test plan:` line or
section, read back against the `## Repro evidence` steps and artifact
it should re-run. Live: the Test plan section of `plan.md` against the
repro steps in the posted repro comment.

**What good looks like.** A before and after a stranger could score:
the command or repro step to run, and the outcome that will differ
after the fix -- an exit code, a specific output value, a named test
case that currently panics and then passes, a rendered result, a
color, a count. The vague ones share a property worth naming: they
return the same result before and after the fix, so they prove
nothing. "Run the full test suite" is in that family whenever the
suite passes today, and so is any outcome phrased as a feeling ("feels
fast", "nothing else seems broken"). Best case, the test plan reuses
the repro's own steps and names what the artifact will say instead.

## Honesty

**Where it lives.** In an eval bundle: a `Risk`, `Risks`, `Unknown`,
`Open question`, or "stated" line in the plan, and any deferral inside
the scope statement. Live: the Risks section of `plan.md` and, after
the build, its `## Deviations` heading.

**What good looks like.** The gap between what the evidence shows and
what the plan assumes is written down, in the plan, before anyone
asks: a measurement not yet taken, a platform not yet tested, a
cross-tool behavior checked but unresolved. False confidence looks the
opposite: a long, polished plan that asserts a mechanism or an outcome
the package never demonstrates, with no risk section at all. After the
build, an honest deviation says what changed and why in `plan.md`; a
deviation that exists only in the diff is not recorded.

## Comms

**Where it lives.** In an eval bundle: the `## Candidate plan comment`
section, read against two places -- the `## Thread highlights` list
(maintainer and owner comments, marked OWNER, MEMBER, or CONTRIBUTOR,
carrying any isolated culprit, proposed or rejected approach, posted
test build, or request) and the `## Repo facts` block's `bug reports:`
template asks and `contribution policy:` line, which is where any
AI-use or own-words requirement is stated. Live: the live issue
thread, plus the repo's CONTRIBUTING.md, AI policy file, and issue
templates.

**What good looks like.** Thread-aware means the comment is visibly
written after reading the thread: it takes up the direction a
maintainer already gave, answers the question that was actually asked,
reports back on a patched build if one was posted, and says plainly
when it departs from a proposed approach and why. Boilerplate reads
the same whether or not the thread exists -- it announces a plan into
the air, and it is worst when the thread already isolated the culprit
the comment ignores. On policy: read the `contribution policy:` line
literally. Where it requires disclosing AI use, a conforming comment
names the tool and the extent of the assistance ("the fix will be
AI-assisted with me reviewing every change"); where it requires the
contributor's own words, a conforming comment reads specific and
human, not generated. Where the line says there is no AI policy, or no
disclosure ask for issue comments, there is nothing to conform to and
the absence of disclosure is not a defect.
