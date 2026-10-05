# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: the repro report's environment record, compared with the issue context's stated target version, platform, or runtime requirements.

What good looks like: the report gives the relevant project/software version or commit and operating system/platform, plus any runtime version needed to interpret the reproduction. If the tested environment differs from the issue's target, that difference is explicitly stated.

## Steps

Where it lives: the repro report's reproduction steps, including setup commands, inputs, referenced files, and the command or action that triggers the behavior.

What good looks like: for a reproduced issue, a stranger with the stated environment and project state can follow the steps from the stated starting point to the reported result without guessing a command, input, file, or setup detail that could affect the outcome. For a cannot-reproduce report, the steps should instead make clear that the relevant issue trigger was actually attempted; they do not have to recreate every detail of the reporter's environment when the report identifies the relevant difference.

## Behavior shown

Where it lives: the repro report's output excerpts, errors, logs, screenshots, or other artifacts, read against the issue description and the trigger used in the report.

What good looks like: when the report says the issue reproduced, the evidence demonstrates the same failure or behavior described by the issue. A different or adjacent error is not enough. When the report says it could not reproduce, the evidence shows that the relevant issue trigger was attempted and what actually happened instead.

## Honesty

Where it lives: the repro report's conclusion and any claim about whether the issue was reproduced, compared with the actual steps and evidence in the report.

What good looks like: the conclusion does not claim more than the evidence shows. A different result is not presented as the issue being reproduced, and an honest cannot-reproduce result is stated as such when the evidence supports it.

## Comms

Where it lives: the claim comment and repro report, compared with the repo-facts block's stated bug-report template, contribution policy, and AI-use requirements.

What good looks like: the comments contain any information the repository explicitly requires and do not contradict its contribution rules. If the repo requires AI-use disclosure, the comment includes the required disclosure. Generic claims or boilerplate do not substitute for an issue-specific statement when the repository requires one.
