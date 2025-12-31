# Defining resources for Bicep

Before deploying any resources we need to define them, how resource names work and how to create resources related to each other.

## Define a resource

Main thing you do with Bicep is defining Azure resources. 

Here is an example of what a typical resource definition looks like in Bicep:

```Bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'toylaunchstorage'
  location: 'westus3'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

Now lets take this example apart:

1. The `resource` at the start tells Bicep that we are about to define a resource.
2. Next we give it a "symbolic name". In this example we named it `storageaccount`.
   * Symbolic names are used within Bicep to refer to the resource, but they wont ever show up in Azure.
3. `Microsoft.Storage/storageAccounts@2022-09-01` is the resource type and API version of the resource.
   * `Microsoft.Storage/storageAccounts` tells Bicep that you are declaring an Azure storage account.
   * The date `2022-09-01` is the version of the Azure Storage API that Bicep uses when it creates the resource.

**Note!**

Bicep extension in VSC helps you find the resource types and API version for the resources you create. If familiar with ARM templates, note that the API version matches the version we would use here too.

4. You have to declare "resource name" which is the name the storage account will be assigned in Azure. We will set a resource name using `name` keyword.

**Important!**

Symbolic names are used only within Bicep file and dont appear in Azure. Resource names do appear in Azure.

5. Then we set other details of the resource, such as its location, SKU (pricing tier) and kind.
   * There are also properties you can define that are different for each resource type.
   * Different API versions might introduce different properties too. In the following example we will set up the storage account tier to "hot"

**Note!**

Resource names often have rules you have to follow, like maximum lengths, allowed characters and uniqueness across all of Azure. The requirements for resource names are different for each Azure resource type. Understand the naming restrictions and requirements before adding them to Bicep file.

## What happens when resources depend on each other?

Bicep file usually includes several resources. Often times you need resource to depend on another resource for it to work.

You might have to extract information from 1 resource to be able to define another. If you are deploying web application for example. you will have to create the server infrastructure before you can add an application to it. This relationship is called "dependencies" as in resources may be dependent on other resources.

For the exercise I need to deploy an App Service app for the Bicep file, but to create an App Service app i need to create an App Service plan. The App Service plan represents the server-hostin resources and is declared like the following example:

```Bicep
resource appServicePlan 'Microsoft.Web/serverFarms@2023-12-01' = {
  name: 'toy-product-launch-plan'
  location: 'westus3'
  sku: {
    name: 'F1'
  }
}
```

1. This resource definition is telling Bicep you want to deploy an App Service plan that has the resource type `Microsoft.Web/serverFarms`.
2. The plan resource is named `toy-product-launch-plan` and its deployed into the West US 3 region.
   * It uses a pricing SKU of F1, which is the App Services free tier.

Now that we have declared App Service plan, the next step is to declare the application itself:

```Bicep
resource appServiceApp 'Microsoft.Web/sites@2023-12-01' = {
  name: 'toy-product-launch-1'
  location: 'westus3'
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

1. This Bicep file instructs Azure to host the app on the plan we created.
2. Notice that the plans definition includes the App Service plans symbolic name on line `serverFarmId: appServicePlan.id`
   * This line means that Bicep will get the App Service plans "resource ID" using the `id` property.
   * Its effectively saying: " this apps server-farm ID is the ID of the App Service plan defined earler.

**Note!**

In Azure a "resource ID" is a unique identifier for each resource. The resource ID includes the Azure subscription ID, the resource group name and the resource name, along with some other information.

By declaring the app resource with a property that references the plans symbolic name, Azure understands the "implicit dependency" between App Service app and the plan itself.

When it deploys the resources, Azure ensures it fully deploys the plan before it starts to deploy the app.
