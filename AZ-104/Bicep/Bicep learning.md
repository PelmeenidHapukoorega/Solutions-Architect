## Bicep learn path

Since i have already covered the overall understanding of IaC and deployment types i will outline things necessary for me to understand here.


## Imperative code

Imperative code approach is accomplished programmatically by using a scripting language like Bash or Azure PowerShell. The scripts execute a series of steps to create, modify, and even remove your resources.

This example shows two Azure CLI commands that create a resource group and a storage account.

Example:

```CLI
#!/usr/bin/env bash
az group create \
--name storage-resource-group \
--location eastus

az storage account create \
--name mystorageaccount \
--resource-group storage-resource-group \
--location eastus \
--sku Standard_LRS \
--kind StorageV2 \
--access-tier Hot \
--https-only true
```

1. First command creates a resource group named `storage-resource-group` in the East US region.
2. Second command creates storage account named `mystorageaccount` in the `storage-resource-group` resource group that was created in the first command.
   * It also configures some properties for the storage account, including the kind of storage account and its access tier.

You can use imperative approach to fully automate resource provisioning, however the approach has some disadvantages:

* Scripts can become complex to manage.
* Commands could be updated or deprecated, which requires reviews of existing scripts.

## Declarative code

In Azue declaritive code approace is accomplished by using templates. Many types of templates are available for use including:

* JSON
* Bicep
* Ansible by RedHat
* Terraform bu HashiCorp

Here i am focusing on understanding Bicep but in the future will also add Terraform.

Here is an example of Bicep template that configures a storage account, it matches the Azure CLI example:

```Bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'mystorageaccount'
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
    supportsHttpsTrafficOnly: true
  }
}
```

Resources section defines the storage account configuration. It contains:

* Name
* Location
* Properties

Of the storage account, including its SKU and the kind of account.

Bicep template doesnt specify in this instance how to deploy the storage account. It specifies only what the storage account needs to look like. The actual steps that are executed behind the scenes to create this storage account or to update it to match the specification are left for Azure to decide.

