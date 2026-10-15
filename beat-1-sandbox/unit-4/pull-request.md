# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/241

**Branch**

`fix/56-structural-chunker-no-headings`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 13/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`. Every clear-accept was rejected: six on Test evidence and pkg-02 on Diff quality.
2. Partial `--only` run on the seven clear-accepts plus canaries pkg-04, pkg-07 and pkg-18: `agreement: 10/10 scored items`. Partial runs don't count toward the bar.
3. Full run, saved as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

**pkg-02** (Textualize/rich#4208, category clear-accept). The gold label is `accept`. My rubric said `reject` in run 1, with the note `failed: Diff quality (unreviewable)`, and `accept` in the final run.

In `save_html` and `save_svg`, the only real change is `clear=clear` becoming `clear=False`. That argument sits inside a multi-line call, so the diff also removes and re-adds lines that didn't change:

```
-        html = self.export_html(
-            theme=theme,
-            clear=clear,
+        html = self.export_html(
+            theme=theme,
+            clear=False,
```

My first Diff quality row failed on "re-printed identical lines", so the grader took `theme=theme,` as formatting churn and rejected a correct fix. The gold note says the diff is "exactly the plan's shape". I rewrote the row to judge the whole statement instead of each line, and pkg-02 then passed.

**Check rationale**

Test evidence (`not-tested`), pass condition:

> The deciding question is whether the plan's own repro (the failing case, not a control case that never broke) is re-run with an observable before showing the bug and an after showing the expected behavior. Pass when it is, every failure mode the test plan names is shown, and the repo's suite is reported by its command and outcome (a one-line summary such as "`cargo test -p ignore` passes (214 tests)" is enough; full output is not required). Controls stated as unchanged need no separate output. Fail when the repro is never shown before and after ("tested locally", "works on my machine", "colors work now"), when the evidence runs only a path that never broke, or when a failure mode the test plan names is not re-run. A check reported honestly as not run or failing, with a concrete reason, does not fail this row by itself.

The earlier version said "The repo's required checks or suite show actual output" and failed on "\"tests pass\" with no pasted output". That rejected six of the seven clear-accepts in run 1. pkg-11, for example, ends with "`cargo test -p ignore` passes (214 tests)", and pkg-19 with "`go test ./...` passes (4108 tests)", and both show the repro before and after. The packages that really are not-tested fail for a different reason: their repro is missing. So I made the repro before/after "the deciding question" and accepted a one-line suite summary. I kept "a control case that never broke" in the row so pkg-07 still fails.

**Trade-offs**

Loosening Test evidence could let a not-tested PR through, so I re-ran canaries pkg-04 and pkg-07 with `--only`. Both still rejected, and for the repro, not the suite output. pkg-04: "Only \"Tested locally... colors work now. `cargo test` passes.\" with no before/after output of the plan's `bat Cargo.toml` repro". pkg-07: "Only the one-file control is run; the two-file repro is never shown before/after".

Loosening Diff quality could let churn through, so I also re-ran pkg-18. It still failed on "identical println! lines removed and re-added (commit 'fmt pass')".

Here's what I accept the new checks will miss. A PR that pastes a real repro but claims a suite pass it never ran will now pass Test evidence, because the grader can't verify a one-line summary. And a diff could hide churn inside a statement that really changes.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
