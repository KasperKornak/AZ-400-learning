# Creating Azure resources using Azure Resource Manager templates
## Azure Resource Manager templates
Using Resource Manager templates will make your deployments faster and more repeatable.

- Templates improve consistency. Resource Manager templates provide a common language for you and others to describe your deployments. Despite the tool or SDK that you use to deploy the template, the template's structure, format, and expressions remain the same.
- Templates help express complex deployments. Templates enable you to deploy multiple resources in the correct order. For example, you wouldn't want to deploy a VM before creating an operating system (OS) disk or network interface. Resource Manager maps out each resource and its dependent resources and creates dependent resources first. Dependency mapping helps ensure that the deployment is carried out in the correct order.
- Templates reduce manual, error-prone tasks. Manually creating and connecting resources can be time-consuming, and it's easy to make mistakes. The Resource Manager ensures that the deployment happens the same way every time.
- Templates are code. Templates express your requirements through code. Think of a template as a type of Infrastructure as Code that can be shared, tested, and versioned like any other piece of software. Also, because templates are code, you can create a record that you can follow. The template code documents the deployment. Also, most users maintain their templates under revision control, such as GIT. Its revision history also records how the template (and your deployment) has evolved when you change the template.
- Templates promote reuse. Your template can contain parameters that are filled in when the template runs. A parameter can define a username or password, a domain name, and other necessary items. Template parameters also enable you to create multiple versions of your infrastructure, such as staging and production, while still using the same template.
- Templates are linkable. You can link Resource Manager templates together to make the templates themselves modular. You can write small templates that define a solution and then combine them to create a complete system.

Azure Resource Manager templates are written in JSON, which allows you to express data stored as an object (such as a virtual machine) in text.

### Parameters
For example, you might allow template users to set a username, password, or domain name.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "Username for the Virtual Machine."
    }
  },
  "adminPassword": {
    "type": "securestring",
    "metadata": {
      "description": "Password for the Virtual Machine."
    }
  }
}
```

### Variables
For example, you might define a storage account name one time as a variable and then use that variable throughout the template.

```json
"variables": {
  "nicName": "myVMNic",
  "addressPrefix": "10.0.0.0/16",
  "subnetName": "Subnet",
  "subnetPrefix": "10.0.0.0/24",
  "publicIPAddressName": "myPublicIP",
  "virtualNetworkName": "MyVNET"
}
```

### Functions
An example that creates a function for creating a unique name to use when creating resources that have globally unique naming requirement.

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

### Resources
This section is where you define the Azure resources that make up your deployment. An example that creates a public IP address resource.

```json
{
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIPAddressName')]",
  "location": "[parameters('location')]",
  "apiVersion": "2018-08-01",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsLabelPrefix')]"
    }
  }
}
```

### Outputs
This section is where you define any information you'd like to receive when the template runs. For example, you might want to receive your VM's IP address or fully qualified domain name (FQDN), the information you won't know until the deployment runs.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
  }
}
```

## Managing dependencies
For any given resource, other resources might need to exist before you can deploy the resource.

Within your template, the `dependsOn` element enables you to define one resource dependent on one or more other resources.

### Circular dependencies
A **circular dependency** is a problem with dependency sequencing, resulting in the deployment going around in a loop and unable to continue.
As a result, the Resource Manager can't deploy the resources.
Resource Manager identifies circular dependencies during template validation.

## Modularizing templates
When using Azure Resource Manager templates, it's best to modularize them by breaking them into individual components. The primary methodology to use is by using **linked templates**.

### Linked template

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <link-to-external-template>
      }
  }
]
```

### Nested template
You can also nest a template within the main template, use the template property, and specify the template syntax.

```json
"resources": [
  {
    "apiVersion": "2017-05-10",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
              "accountType": "Standard_LRS"
            }
          }
        ]
      }
    }
  }
]
```

### External templates and parameters
To link to an external template and parameter file, use `templateLink` and `parametersLink`. When linking to a template, ensure that the Resource Manager service can access it.

```json
"resources": [
    {
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://linkedtemplateek1store.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json?sv=2018-03-28&sr=b&sig=dO9p7XnbhGq56BO%2BSW3o9tX7E2WUdIk%2BpF1MTK2eFfs%3D&se=2018-12-31T14%3A32%3A29Z&sp=r"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
```

## Deployment modes
When deploying your resources using templates, you have three options:

- **Validate**: This option compiles the templates, validates the deployment, ensures the template is functional (for example, no circular dependencies), and correct syntax.
- **Incremental mode** (default): This option only deploys whatever is defined in the template. It doesn't remove or modify any resources that aren't defined in the template. For example, if you've deployed a VM via template and then renamed the VM in the template, the first VM deployed will remain after the template is rerun. It's the default mode.
- **Complete mode**: Resource Manager deletes resources that exist in the resource group but isn't specified in the template. For example, only resources defined in the template will be present in the resource group after the template deploys. As a best practice, use this mode for production environments to achieve idempotency in your deployment templates.

## Managing secrets in templates
When passing a secure value (such as a password) as a parameter during deployment, you can retrieve the value from an Azure Key Vault.
Reference the Key Vault and secret in your parameter file.
The value is never exposed because you only reference its Key Vault ID.
The Key Vault can exist in a different subscription than the resource group you're deploying.

## Lab address
https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/Instructions/Labs/AZ400_M06_L12_Azure_Deployments_Using_Resource_Manager_Templates.html