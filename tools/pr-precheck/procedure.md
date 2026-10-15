# PR precheck procedure

## Read order

1. Determine whether this is live mode or eval mode. Identify the single candidate PR package and its item identifier.
2. In live mode, read `scope.md` first. Confirm that the candidate repository matches its `Repo:` line. Stop if the repository is outside scope or the line is unfilled. Eval mode does not read scope.
3. Read `rubric.md` and `references/evidence-guide.md` to establish the checks, evidence sources, category mapping, and verdict rule.
4. Read `procedure.md` in full and follow these steps. If the rubric has no substantive checks or this procedure has no substantive steps, refuse to grade.
5. In live mode, read `voice-guide.md` before reviewing the outgoing title and description. Eval mode ignores it.
6. Read the issue and its relevant discussion, then the plan that the PR claims to implement. Read the plan before the diff so the intended scope and test plan are known before inspecting implementation.
7. Read the commit list and the complete unified diff. In a local working copy, obtain the diff from the merge base with the target branch, normally `git diff main...HEAD`; use the repository's actual default branch if it differs. Do not infer diff contents from a summary.
8. Read test evidence (`test_evidence.md` in live mode) and compare it with the plan's test plan and the repository's required checks.
9. Read the repository's PR template (`.github/PULL_REQUEST_TEMPLATE.md`), contribution instructions (`docs/CONTRIBUTING.md`), and relevant policies in live mode. Then read the complete draft PR title and description. For eval mode, use only the corresponding artifacts in the supplied bundle.

## Evidence gathering

For each rubric row, collect the evidence before assigning a grade.

- **Plan fidelity / `silent-drift`:** Record the issue's requested behavior, the plan's committed scope and approach, explicit exclusions, risks and deviations, and the files and behavior changed in the diff. Compare each material diff hunk with the plan. Compare each factual claim in the PR description with the diff and test evidence. Record any planned work omitted, unplanned work added, or disclosed deviation.
- **Test evidence / `not-tested`:** Record the plan's regression tests and broader checks. For each, capture the command, actual result, and whether it was run. Inspect the added or changed tests to confirm they exercise the reported failure and relevant guards. Compare claimed results in the description with actual outputs. Distinguish required checks from checks explicitly described as optional or blocked.
- **Diff quality / `unreviewable`:** Record changed paths and commits. Inspect the full diff for unrelated files, generated artifacts, formatting-only churn, debug leftovers, secrets, or unrelated hunks. Determine whether a reviewer can isolate the fix and its tests.
- **Standards and comms / `standards-wall`:** Record the actual PR template sections, contribution requirements, and applicable disclosure rules. Compare the draft title and description against those requirements. Check that every required template section has real content and that omissions or skipped checks are disclosed.
- **Title and description clarity (preferred):** Record the title and the summary's first sentence, and whether each names the specific behavior the diff changes.

For live mode, the issue-side evidence must come from the real repository: issue thread, PR template, and stated policy. Do not replace missing repository evidence with assumptions. For a house-chain student, use the supplied house plan and house repro pack where specified. In eval mode, the bundle is the whole world; do not fetch external material.

## Check execution

1. Execute the checks in the order listed in `rubric.md`.
2. For each check, assign `pass`, `fail`, or `unclear` using only its stated evidence and pass condition.
3. Each evidence field in the final JSON must be one concise line naming the decisive fact or quoting the decisive source. Avoid generic statements such as "looks good."
4. If evidence is absent, record what is missing and grade `unclear` unless the rubric explicitly defines another treatment. Do not fill gaps by guessing.
5. If the diff contradicts the plan or the PR description, grade plan fidelity against the actual diff. If a claimed test result has no actual output, do not count it as run.
6. If the PR template or policy cannot be found in live mode, report that evidence gap. Do not invent template sections or disclosures.
7. In live mode, compare the title and description with the voice guide and report each broken rule in the readable summary. Voice-guide violations alone do not change the verdict unless a rubric check makes that requirement verdict-bearing.
8. Do not reject solely because an explicitly non-required check was skipped for a stated, credible reason. Do reject if a required regression test or required check is missing, or if the omission is hidden or misrepresented.

## Verdict assembly

Apply the verdict rule in `rubric.md`: `accept` only if every required check passes; otherwise `reject`. An `unclear` required check is a failure. Before rejecting, apply the rubric's clear-accept guard and honest-outcome rule: confirm each failing check names a concrete fact from the package (a hunk, a missing output, a missing section), not a judgment about length or polish.

Choose the first failed or unclear required check in rubric order as the lead reason in the readable summary. Mention other material failures or evidence gaps concisely. If all required checks pass, state that the package is ready to submit.

End with the exact JSON schema defined in `SKILL.md`, using the PR URL or bundle ID as `item`, all rubric checks in rubric order, and no extra keys. The fenced JSON object must be the final content in the response.
