# Managing Azure Pipeline agents and pools
## Microsoft vs. self-hosted agents

|Microsoft agent| Self-hosted agent|
|---|---|
|Automatic maintanance and upgrades.| You manage maintatnace and updates.|
|Has job time limits.| Doesn't have job time limits.|
|Each time pipeline is run, new VM is spawned and discarded after one use.| You can install agent on Linux, macOS, Windows or Docker containers.|

## Azure DevOps job types

1. **Agent pool jobs**: Most common type of jobs. The jobs run on an agent that is part of an agent pool.
2. **Container jobs**: Similar jobs to Agent Pool Jobs run in a container on an agent part of an agent pool.
3. **Deployment group jobs**: Jobs that run on systems in a deployment group.
4. **Agentless jobs**: Jobs that run directly on the Azure DevOps. They don't require an agent for execution. It's also-often-called Server Jobs.

## Agent pools
Instead of managing each agent individually, you organize agents into agent pools. An agent pool defines the sharing boundary for all agents in that pool.

In Azure Pipelines, pools are scoped to the entire organization so that you can share the agent machines across projects.
If you create an Agent pool for a specific project, only that project can use the pool until you add the project pool into another project.

When creating a build or release pipeline, you can specify which pool it uses, organization, or project scope.
Pools scoped to a project can only use them across build and release pipelines within a project.
To share an agent pool with multiple projects, use an organization scope agent pool and add them in each of those projects, add an existing agent pool, and choose the organization agent pool. 

### Predefined agent pools
Azure Pipelines provides a pre-defined agent pool-named Azure Pipelines with Microsoft-hosted agents.
It will often be an easy way to run jobs without configuring build infrastructure.
The following virtual machine images are provided by default:

- Windows Server 2022 with Visual Studio 2022.
- Windows Server 2019 with Visual Studio 2019.	
- Ubuntu 22.04.		
- Ubuntu 20.04.		
- macOS 13 Ventura.		
- macOS 12 Monterey.	
- macOS 11 Big Sur.	

By default, all contributors in a project are members of the User role on each hosted pool. It allows every contributor to the author and runs build and release pipelines using a Microsoft-hosted pool.

## Communication between agent and Azure Pipelines
**Agent always starts communication**.
**Traffic betwwen agent and Azure Pipelines is encrypted using asymmetric encryption.**

1. User registers an agent with Azure Pipelines by adding it to an agent pool (you must have admin privileges to do that).
2. After registration is complete, agent downloads an OAuth token and uses it to listen to the job queue.
3. Periodically, the agent checks to see if a new job request has been posted in the job queue in Azure Pipelines, if so the agent downloads the job and job-specific OAuth token when available (used to access resources needed to complete the job).
4. Once the job is completed, the agent discards the job-specific OAuth token and checks if there's a new job request using the listener OAuth token.

## Communication with job target servers
To deploy artifacts to the server, you need "line-of-sight" access to them. By default all Microsoft-hosted agent pools have connectivity to Azure websites and servers running in Azure.

In case of on-premise environments, you will need to configure self-hosted agent to deploy.

![Alt text](<img/azure agents connectivity.png>)

## Interactive vs. service
You may run your agent as a service or an interactive process. For example, to run tasks that use Windows authentication to access an external service, you must run the agent using an account with access to that service.
However, if you're running UI tests such as Selenium or Coded UI tests that require a browser, the browser is launched in the context of the agent account.

## Do self-hosted agents have any performance advantages over Microsoft-hosted agents?
In many cases, yes. Specifically:

- If you use a self-hosted agent, you can run incremental builds. For example, you define a CI build pipeline that doesn't clean the repo or do a clean build. Your builds will typically run faster.
    - You don't get these benefits when using a Microsoft-hosted agent. The agent is destroyed after the build or release pipeline is completed.
- A Microsoft-hosted agent can take longer to start your build. While it often takes just a few seconds for your job to be assigned to a Microsoft-hosted agent, it can sometimes take several minutes for an agent to be allocated, depending on the load on our system.

## Can I install multiple self-hosted agents on the same machine?
Yes. This approach can work well for agents who run jobs that don't consume many shared resources. For example, you could try it for agents that run releases that mostly orchestrate deployments and don't do much work on the agent itself.
In other cases, you might find that you don't gain much efficiency by running multiple agents on the same machine.

## Security of agent pools
In Azure Pipelines, roles are defined on each agent pool.

|Role on an organization agent pool|Purpose|
|---|---|
|Reader| View the organization's agent pool and agents.|
|Service Account|Use the organization agent pool to create a project agent pool in a project.|
|Administrator| Register or unregister agents from the organization's agent pool. They can also refer to the organization agent pool when creating a project agent pool in a project. Finally, they can also manage membership for all roles of the organization agent pool.|

|Role on an project agent pool|Purpose|
|---|---|
|Reader| View the project agent pool and agents.|
|Service Account|Use the project agent pool when authoring build or release pipelines.|
|Administrator| Manage membership for all roles of the project agent pool. The user that created the pool is automatically added to the Administrator role for that pool.|

## Lab address
https://aka.ms/az-400-configure-agent-pools-and-understand-pipeline-styles