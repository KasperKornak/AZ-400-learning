# Azure resources by using Azure CLI
## Azure CLI
**Azure CLI** is a command-line program you use to connect to Azure and execute administrative commands on Azure resources. It runs on Linux, macOS, and Windows operating systems.

For example, to restart a VM, you would use a command such as:

```bash
az vm restart -g MyResourceGroup -n MyVm
```

## Working with Azure CLI
Commands in CLI are structured in **groups** and **subgroups**.
Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.

### Finding commands
For example, if you want to find commands that might help you manage a storage blob, you can use the following find command:

```bash
az find blob
```

If you know the command's name you want, the help argument for that command will get you more detailed information on the command—also, a list of the available subcommands for a command group.

```bash
az storage blob --help
```

### Creating resources
**1. Connecting**

```bash
az login
```

**2. Creating resources**

You'll often need to create a new resource group before you create a new Azure service. So we'll use resource groups as an example to show how to create Azure resources from the Azure CLI. The Azure CLI group create command creates a resource group.

```bash
az group create --name <name> --location <location>
```

**3. Veryfing installation**

For many Azure resources, Azure CLI provides a list subcommand to get resource details.

```bash
az group list
```

To get more concise information, you can format the output as a simple table.

```bash
az group list --output table
``````