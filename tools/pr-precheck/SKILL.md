---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?**

A PR package is the candidate PR (title, description, commits, diff, and test evidence) read against the plan it claims to implement and the issue that plan belongs to. Grade the actual package, not the contributor's confidence or the polish of the description.

Return one binary verdict: `accept` or `reject`. Do not provide a score or a third verdict.

## Inputs and modes

There are exactly two modes.

**Live mode** evaluates the contributor's own current branch before a PR is opened. Read:
- the issue thread on GitHub, from the issue URL in the request (`gh issue view <number> --comments`), including its original report and discussion;
- `plan.md` in the working copy, including scope, not-in-scope items, test plan, risks, and its `## Deviations` section;
- the diff on the branch: every committed change relative to the default branch, from `git diff main...HEAD` (three dots) run in the working copy, plus `git diff main...HEAD --stat` and `git log main..HEAD --oneline`. Uncommitted changes are not part of the PR;
- `pr_draft.md`: the PR title on its first line, then the complete draft description;
- `test_evidence.md`: the before and after repro output and the repo's checks with their results;
- the repo's PR template, `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`, including its AI-use policy.

If any of these files is missing, name it in the summary and grade the checks that depend on it `unclear`.

For a house-chain student, use the supplied house plan and house repro pack instead of the student's own plan and issue reproduction. Do not substitute one package's evidence for another's.

**Eval mode** evaluates a supplied package bundle. The bundle is the whole world. Do not fetch, browse, or read anything outside it. Do not use live repository state, external issue pages, or remembered facts to fill gaps. Apply the complete rubric to every bundle.

If the mode or required inputs cannot be established, state the missing item and do not invent it.

## The scope seam (live mode only)

Read `scope.md` before reading or grading the PR package. The `Repo:` line is the boundary. If the candidate repository is not that repository, refuse to grade it as out of scope. If the `Repo:` line is still a placeholder or blank, stop and tell the contributor to fill it in; never guess.

Apply the house rules in `scope.md`. The expected contribution is a PR from the contributor's own fork, on a branch named with a type prefix (`fix/`, `docs/`, `feat/`, `test/`, `refactor/`, `perf/`, or `chore/`), the issue number, and a short slug (for example `fix/1234-null-check`), against the Path Review repo's `main` branch. Do not push directly to the shared semester repository. Check the repo's stated contribution rules and the branch/target evidence.

Eval mode ignores `scope.md` entirely.

## The voice seam (live mode only)

Read `voice-guide.md`. Check the draft PR title and description against every applicable rule. Report broken rules in the readable summary and identify the offending wording.

The voice guide does not change the verdict unless a rubric check explicitly makes the relevant communication standard a verdict-bearing requirement. Do not invent voice rules or apply this guide in eval mode.

Eval mode ignores `voice-guide.md` entirely.

## Component reads

Read these components before grading:
- `rubric.md` defines the checks, evidence requirements, pass conditions, weights, and verdict rule. It is the grading authority.
- `procedure.md` defines the concrete read order, evidence-gathering steps, check execution, and verdict assembly.
- `references/evidence-guide.md` explains where evidence for each harness category lives and what good evidence looks like.

If `rubric.md` has no substantive checks or `procedure.md` has no substantive steps, refuse to grade. Do not invent missing checks or steps at runtime. If the procedure is silent about an evidence source or action, report that gap rather than improvising a new grading rule.

The evidence-guide headings map one-to-one to the rubric categories: plan fidelity = `silent-drift`; test evidence = `not-tested`; diff quality = `unreviewable`; standards and comms = `standards-wall`. A package that passes every required check is a `clear-accept`; the rubric's clear-accept guard keeps terse but complete PRs from being rejected for polish.

## Verdict and output

Apply the rubric's verdict rule exactly. Each check must be graded `pass`, `fail`, or `unclear`. `Unclear` defaults to failure unless the rubric explicitly says otherwise. A required failed or unresolved check means `reject`; preferred checks are handled only as the rubric specifies.

A PR that honestly discloses a shortfall can still be ready when the rubric permits it. Do not automatically reject a package merely because it does not claim to complete every possible check. Do reject unsupported claims, hidden omissions, scope drift, or missing required evidence.

A short readable summary may precede the final output. In live mode, include any voice-guide violations in that summary. End the response with exactly this fenced JSON object, in this key order, with no text after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Do not add keys, reorder keys, add a score, or write anything after the JSON fence.

## Grading discipline

- Evidence first: gather the artifact before deciding whether a check passes.
- Grade the artifact, not polish, confidence, or effort.
- The rubric decides what counts; the procedure decides how to gather and apply it.
- Compare the diff with the plan and issue, test evidence with the test plan, and description claims with the diff and actual test results.
- Distinguish observed facts from assumptions. Do not treat a contributor's statement as proof when the underlying artifact is available.
- Unclear defaults to fail unless the rubric explicitly provides a different rule.
- Do not silently repair missing evidence, infer tests passed, or invent repository policy.
- Preserve the honest-outcome rule: a clearly disclosed, non-required shortfall is not automatically a rejection.
