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
|Easily scales for very large codebases|Large integrated codebases|
|Granual permission control|Audit & Access control down to a file level|
|Permits monitoring of usage|Hard to merge file types|
|Allows exclusive file locking| - |

### Typical centralized source control workflow

1. Get the latest changes other people have made from the central server.
2. Make your changes and make sure they work correctly.
3. Check in your changes to the main server so that other programmers can see them.