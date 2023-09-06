# Understanding package management
## Packages
A package is a formalized way of creating a distributable unit of software artifacts that can be consumed from another software solution.

The package describes the content it contains and usually provides extra metadata, and the information uniquely identifies the individual packages and is self-descriptive.

Packages are used to define the components you rely on and depend upon in your software solution.

### Types of packages
Over the past years, the packaging formats have changed and evolved. Now there are a couple of de facto standard formats for packages.

- **NuGet** packages are a standard used for .NET code artifacts. It includes .NET assemblies and related files, tooling, and sometimes only metadata. NuGet defines the way packages are created, stored, and consumed. A NuGet package is essentially a compressed folder structure with files in ZIP format and has the `.nupkg` extension.
- **npm** package is used for JavaScript development. It originates from node.js development, where it's the default packaging format. An npm package is a file or folder containing JavaScript files and a `package.json` file describing the package's metadata. For node.js, the package usually includes one or more modules that can be loaded once the package is consumed. 
- **Maven** is used for Java-based projects. Each package has a Project Object Model file describing the project's metadata and is the basic unit for defining a package and working with it.
- **PyPi** The Python Package Index, abbreviated as PyPI and known as the Cheese Shop, is the official third-party software repository for Python.
- **Docker** packages are called images and contain complete and self-contained deployments of components. A Docker image commonly represents a software component that can be hosted and executed by itself without any dependencies on other images. Docker images are layered and might be dependent on other images as their basis. Such images are referred to as base images.

## Package feeds
Packages should be stored in a centralized place for distribution and consumption to take dependencies on the components it contains. The centralized storage for packages is commonly called a package feed. There are other names in use, such as repository or registry.

## Azure Artifacts
**Azure Artifacts** currently supports feeds that can store five different package types:

- NuGet packages.
- npm packages.
- Maven.
- Universal packages.
- Python.

## Lab address
https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/Instructions/Labs/AZ400_M08_L15_Package_Management_with_Azure_Artifacts.html