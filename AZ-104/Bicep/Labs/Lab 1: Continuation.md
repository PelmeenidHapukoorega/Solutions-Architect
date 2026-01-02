# Lab 1 Continuation: Adding parameters and variables to existing Bicep file

This is a continuation of: [Lab 1: Bicep file with storage](./Lab1:%20Bicep%20file%20with%20storage.md)

1. Opened up VSC and main.bicep file i had created in the last lab.
2.  Added following parameters and variables for it at the top of the Bicep file:
```Bicep
param location string = 'eastus'
param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'
param appServiceAppName string = 'toylaunch${uniqueString(resourceGroup().id)}'

var appServicePlanName = 'toy-product-launch-plan'
```

So what did i actually do here?

* I defined `location` where my resources will be deployed, in this case i used `eastus`.
* Set parameter for `storageAccountName` which gives me a unique identifier for the service that will host data objects such as blobs or files.
* Set parameter for `appServiceAppName` which sets the unique DNS endpoint used by users to access the toyshop web app over the internet.
* Set variable for `appServicePlanName` which specifies the name of the underlying server farm and defines the resources and pricing tier for the app.

3. Renamed `name` and `location` for each resource in the file in accordance to the parameters.
```Bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageAccountName < changed
  location: location < changed
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: appServicePlanName < changed
  location: location < changed
  sku: {
    name: 'F1'
  }
}

resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: appServiceAppName < changed
  location: location < changed
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

With the end result looking like:

<img width="729" height="679" alt="image" src="https://github.com/user-attachments/assets/f51b9cb5-a367-4cf8-9d05-3b5d0a4e5d0b" />

4. Saved the file.
