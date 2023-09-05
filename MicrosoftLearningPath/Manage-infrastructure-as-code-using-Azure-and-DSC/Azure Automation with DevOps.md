# Azure Automation with DevOps
## Automation Accounts
**Automation accounts** are like Azure Storage accounts in that they serve as a container to store automation artifacts. These artifacts could be a container for all your runbooks, runbook executions (jobs), and the assets on which your runbooks depend. An Automation account gives you access to managing all Azure resources via an API. To safeguard it, the Automation account creation requires subscription-owner access.

## Runbooks
**Runbooks** serve as repositories for your custom scripts and workflows.
They also typically reference Automation shared resources such as credentials, variables, connections, and certificates.
Runbooks can also contain other runbooks, allowing you to build more complex workflows.
You can invoke and run runbooks either on-demand or according to a schedule by using Automation Schedule assets.

### Types of runbooks

- **Graphical runbooks**: Graphical runbooks and Graphical PowerShell Workflow runbooks are created and edited with the graphic editor in the Azure portal.
- **PowerShell runbooks**:PowerShell runbooks are based on Windows PowerShell. You edit the runbook code directly, using the text editor in the Azure portal.
- **PowerShell Workflow runbooks**: PowerShell Workflow runbooks are text runbooks based on Windows PowerShell Workflow. PowerShell Workflow runbooks use parallel processing to allow for simultaneous completion of multiple tasks. Workflow runbooks take longer to start than PowerShell runbooks because they must be compiled before running.
- **Python runbooks**: Python runbooks compile under Python 2. You can directly edit the code of the runbook using the text editor in the Azure portal, or you can use any offline text editor and import the runbook into Azure Automation. You can also use Python libraries. However, only Python 2 is supported at this time. To use third-party libraries, you must first import the package into the Automation Account.

### Runbook gallery

1. Open your Automation account, and then select Process Automation > Runbooks.
2. In the runbooks pane, select Browse gallery.
3. From the runbook gallery, locate the runbook item you want, select it, and select Import.

### Webhooks
You can automate starting a runbook either by scheduling it or by using a webhook.
A webhook allows you to start a particular runbook in Azure Automation through a single HTTPS request.

It allows external services such as Azure DevOps, GitHub, or custom applications to start runbooks without implementing more complex solutions using the Azure Automation API.

1. In the Azure portal, open the runbook that you want to create the webhook.
2.  In the runbook pane, under Resources, select **Webhooks**, and then choose **+ Add webhook**.
3. Select **Create new webhook**.
4. In the **Create new webhook** dialog, there are several values you need to configure. After you configure them, select Create:

    - **Name**. Specify any name you want for a webhook because the name isn't exposed to the client. It's only used for you to identify the runbook in Azure Automation.
    - **Enabled**. A webhook is enabled by default when it's created. If you set it to Disabled, then no client can use it.
    - **Expires**. Each webhook has an expiration date, at which time it can no longer be used. You can continue to modify the date after creating the webhook providing the webhook isn't expired.
    - **URL**. The webhook URL is the unique address that a client calls with an HTTP POST to start the runbook linked to the webhook. It's automatically generated when you create the webhook, and you can't specify a custom URL. The URL contains a security token that allows the runbook to be invoked by a third-party system with no further authentication. For this reason, treat it like a password. You can only view the URL in the Azure portal for security reasons when the webhook is created. Make a note of the URL in a secure location for future use.
5. Select the **Parameters run settings (Default: Azure)** option. This option has the following characteristics, which allows you to complete the following actions:

    - If the runbook has mandatory parameters, you'll need to provide these required parameters during creation. You aren't able to create the webhook unless values are provided.
    - If there are no mandatory parameters in the runbook, there's no configuration required here.
    - The webhook must include values for any mandatory parameters of the runbook and include values for optional parameters.
    - When a client starts a runbook using a webhook, it can't override the parameter values defined.
    - To receive data from the client, the runbook can accept a single parameter called `$WebhookData` of type `[object]` that contains data that the client includes in the POST request.
    - There's no required webhook configuration to support the `$WebhookData` parameter.

## Automation and shared resources
Azure Automation contains shared resources that are globally available to be associated with or used in a runbook. There are currently eight shared resources categories:

- **Schedules**: It allows you to define a one-off or recurring schedule.
- **Modules**: Contains Azure PowerShell modules.
- **Modules gallery**: It allows you to identify and import PowerShell modules into your Azure automation account.
- **Python packages**: Allows you to import a Python package by uploading: .whl or tar.gz packages.
- **Credentials**: It allows you to create username and password credentials.
- **Connections**: It allows you to specify Azure, Azure classic certificate, or Azure Service principal connections.
- **Certificates**: It allows you to upload certificates in .cer or pfx format.
- **Variables**: It allows you to define encrypted or unencrypted variables of types—for example, String, Boolean, DateTime, Integer, or no specific type.

## PowerShell Workflows
PowerShell Workflow lets IT pros and developers apply the benefits of Windows Workflow Foundation with the automation capabilities and ease of using Windows PowerShell.

### Activities
An **activity** is a specific task that you want a workflow to do. Just as a script is composed of one or more commands, a workflow is composed of activities carried out in sequence.

You can also use a script as a single command in another script and use a workflow as an activity within another workflow.

### Workflow characteristics

- Be long-running.
- Be repeated over and over.
- Run tasks in parallel.
- Be interrupted—can be stopped and restarted, suspended, and resumed.
- Continue after an unexpected interruption, such as a network outage or computer/server restart.

### Checkpoint and parallel processing in Workflows
### Checkpoint
A checkpoint is a snapshot of the current state of the workflow.

Checkpoints include the current value for variables and any output generated up to that point. (For more information on what a checkpoint is, read the checkpoint webpage.)

If a workflow ends in an error or is suspended, the next time it runs, it will start from its last checkpoint instead of at the beginning of the workflow.

```PowerShell
<Activity1>
        Checkpoint-Workflow
            <Activity2>
                <Exception>
            <Activity3>
```

### Parallel processing 
A script block has multiple commands that run concurrently (or in parallel) instead of sequentially, as for a typical script.

It's referred to as parallel processing. (More information about parallel processing is available on the Parallel processing webpage.)

In the following example, two `vm0` and `vm1` VMs will be started concurrently, and vm2 will only start after vm0 and `vm1` have started.

```PowerShell
Parallel
    {
        Start-AzureRmVM -Name $vm0 -ResourceGroupName $rg 
        Start-AzureRmVM -Name $vm1 -ResourceGroupName $rg
    }

    Start-AzureRmVM -Name $vm2 -ResourceGroupName $rg
```

Another parallel processing example would be the following constructs that introduce some extra options:

`ForEach -Parallel`. You can use the `ForEach -Parallel` construct to concurrently process commands for each item in a collection. The items in the collection are processed in parallel while the commands in the script block run sequentially.

In the following example, `Activity1` starts at the same time for all items in the collection.

For each item, `Activity2` starts after `Activity1` completes. Activity3 starts only after both `Activity1` and `Activity2` have been completed for all items.

`ThrottleLimit` - We use the `ThrottleLimit` parameter to limit parallelism. Too high of a `ThrottleLimit` can cause problems. The ideal value for the `ThrottleLimit` parameter depends on several environmental factors. Try starting with a low `ThrottleLimit` value, and then increase the value until you find one that works for your specific circumstances:

```PowerShell
ForEach -Parallel -ThrottleLimit 10 ($<item> in $<collection>)
{
    <Activity1>
    <Activity2>
}
<Activity3>
```