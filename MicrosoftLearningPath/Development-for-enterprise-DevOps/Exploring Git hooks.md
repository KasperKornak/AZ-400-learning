# Exploring Git hooks
Git hooks are a mechanism that allows code to be run before or after certain Git lifecycle events.
For example, one could hook into the commit-msg event to validate that the commit message structure follows the recommended format.

The hooks can be any executable code, including shell, PowerShell, Python, or other scripts. Or they may be a binary executable.
The only criteria are that hooks must be stored in the .git/hooks folder in the repo root. Also, they must be named to match the related events (Git 2.x).

Some examples of where you can use hooks to enforce policies, ensure consistency, and control your environment:

- In Enforcing preconditions for merging.
- Verifying work Item ID association in your commit message.
- Preventing you & your team from committing faulty code.
- Sending notifications to your team's chat room (Teams, Slack, HipChat, etc.).

## Implementing Git hooks
Git ships with several sample hook scripts in the repo .git\hooks directory. Git executes the contents on the particular occasion type they're approached.

You can write your custom scripts and put them in that folder. 