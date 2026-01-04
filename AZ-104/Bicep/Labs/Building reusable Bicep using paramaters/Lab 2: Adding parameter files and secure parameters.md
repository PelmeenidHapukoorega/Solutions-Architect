# Lab 2: Adding parameter file and secure parameters.

1. Went back to the file `new.bicep` and updated `appServicePlanSku` parameter to remove its default value.

<img width="293" height="21" alt="image" src="https://github.com/user-attachments/assets/334d05ed-7423-4484-89a5-f6276e58ba96" />

2. Added `sqlServerAdministratorLogin`, `sqlServerAdministratorPassword` and `sqlDatabaseSku` parameters underneath current parameter declarations:

```Bicep
@secure()
@description('Admin login username for SQL server.')
param sqlServerAdministratorLogin string

@secure()
@description('Admin login password for SQL server')
param sqlServerAdministratorPassword string

@description('Name and tier of the SQL database SKU.')
param sqlDatabaseSku object
```
So what did i do here?

* `@secure` - told Azure not to log the value of the parameter or show it in deployment history, thus protecting secrets from being seen in plain text.
* `param sqlServerAdministratorLogin string` - defined text input for the SQL servers "master" username.
* `param sqlServerAdministratorPasswword string` - defined text input for the SQL servers "master" password. Because i added `@secure` this will act like a hidden password field during deployment.

Code:

<img width="485" height="209" alt="image" src="https://github.com/user-attachments/assets/9d83ea7f-be01-45d2-b48f-9f35e0c0ac63" />

I didnt add specific values for those parameters because apparently its a bad practice to add default values for secure parameters. The goal is to specify those values in a parameters file instead.

3. Added new variables `sqlServerName` and `sqlDatabaseName`
```Bicep
var sqlServerName = '${senvironmentName}-${solutionName}-sql'
var sqlDatabaseName = 'Employees'
```
4. Added SQL server and database resources at the bottom of the file
```Bicep
resource sqlServer 'Microsoft.Sql/servers@2024-05-01-preview' ={
  name: sqlServerName
  location: location
  properties: {
    administratorLogin: sqlServerAdministratorLogin
    administratorLoginPassword: sqlServerAdministratorPassword
  }
}

resource sqlDatabase 'Microsoft.Sql/servers/databases@2024-05-01-preview' ={
  parent: sqlServer
  name: sqlDatabaseName
  location: location
  sku: {
    name: sqlDatabaseSku.name
    tier: sqlDatabaseSku.tier
  }
}
```

5. Saved changes and verified the file
```Bicep
@description('Name of the environment, has to be dev, test or prod.')
@allowed([
  'dev'
  'test'
  'prod'
])
param environmentName string ='dev'

@description('Unique name of the solution, used to ensure that resource names are unique.')
@minLength(5)
@maxLength(30)
param solutionName string = 'toyhr${uniqueString(resourceGroup().id)}'

@description('Number of App Service plan instances.')
@minValue(1)
@maxValue(10)
param appServicePlanInstanceCount int = 1

@description('Name and tier of the App Service plan SKU.')
param appServicePlanSku object 

@description('Azure region where resources will be deployed')
param location string = 'norwayeast'

@secure()
@description('Admin login username for SQL server')
param sqlServerAdministratorLogin string

@secure()
@description('Admin login password for SQL server')
param sqlServerAdministratorPassword string

@description('Name and tier of the SQL database SKU')
param sqlDatabaseSku object

var appServicePlanName = '${environmentName}-${solutionName}-plan'
var appServiceAppName = '${environmentName}-${solutionName}-app'
var sqlServerName = '${environmentName}-${solutionName}-sql'
var sqlDatabaseName = 'Employees'

resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: appServicePlanName
  location: location
  sku: {
    name: appServicePlanSku.name
    tier: appServicePlanSku.tier
    capacity: appServicePlanInstanceCount
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

resource sqlServer 'Microsoft.Sql/servers@2024-05-01-preview' ={
  name: sqlServerName
  location: location
  properties: {
    administratorLogin: sqlServerAdministratorLogin
    administratorLoginPassword: sqlServerAdministratorPassword
  }
}

resource sqlDatabase 'Microsoft.Sql/servers/databases@2024-05-01-preview' ={
  parent: sqlServer
  name: sqlDatabaseName
  location: location
  sku: {
    name: sqlDatabaseSku.name
    tier: sqlDatabaseSku.tier
  }
}
```

I made small addition of adding `@minLength` and `@maxLength` decorators to get used to using them.

## Creating a parameters file

1. Created a new parameters file in VSC to the same folder where `new.bicep` was located and named it `new.parameters.dev.json`

2. Added following code
```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "appServicePlanSku": {
      "value": {
        "name": "F1",
        "tier": "Free"
      }
    },
    "sqlDatabaseSku": {
      "value": {
        "name": "Standard",
        "tier": "Standard"
      }
    }
  }
}
```

So what did i do here?

* `"$schema"` - povided rules for the JSON structure so i could get helpful autocomplete and error checking
* `"contentVersion": "1.0.0.0",` - Internal version number for the parameters file which is usually left at 1.0.0.0. Helps tracking changes to the file over time.
* `parameters` - main container that holds all the actual data that i want to pass into the Bicep template.
* `appServicePlanSku` - this targets the parameter with the exact name in the `new.bicep` file to provide specific values.
* `value:` - Where i assign the data, for the App Service i told Azure to use F1 free tier for this environment.
* `value: { "name": "Standard", "tier": "Standard" }` - Set the SQL database to a standard tier which is a 1 step up from free  tier and provides better performance for dev database.


3. Created new resource group in portal and named it `nasamoonproject`

4. Went ahead and deployed `new.bicep` template with parameters file `new.parameters.dev.json`

```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile new.bicep `
-TemplateParameterFile new.parameters.dev.json
```

5. Terminal asked for admin username and password to be entered so I provided those, will not disclose those for security reasons even tho i will delete the RG group later to save costs.

The admin username can only contai alphanumeric characters and must start with a letter. Password has to be at least 8 characters long, include lowercase letter, uppercase letters, numbers and symbols for security reasons.

Password part i realised after deployment failure for that reason and that it was too short:

<img width="726" height="37" alt="image" src="https://github.com/user-attachments/assets/1013e0fd-b772-4487-9393-9227b12de9a4" />

So I deployed again after revising my password.

<img width="890" height="477" alt="image" src="https://github.com/user-attachments/assets/fcaa9779-8b1c-41be-8a39-d11277dc2f9a" />

Success

## Creating Key vault and secrets

Wanted to create Key vault to store secrets like the admin login and password.

So i ran the following commands in terminal 1by1.

1. First i needed to define the name for the key vault:
```PWSL
$keyVaultName = 'moonproject`
```

2. Then i added prompts so i as the host or someone else who would use the templates could understand what needs to be typed in.
```PWSL
$login = Read-Host "Enter the login name" -AsSecureString
```

Entered my login.

```PWSL
%password = Read-Host "Enter the password" -AsSecureString
```

Entered my password.

3. Set a `-EnabledForTemplateDeployment` setting on the vault so Azure could use the secrets from my vault during deployments.
```PWSL
New-AzKeyVault -VaultName $keyVaultName -Location norwayeast -EnabledForTemplateDeployment
```

4. Then i set up the secrets
```PWSL
Set-AzKeyVaultSecret -VaultName $keyVaultName -Name 'sqlServerAdministratorLogin' -SecretValue $login
Set-AzKeyVaultSecret - VaultName $keyVaultName -Name 'sqlServerAdministratorPassword' -SecretValue $password
```
5. Created a new keyvault
```PWSL
New-AzKeyVault -VaultName $keyVaultName -Location eastus -EnabledForTemplateDeployment
```
And ran into a name issue, since my `moonvault` name was already in use.

I learned that you could name your key vault to whatever and it wont show whether the name is in use, you will find it out after trying to create the vault, so i used `$keyVaultName =` again and renamed it and tried again.

<img width="882" height="660" alt="image" src="https://github.com/user-attachments/assets/a362e9cd-dbb5-421e-bd05-cb841be44e93" />

Success

6. Now i needed to get key vaults resource ID
```PWSL
(Get-AzKeyVault -Name $keyVaultName).ResourceId
```

7. Copied the ID
`/subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/moonproject/providers/Microsoft.KeyVault/vaults/juhimeil`

8. Added key vault reference to parameter file
```JSON
        },
        "sqlServerAdministratorLogin": {
            "reference": {
                "keyVault": {
                    "id": "/subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/newplanetwhodis/providers/Microsoft.KeyVault/vaults/juhimeil"
                },
                "secretName": "sqlServerAdministratorLogin"
            }
        },
        "sqlServerAdministratorPassword": {
            "reference": {
                "keyVault": {
                    "id": "/subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/newplanetwhodis/providers/Microsoft.KeyVault/vaults/juhimeil"
                },
                "secretName": "sqlServerAdministratorPassword"
            }
        }
    }
}
```

9. Saved the file

10. Deployed Bicep with parameter file and Key vault references
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile new.bicep `
-TemplateParameterFile new.parameters.dev.json
```
And it failed.

## Fixing deployment issue

**What happened?**

The goal was to connect a Bicep template to an Azure Key Vault to securely retrieve SQL Administrator credentials during deployment. The process failed multiple times due to configuration, naming, and permission errors before reaching a successful state.

### The "Identity and Address" struggle

Infrastructure and scope connectivity issues.

| Error Message | Root Cause |
| :--- | :--- |
| `Code=ResourceGroupNotFound` | **Typo:** The PowerShell command targeted `moonprojects` (plural), but the actual group was `moonproject` (singular). |
| `Code=KeyVaultParameterReferenceNotFound` | **ID Mismatch:** The `parameters.json` file pointed to the Resource ID of `bankofgrollz`, but the active vault being used was `juhimeil`. |
| `Code=Forbidden (403)` | **RBAC Permissions:** The user had Subscription "Owner" rights (Control Plane) but lacked "Key Vault Secrets Officer" (Data Plane) rights required to write secrets. |

### The Data Plane struggle
Secret management and type issues.

| Error Message | Root Cause |
| :--- | :--- |
| `Code=MultipleErrorsOccurred: NotFound` | **Empty Vault:** The Key Vault resource existed, but the secrets (`sqlServerAdministratorLogin`, etc.) had not yet been created inside it. |
| `Cannot convert "adminUser" to "SecureString"` | **Type Mismatch:** Attempted to pass a plain text string in PowerShell to a parameter that Bicep defined as `@secure()`. |


### The Resolution 
The following sequence successfully cleared all errors and deployed the resources.

### Step 1: Granted Data Permissions
Resolved the 403 Forbidden error by assigning the Data Plane role.
```PWSL
New-AzRoleAssignment -SignInName "your-email@domain.com" `
  -RoleDefinitionName "Key Vault Secrets Officer" `
  -ResourceGroupName "newplanetwhodis"
```

### Step 2: Populating the vault

Resolved the "NotFound" error by actually creating the secrets again. Note the conversion to `SecureString`.

```PWSL
$keyVaultName = 'juhimeil'
$login = Read-Host "Enter the login name" -AsSecureString
$password = Read-Host "Enter the password" -AsSecureString

New-AzKeyVault -VaultName $keyVaultName -Location eastus -EnabledForTemplateDeployment
Set-AzKeyVaultSecret -VaultName $keyVaultName -Name 'sqlServerAdministratorLogin' -SecretValue $login
Set-AzKeyVaultSecret -VaultName $keyVaultName -Name 'sqlServerAdministratorPassword' -SecretValue $password
```

### Step 3: Getting the new resource ID

Resolved the Reference Not Found error by getting the exact ID from Azure rather than guessing.

```PWSL
(Get-AzKeyVault -Name "juhimeil").ResourceId
```

Copied the ID and pasted it over `id` sections in `new.parameter.dev.json` file.

### Final deployment

Ran commands again

```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile main.bicep `
-TemplateParameterFile main.parameters.dev.json
```

<img width="891" height="614" alt="image" src="https://github.com/user-attachments/assets/22ff753d-0195-4a6f-910e-c1b2de935716" />

Finally, success

## Checked deployment in the portal

<img width="642" height="460" alt="image" src="https://github.com/user-attachments/assets/0787cef8-f3b4-4e90-8001-be3d33482fcc" />

Success

Checked `Inputs` tab from left pane for details

<img width="633" height="575" alt="image" src="https://github.com/user-attachments/assets/fd29e40e-7f32-4f14-810c-b99e40f485df" />

Success

### Summary
