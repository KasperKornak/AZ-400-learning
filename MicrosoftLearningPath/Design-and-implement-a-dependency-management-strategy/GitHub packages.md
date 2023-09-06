# GitHub packages
## Publishing packages
GitHub Packages use native package tooling commands to publish and install package versions.

Support for package registries:

|Language|Package format|Package client|
|---|---|---|
|JavaScript|package.json|npm|
|Ruby|Gemfile|gem|
|Java|pom.xml|mvn|
|Java|build.gradle or build.gradle.kts|gradle|
|.NET|nupkg|dotnet CLI|
|N/A|Dockerfile|Docker|

Using any supported package client, to publish your package, you need to:

1. Create or use an existing access token with the appropriate scopes for the task you want to accomplish: Creating a personal access token. When you create a personal access token (PAT), you can assign the token to different scopes depending on your needs. 
2. Authenticate to GitHub Packages using your access token and the instructions for your package client.
3. Publish the package using the instructions for your package client.

## Installing packages
You can install any package you have permission to view from GitHub Packages and use the package as a dependency in your project. After you find a package, read the package's installation and description instructions on the package page. You can install a package using any supported package client following the same general guidelines.

1. Authenticate to GitHub Packages using the instructions for your package client.
2. Install the package using the instructions for your package client.

## Deleting and restoring a package
You can delete it on GitHub if you have the required access:

- An entire private package.
- If there aren't more than 5000 downloads of any version of the package, an entire public package.
- A specific version of a private package.
- A specific version of a public package if the package version doesn't have more than 5000 downloads.
- For packages that inherit their access permissions from repositories, you can delete a package if you have admin permissions to the repository.

You can also restore an entire package or package version, if:

- You restore the package within 30 days of its deletion.
- The same package namespace is still available and not used for a new package.

You can use the REST API to manage your packages.

## Package access control and visibility

|Permission |Access description|
|---|---|
|read|Can download the package. Can read package metadata.|
|write|Can upload and download this package. Can read and write package metadata.|
|admin|Can upload, download, delete, and manage this package. Can read and write package metadata. Can grant package permissions.|