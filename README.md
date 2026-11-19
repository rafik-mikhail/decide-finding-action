# Decide Finding Action

An agent skill that turns one finding's disposition into one proposed action: `fix`, `mitigate`, `accept-risk`, `false-positive`, `out-of-scope`, `duplicate`, or `needs-evidence`. It records rationale, owner, timing, approval state, and a canonical fix key, so later steps can batch findings that need the identical change.

## Use the skill

Use [`SKILL.md`](SKILL.md) in your agent’s skill directory under `decide-finding-action/`. Provide one finding with its disposition from [Validate Scan Finding](https://github.com/rafik-mikhail/validate-scan-finding), and optionally its priority, group, owners, and risk-acceptance policy. The skill proposes and records; a human approves. It never dismisses alerts or changes code.

## Use as agent instructions

Copy the [Decide Finding Action instruction gist](https://gist.github.com/rafik-mikhail/3153bca1739d344764ac17b47ad8fb8c) into your agent’s instruction file. The gist contains the same guidance with `applyTo: "**"` front matter for tools that support it. If your agent accepts plain Markdown instructions, copy the body without the front matter.

## Where it fits

This skill is step 6 of a seven-step workflow that takes a received scan report to fix pull requests. Steps 1–4 work on the whole report, steps 5–6 work on one finding at a time, and step 7 works on one batch at a time.

| Step | Skill | Works on |
|---|---|---|
| 1 | [Normalize Scan Report](https://github.com/rafik-mikhail/normalize-scan-report) | whole report |
| 2 | [Deduplicate Scan Findings](https://github.com/rafik-mikhail/deduplicate-scan-findings) | whole report |
| 3 | [Group Findings by Root Cause](https://github.com/rafik-mikhail/group-findings-by-root-cause) | whole report |
| 4 | [Prioritize Scan Findings](https://github.com/rafik-mikhail/prioritize-scan-findings) | whole report, then again for confirmed findings |
| 5 | [Validate Scan Finding](https://github.com/rafik-mikhail/validate-scan-finding) | one finding at a time |
| 6 | **Decide Finding Action** | one finding at a time |
| 7 | [Batch Finding Fixes](https://github.com/rafik-mikhail/batch-finding-fixes) | one batch at a time |

Verification by rescan and closing with regression monitoring are not covered yet.

## How to test

Follow the [evaluation guide](TESTING.md).

## Notes

[NOTES.md](NOTES.md) covers gotchas, areas for improvement, and the sources behind the guidance.
