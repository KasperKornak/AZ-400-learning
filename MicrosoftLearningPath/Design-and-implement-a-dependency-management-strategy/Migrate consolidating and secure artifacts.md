# Migrate consolidating and secure artifacts
An **artifact** is a deployable component of your application. Azure Pipelines can work with a wide variety of artifact sources and repositories.

When you're creating a release pipeline, you need to link the required artifact sources. Often, it will represent the output of a build pipeline from a continuous integration system like Azure Pipelines, Jenkins, or TeamCity.

**Azure Artifacts** are one of the services that's part of Azure DevOps. Using it can eliminate the need to manage file shares or host private package services. It lets you share code easily by allowing you to store Maven, npm, or NuGet packages together, cloud-hosted, indexed and matched.

## Secure access to package feeds
### Trusted sources
Package feeds are a trusted source of packages. The offered packages will be consumed by other codebases and used to build software that needs to be secure.

### Securing access
Package feeds must be secured for access by authorized accounts, so only verified and trusted packages are stored there.
None should push packages to a feed without the proper role and permissions.

### Securing availability
Another aspect of security for package feeds is about the public or private availability of the packages.
The feeds of public sources are available for anonymous consumption.
Private feeds have restricted access most of the time.

## Azure Artifacts roles

- **Reader**: Can list and restore (or install) packages from the feed.
- **Collaborator**: Can save packages from upstream sources.
- **Contributor**: Can push and unlist packages in the feed.
- **Owner**: Has all available permissions for a package feed.