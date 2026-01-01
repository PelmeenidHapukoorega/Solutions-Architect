# Adding flexibility by using parameters and variables.

Here I have marked down need to knows about both parameters and variables, which are 2 Bicep features that can make Bicep files flexible and reusable as well as introduction to expressions.

## Parameters and Variables

### Parameters

A parameter lets you brind in values from outside the Bicep file. 

For example if you are manually deploying the file by using the Azure CLI or Azure PowerShell, you will be asked to provide values for each parameter.

You can also create a "parameter file" which lists all of the parameters and values you want to use for the deployment. If the Bicep file is deployed from an automated process like a deployment pipeline, the pipeline can provide the parameter values.

Its usually a good idea to use parameters for things that will change between each deployment like:

* Resource names that need to be unique
* Locations into which to deploy the resources
* Settings that affect the pricing of resources, like their SKUs, pricing tiers and instance counts.
* Credentials and information needed to access other systems that arent defined in the Bicep file.

### Variables

A variable is defined and set within the Bicep file. Variables let you store important information in 1 place and refer to it throughout the file without having to copy paste it.

Variables are a good option when:

* You use the same values for each deployment, but you want to make a value reusable within the file.
* When you want to use expressions to create a complex value.
* Use variables for resources that dont need unique names.

**Note!**

Its important to use good naming for parameters and variables so Bicep files are easy to read ad understand. Make sure to use clear, descriptive and consistent names.

## Adding a parameter

In Bicep a parameter is defined like this:

```Bicep
param appServiceAppName string
```

* `param` - Tells Bicep that you are defining a parameter.
* `appServiceAppName` - name of the parameter.
  * If deploying Bicep file manually you might be asked to enter a value, so its important that the name is clear and understandable.
  * Name is also how you refer to the parameter value within the file, just like with resource symbolic names.
  * `String` - A type of parameter.
    * You can specify several different types for Bicep parameters:
      * `string` - for text.
      * `int` - for numbers.
      * `bool` - for Boolean true or false values.
    * You can also pass in more complex parameters by using the `array` and `object` types.

**Note!**

Try not to overgeneralize Bicep files by using too many parameters. Use the minimum number of parameters you need for business scenario. You can always change Bicep files in the future if requirements change.

## Provide default values 

Optionally you can provide a "default value" for a parameter. When specifying a default value, the parameter becomes optional. Person who is deploying Bicep file can specify a value if they want, if they dont Bicep will use default value.

Here is how to add a default value:
```
parama appServiceAppName string = 'toy-product-launch-1'
```

**Note!**

In this example the Azure App Service app name has a hard-coded default value. This isnt a good idea because App Service apps need unique names.

## Using parameter values in Bicep file

When you have declared a parameter, you can refer to it throughout the rest of the Bicep file. 

Here is how you can use the new parameter within the resource definition:
```Bicep
resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: appServiceAppName
  location: 'eastus'
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

Notice that Bicep file now uses parameter value to set the resource name for the app resource instead of a hard coded value.

**Note!**

Bicep extension for VSC shows you visual indicators to let you know when you are not following recommended practices. It warns you if you define a parameter that you dont use. Bicep linter continously runs these checks while you work.

## Adding a variable

You can define variable like this:
```Bicep
var appServicePlanName = 'toy-product-launch-plan'
```

Variables are defined similarly to parameters, but there are a few differences:

* Use the `var` keyword to tell Bicep you are declaring a variable.
* You must provide a value for a variable.
* Variables dont need types. Bicep can determine the type based on the value you set.

## Expressions

* **Dynamic Discovery**
  * Expressions allow you to discover values at runtime (fetching the current region of a resource group) rather than hard-coding them.
* **Resource Naming**
  * You can use expressions to automatically generate unique resource names that follow company naming standards (using `uniqueString()`).
* **Regional consistency**
  * They ensure all resources in a file deploy to the same Azure region by referencing the deployment target's metadata.

### Resource locations

When writing and deploying a template, you dont want to have to specify location of every resource individually. Instead you might have a simple business rule that says "by default, deploy all resources into the same location in which the resource group was created".

In Bicep you can create a parameter called `location`, the use expression to set its value:
```Bicep
param location string = resourceGroup().location
```

Looking at the default value of that parameter we can see that it uses "function" called `resourceGroup()` which gives access to information about the resource group into which the Bicep file is being deployed.

In this example the file ueses the `location` property. Its a common way to deploy resources into same Azure region as the resource group.

If someone is deploying this Bicep file, the might choose to override the default value here and use a different location instead.

**Note**

Some resources in Azure can be deployed only into certain locations. You might need to seperate parameters to set the locations of these resources.

Now we can use the resource location parameter inside Bicep file like this:
```Bicep
resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: appServiceAppName
  location: location
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```

### Resource names

Many Azure resources need unique names. Bicep has another function called `uniqueString()` that comes in handy when you're creating resource names.

When you use this function, you need to provide a seed value, which should be different across different deployments, but consistent across all deployments for the same resources.

If you choose a good seed value, you can get the same name every time you deploy the same set of resources, but you'll get a different name whenever you deploy a different set of resources by using the same Bicep file.

Example of `uniqueString()` function:
```Bicep
param storageAccountName string = uniqueString(resourceGroup().id)
```
This parameters default value uses the `resourceGroup()` function again, like we did when we set the resource location.

Now lets try getting ID for resource group, here is what it looks like:
```
/subscriptions/aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e/resourceGroups/MyResourceGroup
```

The resource group ID includes the Azure subscription ID (`aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e`) and the resource group name (`MyResourceGroup`). The resource group ID is often a good candidate for a seed value for resource names because:

* Every time you deploy the same resources, they'll go into the same resource group. The `uniqueString()` function will return the same value every time.
* If you deploy into two different resource groups in the Azure subscription, the `resourceGroup().id` value will be different because the resource group names will be different. The `uniqueString()` function will give different values for each set of resources.
* If you deploy into two different Azure subscriptions, even if you use the same resource group name, the `resourceGroup().id` value will be different because the Azure subscription ID will be different. The `uniqueString()` function will again give different values for each set of resources.

**Note!**

Its often a good idea to use Bicep file expressions to create resource names. Many Azure resource types have rules about the allowed characters and length of their names. Embedding the creation of resource names in the Bicep file means that anyone who uses the file doesn't have to remember to follow these rules themselves.

### Combined strings

If you just use the `uniqueString()` function to set resource names, you will probably get unique names, but they wont be meaningful. A good resource name should also be descriptive, so that its clear what the resource is for. 

You will want to create a name by combining a meaningful word or string with a unique value. This way, you will have resources that have both meaningful and unique names.

Bicep has a feature called "string interpolation" that lest you combine strings.

This is how it works:
```Bicep
param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'
```

The default value for the `storageAccountName` parameter now has 2 parts to it:

* `toylaunch` is a hard-coded string that helps anyone who looks at the deployed resource in Azure to understand the storage account's purpose.
* `${uniqueString(resourceGroup().id)}` is a way of telling Bicep to evaluate the output of the `uniqueString(resourceGroup().id)` function, then concatenate it into the string.

**Note**

Sometimes the `uniqueString()` function will create strings that start with a number. Some Azure resources, like storage accounts, dont allow their names to start with numbers. This means its a good idea to use string interpolation to create resource names, like in the preceding example.

### Selecting SKUs for resources

Business Requirements

To manage costs effectively while ensuring performance where needed, resources are deployed with different configurations based on the environment type:

*   **Production:** High resiliency and performance.
    *   Storage: `Standard_GRS` (Geo-redundant).
    *   App Service Plan: `P2v3` (Premium V3).
*   **Non-Production:** Cost-effective for testing.
    *   Storage: `Standard_LRS` (Locally redundant).
    *   App Service Plan: `F1` (Free tier).

**Implementation Strategy**

Instead of specifying every SKU as a separate parameter (which becomes difficult to manage), business rules are embedded directly into the Bicep file using a combination of **parameters**, **variables**, and **expressions**.

1. **Defining the Parameter**

First, specify a parameter to indicate the target environment. The `@allowed` decorator ensures only valid values are accepted.

```bicep
@allowed([
  'nonprod'
  'prod'
])
param environmentType string
```

* **Validation:** Bicep prevents deployment if the provided value is not in the allowed list.

2. **Defining Logic with Variables**

Variables are used to determine the specific SKUs based on the `environmentType` parameter.
```Bicep
var storageAccountSkuName = (environmentType == 'prod') ? 'Standard_GRS' : 'Standard_LRS'
var appServicePlanSkuName = (environmentType == 'prod') ? 'P2V3' : 'F1'
```

### Syntax: The Ternary Operator (?:)

The logic uses the ternary operator to evaluate an if/then statement.

**Format**

`(Expression) ? [Value if True] : [Value if False]`

**Breakdown:**

1. environmentType == 'prod'): Evaluates to a Boolean (true or false).
2. ?: The operator checks the result.
3. Value if True: If the expression is true, the value immediately following the ? is used.
4. Value if False: If the expression is false, the value following the colon (:) is used.

**Application**

* **Storage:** If `prod`, use `Standard_GRS`. Otherwise, use `Standard_LRS`.
* **App Service:** If `prod`, use `P2V3`. Otherwise, use `F1`.

### Best practices

* **Use Variables for Logic:** It is best practice to define multipart expressions inside var blocks rather than embedding them directly into resource properties.
* **Readability:** This approach prevents cluttering resource definitions with complex logic, making the Bicep file easier to read and maintain.
* **Reusability:** The file can be reused for any deployment by simply supplying the correct parameter value (prod or nonprod).


