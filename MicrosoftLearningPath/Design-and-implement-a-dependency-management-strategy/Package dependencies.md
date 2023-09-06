# Package dependencies
## Elements of a dependency management strategy

- **Standardization** Managing dependencies benefit from a standardized way of declaring and resolving them in your codebase.
Standardization allows a repeatable, predictable process and usage that can be automated as well.
- **Package formats and sources** The distribution of dependencies can be performed by a packaging method suited for your solution's dependency type.
Each dependency is packaged using its usable format and stored in a centralized source.
Your dependency management strategy should include the selection of package formats and corresponding sources where to store and retrieve packages.
- **Versioning** Just like your own code and components, the dependencies in your solution usually evolve.
While your codebase grows and changes, you need to consider the changes in your dependencies as well.
It requires a versioning mechanism for the dependencies to be selective of the version of a dependency you want to use.

## Source and package componentization

- **Source componentization** The first way of componentization is focused on source code. It refers to splitting the source code in the codebase into separate parts and organizing it around the identified components.
It works if the source code isn't shared outside of the project. Once the components need to be shared, it requires distributing the source code or the produced binary artifacts created from it.
- **Package componentization** The second way uses packages. Distributing software components is performed utilizing packages as a formal way of wrapping and handling the components.
A shift to packages adds characteristics needed for proper dependency management, like tracking and versioning packages in your solution.

## Scanning codebase for dependencies

- **Duplicate code**: When certain pieces of code appear in several places, it's a good indication that this code can be reused. Keep in mind that code duplication isn't necessarily a bad practice. However, if the code can be made available properly, it does have benefits over copying code and must manage that. The first step to isolate these pieces of duplicate code is to centralize them in the codebase and componentize them in the appropriate way for the type of code.
- **High cohesion and low coupling**: A second approach is to find code that might define components in your solution. You'll look for code elements that have high cohesion and low coupling with other parts of code. It could be a specific object model with business logic or code related to its responsibility, such as a set of helper or utility codes or perhaps a basis for other code to be built upon.
- **Individual lifecycle**: Related to high cohesion, you can look for parts of the code that have a similar lifecycle and can be deployed and released individually. If such code can be maintained by a team separate from the codebase that it's currently in, it's an indication that it could be a component outside of the solution.
- **Stable parts**: Some parts of your codebase might have a slow rate of change. That code is stable and isn't altered often. You can check your code repository to find the code with a low change frequency.
- **Independent code and components**: Whenever code and components are independent and unrelated to other parts of the system, they can be isolated to a separate component and dependency.