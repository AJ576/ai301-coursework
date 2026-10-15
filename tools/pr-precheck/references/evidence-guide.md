# Evidence guide: where evidence lives in a PR package

Each heading below is one evidence family. The rubric check of the same name reads it. Under each one: where to look in an eval bundle, where to look in live mode, and what good looks like. The families map to the harness categories one to one: plan fidelity = `silent-drift`, test evidence = `not-tested`, diff quality = `unreviewable`, standards and comms = `standards-wall`. A package that fails none of them is a `clear-accept`.

## Plan fidelity (harness category: silent-drift)

**Where it lives (eval bundle):**
- `## Plan context`: the plan's scope (the files and behavior it commits to change), its explicit "not in scope" or "out of scope" items, its test plan, and any deviation or deferral note.
- `## Issue` and `## Thread highlights`: what was actually asked for, and any maintainer direction the plan adopted.
- `### Diff` under `## Candidate PR`: every changed file and hunk.
- `### Description` under `## Candidate PR`: every claim about what the change does ("implements the plan exactly", "no other changes", "docs now document...").

**Where it lives (live mode):**
- `plan.md` in the working copy, including its `## Deviations` section.
- `git diff main...HEAD` (three dots) for the diff, and `git diff main...HEAD --stat` for the changed-file list.
- `pr_draft.md` for the title and description.
- The issue thread on GitHub (`gh issue view <number> --comments`).

**What good looks like:**
- Every changed file and hunk falls inside the plan's stated scope, is a test for that scope, or is named in a deviation note with a reason.
- Every item the plan commits to is in the diff, or the plan's note and the description both say it was deferred and why.
- Every claim in the description about what changed can be pointed at in the diff.

**Silent drift, both directions:**
- More than the plan: a new config option, flag, or setting; a rename or refactor pass; or a rewrite of a function or file the plan never names, with no deviation note. It is drift even when the description says "exactly as planned". It is drift *especially* then.
- Less than the plan: a planned item (a docs update, a second fix, a test) is missing from the diff with no note. It is also drift when the description claims that item was delivered.
- Not drift: an adjacent problem noticed and explicitly deferred in the plan note and the description.

## Test evidence (harness category: not-tested)

**Where it lives (eval bundle):**
- `### Test evidence` under `## Candidate PR`: commands run and their pasted output.
- `## Plan context`: the plan's test plan and the repro evidence it was built on (the exact failing input or steps).
- `### Diff`: any added or changed test files.
- `## Repo facts`: the repo's own required checks or test commands, if stated.

**Where it lives (live mode):**
- `test_evidence.md` next to `plan.md`: before (on `main`) and after (on the branch) output of the unit 2 repro steps, plus the output of `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`.
- The test plan section of `plan.md`.
- Test files in `git diff main...HEAD`.
- The Testing section of `pr_draft.md`, which should paste from `test_evidence.md`.

**What good looks like:**
- The plan's own repro (the failing input, not a nearby control case) is re-run, with the before output showing the bug and the after output showing the expected behavior. The observable result is named.
- If the plan's test plan names several failure modes or repro cases, each one is re-run.
- The repo's required checks are reported by command and outcome. A one-line summary such as "`go test ./...` passes (4108 tests)" is enough; the repro before/after is what must be shown in full. A check that could not run, or failed for a reason outside the change, is reported as is, with the reason. Example: Path Review's `make test-integration` stops with "no tests ran".
- Any added test exercises the changed code path, not behavior that already worked.

**Not tested looks like:** "tested locally", "works on my machine", or "tests pass" with no output; evidence that runs only the path that never broke; or a claimed check result with nothing pasted.

## Diff quality (harness category: unreviewable)

**Where it lives (eval bundle):**
- `### Diff`: every hunk, including context lines.
- `### Commits`: the commit messages.

**Where it lives (live mode):**
- `git diff main...HEAD` and `git diff main...HEAD --stat`.
- `git log main..HEAD --oneline` for the commits.

**What good looks like:**
- Every hunk is part of the fix or its test.
- No debris. Debris means:
  - debug prints or logging left in (`print`, `eprintln!`, `console.log`, `DEBUG`)
  - commented-out code or abandoned first attempts
  - dead functions, including ones kept alive with `allow(dead_code)` or an `_unused` prefix
  - stray TODOs added in the diff
  - re-indentation or formatting-only churn
  - import reshuffles, or whole blocks of statements removed and re-added unchanged (lines re-printed only because they share a statement with a real edit, like the start of a multi-line call whose argument changed, are not churn)
  - edits to files unrelated to the fix
- In live mode, `plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` must not appear in the diff.
- Commit messages say what changed. "wip", "fix", "fmt + cleanup", or "misc cleanups while debugging" signal debris and are worth checking hunk by hunk.

**The test:** could a reviewer read the diff top to bottom and see only the fix and its tests? Even one debug leftover or commented-out block fails this check, even if the fix itself is right.

## Standards and comms (harness category: standards-wall)

**Where it lives (eval bundle):**
- `## Repo facts`: the `pull requests:` line (template sections, checklist items such as "closes #", changelog or whatsnew entries) and the `contribution policy:` line (CONTRIBUTING and any AI policy).
- `### Title` and `### Description` under `## Candidate PR`.
- `### Diff`: whether a changelog or whatsnew file the template requires is actually changed.

**Where it lives (live mode):**
- `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the Path Review repo.
- `pr_draft.md`: title on line 1, then the filled template.
- The branch name (`git branch --show-current`). It must have a type prefix (`fix/`, `docs/`, `feat/`, `test/`, `refactor/`, `perf/`, or `chore/`), the issue number, and a short slug.

**What good looks like:**
- Every section and checklist item the repo's template asks for is present with real content. For Path Review that is Summary, Issue (`Closes #N`), Changes, Testing (only true boxes ticked, CI left unticked), Screenshots / Demo (or "N/A" with a reason), and Notes for Reviewers.
- Any file the template requires (a changelog entry, a whatsnew note) is in the diff.
- Where the repo's policy asks for AI-use disclosure, the description states the tool used and how much it did, in the contributor's own words. Treat every package as AI-assisted: a missing disclosure in a repo that requires one is a fail, even when the code is excellent. The course requires this disclosure in Notes for Reviewers in live mode.
- The title names the issue-specific behavior changed.
- If the repo has no template and no stated policy, there is nothing to fail here. Do not invent requirements.

**Standards wall looks like:** a strong fix whose description skips the template's checklist, has no "closes #", omits the required changelog entry, or has no AI disclosure where the policy requires one.
