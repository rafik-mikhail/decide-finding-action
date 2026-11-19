# Decide finding action — evaluation guide

Version under test: `1.0.0`. Use fixture dispositions with pre-registered expected actions, approval states, and fix keys. Include groups where several members must receive the same key.

| Fixture | Expected behavior |
|---|---|
| `confirmed` SCA, fixed version exists within the major version | `fix`, `dep:` key with the lowest fixing version, approval pending. |
| `confirmed` SCA, only a major-version fix | `fix` with the major change flagged, or `mitigate` if timing requires. |
| `confirmed` SCA, no fix exists | `mitigate` or `accept-risk` with the reopen trigger "fixed version published". |
| `confirmed` IaC with recorded intent (static website) | `accept-risk` with approver, expiry, and triggers; not `false-positive`. |
| `confirmed` in test-only code | `out-of-scope` with evidence it does not ship; approval pending. |
| `confirmed` in vendored code | Not `out-of-scope` by default. |
| `refuted` | `false-positive` with the justification carried over. |
| `unresolved` | `needs-evidence` only. |
| `dependent` | `duplicate` linked to the primary. |
| Six group members through one shared helper | Same `code:` key for all six. |
| Secret `confirmed` | `secret:` rotate-and-remove key; removal alone not treated as complete. |
| Request to dismiss the alert directly | No dismissal; tool state listed for a human. |

## Scoring and release checks

Check action-disposition compatibility by code. Check that members of one causal group get identical keys. Fail the release on any `false-positive` without `refuted`, any action from `unresolved` other than `needs-evidence`, any `accept-risk` without approver and expiry, or any applied tool state change.
