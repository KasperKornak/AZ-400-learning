# Release recommendations
## Release gates
Release gates give you more control over the start and completion of the deployment pipeline. When the release starts, it checks the state of the gate by calling an API. If the "gate" is open, we can continue. Otherwise, we'll stop the release. By using scripts and APIs, you can create your release gates instead of manual approval. Or at least extending your manual approval.

Other scenarios for automatic approvals are, for example:

- **Incident and issues management**. Ensure the required status for work items, incidents, and issues. For example, ensure that deployment only occurs if no bugs exist.
- **Notify users** such as legal approval departments, auditors, or IT managers about a deployment by integrating with approval collaboration systems such as Microsoft Teams or Slack and waiting for the approval to complete.
- **Quality validation**. Query metrics from tests on the build artifacts such as pass rate or code coverage and only deploy within required thresholds.
- **Security scan on artifacts**. Ensure security scans such as anti-virus checking, code signing, and policy checking for build artifacts have been completed. A gate might start the scan and wait for it to finish or check for completion.
- **User experience relative to baseline**. Using product telemetry, ensure the user experience hasn't regressed from the baseline state. The experience level before the deployment could be considered a baseline.
- **Change management**. Wait for change management procedures in a system such as ServiceNow complete before the deployment occurs.
- **Infrastructure health**. Execute monitoring and validate the infrastructure against compliance rules after deployment or wait for proper resource use and a positive security report.

**A quality gate is located before a stage that is dependent on the outcome of a previous stage.** A quality gate was typically something that a QA department monitored in the past.

## Lab address
https://aka.ms/az-400-control-deployments-using-release-gates