# Identifying technical debt
## Five traits to measure quality of code

1. Reliability.
2. Maintainabilty.
3. Testability.
4. Portability.
5. Reusability.

## Quality-related metrics
- **Failed builds percentage**: Overall, what percentage of builds are failing?
- **Failed deployments percentage**: Overall, what percentage of deployments are failing?
- **Ticket volume**: What is the overall volume of customer or bug tickets?
- **Bug bounce percentage**: What percentage of customer or bug tickets are reopened?
- **Unplanned work percentage**: What percentage of the overall work is unplanned?

## Technical debt sources
- Lack of coding style and standards.
- Lack of or poor design of unit test cases.
- Ignoring or not understanding object oriented design principles.
- Monolithic classes and code libraries.
- Poorly envisioned the use of technology, architecture, and approach. (Forgetting that all system attributes, affecting maintenance, user experience, scalability, and others, need to be considered).
- Over-engineering code (adding or creating code that isn't required, adding custom code when existing libraries are sufficient, or creating layers or components that aren't needed).
- Insufficient comments and documentation.
- Not writing self-documenting code (including class, method, and variable names that are descriptive or indicate intent).
- Taking shortcuts to meet deadlines.
- Leaving dead code in place.

### Quality tools
- NDepend.
- Visual Studio marketplace (with keyword *Quality*).
- Resharper Code Quality Analysis.