# Introduction to Secure DevOps
## SQL injection attack
SQL Injection is an attack that makes it possible to execute malicious SQL statements. These statements control a database server behind a web application.

Attackers can use SQL Injection vulnerabilities to bypass application security measures. They can go around authentication and authorization of a web page or web application and retrieve the content of the entire SQL database. They can also use SQL Injection to add, modify, and delete records in the database.

## DevSecOps pipeline

![Alt text](img/secure-devops-workflow-895bcdec.png)

Two essential features of Secure DevOps Pipelines that aren't found in standard DevOps Pipelines are:

- Package management and the approval process associated with it. The previous workflow diagram details other steps for adding software packages to the Pipeline and the approval processes that packages must go through before they're used. These steps should be enacted early in the Pipeline to identify issues sooner in the cycle.
- Source Scanner is also an extra step for scanning the source code. This step allows for security scanning and checking for vulnerabilities that aren't present in the application code. The scanning occurs after the app is built before release and pre-release testing. Source scanning can identify security vulnerabilities earlier in the cycle.

## Continuous security validation
Several tools can be used for it:

- SonarQube.
- Visual Studio Code Analysis and the Roslyn Security Analyzers.
- Checkmarx: A Static Application Security Testing (SAST) tool.
- BinSkim: A binary static analysis tool that provides security and correctness results for Windows portable executables and many more.