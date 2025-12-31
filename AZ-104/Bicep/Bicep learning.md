## Bicep learn path

Since i have already covered the overall understanding of IaC and deployment types i will outline things necessary for me to understand here.

## Bicep Language

Bicep is ARM template language that is used to declaratively deploy Azure resources.
Its designed for a specific scenario or domain which makes it a domain specific language.

Its not meant to be used as a standard programming language for writing apps. Its used only to create ARM templates.

Its intentended to be easy to understand and straightforward to learn, regardless of experience with other programming languages.

All resource types, API versions and properties are valid in Bicep templates.

### Bicep over JSON

Bicep provides many improvements over JSON for template authoring:

* **Simpler syntax:** Easier to write templates. You can reference parameters and variables directly, without using complicated functions. String inerpolation (combining) is used in place of concatenation to combine values for names and other items. You can reference the properties of a resource directly by using its symbolic name instead of complex reference statements. These syntax improvements help both with authoring and reading Bicep templates.

* **Modules:** You can break down complex template deployments into smaller module files and reference them in a main template. These modules provide easier management and greater reusability. You can even share your modules with your team.

* **Automatic dependency management:** In most situations, Bicep automatically detects dependencies between your resources. This process removes some of the work involved in template authoring.

* **Type validation and IntelliSense:** The Bicep extension for Visual Studio Code features rich validation and IntelliSense for all Azure resource type API definitions. This feature helps provide an easier authoring experience.

**In short:**

* Bicep simplifies the template creation experience.
* Syntax is much easier to understand.
* Better support for modularity and reusable code.
* Improved type safety.
* Creating JSON ARM template requires complicated expressions and the final result might be verbose.

Here is an example of Bicep template that defines Azure storage account:

```Bicep
param location string = resourceGroup().location
param namePrefix string = 'storage'

var storageAccountName = '${namePrefix}${uniqueString(resourceGroup().id)}'
var storageAccountSku = 'Standard_RAGRS'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageAccountName
  location: location
  kind: 'StorageV2'
  sku: {
    name: storageAccountSku
  }
  properties: {
    accessTier: 'Hot'
    supportsHttpsTrafficOnly: true
  }
}

output storageAccountId string = storageAccount.id
```

The template automatically generates the name of the storage account. After deployment the resource ID is returned as output to the user who executes the template.

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

