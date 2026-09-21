# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56

**Verdict output**

Live-mode run on 2026-09-21 against three candidates (#66, #62, #56). All three were
accepted; the ranked read-out and the closing JSON block are pasted verbatim below.

```
### Accepted, in order

**1. #56 — Structural chunker drops heading-less documents.** Best match to your profile:
`ingestion/chunking/` is a data-processing pipeline, which lines up with your Bloomberg
real-time ingestion work, and the bug is a genuine behavioral defect rather than a typo —
you have to read the chunker, pick a fallback strategy, and make
`test_document_with_no_headings` go green. That existing failing test plus the `xfail`
marker CONTRIBUTING names gives you a precise definition of done, which fits "concrete
technical problem, reasonably clear scope." It's the one that actually exercises
"debugging unfamiliar systems."

**2. #62 — Health check references `settings.redis_host`.** Squarely in your stack
(FastAPI route, Redis client, config layer) and the most certain to land — the cause is
already located at `api/routes/health.py:47-48`. It drops to second only because the fix
is a one-attribute swap to `redis_url`; given your experience it's a fast merge but thin
practice. Also the only one with another student's claim on it, which doesn't block you
here but means you may not be the only PR.

**3. #66 — structlog not captured by pytest caplog.** Real Python debugging, but it's test
infrastructure in `tests/conftest.py` rather than backend or systems code, and it has no
`good first issue` label. Its suite-wide blast radius means more verification work for
less of the domain learning you said you're after.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 5 days before grading."},
      {"name": "Repository in use", "grade": "pass", "evidence": "archived: false; default-branch commit 2026-09-16, within the last 3 months."},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "'good first issue' label; single module StructuralChunker.chunk() returning [] for heading-less docs, with a named failing test test_document_with_no_headings."},
      {"name": "Nobody is already working on it", "grade": "pass", "evidence": "assignees: []; 0 comments; no linked or cross-referenced PRs; repo has 0 PRs total."},
      {"name": "Contribution policy allows the work", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI prohibition; no AI_POLICY file and no AI-disclosure clause in the PR template."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 5 days before grading."},
      {"name": "Repository in use", "grade": "pass", "evidence": "archived: false; default-branch commit 2026-09-16, within the last 3 months."},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Bounded bug with named cause and fix: api/routes/health.py:47-48 uses settings.redis_host/redis_port; Settings only defines redis_url."},
      {"name": "Nobody is already working on it", "grade": "pass", "evidence": "assignees: []; repo has 0 PRs total; sole comment is a classmate claim (skonda29, NONE) which the Path Review house rule directs us to ignore."},
      {"name": "Contribution policy allows the work", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI prohibition; no AI_POLICY file and no AI-disclosure clause in the PR template."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/66",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Non-bot default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 5 days before grading."},
      {"name": "Repository in use", "grade": "pass", "evidence": "archived: false; default-branch commit 2026-09-16, within the last 3 months."},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Clear bug with repro command and a named fix location (configure structlog in tests/conftest.py, which currently has no structlog config); multiple suggested approaches do not fail this check."},
      {"name": "Nobody is already working on it", "grade": "pass", "evidence": "assignees: []; 0 comments; no linked or cross-referenced PRs; repo has 0 PRs total."},
      {"name": "Contribution policy allows the work", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI prohibition; no AI_POLICY file and no AI-disclosure clause in the PR template."}
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

One full run. Agreement: `19/20 scored items  (bar: 18/20: PASS)`, matching the
agreement line in the committed `eval-run.txt` (run written `2026-09-21T03:30:50Z`,
rubric `sha256:e9676164d3a8de79`). Per-category tallies on that run:
`claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 3/4`.

**Issue analysis**

`issue-15` (source `zulip/zulip#19589`, category `scope`). My rubric returned **accept**;
the gold label is **reject**, noted as "years of design debate and two abandoned PRs
behind a friendly label". This is the single disagreement in the run.

My rubric accepted it because every required check found a passing fact. The repo is
plainly alive (`last push to any branch: 2026-08-04`, `latest release: 12.1 (2026-06-26)`,
`archived: no`), the contribution policy allows AI use with conditions rather than banning
it, and the "Nobody is already working on it" check passed on the literal evidence: the
bundle records `assignees: none` and both linked PRs — `zulip/zulip#20840 (closed)`,
`zulip/zulip#23123 (closed)` — are closed, which my pass condition explicitly forgives
("A closed or abandoned PR does not fail by itself"), while the many `@zulipbot claim`
comments are all followed by a bot unassigning the claimer for inactivity, so none is a
current claim.

The check that should have caught it is "Scope fits a newcomer", and it passed. The issue
body reads as a bounded, well-specified change — separate the bot mention into a `command`
field and leave the rest in `text` — and my pass condition grades exactly that: whether a
contributor could act on the stated problem. What the gold label is grading is the thread,
not the body: 97 comments, four years of maintainers still asking for payload evidence,
and a queue of contributors who claimed it and vanished. My check reads the request and
never reads that history, so a well-written request with a rotten thread behind it slips
through.

**Check rationale**

From `tools/issue-select/rubric.md`, the "Scope fits a newcomer" row, quoted as it is
currently written:

> Pass if the issue describes a reasonably clear bug, task, documentation change, cleanup,
> or feature request that a contributor could act on. The exact implementation does not
> need to be specified. A bug passes when its affected behavior or causes are clearly
> identified, even if the issue lists multiple possible fixes or implementation
> approaches. Fail if it is an umbrella/tracking issue, a pure usage/support question, an
> unresolved design or product decision, or explicitly requires broad/core-internal
> changes. Multiple suggested approaches, technical investigation, or related
> implementation changes do not by themselves make an issue out of scope. If the intended
> problem or outcome is genuinely unclear after reading the issue and thread, grade
> unclear.

It is written this way because my first instinct — reject anything that looks
complicated — was the wrong failure mode. Real bugs in real repos come with speculation
attached: contributors propose two or three fixes in the body, maintainers reason about
causes in the thread, and none of that makes the work big. The two sentences that forgive
"multiple possible fixes" and "technical investigation" exist to stop the check from
punishing a thorough writeup. The fail list is deliberately a list of shapes rather than
adjectives — umbrella/tracking issue, usage question, unresolved design decision,
broad/core-internal — so the check turns on what kind of thing the issue is, not on how
hard it feels to me. That shape list is what caught `issue-05` (a codebase-wide
type-annotation umbrella), `issue-10` (a self-described megaissue) and `issue-20` (a
one-line feature wish with a product decision inside it), all three under a friendly
label.

**Trade-offs**

It gives up thread archaeology, and `issue-15` is the case whose result that changes. The
check grades the clarity of the problem as stated, so it is blind to history: four years
open, 97 comments, and two abandoned PRs are all invisible to it as long as the body
describes one bounded change. The evidence guide names that signal — "an issue open for
years with several abandoned attempts is telling you something about its real
difficulty" — and my pass condition does not encode it.

I accepted the miss rather than patching it. The obvious fix is a clause failing issues
with N+ abandoned attempts or a thread where maintainers are still gathering requirements,
but the four `scope`-category items are all gold-reject, so that clause has no
accept-labelled scope item to protect it from over-firing — the only place it could be
checked is against the eight `clear-accept` items, which the current wording gets 8/8. A
history-based rule that fires on "long thread, old issue" is exactly the rule that would
start rejecting healthy, popular issues, and trading a confirmed 8/8 for one recovered
scope item is the wrong trade at a 19/20 pass. The honest statement is that my rubric will
keep accepting well-written issues that the community has quietly given up on, and in live
mode I compensate by reading the thread myself before I claim.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to my interests and the time available.** I want backend and data-pipeline work,
   and `ingestion/chunking/structural_chunker.py` is exactly that — a document-processing
   stage that silently drops input, which is the same class of bug I chased on the C++
   real-time ingestion system I worked on at Bloomberg. It is Python in a FastAPI service,
   both of which I have used. On time: the module is 133 lines, the reproduction in the
   issue is four lines I can paste into a REPL, and `docs/CONTRIBUTING.md` names this issue
   directly as a seeded bug whose test carries `@pytest.mark.xfail(strict=True, reason="issue
   #56: structural chunker drops heading-less docs")`. That gives me an unambiguous finish
   line — make `test_document_with_no_headings` pass and delete the marker — which is what
   makes it fit inside a Unit 2 window rather than sprawling.

2. **What the verdict identified correctly, and what I weighed that the rubric could not.**
   The rubric got the verifiable half right: the repo is alive (human commit four days
   before I graded), nothing is assigned, the repo has zero PRs so nobody is mid-flight,
   and `docs/CONTRIBUTING.md` sets conditions on contributions without banning AI-assisted
   work. What it could not weigh is that it accepted all three candidates, so the rubric
   did not actually choose for me — ranking did, and ranking is fit, not quality. Two
   things I weighed myself: #62 is the safer merge but it is a one-attribute swap
   (`redis_host` → `redis_url`) that teaches me nothing about this codebase, and #56 leaves
   a real decision to me that the issue deliberately does not settle — chunk the
   heading-less document as a single block, or fall back to another strategy. The rubric
   reads that open end as "implementation not specified, still passes"; I read it as the
   part worth doing, because choosing the fallback means understanding how the rest of the
   RAG pipeline consumes chunks. I also passed over #66 despite it being genuine debugging,
   because a change to `tests/conftest.py` alters how every test in the suite observes
   logging, and I would rather my first PR here have a blast radius I can fully verify.

3. **Anticipated difficulty in claiming it.** Low. It is unassigned, has zero comments,
   and no PR in the repo references it, so I am not stepping on anyone. The Path Review
   house rule makes this easier still: classmates' claims do not block an issue and credit
   attaches to the PR I open, not to whether it merges. The real friction is after the
   claim, not during it — `docs/CONTRIBUTING.md` warns that a first-time contributor's PR
   sits at "waiting for approval to run workflows" until a maintainer releases CI, and the
   PR template requires all five CI jobs green before review. So I should expect a delay I
   do not control between opening the PR and getting any signal, and I need to remember
   that removing the `xfail` marker is part of the fix — leaving it in makes CI fail *because*
   my fix worked.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
