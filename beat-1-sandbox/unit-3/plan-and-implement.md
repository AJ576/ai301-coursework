# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

AJ576

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56#issuecomment-5987206305

> Here's my plan for this, following up on the repro above.
>
> **Cause:** `_extract_sections` in `ingestion/chunking/structural_chunker.py` only keeps lines once it has seen a heading. The collect check on line 120 and both save checks (lines 94 and 124) all depend on `heading_stack`, so a heading-less document comes back with no sections and `chunk()` returns `[]`.
>
> **Fix:** add a fallback in `chunk()`. When `_extract_sections` returns nothing and the text has no heading line, treat the whole document as one section with `heading_path == ""` and `heading_level == 0`. The existing loop then makes it a single chunk, or sends it to the semantic chunker if it's over 800 tokens. The heading-line check matters because heading-only documents like `"# Title"` also return no sections today, and I don't want the fallback to change those. `_extract_sections` stays as it is, so headed documents chunk exactly as before.
>
> **Tests:** remove the `xfail` marker from `test_document_with_no_headings`, and add tests for the no-heading chunk's metadata, a heading-less document over 800 tokens, and a heading-only document still returning `[]`. After the fix, my repro script should print `plain document chunks: 1`.
>
> **Not in this PR:** while tracing this I noticed that the same check also drops intro text above a document's first heading (`"Intro paragraph.\n# Title\nBody"` returns only `['Body']`). Separately, empty sections between two headings produce an empty `''` chunk. Both change how headed documents chunk, and this issue doesn't ask for that, so I'm leaving them for separate issues.
>
> One open question: heading-less documents will now produce chunks with an empty `heading_path`. I haven't checked yet whether anything downstream expects it to always be set, and I'll do that before opening the PR.
>
> I used Claude Code to help trace the cause and draft this plan.

---

## Your branch

**Branch**

fix/56-structural-chunker-no-headings

**Evidence**

Environment: macOS 15.7.2 arm64, Python 3.14.7, tiktoken 0.14.0, pytest 9.1.1.
Base commit: `2f4e82f52efbcfcc57d65b3fa5348672163ca088` on branch `fix/56-structural-chunker-no-headings`.
"Before" = fix in `ingestion/chunking/structural_chunker.py` stashed (new tests and xfail removal still applied). "After" = fix applied.

Repro script (my Unit 2 reproduction):

```python
from ingestion.chunking.structural_chunker import StructuralChunker

chunker = StructuralChunker()

plain = "This is a plain document with no headings at all. " * 20
headed = "# Heading\nThis is content."

print("plain document chars:", len(plain))
print("plain document chunks:", len(chunker.chunk(plain, {"source": "issue-56-repro"})))
print("headed document chunks:", len(chunker.chunk(headed, {"source": "control"})))
```

Before:

```text
plain document chars: 1000
plain document chunks: 0
headed document chunks: 1
```

After:

```text
plain document chars: 1000
plain document chunks: 1
headed document chunks: 1
```

Command: `python -m pytest tests/unit/test_structural_chunker.py -v`

Before:

```text
TestStructuralChunker::test_empty_input_returns_empty_list PASSED [  5%]
TestStructuralChunker::test_whitespace_only_input PASSED [ 11%]
TestStructuralChunker::test_document_with_no_headings FAILED [ 16%]
TestStructuralChunker::test_no_headings_chunk_metadata FAILED [ 22%]
TestStructuralChunker::test_large_document_with_no_headings_is_sub_chunked FAILED [ 27%]
TestStructuralChunker::test_heading_only_document_unchanged PASSED [ 33%]
TestStructuralChunker::test_document_with_nested_headings PASSED [ 38%]
TestStructuralChunker::test_heading_path_format PASSED [ 44%]
TestStructuralChunker::test_large_section_sub_chunked PASSED [ 50%]
TestStructuralChunker::test_chunk_metadata_includes_heading_level PASSED [ 55%]
TestStructuralChunker::test_chunk_metadata_structure PASSED [ 61%]
TestStructuralChunker::test_preserve_source_metadata PASSED [ 66%]
TestStructuralChunker::test_multiple_h1_headings PASSED [ 72%]
TestStructuralChunker::test_heading_path_breadcrumb PASSED [ 77%]
TestStructuralChunker::test_chunks_have_text_content PASSED [ 83%]
TestStructuralChunker::test_section_extraction_with_multiple_levels PASSED [ 88%]
TestStructuralChunker::test_heading_not_in_middle_of_content PASSED [ 94%]
TestStructuralChunker::test_empty_sections_handled PASSED [100%]
FAILED tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings
FAILED tests/unit/test_structural_chunker.py::TestStructuralChunker::test_no_headings_chunk_metadata
FAILED tests/unit/test_structural_chunker.py::TestStructuralChunker::test_large_document_with_no_headings_is_sub_chunked
========================= 3 failed, 15 passed in 0.31s =========================
```

After:

```text
TestStructuralChunker::test_empty_input_returns_empty_list PASSED [  5%]
TestStructuralChunker::test_whitespace_only_input PASSED [ 11%]
TestStructuralChunker::test_document_with_no_headings PASSED [ 16%]
TestStructuralChunker::test_no_headings_chunk_metadata PASSED [ 22%]
TestStructuralChunker::test_large_document_with_no_headings_is_sub_chunked PASSED [ 27%]
TestStructuralChunker::test_heading_only_document_unchanged PASSED [ 33%]
TestStructuralChunker::test_document_with_nested_headings PASSED [ 38%]
TestStructuralChunker::test_heading_path_format PASSED [ 44%]
TestStructuralChunker::test_large_section_sub_chunked PASSED [ 50%]
TestStructuralChunker::test_chunk_metadata_includes_heading_level PASSED [ 55%]
TestStructuralChunker::test_chunk_metadata_structure PASSED [ 61%]
TestStructuralChunker::test_preserve_source_metadata PASSED [ 66%]
TestStructuralChunker::test_multiple_h1_headings PASSED [ 72%]
TestStructuralChunker::test_heading_path_breadcrumb PASSED [ 77%]
TestStructuralChunker::test_chunks_have_text_content PASSED [ 83%]
TestStructuralChunker::test_section_extraction_with_multiple_levels PASSED [ 88%]
TestStructuralChunker::test_heading_not_in_middle_of_content PASSED [ 94%]
TestStructuralChunker::test_empty_sections_handled PASSED [100%]
============================== 18 passed in 0.24s ==============================
```

CI checks after the fix:

- `make lint`: All checks passed!
- `make typecheck`: Success: no issues found in 76 source files
- `make test-unit`: 379 passed, 52 xfailed
- `black --check` on both changed files: 2 files would be left unchanged
- Not run: `make test-integration` (needs Docker), frontend job (no frontend changes)

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 20/20 agreement (`agreement: 20/20 scored items  (bar: 18/20: PASS)`)

This was my only full run, and it is the one committed in `eval-run.txt`.

**Package analysis**

pkg-06 (kubernetes/minikube#21408, category scope-creep). My rubric's verdict was **reject**, and the gold label was **reject**.

The issue is narrow. Saving a preloaded image on containerd writes an empty tar and still exits 0. The repro evidence points to one place: "the underlying export error is swallowed." Its control run shows that saving a pulled image (`minikube image save nginx:alpine nginx.tar`) produces a valid tar, so the save path itself works.

The plan fails **Bounded change**, and its own problem statement says why: "Fixing only the save path would leave the deeper mismatch in place, so this plan addresses the pipeline as a whole." Only step 4, "Surface export errors to the user (non-zero exit) instead of swallowing them", fixes the reported behavior. The other steps are work the issue never asked for:

- "Upgrade the bundled containerd from 1.7.23 to 1.7.28 for every runtime and driver combination, since we are on an old patch series anyway"
- "Introduce a unified image-operations abstraction so `image save`, `image load`, and `image ls` share one code path across docker, containerd, and cri-o instead of three divergent ones"
- "Add a CI matrix job exercising image save/load for each runtime"

My check makes "migrations, new options, abstractions, or cleanups the issue did not ask for" an F "even when the core fix inside them is correct." That fits this plan exactly, because step 4 is a good fix wrapped inside a pipeline rewrite. Bounded change is required, so one F is enough for reject. The comment repeats the same scope ("bump the bundled containerd to 1.7.28 across runtimes, unify the image save/load code paths behind one abstraction"). Comment matches the plan therefore passes, since the comment accurately describes a plan that is too big.

**Check rationale**

> | Bounded change | The plan's scope statement, the files or areas named, and the steps of its approach, read against what the issue actually asks for | Every change the plan commits to is needed to fix the reported behavior. Rewrites, migrations, new options, abstractions, or cleanups the issue did not ask for make it F, even when the core fix inside them is correct. Deferring adjacent work with a reason is a pass, not a fail. An explicit not-in-scope line is useful evidence but is not required: a plan whose named work is already one bounded change passes without one. | required |

This row is exactly as it was written before the eval. I never revised it: my first full run scored 20/20, and scope-creep went 4/4, so no package gave me a reason to change it.

It reads this way because of what it protects against. Scope-creep plans usually contain a correct fix, as pkg-06 does with step 4, and a grader looking at that fix would want to pass the plan. The phrase "even when the core fix inside them is correct" exists to stop that. The check asks whether each change is needed for the reported behavior, not whether it's a good idea.

The row also leaves two things out on purpose. First, "Deferring adjacent work with a reason is a pass" means a plan is never punished for noticing a nearby problem, only for committing to fix it. Second, I chose not to require a not-in-scope section. That would check the plan's layout, not its scope, and the rubric warns that "structure-shaped checks are what make graders disagree with themselves." A requirement like that would reject a terse clear-accept plan that does one thing and simply has no not-in-scope heading. It would also pass a plan that has the heading but still commits to a rewrite.

I saw the check work on my own plan for #56. While tracing the cause I found that the same `heading_stack` gate also drops intro text above a document's first heading, and my draft plan fixed that too. plan-check rejected the draft on Bounded change. I moved the fix into "Not in scope" and Risks with a reason ("it changes chunking for headed documents and #56 doesn't ask for that"), and the revised plan passed. That was a revision to my plan, not to the rubric.

**Trade-offs**

Bounded change can reject a plan that fixes a real adjacent bug, and my own plan is the example. The intro-text bug is real and affects READMEs that start with a badge line or intro paragraph, but #56 only reports documents with no headings. Fixing it in the same plan would have failed this check. I accept that cost: the bug goes on the issue as a candidate for its own issue instead of riding along in this PR.

The check will also miss one kind of case. A plan that really does need a dependency bump or refactor for its fix, but doesn't say why, looks the same as scope creep and would be rejected.

Nothing else changed after my run, and I know because the first full run scored 20/20. By category, scope-creep was 4/4 and clear-accept was 7/7. So the check rejected every scope-creep package and none of the plans that should pass. No package failed, so I made no rubric changes and there was no canary to re-run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
