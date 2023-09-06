# Security monitoring and governance
## Pipeline security
- **Authentication and authorization**: Use multifactor authentication (MFA), even across internal domains, and just-in-time administration tools such as Azure PowerShell Just Enough Administration (JEA), to protect against privilege escalations.  
- **The CI/CD Release Pipeline**: If the release pipeline and cadence are damaged, use this pipeline to rebuild infrastructure. Manage Infrastructure as Code (IaC) with Azure Resource Manager or use the Azure platform as a service (PaaS) or a similar service. Your pipeline will automatically create new instances and then destroy them. It limits the places where attackers can hide malicious code inside your infrastructure. Azure DevOps will encrypt the secrets in your pipeline. As a best practice, rotate the passwords just as you would with other credentials.
- **Permissions management**: You can manage permissions to secure the pipeline with role-based access control (RBAC), just as you would for your source code. It keeps you in control of editing the build and releases definitions that you use for production.
- **Dynamic scanning**: It's the process of testing the running application with known attack patterns. You could implement penetration testing as part of your release.
- **Production monitoring**: It's a critical DevOps practice. The specialized services for detecting anomalies related to intrusion are known as Security Information and Event Management. 

## Microsoft Defender for Cloud
Microsoft Defender for Cloud is a monitoring service that provides threat protection across all your services both in Azure and on-premises. Microsoft Defender can:

- Provide security recommendations based on your configurations, resources, and networks.
- Monitor security settings across on-premises and cloud workloads and automatically apply required security to new services as they come online.
- Continuously monitor all your services and do automatic security assessments to identify potential vulnerabilities before they can be exploited.
- Use Azure Machine Learning to detect and block malicious software from being installed on your virtual machines (VMs) and services. You can also define a list of allowed applications to ensure that only the validated apps can execute.
- Analyze and identify potential inbound attacks and help investigate threats and any post-breach activity that might have occurred.
- Provide just-in-time (JIT) access control for ports by reducing your attack surface by ensuring the network only allows traffic that you require.

## Azure Policy
Azure Policy is an Azure service that you can create, assign, and manage policies.
Policies enforce different rules and effects over your Azure resources, ensuring that your resources stay compliant with your standards and SLAs.

Azure Policy uses policies and initiatives to provide policy enforcement capabilities.
Azure Policy evaluates your resources by scanning for resources that don't follow the policies you create.

### CI/CD pipeline integration
The Check Gate task is an example of an Azure policy that you can integrate with your DevOps CI/CD pipeline.

Using Azure policies, Check gate provides security and compliance assessment on the resources with an Azure resource group or subscription that you can specify.

Check gate is available as a Release pipeline deployment task.

### Policies
A **policy** definition specifies the resources to be evaluated and the actions to take on them. For example, you could prevent VMs from deploying if exposed to a public IP address. You could also contain a specific hard disk for deploying VMs to control costs. Policies are defined in the JavaScript Object Notation (JSON) format.

```JSON
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

The following list is example policy definitions:

- **Allowed Storage Account SKUs (Deny)**: Determines if a storage account being deployed is within a set of SKU sizes. Its effect is to deny all storage accounts that don't adhere to the set of defined SKU sizes.
- **Allowed Resource Type (Deny)**: Defines the resource types that you can deploy. Its effect is to deny all resources that aren't part of this defined list.
- **Allowed Locations (Deny)**: Restricts the available locations for new resources. Its effect is used to enforce your geo-compliance requirements.
- **Allowed Virtual Machine SKUs (Deny)**: Specify a set of virtual machine SKUs you can deploy.
- **Add a tag to resources (Modify)**: Applies a required tag and its default value if the deploy request does not specify it.
- **Not allowed resource types (Deny)**: Prevents a list of resource types from being deployed.

### Policy assignment
Policy definitions, whether custom or built-in, need to be assigned.
A policy assignment is a policy definition that has been assigned to a specific scope. Scopes can range from a management group to a resource group.

Child resources will inherit any policy assignments applied to their parents.

### Remediation
Resources found not to follow a deployIfNotExists or modify policy condition can be put into a compliant state through Remediation.

Remediation instructs Azure Policy to run the deployIfNotExists effect or the tag operations of the policy on existing resources.

### Initiatives
Initiatives work alongside policies in Azure Policy. An initiative definition is a set of policy definitions to help track your compliance state for meeting large-scale compliance goals.
Even if you have a single policy, we recommend using initiatives if you anticipate increasing your number of policies over time.
Applying an initiative definition to a specific scope is called an initiative assignment.

**Initiative definitions** simplify the process of managing and assigning policy definitions by grouping sets of policies into a single item.

For example, you can create an initiative named Enable Monitoring in Azure Security Center to monitor security recommendations from Azure Security Center.

Under this example initiative, you would have the following policy definitions:

- **Monitor unencrypted SQL Database in Security Center**. This policy definition monitors unencrypted SQL databases and servers.
- **Monitor OS vulnerabilities in Security Center**. This policy definition monitors servers that don't satisfy a specified OS baseline configuration.
- **Monitor missing Endpoint Protection in Security Center**. This policy definition monitors servers without an endpoint protection agent installed.

### Resource locks
Locks help you prevent accidental deletion or modification of your Azure resources. You can manage locks from within the Azure portal.

In the Azure portal, locks are called `Delete` and `Read-only`, respectively.

You might need to lock a subscription, resource group, or resource to prevent users from accidentally deleting or modifying critical resources.

You can set a lock level to `CanNotDelete` or `ReadOnly`:

- `CanNotDelete` means that authorized users can read and modify a resource, but they can't delete it.
- `ReadOnly` means that authorized users can read a resource, but they can't modify or delete it.

## Azure Blueprints
**Azure Blueprints** enables cloud architects to define a repeatable set of Azure resources that implement and adhere to an organization's standards, patterns, and requirements.

Azure Blueprints helps development teams build and deploy new environments rapidly with a set of built-in components that speed up development and delivery.

Azure Blueprints provides a declarative way to orchestrate deployment for various resource templates and artifacts, including:

- Role assignments.
- Policy assignments.
- Azure Resource Manager templates.
- Resource groups.

To implement Azure Blueprints, complete the following high-level steps:

1. Create a blueprint.
2. Assign the blueprint.
3. Track the blueprint assignments.

With Azure Blueprints, the relationship between the blueprint definition (what should be deployed) and the blueprint assignment (what is deployed) is preserved.