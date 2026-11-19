# Decide Finding Action

An agent skill that turns one finding's disposition into one proposed action: `fix`, `mitigate`, `accept-risk`, `false-positive`, `out-of-scope`, `duplicate`, or `needs-evidence`. It records rationale, owner, timing, approval state, and a canonical fix key, so later steps can batch findings that need the identical change. It also records fix-first decisions for unvalidated upgrades and rule-level tuning proposals from rule samples.

## Use the skill

Use [`SKILL.md`](SKILL.md) in your agent’s skill directory under `decide-finding-action/`. Provide one finding with its disposition from [Validate Scan Finding](https://github.com/rafik-mikhail/validate-scan-finding), or a `fix-first` item from prioritization, and optionally its priority, fix target, owners, and risk-acceptance policy. The skill proposes and records; a human approves. It never dismisses alerts or changes code.

## Use as agent instructions

Copy the [Decide Finding Action instruction gist](https://gist.github.com/rafik-mikhail/3153bca1739d344764ac17b47ad8fb8c) into your agent’s instruction file. The gist contains the same guidance with `applyTo: "**"` front matter for tools that support it. If your agent accepts plain Markdown instructions, copy the body without the front matter.

## Where it fits

This skill is step 5 of a six-step workflow that takes a received scan report to fix pull requests. Steps 1–3 work on the whole report, steps 4–5 work on one finding at a time, and step 6 works on one batch at a time.

| Step | Skill | Works on |
|---|---|---|
| 1 | [Normalize Scan Report](https://github.com/rafik-mikhail/normalize-scan-report) | whole report |
| 2 | [Deduplicate Scan Findings](https://github.com/rafik-mikhail/deduplicate-scan-findings) | whole report |
| 3 | [Prioritize Scan Findings](https://github.com/rafik-mikhail/prioritize-scan-findings) | whole report: sets the order and route; again for confirmed findings |
| 4 | [Validate Scan Finding](https://github.com/rafik-mikhail/validate-scan-finding) | one finding at a time (`validate` route and rule samples) |
| 5 | **Decide Finding Action** | one finding at a time; one rule per rule sample |
| 6 | [Batch Finding Fixes](https://github.com/rafik-mikhail/batch-finding-fixes) | one batch at a time |

Step 3 sends each finding down one route:

| Route | When | Path |
|---|---|---|
| `fix-first` | Dependency or base-image upgrade, patch or minor within the major version, tests in CI | Decide (`not-validated`, `fix`) → Batch. A failing test sends the findings to Validate. |
| `sample-rule` | Many findings from one rule, none `immediate` or `out-of-cycle` | Validate about 5 → Decide. If refuted for one shared reason, propose a rule change and hold the rest until rescan. |
| `validate` | Everything else | Validate → Decide → Batch |
| `defer-track` | SSVC `defer` | No review now; reassess on a KEV listing, a new exploit, an exposure change, or a new fix. |

Findings that share a fix are linked in step 2 only when the fix is known mechanically (dependency, image, IaC module, secret). Shared code causes are grouped in step 5 through the fix key, after validation. [Group Findings by Root Cause](https://github.com/rafik-mikhail/group-findings-by-root-cause) is retired.

Verification by rescan and closing with regression monitoring are not covered yet.

## How to test

Follow the [evaluation guide](TESTING.md).

## Notes

[NOTES.md](NOTES.md) covers gotchas, areas for improvement, and the sources behind the guidance.
