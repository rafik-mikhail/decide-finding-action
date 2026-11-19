# Notes: Decide finding action

## Gotchas

- **The fix key is the batching contract.** Step 7 batches by identical keys. Inconsistent wording ("upgrade lodash" vs "bump lodash to 4.17.21") splits batches. Use the key forms exactly.
- **Accept, mitigate, transfer, avoid.** NIST SP 800-40r4 lists four risk responses. This skill folds avoid (remove the feature) into `mitigate` or `fix`, and transfer (a vendor or SaaS takes over patching) into `accept-risk` with the reason. Add them as separate actions if your audit requires it.
- **Mitigations decay.** SSVC notes a firewall or IDS rule is valid only until someone changes it. Every `mitigate` needs a review date.
- **Dismissal reasons affect future scans.** GitHub says the chosen dismissal reason can affect whether a query keeps running in future analyses, and a dismissed alert stays dismissed on all branches. That is why a human applies it.
- **Remediation is not a final state.** Rollbacks and redeployments can reintroduce a fixed vulnerability; reopen triggers cover the decision side, and the later monitoring step covers the code side.

## Areas for improvement

- Add the organization's risk-acceptance template and approver matrix as inputs.
- Add an exporter that turns the ledger into VEX statements or tracker tickets.
- Add separate `avoid` and `transfer` actions if needed for audit.

## Sources

- [NIST SP 800-40 Rev. 4](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-40r4.pdf): accept, mitigate, transfer, avoid; maintenance groups.
- [CERT/CC SSVC Deployer](https://certcc.github.io/SSVC/howto/deployer_tree/): mitigation versus remediation.
- [DefectDojo: Risk acceptances](https://docs.defectdojo.com/triage_findings/findings_workflows/pro__risk_acceptance/).
- [GitHub REST API: update a code scanning alert](https://docs.github.com/en/enterprise-cloud@latest/rest/code-scanning/code-scanning): dismissal reasons.
- [GitHub: Resolving code scanning alerts](https://docs.github.com/en/code-security/how-tos/manage-security-alerts/manage-code-scanning-alerts/resolve-alerts): dismissal effects.
- [CISA: Minimum requirements for VEX](https://www.cisa.gov/sites/default/files/2023-04/minimum-requirements-for-vex-508c.pdf): VEX statuses.
- [Renovate: vulnerabilityAlerts](https://docs.renovatebot.com/configuration-options/#vulnerabilityalerts): lowest fixing version as the default strategy.
