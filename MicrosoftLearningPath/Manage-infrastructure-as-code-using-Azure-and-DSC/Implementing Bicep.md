# Implementing Bicep
## Bicep introduction
**Azure Bicep** is the next revision of ARM templates designed to solve some of the issues developers were facing when deploying their resources to Azure. It's an Open Source tool and, in fact, a domain-specific language (DSL) that provides a means to declaratively codify infrastructure, which describes the topology of cloud resources such as VMs, Web Apps, and networking interfaces. It also encourages code reuse and modularity in designing the infrastructure as code files.

### Installing Bicep

```bash
# for new installation
az bicep install

# upgrading existing installation
az bicep upgrade
```

### Deploying Bicep

```bash
az group create --name Bicep --location eastus

az deployment group create --resource-group Bicep --template-file main.bicep --parameters storageName=uniqueName
```

## Bicep syntax

```Bicep
@minLength(3)
@maxLength(11)
param storagePrefix string

param storageSKU string = 'Standard_LRS'
param location string = resourceGroup().location

var uniqueStorageName = '${storagePrefix}${uniqueString(resourceGroup().id)}'

resource stg 'Microsoft.Storage/storageAccounts@2019-04-01' = {
    name: uniqueStorageName
    location: location
    sku: {
        name: storageSKU
    }
    kind: 'StorageV2'
    properties: {
        supportsHttpsTrafficOnly: true
    }

    resource service 'fileServices' = {
        name: 'default'

        resource share 'shares' = {
            name: 'exampleshare'
        }
    }
}

module webModule './webApp.bicep' = {
    name: 'webDeploy'
    params: {
        skuName: 'S1'
        location: location
    }
}

output storageEndpoint object = stg.properties.primaryEndpoints
```

Legend:

- **Scope**: By default the target scope of all templates is set for `resourceGroup`, however, you can customize it by setting it explicitly. As other `allowed` values, `subscription`, `managementGroup`, and `tenant`.
- **Parameters**: They allow you to customize your template deployment at run time by providing potential values for names, location, prefixes, etc.
- **Variables**: Similar to parameters, variables play a role in making a more robust and readable template. Any complex expression can be stored in a variable and used throughout the template. When you define a variable, the type is inferred from the value.
- **Resources**: The resource keyword is used when you need to declare a resource in your templates. The resource declaration has a symbolic name for the resource that can be used to reference that resource later either for defining a subresource or for using its properties for an implicit dependency like a parent-child relationship.
- **Modules**: If you want truly reusable templates, you can't avoid using a module. Modules enable you to reuse a Bicep file in other Bicep files. In a module, you define what you need to deploy, and any parameters needed and when you reuse it in another file, all you need to do is reference the file and provide the parameters.
- **Outputs**: You can use outputs to pass values from your deployment to the outside world, whether it is within a CI/CD pipeline or in a local terminal or Cloud Shell. That would enable you to access a value such as storage endpoint or application URL after the deployment is finished.