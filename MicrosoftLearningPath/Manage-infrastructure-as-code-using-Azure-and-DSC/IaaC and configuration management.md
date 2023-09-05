# IaaC and configuration management
## Infrastructure as a Code

|Manual deployment	|Infrastructure as code|
|---|---|
|Snowflake servers.|	A consistent server between environments.|
|Deployment steps vary by environment.	|Environments are created or scaled easily.|
|More verification steps and more elaborate manual processes.|	Fully automated creation of environment Updates.|
|Increased documentation to account for differences.|	Transition to immutable infrastructure.|
|Deployment on weekends to allow time to recover from errors.|	Use blue/green deployments.|
|Slower release cadence to minimize pain and long weekends.|	Treat servers as cattle, not pets.|

## Benefits of IaaC

- Promotes auditing by making it easier to trace what was deployed, when, and how. (In other words, it improves traceability.)
- Provides consistent environments from release to release.
- Greater consistency across development, test, and production environments.
- Automates scale-up and scale-out processes.
- Allows configurations to be version-controlled.
- Provides code review and unit-testing capabilities to help manage infrastructure changes.
- Uses immutable service processes, meaning if a change is needed to an environment, a new service is deployed, and the old one was taken down, it isn't updated.
- Allows blue/green deployments. This release methodology minimizes downtime where two identical environments exist—one is live, and the other isn't. Updates are applied to the server that isn't live. When testing is verified and complete, it's swapped with the different live servers. It becomes the new live environment, and the previous live environment is no longer the live.
- Treats infrastructure as a flexible resource that can be provisioned, de-provisioned, and reprovisioned as and when needed.

## Environment configuration
**Configuration management** refers to automated configuration management, typically in version-controlled scripts, for an application and all the environments needed to support it.

|Manual configuration|	Configuration as code|
|---|---|
|Configuration bugs are challenging to identify.|	Bugs are easily reproducible.|
|Error-prone.|	Consistent configuration.|
|More verification steps and more elaborate manual processes.	|Increase deployment cadence to reduce the amount of incremental change.|
|Increased documentation.	|Treat environment and configuration as executable documentation.|
|Deployment on weekends to allow time to recover from errors.|- |	
|Slower release cadence to minimize the requirement for long weekends.|-|	

## Benefits of configuration management

- Bugs are more easily reproduced, audit help, and improve traceability.
- Provides consistency across environments such as dev, test, and release.
- It increased deployment cadence.
- Less documentation is needed and needs to be maintained as all configuration is available in scripts.
- Enables automated scale-up and scale out.
- Allows version-controlled configuration.
- Helps detect and correct configuration drift.
- Provides code-review and unit-testing capabilities to help manage infrastructure changes.
- Treats infrastructure as a flexible resource.
- Promotes automation.

## Declarative vs. imperative configuration

- **Declarative** (functional): The declarative approach states what the final state should be. When run, the script or definition will initialize or configure the machine to have the finished state declared without defining how that final state should be achieved.
- **Imperative** (procedural): In the imperative approach, the script states the how for the final state of the machine by executing the steps to get to the finished state. It defines what the final state needs to be but also includes how to achieve that final state. It also can consist of coding concepts such as for, *if-then, loops, and matrices.

## Idempotent  configuration
**Idempotence** is a mathematical term that can be used in Infrastructure as Code and Configuration as Code. It can apply one or more operations against a resource, resulting in the same outcome.

For example, running a script on a system should have the same outcome despite the number of times you execute the script. It shouldn't error out or do the same actions irrespective of the environment's starting state.

In essence, if you apply a deployment to a set of resources 1,000 times, you should end up with the same result after each application of the script or template.