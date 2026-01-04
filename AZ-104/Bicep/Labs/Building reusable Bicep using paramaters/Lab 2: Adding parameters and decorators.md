# Lab 2: Creating Bicep file that includes parameters, variables and decorators

1. Opened up VSC and created a new bicep file titled `new.bicep` since i have already `main.bicep` from previous lab and i want to keep those files in the same folder.
2. Added the following variables and parameters:
```Bicep
param environmentName string ='dev'
param solutionName string = 'toyhr${uniqueString(resourceGroup().id)}'
param appServicePlanSku object = {
  name: 'F1'
  tier: 'Free'
}
param location string = 'norwayeast'

var appServicePlanName = '${environmentName}-${solutionName}-plan'
var appServiceAppName = '${environmentName}-${solutionName}-app'
```

So what did i do here?

* `param environmentName string ='dev'` - Created a setting for the environment and left it default.
* `param solutionName string = 'toyhr${uniqueString(resourceGroup().id)}'` - Set a base name for the project using a function to generate short unique string based on RG group ID to prevent naming conflicts.
* `param appServicePlanSku object = { ... }` - Define the size and cost tiers of the hosting plan as objects, set it on `F1` which is free tier by default.
* `param location string = 'norwayeast'` - Specified location where resources will be deployed.
* `var appServicePlanName = '${environmentName}-${solutionName}-plan'` - Set a variable that combines the environment and solution name to create a standardized name for the App Service plan.
* `var appServiceAppName = '${environmentName}-${solutionName}-app'` - Same as above but this creates the name for the Web App itself.

3. Added following resources with values defined with parameters and variables before:
```Bicep
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
```

4. Saved changes

## Adding parameter descriptions

1. Went back to `new.bicep` and added descriptions for parameters:

* `param environmentName` - "Name of the environment, has to be dev, test or prod."
* `param solutionName` - "Unique name of the solution, used to ensure that resource names are unique."
* `param appServicePlanInstanceCount` - Number of App Service plan instances."
* `param appServicePlanSku` - "Name and tier of the App Service Plan SKU."

Code:
```Bicep
@description('Name of the environment, has to be dev, test or prod.')
param environmentName string ='dev'

@description('Unique name of the solution, used to ensure that resource names are unique.')
param solutionName string = 'toyhr${uniqueString(resourceGroup().id)}'

@description('Number of App Service plan instances.')
param appServicePlanInstanceCount int = 1

@description('Name and tier of the App Service plan SKU.')
param appServicePlanSku object = {
  name: 'F1'
  tier: 'Free'
}

@description('Azure region where resources will be deployed')
param location string = 'norwayeast'
```

So what did i do here?

I added descriptions for parameters in the case that this template would go to the hands of other users they would have "guidelines" and understanding what each parameter is used for using `@description`

2. Saved changes

## Limiting input values

1. In the main file i went to `environmentName` parameter and inserted `@allowed` decorator underneath its `@description` decorator with the following values:
```Bicep
@allowed([
  'dev'
  'test'
  'prod'
])
```

So what did i do here?

I limited values of `environmentName` parameter to 3 options. If in the future i add mroe environments then i would need to update the list, but for now `dev`, `test` and `prod` values will do.

2. Saved changes

## Limiting numeric values

1. In the main file i went to `appServicePlanInstanceCount` parameter and inserted `@minValue` and `@maxValue` decorator underneath its `@description` decorator with the following values:

```Bicep
@minValue (1)
@maxValue (10)
```

So what did i do here?

I added limits to instances, in this particular case the minimum instances that can be had is at least 1, maximum value of instances can be 10 and not more. This guarantees that at least 1 instance will be made and if in the future more instances need to be had then the current limit will allow for 10 instances total. This can be changed any time by editing these values.

2. Saved changes

## Veryfied Bicep file:

<img width="797" height="960" alt="image" src="https://github.com/user-attachments/assets/9d953ce8-d15c-4c9f-b818-bafbd894304d" />

Success

## Deploying Bicep

1. Went to portal and quickly created resource group titled `twistedwonka`

<img width="600" height="32" alt="image" src="https://github.com/user-attachments/assets/da20b79c-7d2e-4c43-a800-911c7834bbab" />

2. Deployed Bicep
```PWSL
New-AzResourceGroupDeployment -Name main -TemplateFile new.bicep
```

<img width="819" height="349" alt="image" src="https://github.com/user-attachments/assets/3c964cb9-26e8-4966-9368-0a5902dbc336" />

Success

3. Went to portal for details

<img width="611" height="39" alt="image" src="https://github.com/user-attachments/assets/f84cccca-0e03-4218-af8a-85ad621d6777" />

4. Selected `main` to see what was deployed

<img width="617" height="164" alt="image" src="https://github.com/user-attachments/assets/fab064e4-afe4-47f8-b244-7e2fa7c74389" />

I could see that website and serverfarm both were deployed.

5. Opened up `inputs` tab from left pane to see parameters and values listed

<img width="609" height="337" alt="image" src="https://github.com/user-attachments/assets/d7d6248b-36f1-4430-9a01-281d968d4ce1" />

Success

