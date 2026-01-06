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

3. Added parameters for auditing and 
