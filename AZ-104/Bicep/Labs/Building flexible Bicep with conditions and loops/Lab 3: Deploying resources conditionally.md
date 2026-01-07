# Lab 3: Creating Bicep with logical server and database then deploying resources conditionally.

1. Opened VSC and created new bicep file `lab3.bicep`
2. Added following content with parameters and variables to define logical server and database
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

var sqlServerName = 'dragon${location}${uniqueString(resourceGroup().id)}'
var sqlDatabaseName = 'DragonNest'

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
```

Added `@secure` decorator because login and password are both sensitive values and `@description` for ease of understanding.

3. Added decorators to constrain environment names and defined a parameter with default value for audit storage SKU.
```Bicep
@description('Name of environment, has to be either Development or Production.')
@allowed ([
  'Development'
  'Production'
])
param environmentName string = 'Development'

@description('Name of audit storage account SKU.')
param auditStorageAccountSkuName string = 'Standard_LRS'
```

4. Added variables to enable auditing
```Bicep
var auditingEnabled = environmentName == 'Production'
var auditStorageAccountName = take('bearaudit${location}${uniqueString(resourceGroup().id)}', 24)
```

* `auditingEnabled` i will use as the condition for deploying auditing resources and for ease of read.
* Used function `take()` to trim the end off the string to ensure name is valid.

5. Added `auditStorageAccount` resource in the code.
```Bicep
resource auditStorageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = if (auditingEnabled) {
  name: auditStorageAccountName
  location: location
  sku: {
    name: auditStorageAccountSkuName
  }
  kind: 'StorageV2'  
}
```

6. Added slServerAudit resource with settings.
```Bicep
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

So what did i do here?

* Used `if (auditingEnabled` expression to tell Azure to only create this auditing resource if environment is set to 'Production'
* Defined `parent: sqlServer` to link auditing settings directly to SQL logical server.
* Set the name to `default` because Azure requires SQL auditing settings to use this specific name at resource level.
* Set the state to `state: 'Enabled'` with properties to turn auditing feature on once resources are deployed.
* Used ternary operator `? :` to provide storage URL only if environment is `Production` otherwise leaving it as empty string.
* Used `.listKeys()` function to dynamically grab storage accounts access key so SQL server would have permission to write logs to the storage account.

7. Saved changes and verified code.
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

8. Created new RG group named deployed the Bicep template
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile lab3.bicep `
-Location norwayeast
```

9. Entered SQL login and password

<img width="882" height="493" alt="image" src="https://github.com/user-attachments/assets/53043235-b1e6-4f66-ad2b-790237d37be1" />

Success

10. Verified deployment in the portal

<img width="676" height="466" alt="image" src="https://github.com/user-attachments/assets/4e3399ce-912f-43bd-8c5e-4c89d2591a6e" />

Success

11. Redeployed to use 'Production' environment
```PWSL
New-AzResourceDeployment `
-Name main `
-TemplateFile lab3.bicep `
-environmentName Production `
-location norwayeast
```

<img width="895" height="475" alt="image" src="https://github.com/user-attachments/assets/98e0f535-1c63-4bbd-a54e-274a3f417ccd" />

Success

11. Verified deployment again

<img width="657" height="499" alt="image" src="https://github.com/user-attachments/assets/9c45a5ed-c560-46ae-8b70-73b31d4f669e" />

12. Selected SQL server to verify if auditing is enabled

<img width="825" height="581" alt="image" src="https://github.com/user-attachments/assets/efdcedc2-f482-4e54-b61b-e2f8f49d824a" />

Success.

12. Deleted resource group to avoid unnecessary costs aka cleaned up.

### Summary

In this lab i shifted focus to conditional deployment and dynamic resource logic. I learned how to use `if` expressions to only deploy expensive or heavy resources like the audit storage account when the `environmentName` is set to 'Production' keeping the 'Development' environment lean and cost effective.

Practiced using ternary operators such as `? ;` to route data such as providing storage endpoint on when auditing is actually active. Refined naming conventions using `take()` function to ensure storage account names would stay withing 24 char limit. By verifying deployment in the portal i confirmed that conditional logic worked, auditing was enabled and connected to its storage backend onl when i requested speficifally 'Production' build.
