# Continuous Integration with GitHub Actions
## Basic building blocks of workflows

```yaml
name: dotnet Build

on: [push]

jobs:
    build:
        runs-on: ubuntu-latest
        strategy:
            matrix:
                node-version: [10.x]
        steps:

        - uses: actions/checkout@main
        - uses: actions/setup-dotnet@v1
            with:
                dotnet-version: '3.1.x'

        - run: dotnet build awesomeproject
```

Legend:

- **On**: Specifies what will occur when code is pushed.
- **Jobs**: There's a single job called build.
- **Strategy**: It's being used to specify the Node.js version.
- **Steps**: Are doing a checkout of the code and setting up dotnet.
- **Run**: Is building the code.

## Environment variables
GitHub provides a series of built-in environment variables. It all has a `GITHUB_` prefix.

- **GITHUB_WORKFLOW** is the name of the workflow.
- **GITHUB_ACTION** is the unique identifier for the action.
- **GITHUB_REPOSITORY** is the name of the repository (but also includes the name of the owner in owner/repo format).

Variables are set in the YAML workflow files. They're passed to the actions that are in the step.

## Sharing artifacts between jobs
The most common ways to do it are by using the `upload-artifact` and `download-artifact` actions.

### `upload-artifact`
This action can upload one or more files from your workflow to be shared between jobs.

```yaml
- uses: actions/upload-artifact
  with:
    name: harness-build-log
    path: bin/output/logs/harness.log
```

### `download-artifact`
There's a corresponding action for downloading (or retrieving) artifacts.
```yaml 
- uses: actions/download-artifact
  with:
    name: harness-build-log
```

### Artifact retention
A default retention period can be set for the repository, organization, or enterprise.
You can set a custom retention period when uploading, but it can't exceed the defaults for the repository, organization, or enterprise.

```yaml
- uses: actions/upload-artifact
  with:
    name: harness-build-log
    path: bin/output/logs/harness.log
    retention-days: 12
```

## Workflow badges
Badges can be used to show the status of a workflow within a repository.
They show if a workflow is currently passing or failing. While they can appear in several locations, they typically get added to the README.md file for the repository.

Badges are added by using URLs. The URLs are formed as follows:

`https://github.com/<OWNER>/<REPOSITORY>/actions/workflows/<WORKFLOW_FILE>/badge.svg`

## Best practices for creating actions

- Create chainable actions. Don't create large monolithic actions. Instead, create smaller functional actions that can be chained together.
- Version your actions like other code. Others might take dependencies on various versions of your actions. Allow them to specify versions.
- Provide the latest label. If others are happy to use the latest version of your action, make sure you provide the latest label that they can specify to get it.
- Add appropriate documentation. As with other codes, documentation helps others use your actions and can help avoid surprises about how they function.
- Add details `action.yml` metadata. At the root of your action, you'll have an `action.yml` file. Ensure it has been populated with author, icon, expected inputs, and outputs.
- Consider contributing to the marketplace. It's easier for us to work with actions when we all contribute to the marketplace. Help to avoid people needing to relearn the same issues endlessly.

## Marking releases with Git tags
Releases are software iterations that can be packed for release.
In Git, releases are based on Git tags. These tags mark a point in the history of the repository. Tags are commonly assigned as releases are created.

## Encrypted secrets
Secrets are similar to environment variables but encrypted. They can be created at two levels:

- Repository.
- Organization.

If secrets are created at the organization level, access policies can limit the repositories that can use them.
You can create secrets in **Settings** -> **Secrets** -> **New Secret**.

### Using secrets in workflow
Secrets aren't passed automatically to the runners when workflows are executed.
Instead, when you include an action that requires access to a secret, you use the secrets context to provide it.

```yaml
steps:

  - name: Test Database Connectivity
    with:
      db_username: ${{ secrets.DBUserName }}
      db_password: ${{ secrets.DBPassword }}
```

Workflows can use up to 100 secrets. Their size is limited to 64kB of data.