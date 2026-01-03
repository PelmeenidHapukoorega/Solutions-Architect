# Lab 1 Continuation: Refactoring Bicep to use modules.

1. Opened up VSC and created a new folder `modules` in the `Bicep Projects` folder.
   * Added a new filed called `appService.bicep`.

2. Copied from `main.bicep` and pasted the following into the `appService.bicep`:
```Bicep
param location string
param appServiceAppName string

@allowed([
  'nonprod'
  'prod'
])
param environmentType string

var appServicePlanName = 'toy-product-launch-plan'
var appServicePlanSkuName = (environmentType == 'prod') ? 'P2v3' : 'F1'

resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: appServicePlanName
  location: location
  sku: {
    name: appServicePlanSkuName
  }
}

resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: appServiceAppName
  location: location
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

So what did i do here?

* I moved the `App Service` logic into its own file to create reusable module for future use regarding other projects.
* Made the module self contained by defining its parameters and variables so it wouldnt rely on external data it hasnt been given.
* Simplified the interface by adding
