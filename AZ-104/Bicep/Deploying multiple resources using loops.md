# Bicep Resource Iteration (Loops)

## Dynamic Scaling
**Purpose:** Deploy multiple instances of similar resources (e.g., SQL Servers in different regions) without duplicating code blocks.

**Mechanism:** Use **Copy Loops** to dynamically set the number of instances or customize properties based on input arrays.

## Basic Iteration (Looping over Arrays)
The most common pattern is iterating through an array of objects or strings (passed as parameters).

### Syntax
*   Enclose the resource definition in square brackets `[]`.
*   Use the `for` keyword: `for <variable> in <array>`.

```Bicep
param storageAccountNames array = [
  'saauditus'
  'saauditeurope'
  'saauditapac'
]

resource storageAccountResources 'Microsoft.Storage/storageAccounts@2023-05-01' = [for storageAccountName in storageAccountNames: {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}]
```

**Logic:** Bicep creates one resource per item in the array, using `storageAccountName` as the specific value for that iteration.

## Numeric Iteration (range() function)

Used when you need a specific quantity of resources (e.g., "Create 4 servers") rather than a named list.

**Syntax**

`range(startValue, count)`

* **Start Value:** The number to begin with.
* **Count:** The total number of items to generate (not the ending index).

```Bicep
// Creates 4 accounts: sa1, sa2, sa3, sa4
resource storageResources 'Microsoft.Storage/storageAccounts@2023-05-01' = [for i in range(1,4): {
  name: 'sa${i}' // String interpolation using the index
  location: resourceGroup().location
  // ...
}]
```

* **Warning:** `range(3,4)` results in `3, 4, 5, 6` (Start at 3, generate 4 items).

## Accessing Index

If you are iterating over an array but also need a number (e.g., to number your servers `sqlserver-1`, `sqlserver-2`), you can retrieve the current index.

**Syntax**

`for (item, index) in <array>`

```Bicep
param locations array = [
  'westeurope'
  'eastus2'
]

resource sqlServers 'Microsoft.Sql/servers@2024-05-01-preview' = [for (location, i) in locations: {
  // i starts at 0, so we use ${i+1} to start naming at 1
  name: 'sqlserver-${i+1}'
  location: location
  properties: { ... }
}]
```

* **Convention:** `i` is the standard variable name for the index, but any name can be used.

## Filtering (Loops + Conditions)

You can filter which items in a loop actually get deployed by combining `for` with `if`.

**Syntax**

`[for <variable> in <array>: if (<condition>) { ... }]`

**Example**

Deploy SQL servers to a list of locations, but only if the configuration object is marked as `'Production'`.

```Bicep
param sqlServerDetails array = [
  { name: 'sql-we', environmentName: 'Production' }
  { name: 'sql-eus', environmentName: 'Development' } // Skipped
]

resource sqlServers 'Microsoft.Sql/servers@2024-05-01-preview' = [for sqlServer in sqlServerDetails: if (sqlServer.environmentName == 'Production') {
  name: sqlServer.name
  location: sqlServer.location
  // ...
}]
```

**Result:** Only `sql-we` is deployed. The 'Development' entry is ignored by the engine.
