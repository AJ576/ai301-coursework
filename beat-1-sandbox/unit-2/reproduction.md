# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

AJ576

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56#issuecomment-5851954501

I'd like to work on issue [#56](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56). I'll first reproduce the heading-less document case described in the issue, then trace the structural chunker behavior and add or update the relevant test before making a fix.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56#issuecomment-5860224306

I reproduced [#56](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56).

## Environment

- macOS 15.7.2, Apple Silicon (`arm64`)
- Python 3.14.7
- `tiktoken` 0.14.0
- Commit: `2f4e82f52efbcfcc57d65b3fa5348672163ca088`

## Steps

From the repository root, I created a Python 3.14 virtual environment and installed `pytest` and `tiktoken`.

I then ran:

```python
from ingestion.chunking.structural_chunker import StructuralChunker
chunker = StructuralChunker()
plain = "This is a plain document with no headings at all. " * 20

headed = "# Heading\nThis is content."
print("plain document chars:", len(plain))

print("plain document chunks:", len(chunker.chunk(plain, {"source": "issue-56-repro"})))

print("headed document chunks:", len(chunker.chunk(headed, {"source": "control"})))
```

The output was:

```text
plain document chars: 1000

plain document chunks: 0

headed document chunks: 1
```

I also ran the existing test:

```bash
python -m pytest -q tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings -rx
```

which reported:

```text
XFAIL ...test_document_with_no_headings - issue #56: structural chunker drops documents with no headings
```

## Result

The heading-less document produces no chunks, while the headed control document produces one chunk. This matches the behavior described in issue [#56](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56).

The existing test is marked as an expected failure for this issue. I reproduced the issue locally and will investigate the chunking logic before making the fix.

## Eval iterations

**Run history**

- 17/20 agreement
- 20/20 agreement
- 19/20 agreement

**Package analysis**

`pkg-05`

My rubric decided **reject**, while the gold label said **accept**.

The package reproduced the reported Conda issue, but my original `steps-reproducible` check was too strict for a cannot-reproduce report. It required the steps to allow another person to reproduce the reported behavior, rather than allowing an honest reproduction attempt to pass when it clearly documented the attempted trigger and observed result.

I revised the check so that a reproduction package passes when the steps are sufficient to understand and repeat the attempted trigger, even when the issue itself is not reproduced.

**Check rationale**

> | steps-reproducible | The reproduction steps, commands, inputs, setup, and any referenced files in the repro report | Pass if another person with the stated environment and required project state could understand and repeat the attempted trigger without guessing a command, input, or setup detail that could affect the outcome. For a cannot-reproduce report, the steps only need to establish a concrete attempt at the issue's relevant trigger; they do not need to reproduce the bug or exactly duplicate the reporter's setup when the report clearly identifies the relevant environmental or setup difference. | required |

I revised this check after the first full run because the original wording treated failure to reproduce the bug as a failure of the reproduction steps. The revised wording distinguishes whether the package documents a reproducible bug from whether it documents a concrete reproduction attempt. This was necessary for the `clear-accept` cases where an honest cannot-reproduce report is itself the correct outcome.

**Trade-offs**

The revision changed the results of the three packages that initially disagreed with the gold labels: `pkg-05`, `pkg-09`, and `pkg-10`. I reran those packages with `--only` after revising the rubric and the result reached `20/20` agreement.

The trade-off is that `steps-reproducible` is now more permissive for cannot-re