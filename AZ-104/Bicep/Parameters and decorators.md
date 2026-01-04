# Bicep Parameters & Decorators

## Flexible Deployments
**Goal:** Use **Parameters** to input values at deployment time (e.g., Environment Names, SKUs, Locations) instead of hardcoding them. This makes templates reusable across different environments.

**Key Syntax:**
```bicep
param parameterName type = defaultValue
```

## Parameter types

* `string` - text values, examples: `eastus`,`vnet-01`
* `int` - whole numbers, examples: `1`,`5`,`10`
* `bool` - True/False flags, examples: `true`,`false`
* `object` - Structured data (key value pairs), see below:
```Bicep
param appServicePlanSku object = {
  name: 'F1'
  tier: 'Free'
  capacity: 1
}

// Usage in Resource:
sku: {
  name: appServicePlanSku.name
  tier: appServicePlanSku.tier
}
```
* `array` - A list of items, see below:
```Bicep
param cosmosLocations array = [
  { locationName: 'eastus' }
  { locationName: 'westeurope' }
]
```

## Decorators (Validation and Metadata)

Decorators attach rules to parameters. They enforce constraints before deployment starts.

**Resticting Values (`@allowed`)**

Forces the user to pick from a specific list. Great for preventing typos or enforcing specific SKUs.
```Bicep
@allowed([
  'prod'
  'dev'
])
param environmentType string
```

**Length limits (`@minLength`, `@maxLength`)

Used for strings (character count) or arrays (item count). Essential for resources with naming limits (e.g., Storage Accounts: 3-24 chars).
```Bicep
@minLength(5)
@maxLength(24)
param storageName string
```

**Numeric Limits (`@minValue`, `@maxValue`)

Used for integers, typically to control scale (e.g., Instance Count).
```Bicep
@minValue(1)
@maxValue(10)
param instanceCount int
```

**Documentation (`@description`)

Adds human readable help text. This text appears in the Azure Portal if you deploy the template via the GUI.
```Bicep
@description('The name of the environment. Must be dev or prod.')
param envName string
```

## Default Values

Assigning a value makes the parameter optional. If the user doesn't provide one, the default is used.

**Static Default:**

```Bicep
param location string = 'eastus'
```

**Dynamic Default (Expression):**

```Bicep
// Defaults to the location of the Resource Group being deployed to
param location string = resourceGroup().location
```

## Best practices

* **Naming:** Use clear, descriptive names (e.g., `storageAccountName` vs `stg`).
* **Defaults:** Use safe, low-cost defaults (e.g., Free Tier) to prevent accidental charges during testing.
* **Secrets:** Never use default values for passwords or secrets.
