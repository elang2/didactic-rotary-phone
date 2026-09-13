# Example submissions, for CI

Not submissions. Nothing here is under `submissions/`, and nothing here should be moved
there — everything added to that directory counts as somebody's submission and appears
on the public landing page.

`.github/workflows/repository-tests.yml` copies each file into `submissions/` one at a
time, runs the command `validation/README.md` tells submitters to run, and checks the
result against the schema. One at a time because a submission pull request may contain
exactly one file, so the validator refuses two at once by design.

| File | What it is for |
|---|---|
| `jailbreak-success-rate.yml` | Worked example, byte-identical to the open pull request below. |
| `toxicity-score.yml` | Worked example, byte-identical to the open pull request below. |
| `minimal-metric.yml` | The seven required fields and nothing else — the "no optional keys" path. |

## Why the worked examples are copied here

The two reference submissions are deliberately kept as [open pull
requests](../../../../../pulls?q=is%3Apr+label%3Aexample-submission) rather than merged
files, which is right for readers and useless for CI: no workflow can see them. They are
the drift canary — if they stop validating, `SUBMISSION_FORMAT.md` and
`validation/schemas/v1.json` have diverged — and a canary nothing checks is not a canary.
So a copy lives here.

The cost of a copy is that it can go stale. These were taken from:

| File | Source commit |
|---|---|
| `jailbreak-success-rate.yml` | `e9cd4d8981b040063e459ec2d85ee64cb671e3fb` |
| `toxicity-score.yml` | `6584f71adfe6cb17dcdafffa5184ab81ace55530` |

Re-sync after editing either pull request, and confirm the copy still matches:

```sh
gh api "repos/usnistgov/ai-metrology-submissions/contents/submissions/toxicity-score.yml?ref=<sha>" \
  -H "Accept: application/vnd.github.raw" > validation/tests/examples/toxicity-score.yml
```

If a file here fails, fix the divergence it found. Editing the example until it passes
is how the canary stops being one.
