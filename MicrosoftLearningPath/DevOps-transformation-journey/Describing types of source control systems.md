# Describing types of source control systems
## Centrailized source control
- Based on the idea that there's a single "central" copy of project somewhere (like server).
- Programmers check in (commit) their changes to central copy.
- Version control tool can talk to the central copy and retrieve any version they need on the fly.

Most popular centralized version control systems are:

- Team Foundation Version Control (TFVC),
- CVS,
- Subversion (SVN),
- Perforce.

|Strengths|Best used for|
|---|---|
|Easily scales for very large codebases|Large integrated codebases.|
|Granual permission control|Audit & Access control down to a file level.|
|Permits monitoring of usage|Hard to merge file types.|
|Allows exclusive file locking| - |

### Typical centralized source control workflow

1. Get the latest changes other people have made from the central server.
2. Make your changes and make sure they work correctly.
3. Check in your changes to the main server so that other programmers can see them.

## Distributed source control
Most popular distributed version control systems:

- Git,
- Mercurial,
- Bazaar.

|Strengths|Best used for|
|---|---|
|Cross Platform support|Small & Modular codebases.|
|An open source friendly code review model via pull requests|Evolving through open source.|
|Complete offline support|Highly distributed teams.|
|Portable history|Teams woring accross platforms.|
|An enthusiastic growing user base|Greenfield codebases.|

### Advantages over centralized source control
- Doing actions other than pushing and pulling changesets is fast because the tool only needs to access the local storage, not a remote server.
- Committing new changesets can be done locally without anyone else seeing them. Once you have a group of changesets ready, you can push all of them at once.
- Everything but pushing and pulling can be done without an internet connection. So, you can work on a plane, and you won't be forced to commit several bug fixes as one large changeset.
- Since each programmer has a full copy of the project repository, they can share changes with one, or two other people to get feedback before showing the changes to everyone.

### Disdvantages over centralized source control
- If your project contains many large, binary files that can't be efficiently compressed, the space needed to store all versions of these files can accumulate quickly.
- If your project has a long history (50,000 changesets or more), downloading the entire history can take an impractical amount of time and disk space.