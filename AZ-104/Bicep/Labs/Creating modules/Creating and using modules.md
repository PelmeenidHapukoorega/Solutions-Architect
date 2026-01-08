# Lab 4: Creating and using modules

1. Created new file `lab4.bicep` in VSC
2. Created new folder `lab4modules`
3. Created new module file in the folder and named it `app.bicep`
4. Added following parameters to `app.bicep`
```Bicep
@description('Azure region for resources')
param location string

@description('Name for app service app')
param appServiceAppName string

@description('Name of the app service plan')
param appServicePlanName string

@description('Name of the app service plan SKU')
param appServicePlanSkuName string

resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' ={
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
    httpsonly: true
  }
}

@description('Default host name of the app service app')
output appServiceAppHostName string = appServiceApp.properties.defaultHostName
```

So what did i do here?

Added parameters for names, locations where resources are deployed and `appServicePlan` and `appServiceApp` resources. Additionally added output for default host name for the app service app.

With this module was done.

5. Added the module app to Bicep template, opened `lab4.bicep` and added parameters for deployment.
```Bicep
module app 'lab4modules/app.bicep' = {
    name: 'toy-launch-app'
    params: {
        appServiceAppName: appServiceAppName
        appServicePlanName: appServicePlanName
        appServicePlanSkuName: appServicePlanSkuName
        location: location
    }
}
```

6. Added output for host name to access the website
```Bicep
@description('The host name to use to access the website.')
output websiteHostName string = app.outputs.appServiceAppHostName
```

7. Saved changes.
8. Created a module for content delivery network, so added new file to `lab4modules` and named it `cdn.bicep`
9. Added parameters, necessary resources and outout for endpoint name.
```Bicep
@description('Host name (address) of origin server')
param originHostName string

@description('name of the CDN profile')
param profileName string = 'cdn-${uniqueString(resourceGroup().id)}'

@description('Name of CDN endpoint')
param endPointName string = 'endpoint-${uniqueString(resourceGroup().id)}'

@description('indicates whether CDN endpoint requires HTTPS connections.')
param httpsOnly bool

var originName = 'my-origin'

resource cdnProfile 'Microsoft.Cdn/profiles@2025-09-01-preview' = {
  name: profileName
  location: 'global'
  sku: {
    name: 'Standard_Microsoft'
  }
}

resource endpoint 'Microsoft.Cdn/profiles/endpoints@2025-09-01-preview' = {
  parent: cdnProfile
  name: endPointName
  location: 'global'
  properties: {
    originHostHeader: originHostName
    isHttpAllowed: !httpsOnly
    isHttpsAllowed: true
    queryStringCachingBehavior: 'IgnoreQueryString'
    contentTypesToCompress: [
      'text/plain'
      'text/html'
      'text/css'
      'application/x-javascript'
      'text/javascript'
    ]
    isCompressionEnabled: true
    origins: [
      {
        name: originName
        properties: {
          hostName: originHostName
        }
      }
    ]
  }
}

@description('Host name of CDN endpoint.')
output endpointHostName string = endpoint.properties.hostName
```

So what did i do here? 

**Setting the rules with parameters**

* `param originHostName`: This is where i defined the address of the actual source server where my files are stored.
* `param profileName`:Created a unique name for the CDN container by mixing the "cdn" prefix with a unique string based on the resource group ID.
* `param endPointName`: Set up a unique public address for the CDN endpoint so users can actually access the cached content.
* `param httpsOnly`: Set up a simple true/false switch i used to decide if I want to force everyone to use a secure connection.

**Internal logic aka variables**

* `var originName`: Gave it a simple internal label to the source server so Bicep can keep track of it.

**Building the CDN stack**

* `resource cdnProfile`: Told Azure to spin up the main CDN "parent" container using the standard Microsoft tier.
* `location: 'global'`: Since CDNs work across the whole world, i had to set the location to global instead of a specific region.
* `resource endpoint`: Built the actual "doorway" that users hit to get the data from the CDN.
* `parent: cdnProfile`: Linked this specific endpoint to the main CDN profile I just created.
* `originHostHeader`: Ensured the CDN sends the correct header to the source server so it knows which site is being requested.
* `isHttpAllowed: !httpsOnly`: Used logic here to say "if HTTPS is NOT required, then regular HTTP is allowed."
* `queryStringCachingBehavior`: Told the CDN to ignore any extra bits at the end of the URL so it would cache the same version of the file for everyone.
* `contentTypesToCompress`: Listed file types, like CSS and JavaScript, that i wanted the CDN to "shrink" to make them load faster.
* `isCompressionEnabled: true`: Master switch that actually turns on that file shrinking feature.
* `origins: [...]`: This block told the CDN exactly which source server it needs to pull the original files from.

**Output**

* `output endpointHostName`: Once everything would be built, Bicep would spit out the final public URL of the CDN so i could use it in my application.

10. Added whether CDN should be deployed parameter to `lab4.bicep`
```Bicep
@description('Indicates whether a CDN should be deployed.')
param deployCdn bool = true
```

11. Defined cdn module in `lab4.bicep` file
```Bicep
module cdn 'lab4modules/cdn.bicep' = if (deployCdn) {
    name: 'to-launch-cdn'
    params: {
        httpsOnly: true
        originHostName: app.outputs.appServiceAppHostName
    }
}
```

12. Updated host name output with CDN
```Bicep
@description('Host name used to access website')
output WebSiteHostName string = deployCdn ? cdn.outputs.endpointHostName : app.outputs.appServiceAppHostName
```

13. Verified files

`lab4.bicep`

```Bicep
@description('Azure region for resource deployment')
param location string = 'westus3'

@description('Name of the app service app')
param appServiceAppName string = 'toy-${uniqueString(resourceGroup().id)}'

@description('Name of the app service plan SKU')
param appServicePlanSkuName string = 'F1'

@description('Indicates whether CDN should be deployed')
param deployCdn bool = true

var appServicePlanName = 'toy-product-launch-plan'

module app 'lab4modules/app.bicep' = {
    name: 'toy-launch-app'
    params: {
        appServiceAppName: appServiceAppName
        appServicePlanName: appServicePlanName
        appServicePlanSkuName: appServicePlanSkuName
        location: location
    }
}

module cdn 'lab4modules/cdn.bicep' = if (deployCdn) {
    name: 'to-launch-cdn'
    params: {
        httpsOnly: true
        originHostName: app.outputs.appServiceAppHostName
    }
}


@description('Host name used to access website')
output WebSiteHostName string = deployCdn ? cdn.outputs.endpointHostName : app.outputs.appServiceAppHostName
```

14. Created new RG group in portal `toyfactory`
15. Deployed template `lab4.bicep`
```PWSL
New-AzResourceGroupDeployment `
-Name main `
-TemplateFile lab4.bicep
```

### Troubleshooting

Ran into error:

<img width="700" height="43" alt="image" src="https://github.com/user-attachments/assets/334ccbc1-9b8e-406c-9dab-47e8136f1408" />

After googling i understood that this told me that SKU itself is dead since Microsoft had retired it, to fix this i needed to pivot to newer Azure front door or standard azure cdn tiers.

Updated `cdn.bicep` by adding `Standard_AzureFrontDoor` under `name:` and changed `cdnProfile` resource to `Microsoft.Cdn/profiles@2024-02-01`.

```Bicep
resource cdnProfile 'Microsoft.Cdn/profiles@2024-02-01-preview' = {
  name: profileName
  location: 'global'
  sku: {
    name: 'Standard_AzureFrontDoor'
  }
}
```

Tried again and was met with the same wall, i tried every classic SKU in order to keep my architecture intact before doing architectural pivot to Azure front door. Tried Microsoft, akamai and verizon, still the same wall. So after intense forum reading and clueing myself in with Azure front door i wrote the following:

```Bicep
@description('Host name (address) of origin server')
param originHostName string

@description('name of the CDN profile')
param profileName string = 'cdn-${uniqueString(resourceGroup().id)}'

@description('Name of CDN endpoint')
param endPointName string = 'endpoint-${uniqueString(resourceGroup().id)}'

@description('indicates whether CDN endpoint requires HTTPS connections.')
param httpsOnly bool

var originName = 'my-origin'

resource cdnProfile 'Microsoft.Cdn/profiles@2024-02-01' = {
  name: profileName
  location: 'global'
  sku: {
    name: 'Standard_AzureFrontDoor'
  }
}

resource endpoint 'Microsoft.Cdn/profiles/afdEndpoints@2024-02-01' = {
  parent: cdnProfile
  name: endPointName
  location: 'global'
  properties: {
    enabledState: 'Enabled'
  }
}

@description('App service address')
resource originGroup 'Microsoft.Cdn/profiles/originGroups@2024-02-01' = {
  parent: cdnProfile
  name: 'default-origin-group'
  properties: {
    loadBalancingSettings: {
      sampleSize: 4
      successfulSamplesRequired: 3
    }
    healthProbeSettings: {
      probePath: '/'
      probeRequestType: 'GET'
      probeProtocol: 'Http'
      probeIntervalInSeconds: 100
    }
  }
}

@description('Connecting endpoint to origin group')
resource origin 'Microsoft.Cdn/profiles/originGroups/origins@2024-02-01' = {
  parent: originGroup
  name: 'app-service-origin'
  properties: {
    hostName: originHostName
    httpPort: 80
    httpsPort: 443
    originHostHeader: originHostName
    priority: 1
    weight: 1000
  }
}

@description('Connects the endpoint to origin group')
resource route 'Microsoft.Cdn/profiles/afdEndpoints/routes@2024-02-01' = {
  parent: endpoint
  name: 'default-route'
  dependsOn: [
    origin
  ]
  properties: {
    originGroup: {
      id: originGroup.id
    }
    supportedProtocols: [
      'Http'
      'Https'
    ]
    patternsToMatch: [
      '/*'
    ]
    forwardingProtocol: 'MatchRequest'
    linkToDefaultDomain: 'Enabled'
    httpsRedirect: httpsOnly ? 'Enabled' : 'Disabled'
  }
}

@description('Host name of CDN endpoint.')
output endpointHostName string = endpoint.properties.hostName
```

**Error 2**

This is the correct code but before hitting this milestone i ran into Error2: `PreFlightUnsupportedApiVersion` when i tried to use version `2025-09-01-preview`.

This meant that i was trying to use a future date API that the global location didnt support yet. Luckily the error gave me a list of supported versions so i picked `2024-02-01` from the list and removed `-preview` tag to keep it stable.

**Error 3**

Tried to deploy again and ran into Error3: `Invalid Template/SKU mismatch`, i had changed the SKU to `Standard_AzureFrontDoor` but the code was still failing validation.

Realized that Front Door is a completely different building block. I was trying to use the `Microsoft.Cdn/profiles/endpoints` resource type which is a classic, inside a modern Front Door profile. Apparently they dont mix which in hindsight is logical. So i had to stop trying to force the old code to work and completely rewrite `cdn.bicep` module to use Front Door resource types such as: `afdEndpoints`, `originGroups` and `origins` which i pasted above.

**Error 4**

Got a `BadRequest` saying that "Make sure originGroup is created successfully" even though i had the code right there.

Turned out that Azure was trying to built the route before the origin group was finished. So i added `dependsOn: [ origin ]` to the route resource which forced Azure to wait until the origin was fully built before trying to connect the pipes.

**Final pivot**

In the end i moved from simple classic CDN setup to full Azure front door architecture because im not just giving up when i hit a wall, i pivot and look for alternatives until the code works.

So basically:

* Old way: 1 profile > 1 endpoint (failed/retired)
* New way: 1 profile > 1 front door endpoint > 1 origin group > 1 origin > 1 route

This solved all SKU retirement issues and successfully deployed the global network. The good part about it was that i didnt have to make edits to `lab4.bicep` or `app.bicep` just the `cdn.bicep`. 






