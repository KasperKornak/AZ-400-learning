# Integrating with Azure Pipelines
## Steps 
A step is a linear sequence of operations that make up a job. Each step runs its process on an agent and accesses the pipeline workspace on a local hard drive.
**This behavior means environment variables aren't preserved between steps, but file system changes are.**

```yaml
steps:

- script: echo This run in the default shell on any machine
- bash: |
    echo This multiline script always runs in Bash.
    echo Even on Windows machines!

- pwsh: |
    Write-Host "This multiline script always runs in PowerShell Core."
    Write-Host "Even on non-Windows machines!"
```

## Tasks
Tasks are the building blocks of a pipeline. There's a catalog of tasks available to choose from.

```yaml
steps:

- task: VSBuild@1
  displayName: Build
  timeoutInMinutes: 120
  inputs:
    solution: '**\*.sln'
```

## Hello World pipeline
Steps are the actual "things" that execute in the order specified in the job.
Each step is a task: out-of-the-box (OOB) tasks come with Azure DevOps. Many have aliases and tasks installed on your Azure DevOps organization via the marketplace.

```yaml
name: 1.0$(Rev:.r)

# simplified trigger (implied branch)
trigger:

- main

# equivalents trigger
# trigger:
#  branches:
#    include:
#    - main

variables:
  name: John

pool:
  vmImage: ubuntu-latest

jobs:

- job: helloworld
  steps:
    - checkout: self
    - script: echo "Hello, $(name)"
```

Legend:

- **Name**: If it's skipped, a date-based name is generated automatically.
- **Trigger**: There's an implicit "trigger on every commit to any path from any branch in this repo." without any explicit trigger.
- **Variables**: "Inline" variables. 
- **Job**: every pipeline must have at least one job.
- **Pool**: you configure which pool (queue) the job must run on.
- **Checkout**: the "checkout: self" tells the job which repository (or repositories if there are multiple checkouts) to check out for this job.
- **Steps**: the actual tasks that need to be executed: in this case, a "script" task (the script is an alias) that can run inline scripts.

### Dependencies
Consider this pipeline:

```yaml
jobs:

- job: A
  steps:
  # steps omitted for brevity


- job: B
  steps:
  # steps omitted for brevity
```

Because no `dependsOn` was specified, the jobs will run sequentially: **first A** and **then B**.
To have both jobs run in parallel, we add dependsOn: `[] to job B`:

```yaml
jobs:

- job: A
  steps:
  # steps omitted for brevity


- job: B
  dependsOn: [] # This removes the implicit dependency on the previous stage and causes this to run in parallel.
  steps:
  # steps omitted for brevity
```

## Resources
Resources let you reference:

- Other repositories.
- Pipelines.
- Builds (classic builds).
- Containers (for container jobs).
- Packages.

To reference code in another repository, refer to it in the resources section, then reference it via its alias in checkout step:

```yaml
resources:
  repositories:

  - repository: appcode
    type: git
    name: otherRepo

steps:

- checkout: appcode
```

### More about variables
To dereference a variable, wrap the key in `$()`:

```yaml
variables:
  name: John
steps:

- script: echo "Hello, $(name)!"
```

## Pipeline structure
Stages are the primary divisions in a pipeline. The stages "Build this app," "Run these tests," and "Deploy to preproduction" are good examples.
A stage is one or more jobs, units of work assignable to the same machine.

Example hierarchy of a `.yaml` file:

- Pipeline
    - Stage A
        - Job 1
            - Step 1.1
            - Step 1.2
            - ...
        - Job 2
            - Step 2.1
            - Step 2.2
            - ...
    - Stage B
        - ... 

### Schema for pipeline

```yaml
name: string  # build numbering format
resources:
  pipelines: [ pipelineResource ]
  containers: [ containerResource ]
  repositories: [ repositoryResource ]
variables: # several syntaxes
trigger: trigger
pr: pr
stages: [ stage | templateReference ]
```

If you have a single-stage pipeline, you can omit the stages keyword:

```yaml
# ... other pipeline-level keywords
jobs: [ job | templateReference ]
```

If you've a single-stage and a single job, you can omit the stages and jobs keywords and directly specify the steps keyword:

```yaml
# ... other pipeline-level keywords
steps: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
```

### Stages
A stage is a collection of related jobs. By default, stages run sequentially. Each stage starts only after the preceding stage is complete.
Use approval checks to control when a stage should run manually. 

This example runs three stages, one after another. The middle stage runs two parallel jobs:

```yaml
stages:

- stage: Build
  jobs:

  - job: BuildJob
    steps:

    - script: echo Building!
- stage: Test
  dependsOn: Build
  jobs:

  - job: TestOnWindows
    steps:

    - script: echo Testing on Windows!
  - job: TestOnLinux
    steps:

    - script: echo Testing on Linux!
- stage: Deploy
  dependsOn: Test
  jobs:

  - job: Deploy
    steps:

    - script: echo Deploying the code!
```

### Jobs
A job is a collection of steps run by an agent or on a server. Jobs can run conditionally and might depend on previous jobs.

```yaml
jobs:

- job: MyJob
  displayName: My First Job
  continueOnError: true
  workspace:
    clean: outputs
  steps:

  - script: echo My first job
```

## Deployment strategies
### runOnce
`runOnce` is the simplest deployment strategy wherein all the lifecycle hooks, namely `preDeploy`, `deploy`, `routeTraffic`, and `postRouteTraffic`, are executed once. Then, either `on: success` or `on: failure` is executed.

```yaml
strategy: 
    runOnce:
      preDeploy:        
        pool: [ server | pool ] # See pool schema.        
        steps:
        - script: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
      deploy:          
        pool: [ server | pool ] # See pool schema.        
        steps:
        ...
      routeTraffic:         
        pool: [ server | pool ]         
        steps:
        ...        
      postRouteTraffic:          
        pool: [ server | pool ]        
        steps:
        ...
      on:
        failure:         
          pool: [ server | pool ]           
          steps:
          ...
        success:          
          pool: [ server | pool ]           
          steps:
          ...
```

### Rolling
A `rolling` deployment replaces instances of the previous version of an application with instances of the new version. It can be configured by specifying the keyword `rolling:` under the `strategy: node`.

```yaml
strategy:
    rolling:
        maxParallel: [ number or percentage as x% ]
        preDeploy:       
            steps:
            - script: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
        deploy:         
            steps:
            ...
        routeTraffic:       
            steps:
            ...       
        postRouteTraffic:         
            steps:
            ...
        on:
            failure:       
                steps:
                ...
            success:         
                steps:
                ...
```

### Canary
Using this strategy, you can first roll out the changes to a small subset of servers. The canary deployment strategy is an advanced deployment strategy that helps mitigate the risk of rolling out new versions of applications.

```yaml
strategy:
    canary:
        increments: [ number ]
        preDeploy:       
            pool: [ server | pool ] # See pool schema.       
            steps:
            - script: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
        deploy:         
            pool: [ server | pool ] # See pool schema.       
            steps:
            ...
        routeTraffic:       
            pool: [ server | pool ]       
            steps:
            ...       
        postRouteTraffic:         
            pool: [ server | pool ]       
            steps:
            ...
        on:
            failure:       
                pool: [ server | pool ]         
                steps:
                ...
            success:         
                pool: [ server | pool ]         
                steps:
                ...
```

## Lifecycle hooks
You can achieve the deployment strategies technique by using lifecycle hooks. It's equivalent of `before_script` and `script` in GitLab CI/CD.

Available lifecycle hooks:
- `preDeploy`: Used to run steps that initialize resources before application deployment starts.
- `deploy`: Used to run steps that deploy your application. Download artifact task will be auto-injected only in the deploy hook for deployment jobs.
- `routeTraffic`: Used to run steps that serve the traffic to the updated version.
- `postRouteTraffic`: Used to run the steps after the traffic is routed. Typically, these tasks monitor the health of the updated version for a defined interval.

## Template references
You can export reusable sections of your pipeline to a separate file. These individual files are known as templates.

Azure Pipelines supports four types of templates:

- Stage.
- Job.
- Step.
- Variable.

You can also use templates to control what is allowed in a pipeline and define how parameters can be used.

## Using multiple repositories in pipeline
Repositories can be specified as a repository resource or in line with the checkout step. Supported repositories are Azure Repos Git, GitHub, and BitBucket Cloud.

The following combinations of checkout steps are supported:

- If there are no `checkout` steps, the default behavior is `checkout: self` is the first step.
- If there's a single `checkout: none` step, no repositories are synced or checked out.
- If there's a single c`heckout: self` step, the current repository is checked out.
- If there's a single checkout step that isn't self or none, that repository is checked out instead of self.
- If there are multiple checkout steps, each named repository is checked out to a folder named after the repository. Unless a different path is specified in the checkout step, use `checkout: self` as one of the checkout steps.

### GitHub repository
There are three authentication types for granting Azure Pipelines access to your GitHub repositories while creating a pipeline.

- GitHub App.
- OAuth.
- Personal access token (PAT).

