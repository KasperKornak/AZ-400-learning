# Managing Git branches and workflows
## Trunk-based development
The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the main branch.

## Forking workflow
Instead of using a single server-side repository to act as the "central" codebase, it gives every developer a server-side repository.
It means that each contributor has two Git repositories:

- A private local one.
- A public server-side one.

## Commands used to set up Azure Repos
Configuring organisation and projcet name:

```sh
az devops configure --defaults organization=https://dev.azure.com/organization project="project name"
```

Creating a PR:

```sh
az repos pr create --title "Review Feature-1 before merging to main" --work-items 38 39 `
--description "#Merge feature-1 to main" `
--source-branch feature/myFeature-1 --target-branch main `
--repository myWebApp --open
```

