# Work with Azure Repos and GitHub
## Migrating from TFVC to Git

1. Create an empty Git repository.
2. Get-latest from TFS.
3. Copy/reorganize the code into the empty Git repository.
4. Commit and push.

It is also possible to import a TFVC data into Azure Repos using *Import* in Azure DevOps.

### Migrating multiple branches to Git from TFVC
**GIT-TFS** is an open-source project used to synchronize Git and TFVC repositories.

1. Run `choco install gittfs`.
2. Add GIT-TFS folder to path.
3. Clone the repository: `git tfs clone http://tfs:8080/tfs/DefaultCollection $/some_project`.
4. Enter the repository.