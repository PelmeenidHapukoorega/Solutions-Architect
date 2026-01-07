# Lab 3: Using variable and output loops

1. Opened up `lab3.bicep` in VSC and added following parameters and subnets array
```Bicep
@description('IP address range for all VNets to use')
param virtualNetworkAddressPrefix string = '10.10.0.0/16'

@description('Name and IP address range for each subnet in the VNet')
param subnets array = [
  {
    name: 'frontend'
    ipAddressRange: '10.10.5.0/24'
  }
  {
    name: 'backend'
    ipAddressRange: '10.10.10.0/24'
  }
]
```

So what did i do here?

* `param virtualNetworkAddressPrefix`: Defined a global IP range (10.10.0.0/16) that acts as the "container" for all my virtual networks.
* `param subnets array`: Created a list of objects to store the names and specific IP ranges for internal network segments.
* `name: 'frontend'`: Labeled the first segment to identify where public facing resources will live.
* `ipAddressRange: '10.10.5.0/24'`: Assigned a specific sub slice of IP addresses to the frontend segment.
* `name: 'backend'`: Labeled the second segment for private resources like databases.
* `ipAddressRange: '10.10.10.0/24'`: Assigned a different sub slice of IP addresses to the backend segment to keep traffic separated.

2. Added a blank line under parameters and `subnetProperties` variable loop
```Bicep
var subnetProperties = [for subnet in subnets: {
  name: subnet.name
  properties: {
    addressPrefix: subnet.ipAddressRange
  }
}]
```

So what did i do here?

* `var subnetProperties = [for subnet in subnets: { ... }]`: Created a variable loop that transforms simple subnet list into the specific JSON format Azure requires.
* `name: subnet.name`: Loop pulls the name (like 'frontend') from the array for each iteration.
* `addressPrefix: subnet.ipAddressRange`: Loop maps my IP ranges to the standard Azure addressPrefix property.

3. Added virtualNetworks resource loop at the bottom of the file
```Bicep
resource virtualNetworks 'Microsoft.Network/virtualNetworks@2024-05-01' = [for location in locations: {
  name: 'dragons-${location}'
  properties:{
    addressSpace:{
      addressPrefixes:{
        virtualNetworkAddressPrefix
      }
    }
    subnets: subnetProperties
  }
}]
```

So what did i do here?

* `resource virtualNetworks ... = [for location in locations: { ... }]`: Initiated a resource loop to deploy a unique VNet into every region listed in the locations array.
* `name: 'dragons-${location}'`: Used string interpolation to give each VNet a unique, region specific name like `dragons-norwayeast`.
* `addressSpace: { addressPrefixes: [ virtualNetworkAddressPrefix ] }`: Assigned the broad /16 IP range i had defined in Step 1 to the entire network.
* `subnets: subnetProperties`: Injected the entire list of formatted subnets (frontend and backend) into every VNet being deployed.

4. Saved changes and opened up `database.bicep` to add outputs
```Bicep
output serverName string = sqlServer.name
output location string = location
output serverFullyQualifiedDomainName string = sqlServer.properties.fullyQualifiedDomainName
```

So what did i do here?

* `output serverName`: This basically spits out the name of the SQL server i just built so i dont have to go hunting for it in the portal.
* `output location`: Confirms exactly which region the deployment landed in.
* `output serverFullyQualifiedDomainName`: Grabs the full internet address of the server whichs is the exact string i would need later if i wanted to actually connect the app to the database itself.

5. Went back to `lab3.bicep` and added output loop at the bottom
```Bicep
output serverInfo array = [for i in range(0, length(locations)): {
  name: databases[i].outputs.serverName
  location: databases[i].outputs.location
  fullyQualifiedDomainName: databases[i].outputs.serverFullyQualifiedDomainName
}]
```

6. Saved the file and verified both files.

`lab3.bicep` Code:
```Bicep
@description('Azure region where resources will be deployed')
param locations array = [
  'westus'
  'eastus2'
  'eastasia'
]

@secure()
@description('Admin login username for SQL server')
param sqlServerAdministratorLogin string

@secure()
@description('Admin password for SQL server')
param sqlServerAdministratorLoginPassword string

@description('IP address range for all VNets to use')
param virtualNetworkAddressPrefix string = '10.10.0.0/16'

@description('Name and IP address range for each subnet in the VNet')
param subnets array = [
  {
    name: 'frontend'
    ipAddressRange: '10.10.5.0/24'
  }
  {
    name: 'backend'
    ipAddressRange: '10.10.10.0/24'
  }
]

var subnetProperties = [for subnet in subnets: {
  name: subnet.name
  properties: {
    addressPrefix: subnet.ipAddressRange
  }
}]


module databases 'Modules2/database.bicep' = [for location in locations: {
  name: 'database-${location}'
  params: {
    location: location
    sqlServerAdministratorLogin: sqlServerAdministratorLogin
    sqlServerAdministratorLoginPassword: sqlServerAdministratorLoginPassword
  }
}]

resource virtualNetworks 'Microsoft.Network/virtualNetworks@2024-05-01' = [for location in locations: {
  name: 'dragons-${location}'
  properties:{
    addressSpace:{
      addressPrefixes:[
        virtualNetworkAddressPrefix
      ]
    }
    subnets: subnetProperties
  }
}]

output serverInfo array = [for i in range(0, length(locations)): {
  name: databases[i].outputs.serverName
  location: databases[i].outputs.location
  fullyQualifiedDomainName: databases[i].outputs.serverFullyQualifiedDomainName
}]
```

`database.bicep` Code:
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

output serverName string = sqlServer.name
output location string = location
output serverFullyQualifiedDomainName string = sqlServer.properties.fullyQualifiedDomainName
```

7. Create RG group named `appdatabase` in portal and Deployed `lab3.bicep`
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile lab3.bicep
```

Got the following error:

<img width="669" height="22" alt="image" src="https://github.com/user-attachments/assets/52ceb704-0acf-4071-a863-7fa487bc3497" />

I forgot to add `location: location` line to `virtualNetworks` loop. Basically i forgot to tell each VNet where it belongs.

Quickly added it to vnets:
```Bicep
resource virtualNetworks 'Microsoft.Network/virtualNetworks@2024-05-01' = [for location in locations: {
  name: 'dragons-${location}'
  location: location < ADDED THIS
```

8. Ran the deployment again

<img width="1023" height="622" alt="image" src="https://github.com/user-attachments/assets/67b90037-200c-4e27-9f57-cd7155919d77" />

Success

9. Went to portal to verify if subnets had the anems and IP addresses specified in `subnets` paramaters.

<img width="601" height="103" alt="image" src="https://github.com/user-attachments/assets/ad98a849-f6e9-4486-8a0d-c22e6ea34438" />

VNets successfully deployed.

**Dragons eastasia**

<img width="815" height="321" alt="image" src="https://github.com/user-attachments/assets/8b4db8d5-3c20-49f8-b964-e17b5e053037" />

**Dragons westeurope**

<img width="808" height="325" alt="image" src="https://github.com/user-attachments/assets/72e6df96-4043-424b-b595-100005581281" />

**Dragons eastus2**

<img width="810" height="321" alt="image" src="https://github.com/user-attachments/assets/c4769d11-f30d-4fa2-abf8-38354c6e5855" />

Success

### Summary
