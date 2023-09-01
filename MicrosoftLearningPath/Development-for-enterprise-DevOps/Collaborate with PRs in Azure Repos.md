# Collaborate with PRs in Azure Repos
Once a pull request is sent, interested parties can review the set of changes, discuss potential modifications, and even push follow-up commits if necessary.

Once you're done fixing a bug or new feature in a branch, create a new pull request.
Add the team members to the pull request so they can review and vote on your changes.

- Get your code reviewed.
- Protect branches with policies.

## Setting PR collaboration in Azure Repos

1. From Azure DevOps, enter your repository, select the main branch and hit *Branch policies*.
2. In the *policies* view, enter your desired branch policies, like number of Approvers.
3. You may want to use the *review policy* with the comment-resolution policy. It allows you to enforce that the code review comments are resolved before the changes are accepted. The requester can take the feedback from the comment and create a new work item and resolve the changes.
4. Select *Check for linked items*, because without it, it becomes hard to understand why it was made over time. 
5. Select the option to automatically include reviewers when a pull request is raised automatically.
