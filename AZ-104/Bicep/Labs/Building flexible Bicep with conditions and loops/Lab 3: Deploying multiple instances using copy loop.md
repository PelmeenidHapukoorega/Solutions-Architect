# Lab 3: Deplying multiple instance using copy loop

1. Made a new folder in VSC `Modules2`. Moved `lab3.bicep` into that folder and renamed it to `database.bicep`.
2. Created new `lab3.bicep` module file and added following parameters.
```Bicep
@description('The Azure regions into which the resources should be deployed.')
param locations array = [
  'westus'
  'eastus2'
]

@secure()
@description('The administrator login username for the SQL server.')
param sqlServerAdministratorLogin string

@secure()
@description('The administrator login password for the SQL server.')
param sqlServerAdministratorLoginPassword string
```

I defined locations where resource deployment would occur and added `@secure()` decorator for login and password.

3. Added parameters for module declaration
```Bicep
module databases 'Modules2/database.bicep' = [for location in locations: {
  name: 'database-${location}'
  params: {
    location: location
    sqlServerAdministratorLogin: sqlServerAdministratorLogin
    sqlServerAdministratorLoginPassword: sqlServerAdministratorLoginPassword
  }
}]
```

4. Saved changes and verified file.

Module file
```Bicep
@description('Azure region where resources will be deployed')
param locations array = [
  'norwayeast'
  'norwaywest'
]

@secure()
@description('Admin login username for SQL server')
param sqlServerAdministratorLogin string

@secure()
@description('Admin password for SQL server')
param sqlServerAdministratorLoginPassword string

module databases 'Modules2/database.bicep' = [for location in locations: {
  name: 'database-${location}'
  params: {
    location: location
    sqlServerAdministratorLogin: sqlServerAdministratorLogin
    sqlServerAdministratorLoginPassword: sqlServerAdministratorLoginPassword
  }
}]
```

Database file
```Bicep
@description('Azure region for resource deployment.')
param location string

@secure()
@description('Admin login username for SQL server')
param sqlServerAdministratorLogin string

@secure()
@description('Admin login password for SQL server')
param sqlServerAdministratorLoginPassword string

@description('Name and tier of SQL database SKU')
param sqlDatabaseSku object = {
  name: 'Standard'
  tier: 'Standard'
}

@description('Name of environment, has to be either Development or Production.')
@allowed ([
  'Development'
  'Production'
])
param environmentName string = 'Development'

@description('Name of audit storage account SKU.')
param auditStorageAccountSkuName string = 'Standard_LRS'

var sqlServerName = 'dragon${location}${uniqueString(resourceGroup().id)}'
var sqlDatabaseName = 'DragonNest'
var auditingEnabled = environmentName == 'Production'
var auditStorageAccountName = take('bearaudit${location}${uniqueString(resourceGroup().id)}', 24)

resource sqlServer 'Microsoft.Sql/servers@2024-05-01-preview' = {
  name: sqlServerName
  location: location
  properties: {
    administratorLogin: sqlServerAdministratorLogin
    administratorLoginPassword: sqlServerAdministratorLoginPassword
  }
}

resource sqlDatabase 'Microsoft.Sql/servers/databases@2024-05-01-preview' = {
  parent: sqlServer
  name: sqlDatabaseName
  location: location
  sku: sqlDatabaseSku
}

resource auditStorageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = if (auditingEnabled) {
  name: auditStorageAccountName
  location: location
  sku: {
    name: auditStorageAccountSkuName
  }
  kind: 'StorageV2'
}

resource sqlServerAudit 'Microsoft.Sql/servers/auditingSettings@2024-05-01-preview' = if (auditingEnabled) {
  parent: sqlServer
  name: 'default'
  properties: {
    state: 'Enabled'
    storageEndpoint: environmentName == 'Production' ? auditStorageAccount.properties.primaryEndpoints.blob : ''
    storageAccountAccessKey: environmentName == 'Production' ? auditStorageAccount.listKeys().keys[0].value : ''
  }
}
```
5. Made new RG group `Database`
6. Deployed Bicep
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile lab3.bicep 
```

Ran into region error, couldnt deploy SQL server to `norwaywest`. Went back to modeule `lab3.bicep` and changed locations array to `westus` and `eastus2`.
```Bicep
@description('Azure region where resources will be deployed')
param locations array = [
  'westus'
  'eastus2'
]
```

Deployed again:

<img width="824" height="290" alt="image" src="https://github.com/user-attachments/assets/0f93e61d-8cd1-46b9-8430-b012ca4f38f8" />

Success.

7. Verified deployment in portal

<img width="643" height="108" alt="image" src="https://github.com/user-attachments/assets/03b0b4b4-1e18-4787-af95-1234e24cab03" />

8. Went back to `lab3.bicep` and added new value `eastasia` region to array
```Bicep
@description('Azure region where resources will be deployed')
param locations array = [
  'westus'
  'eastus2'
  'eastasia'
]
```

9. Deployed again
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile lab3.bicep 
```

<img width="883" height="270" alt="image" src="https://github.com/user-attachments/assets/ef0eff8e-9ad1-4b29-be95-57773d36ce9d" />

Success.

10. Verified in portal

<img width="564" height="37" alt="image" src="https://github.com/user-attachments/assets/6dde031b-543a-45f5-a060-1e7510bc2c55" />

And

<img width="597" height="33" alt="image" src="https://github.com/user-attachments/assets/7c97d5a4-7fe3-43bb-87bd-fffd9eeed669" />

10. Deleted RG group to not have unnecessary costs.

### Summary

In this lab i leveled up from deplying single resources to building scalabel, multi region infra using Bicep loops and modules (which i learned previously). Instead of writing the same code over and over i moved my SQL database logic into dedicated `database.bicep` and called it reusable function.

Here i learned copy loop syntax. To deploy multiple places at once i had to define `locations` array like `['westus','eastus2']` and use `for` lopp to tell Bicep to go through that list. Lesson here was avoiding naming conflicts. Found out the hardway that you cant name loop variables the same thing as parameter array.

Ran into region error when `norwayseast` wouldnt accept SQL server, which taught me that Azures map isnt the same for every service. Had to pivto and use different regions like `westus` to get green light. In the end i had successfully deployed my stack across 3 different regions in 1 go, verified it in the portal and then cleaned up resources to keep credit balance safe.
