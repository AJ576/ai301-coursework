# Contribution: DataMapper: Improve `choice` modal

**Contribution Number:** [1]
**Student:** Aditya Jha
**GitHub Username:** AJ576
**Issue:** https://github.com/KaotoIO/kaoto/issues/3541
**Pull Request:** https://github.com/KaotoIO/kaoto/pull/4385

---

## Toolkit

**Unit 5: Built issue-scout and patched its strategy and rubric after two scans**

I wrote a search strategy (three `gh search issues` queries from my C++/systems profile, plus ranking filters) and a six-check rubric. My first scan broke in two ways: queries 2 and 3 returned 0 results because gh searched the quoted `OR` list as one exact phrase, and all four accepted issues were in 0-star repos with a single committer, one created the day before the scan. I removed the quotes, added a filter that drops issues created in the last 7 days, and added a required "Has outside users" check (at least 50 stars and at least 3 contributors besides the owner). The second scan then rejected all five for repo size, which showed that ranking can't see stars, and grading my real picks showed that the rubric has no check for whether I can build and test the work on my own machine.

**Unit 6: Installed the voice guard, wrote repo memory for Kaoto, and patched my scout's scope check**

I merged my three voice-guide copies into one guide, added Upstream rules for claims, questions and pings, and installed the guard as a PreToolUse hook. I tested it by feeding it a body-less `gh issue comment`, which it stopped, while an ordinary command passed. I'd already posted both of my Kaoto comments from the browser before the guard existed, so I ran `check_voice.py --draft` on them afterwards, and it stopped both: the claim broke my own "never promise a date" rule ("I'm aiming to open a PR by the weekend"), and the "PR is up" note broke "be specific to the issue" ("It's my first open source PR, so please let me know if anything should be changed"). I also wrote `CLAUDE.local.md` in my clone (37 lines, excluded from git) and replaced my scout's "Scope fits a newcomer" check with the `scope-feasible` row plus a difficulty note; it didn't change any verdict on the three issues I regraded.

**Unit 7: Ran the ship-gate on my already-open PR branch: two stops, then a pass**

I opened PR #4385 before installing the ship-gate, then ran it by hand on the branch afterwards. My first manifest used the full `@kaoto/kaoto test` suite, and the gate stopped on 4 failures in files my PR doesn't touch. Rerunning them on `main` (ceb5be9b) showed that `mapping-serializer.service.test.ts` fails there too, and that the `Form.*-end` tests only time out at random in the randomly ordered full run, so I moved the full suite to the excluded table with that reason and ran the four areas my change touches instead. The second run passed every repo check, but my own pr-precheck rejected the description for silent drift: it quotes the empty-state text as "Select a member to view its details." while the code says "Select a candidate to see its details.", and it claims test commands my evidence doesn't show. After I fixed those lines in `pr_draft.md`, the third run printed `ship-gate: PASS`.

**Unit 8: Wrote a playbook from Kaoto's real response times and ran the PR monitor once**

I set my staleness thresholds from five merged PRs by outside contributors, whose first maintainer review came in 2–19 hours, or about 2.8 days over a weekend: 5 quiet days before a first-review nudge, 7 for a stalled review. The monitor read #4385 as FEEDBACK-IN, because CodeRabbit's "Linked Issues" warning says the expanded schema is incomplete (no nested fields, nothing for abstract members inside a choice). It drafted a reply that doesn't code anything and asks the maintainers to choose, following my rule "I give the reason once and ask which way they want it". It also noted that no human has reviewed yet, so the reply shouldn't tag anyone. I held the reply: so far only a bot has reviewed, and I want the human maintainers to weigh in before I answer the nested-schema question.

---

## Why I Chose This Issue

I ran two scans with issue-scout, then graded the picks I was considering. The scout accepted only ROCm/iris#589 (a GPU collective-kernel optimization). I didn't take it, because its checklist needs correctness runs on 2/4/8 AMD GPUs and a benchmark on an MI355X, and I work on a Mac. meteor/meteor#14909 already had an open PR from someone else, and aibrix#3100 turned out to be a call for testers rather than one piece of work. My next choice was ImperialCollegeLondon/proCAT#861. The scout rejected it only on star count, and I was going to choose it anyway, but the maintainer replied that they're keeping it for their own new team members. I then graded KaotoIO/kaoto#3541 and the scout accepted it, passing all six checks. The maintainers are active (2.14.0 released 2026-10-29), the repo has 131 stars and 49 contributors, a MEMBER filed the issue, the previous assignee was removed on 2026-10-26 with no PR, and CONTRIBUTING.md explicitly allows AI-assisted work. It's frontend work in TypeScript/React, not the systems work my profile asks for, but it's small and clearly scoped (one modal component, reusing the existing `ResizableSplitPanels`), and I can build and test it end to end on my own machine before the deadline. I hope to learn how to work in a large monorepo and follow a project's review process.

The scout held up on the checks it has: it caught an issue that already had an open PR (meteor#14909) and an issue pointing at files that no longer exist (New-Grad-Jobs#113). Its gaps were real, though. Star count undercounts institutional research tools like proCAT; ranking can't see repo size, so a whole scan spent its grading slots on repos that were bound to fail; and nothing checks whether I can build and test the work with the hardware I have. That last gap is why its only accept was an issue I couldn't do.

**Claim comment:** https://github.com/KaotoIO/kaoto/issues/3541#issuecomment-6646914305

**Toolkit**

- Tool: issue-scout (my search strategy and my six-check rubric). I ran two scans, then graded 7 hand-picked issues directly.
- What it produced: an accept for kaoto#3541, with all six checks passing (active maintainers, release 2.14.0 on 2026-10-29, 131 stars and 49 contributors, no assignee or open PR, AI use allowed); earlier, a single accept (iris#589) out of five picks.
- Difficulties: the first scan's queries silently returned 0 results; the stars threshold rejected a real institutional project (proCAT); ranking can't see stars, so a scan's grading slots went to repos that were bound to fail; and there's no check for whether I can test the work with the hardware I have.
- What changed: removed the quotes from the queries, added a 7-day minimum issue age and the "Has outside users" check, and logged the hardware and stars gaps in scan-notes.md to fix next. In Unit 6 I swapped my scope check for `scope-feasible` plus a difficulty note. Regrading Kaoto afterwards gave "reject", but only because the assignee and the open PR are my own: the "Nobody is already working on it" check can't tell my claim from a stranger's.

---

## Understanding the Issue

**Problem Description**

When an XML schema has an `xs:choice` with more than 10 members, the DataMapper's "Select Member..." modal shows the members as one narrow column in the middle of a large modal. Each row repeats the member's description plus a "Fields:" line cut off at 3 names, and there's no way to see the full shape of the member you're about to pick.

**Expected Behavior**

The member list sits on the left, in a resizable split panel like the RestDslEditor's, and picking a member shows its schema (its description and fields) on the right.

**Current Behavior**

One centered column in a `medium` modal. Selecting a member only checks its radio button. At most 3 child field names are ever shown (`MAX_CHILDREN_PREVIEW = 3`), with no types.

**Affected Components**

- `packages/ui/src/components/Document/actions/FieldContextMenu/WrapperSelectionModal.tsx` (+ `.scss` and `.test.tsx`): the modal. It's shared with the abstract substitution flow ("Select Substitute...").
- `models/datamapper/field-action.ts`: `IWrapperCandidate`, the data each row is built from.
- `services/visualization/wrapper-base.service.ts` and `choice-field.service.ts`: build the candidates, and decide when the modal opens (`INLINE_CHOICE_LIMIT = 10`).
- `components/ResizableSplitPanels/`: the existing split panel the RestDslEditor uses.

**Toolkit**

- Tool: Claude Code in my Kaoto clone, reading the DataMapper folder only (the scoped orientation from Unit 6), plus my `CLAUDE.local.md`.
- What it produced: the 2–4 files above, how they connect (service builds candidates → hook opens modal → modal renders them), and where their tests live.
- Difficulties: I first assumed the modal opens for "complex" members. Re-reading `choice-field.service.ts` later showed that the real trigger is more than 10 members, and my original PR description stated the wrong reason.
- What changed: I wrote the correct trigger into `CLAUDE.local.md` and my repro steps, and corrected the PR description.

---

## Reproduction Process

**Environment Setup**

- macOS (Darwin 24.6.0), Node v26.13.0 (the repo asks for Node 22 or later), Yarn 4.18.1, Kaoto `main` at `ceb5be9b`.
- I forked KaotoIO/kaoto, cloned my fork (AJ576/kaoto), added `upstream`, ran `yarn install`, then `yarn workspace @kaoto/kaoto start` → http://localhost:5173, DataMapper debugger at `#/datamapper`.
- Challenges: `yarn` wasn't on my PATH, so I ran the repo's pinned release directly (`node .yarn/releases/yarn-4.18.1.cjs ...`). The bigger one: the repo's `ChoiceWithAbstract.xsd` stub never opens the modal, because its choices are small (10 or fewer members are shown inline). I wrote my own demo schema, `ChoiceModalDemo.xsd`, with 12 complex choice members, each with a description and 2–3 fields.

**Steps to Reproduce**

1. Start the dev server and open `http://localhost:5173/#/datamapper`.
2. Attach `ChoiceModalDemo.xsd` as the target document schema (any `xs:choice` with more than 10 members works).
3. Right-click the choice field and pick **Select Member...**.
4. Observed: a `medium` modal with the search box and radio list in one centered column; each row shows its description and "Fields:" cut off at 3 names; selecting a member changes nothing but the radio button. The same layout appears in "Select Substitute..." for abstract elements, because both use `WrapperSelectionModal`.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/AJ576/kaoto/commit/ceb5be9b (the `main` commit I reproduced on; the bug is a layout, so there's no failing test to commit. The before screenshots are the evidence.)
- **Screenshots/logs:** before (and after) screenshots of both modals are in the PR #4385 description.
- **My findings:** the narrow layout reproduced exactly as in the issue's screenshot. Two things the issue didn't say: the modal only appears above 10 choice members, which is why the repo's own stub couldn't show it, and the same component also drives the abstract substitution modal, so any change hits both.

**Toolkit**

- Tool: repro-check (my Unit 2 skill, now using the Unit 6 "wild" scope), run on my posted claim (`claim-draft.md`) and a repro report (`repro-draft.md`).
- What it produced: **reject**. behavior-matches, environment-recorded, honest-conclusion and comms-compliant passed; steps-reproducible failed because step 2 depends on `ChoiceModalDemo.xsd`, which isn't published anywhere, and doesn't name the control that attaches the schema. It also flagged that my posted claim breaks my own voice rule about promising dates.
- Difficulties: I ran it after the fact, on 2026-11-08, two days after my PR was already open, so it graded a repro I never posted. The honest result is that a stranger couldn't repeat my repro without my demo schema.
- What changed: I put the >10-members trigger into the steps above. I haven't published `ChoiceModalDemo.xsd` yet, so step 2 still can't be repeated exactly. Anyone with their own `xs:choice` of 11 or more members can follow the rest.

---

## Solution Approach

**Analysis**

This isn't a crash. It's a layout that never used the space it had. `WrapperSelectionModal` rendered the search box and the radio list straight into `ModalBody`, with no second pane, so the list sat in one narrow column of a modal much wider than it. There was also nothing to put in a details pane: the candidate data (`IWrapperCandidate`) only carried `childrenPreview`, a list of at most 3 field names built by `WrapperBaseService.formatChildrenPreview` (`MAX_CHILDREN_PREVIEW = 3`), with no types.

**Proposed Solution**

Wrap the modal body in the existing `ResizableSplitPanels` (the RestDslEditor's component, as the issue suggests). The left pane holds the filter and a compact radio list; the right pane shows the selected candidate's name, type badge, description, and every direct child field with its type. Replace `childrenPreview` with `children: { name, type }[]` so the pane has real data.

Deliberately not changing: the 10-member threshold for opening the modal; the nested schema (only **direct** fields are shown, and I asked the maintainers whether they want more); and substitution-specific data (the substitution modal gets the new layout because it shares the component, but its candidates have no fields).

**Implementation Plan**

1. `WrapperSelectionModal.tsx`: wrap `ModalBody` content in `ResizableSplitPanels` (`defaultLeftWidth={45}`); move the filter and list to the left; add a `CandidateDetails` pane on the right with an empty state and a "No additional details available." fallback.
2. `field-action.ts` / `wrapper-base.service.ts`: replace `childrenPreview?: string[]` with `children?: IWrapperCandidateChild[]`, filled by `formatChildren` without the 3-name cutoff.
3. `WrapperSelectionModal.scss`: give the split panel a fixed height inside the modal and let each pane scroll on its own; switch the modal to `ModalVariant.large`.
4. Tests: rewrite the list-text assertions in `WrapperSelectionModal.test.tsx` for the details pane, and update `useChoiceContextMenu.test.tsx` and `choice-field.service.test.ts`.

Deviations: I didn't plan the fixed height or the `large` variant up front. Both came from the split panel collapsing inside the modal (see Implementation Notes). I wrote `plan.md` after the PR was open, so it records the plan I actually followed rather than one I posted first.

**Toolkit**

- Tool: plan-check (my Unit 3 skill with my own `procedure.md`, now using the wild scope), run on `plan.md` and `plan-comment.md`.
- What it produced: **accept**, with all six required checks passing. It noted that the plan follows the issue author's direction ("use a resizable panel like we use for the RestDslEditor"), and that the test plan is decisive because it names test cases that fail against the old modal.
- Difficulties: I ran it after the fact, so the accept only says the plan I followed holds up; it didn't steer the build. It also exposed a gap in my procedure: it takes the repro evidence from "the student's posted repro comment", and I never posted one.
- What changed: nothing in the plan. I logged the procedure gap for my unit 9 portfolio. Per the wild scope, `plan-comment.md` stays off the issue thread.

---

## Testing Strategy

- **Re-run the repro, before and after.** Same steps with `ChoiceModalDemo.xsd`. Before (`ceb5be9b`): one centered column, "Fields:" cut off at 3 names. After (`4d436136`): a `large` modal split 45/55 with a drag handle; selecting CreditCard shows cardNumber, holderName and expiry with their types; nothing selected shows "Select a candidate to see its details."; filtering still works on the left. I checked the substitution modal too: same layout, with the fallback text on the right. The screenshots are in the PR.
- **Named unit tests that fail against the old modal:** "should show an empty details pane when nothing is selected", "should show description and fields of the selected candidate in the details pane", "should update the details pane when the selection changes", "should show a fallback when the selected candidate has no details", "should show details of the pre-selected candidate", "should render the candidate list and details in resizable split panels".
- **Repo checks (AGENTS.md Definition of Done), run 2026-11-08:** the changed areas (`test src/components/DataMapper src/components/Document src/services/visualization`) gave 90 files passed / 1 skipped, 1930 tests passed / 24 skipped; the focused modal and service tests gave 11 files, 190 tests passed; `lint`, `lint:style`, `build` (typecheck) and `commitlint` all exited 0. On the PR: SonarCloud Quality Gate passed, Snyk passed.
- **What isn't green, stated plainly:** the full `@kaoto/kaoto test` suite has one failure, `mapping-serializer.service.test.ts > should serialize mappings with respecting Document field order`, which fails on `main` without my commit too; and the `Form.*-end` tests time out at random in the full run but pass on their own. Cypress E2E and Chromatic run only in CI, and I didn't test inside the VS Code extension.

**Toolkit**

- Tool: the repo's own Yarn scripts, run through the ship-gate's manifest, and `test_evidence.md` as the record.
- What it produced: the counts above, plus the discovery that the full suite isn't green on `main` on my machine.
- Difficulties: the full suite fails for reasons my change doesn't touch, and Kaoto runs its tests in a random order, so the timeout failures moved around between runs. Telling "my change broke it" from "it was already broken" meant checking out `main` and rerunning the failing files there.
- What changed: I test the changed areas plus lint, style lint and build, and record the full-suite failure with its cause instead of claiming "all tests pass".

---

## Implementation Notes

**2026-11-04**

Picked Kaoto #3541 after proCAT #861 was reserved by its maintainer. The scout accepted it, with all six checks passing. I read the modal (`WrapperSelectionModal.tsx`, 139 lines) and found `ResizableSplitPanels` already in use in the RestDslEditor, so the plan became "plug in an existing component" rather than "build a resizable panel". Posted my claim comment (it shows as 2026-11-05 in UTC).

**2026-11-06**

igarashitm assigned the issue to me. While building: the first version of the split panel collapsed to almost no height inside `ModalBody`, because the panel sizes itself to its parent and the modal body has no fixed height. I gave the panel's container a fixed height and let each pane scroll on its own. With two panes, `medium` was too cramped, so I switched to `ModalVariant.large`. Moving the description and "Fields:" lines out of the list rows broke the tests that looked for that text in the list; I rewrote them to check the details pane instead (empty state, after selecting, on changing the selection, the fallback). The repo's `ChoiceWithAbstract.xsd` never opened the modal, so I had Claude Code generate a demo schema with 12 complex members. Opened PR #4385 at 20:09 UTC and posted a note on the issue.

**2026-11-08**

Ran the course tools on the finished branch, out of order. repro-check rejected my repro (the demo schema isn't published); plan-check accepted my plan. The ship-gate stopped twice: first on full-suite failures I traced to `main`, then on my own PR description, which quotes the empty-state text wrong ("Select a member to view its details." vs the code's "Select a candidate to see its details.") and claims test commands I can't show. Fixed both in my local draft, then updated the live PR description. Also found that the modal really opens above 10 choice members, not for "complex" members as my PR originally said. The PR monitor read CodeRabbit's "expanded schema is incomplete" warning and drafted a reply asking the maintainers which way they want it.

**Toolkit**

- Tool: Claude Code (Claude Opus 5.5) in my Kaoto clone, with `CLAUDE.local.md` and Kaoto's own `AGENTS.md` loaded.
- What it produced: the implementation across 8 files (+244/−100), the rewritten tests, and the demo XSD used for manual testing.
- Difficulties: it reproduced my own wrong assumption about when the modal opens, and it wrote the PR description's empty-state text from an earlier draft of the code, so the description drifted from the diff. Neither showed up until the ship-gate's pr-precheck compared them.
- What changed: I now check the description against `git diff` before opening a PR, which is exactly what the ship-gate's pr-precheck does.

---

## Pull Request

**PR Link:** https://github.com/KaotoIO/kaoto/pull/4385

**PR Description:** Titled "feat(DataMapper): use resizable split panels in choice/abstract selection", with `Fixes #3541`. It moves the choice modal to `ResizableSplitPanels`: the filter and candidate list on the left, the selected member's name, type, description and every direct field on the right, an empty state and a "no details" fallback, a fixed panel height, and `ModalVariant.large`. It replaces `childrenPreview` (3 names) with `children` (every direct field with its type), notes that the substitution modal gets the same layout, includes before and after screenshots of both modals, lists the tests, asks three open questions (nested schema? descriptions no longer inline; substitution has no field data), and has an AI Assistance section naming Claude Code. When I first posted it, the description had the empty-state text wrong and an inaccurate test-command line; I corrected it on 2026-11-08 after the ship-gate's pr-precheck caught both.

**Maintainer Feedback:**

- 2026-11-06: igarashitm (MEMBER) assigned the issue to me ("Thank you for your interest on contributing to Kaoto project! I assign this issue to you.").
- 2026-11-06: CodeRabbit (bot) review: a "Linked Issues" warning that the expanded schema is incomplete (no nested fields, and no details for abstract members inside a choice). SonarCloud Quality Gate passed. No human review yet.
- 2026-11-08: the PR monitor drafted a reply to the CodeRabbit warning that asks the maintainers to choose rather than coding it. I held it until a human reviewer weighs in.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (Unit 7), run by hand after the PR was already open, and the PR monitor (Unit 8) with my playbook.
- What it produced: the ship-gate passed on its third run, after one manifest fix and one description fix. The monitor read the PR as FEEDBACK-IN (the CodeRabbit warning) and drafted a reply asking the maintainers to decide on nested fields.
- Difficulties: I did the steps out of order: I opened the PR on 2026-11-06 and installed the ship-gate on 2026-11-08, so the gate checked a PR that was already public, so the description errors it caught had already been posted. The PR title was also cut off ("...choice/abstract selec…") until I noticed it.
- What changed: I corrected the live PR description on 2026-11-08 after pr-precheck caught the errors (the empty-state text now matches the code). The PR title had been cut off at "selec…" when I opened it; I fixed it to "…choice/abstract selection" on 2026-11-08. I also tested both hooks in a fresh Claude Code session with `gh pr create --help`: the voice guard stopped it ("no body the guard can read") and the ship-gate stopped it ("no PR text to check"), and both stops are in their logs.

---

## Learnings and Reflections

**Technical Skills Gained**

- Reading a large TypeScript/React monorepo by following one feature: from the service that builds candidates (`choice-field.service.ts`) through the hook that opens the modal to the PatternFly component that renders it.
- PatternFly modals and layout: why a resizable split panel collapses inside `ModalBody` without a fixed height, and how `ModalVariant` sizes work.
- Running Vitest per file and per area, and noticing that Kaoto randomizes test order (`Running tests with seed ...`), which is why some failures move around between runs.
- Telling a pre-existing failure from a regression by checking out `main` and rerunning the exact failing files.

**Challenges Overcome**

- Finding an issue I could actually finish: my scout's only accept needed AMD GPUs, my next pick was reserved by its maintainer, and another already had a PR. I re-picked three times before Kaoto.
- Reproducing the bug: the repo's own stub schema never opened the modal. Reading the code showed the real trigger (more than 10 members), and I built a demo schema to meet it.
- The ship-gate's full-suite stop: I proved the failures were on `main` too before excluding the check, rather than deleting it.

**What I'd Do Differently Next Time**

Run the tools in order. I opened my PR before my ship-gate existed, and when I ran it afterwards it caught real mistakes in my description that are now public. I'd also write `plan.md` and post the repro before building, so the plan and the repro could steer the work instead of grading it afterwards; publish any test fixture a reviewer needs; and keep claim comments free of dates I haven't earned yet.

---

## Resources Used

- Kaoto's `AGENTS.md` (Commands, Definition of Done, "Focused validation during development"): the exact test, lint and build commands, and what CI runs.
- Kaoto's `CONTRIBUTING.md`, "AI-Assisted Contributions": the disclosure and human-responsibility rules I followed in the PR.
- `packages/ui/src/pages/RestDslEditor/RestDslEditorPage.tsx`: the existing use of `ResizableSplitPanels` the issue pointed to, which I copied the props pattern from.
- CodeRabbit's review on #4385: its "Linked Issues" warning is what made me separate direct fields from nested schema in Open Question 1.
