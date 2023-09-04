# Managing and modularizing tasks and templates
## Task groups
A task group allows you to encapsulate a sequence of tasks, already defined in a build or a release pipeline, into a single reusable task that can be added to a build or release pipeline, just like any other task.

Task groups are a way to standardize and centrally manage deployment steps for all your applications.

## Variables in release pipelines
### Predefined variables
When running your release pipeline, you always need variables that come from the agent or context of the release pipeline.
For example, the agent directory where the sources are downloaded, the build number or build ID, the agent's name, or any other information.
This information is accessible in predefined variables that you can use in your tasks.

### Release pipeline variables
Choose a release pipeline variable when you need to use the same value across all the stages and tasks in the release pipeline, and you want to change the value in a single place.

### Stage variables
Share values across all the tasks within one specific stage by using stage variables.
Use a stage-level variable for values that vary from stage to stage (and are the same for all the tasks in a stage).

### Normal and secret variables
Because the pipeline tasks are executed on an agent, variable values are passed to the various tasks using environment variables.
The task knows how to read it. You should be aware that a variable contains clear text and can be exposed to the target system.
When you use the variable in the log output, you can also see the variable's value.
When the pipeline has finished, the values will be cleared.
You can mark a variable in the release pipeline as secret. This way, the secret is hidden from the log output. It's beneficial when writing a password or other sensitive information.

## Variable groups 
A variable group stores values that you want to make available across multiple builds and release pipelines.

Examples:

- Store the username and password for a shared server.
- Store a share connection string.
- Store the geolocation of an application.
- Store all settings for a specific application.

