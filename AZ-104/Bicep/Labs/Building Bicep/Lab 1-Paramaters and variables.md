# Lab 1: Adding parameters and variables to existing Bicep file

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
  name: storageAccountName < CHANGED
  location: location < CHANGED
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: appServicePlanName < CHANGED
  location: location < CHANGED
  sku: {
    name: 'F1'
  }
}

resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: appServiceAppName < CHANGED
  location: location < CHANGED
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

With the end result looking like:

<img width="729" height="679" alt="image" src="https://github.com/user-attachments/assets/f51b9cb5-a367-4cf8-9d05-3b5d0a4e5d0b" />

4. Saved the file.

After defining the parameters and variables i realised that if i were to deploy the code now and run into a region error for example, i could simply edit the parameter at the top to a different region, save it and deploy again. Whereas without parameters i would have to edit each resource seperately.

## Setting SKUs for each environment type

1. Went back to main.bicep file and added following:
```Bicep
@allowed([
  'nonprod'
  'prod'
])
param environmentType string
```

So what did i do here?

* I added a decorator `@allowed` which is a safety constraint that restricts inputs to only specific values listed preventing errors caused by typos.
* `nonprod` which triggers cheaper, low tier resources like the `F1` in the file.
* `prod` which triggers premium, scalable tiers like `P1v2` or `S1` with features like auto scaling and backups for live traffic.
* `environmentType` acts as conditional switch like selecting cost effective SKUs for `nonprod` VS high availability configs for `prod`

2. Added following variable definitions below `appServicePlanName` variable:
```Bicep
var storageAccountSkuName = (environmentType == 'prod') ? 'Standard_GRS' : 'Standard_LRS'
var appServicePlanSkuName = (environmentType == 'prod') ? 'P2v3' : 'F1'
```

So what did i do here?

Previously i had added a decorator with specific values, here i defined those values.

It uses `if/then/else` logic.

I used `prod` so only premium and scalable tier would be used, since its a web application.

So the logic is quite simple:

When the deployment is run you have to provide value for `environmentType` which can either be `prod` or `nonprod`

If you use `prod` it will use use Standard_GRS and P2v3, if you use `nonprod` it will use Standard_LRS and F1.

3. Made changes to code for `sku` properties:
```Bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: storageAccountSkuName < CHANGED
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: appServicePlanName
  location: location
  sku: {
    name: appServicePlanSkuName < CHANGED
  }
}
```
4. Verified Bicep file so there wouldnt be any syntax errors and structure properly

<img width="889" height="872" alt="image" src="https://github.com/user-attachments/assets/3e456100-9305-40b8-a5ce-1782f7f3749d" />

Checked.

## Deploying the updated file

1. Ran the following in VSC PowerShell:
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile main.bicep `
-environmentType nonprod
```

Success:

<img width="779" height="291" alt="image" src="https://github.com/user-attachments/assets/7771da9c-9807-4c4e-a53e-084c8b8f1d43" />

2. Checked the deployment

<img width="656" height="459" alt="image" src="https://github.com/user-attachments/assets/a7b178e8-6330-4b95-94e7-321242555ba4" />

The App Service app and storage account have been deployed with randomly generated names, since we used parameters it will generate names automatically. So for quick deployments this is perfect, if there is name specifications then those need to be clearly defined.

## Summary

Parameters and variables make deployments much quicker since there is only a few keywords that need to be changed in order for it to work, during deployment i instantly ran into the issue of limits in the eastus region, i went back to bicep, changed parameter to norway east and worked, deployed again and ran into no issues.

In short, its extremely convenient way to deploy Bicep without needing manual labor for changing the code.
