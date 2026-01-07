# Variable and output loops

# Bicep Variable and Output Loops

## Variable Loops
**Purpose:** Create complex arrays or objects dynamically within the Bicep file. This is often used to transform simple, user-friendly parameters into the complex structure required by Azure resource definitions.

### Syntax
Use the `for` keyword inside a variable definition.
```bicep
var items = [for i in range(1, 5): 'item${i}']
// Result: ['item1', 'item2', 'item3', 'item4', 'item5']
```

**Use Case: Data Transformation**

Instead of asking the user to provide the exact JSON structure Azure requires, you can ask for a simple list and "map" it using a variable loop.

**Example: Subnet Configuration**
```Bicep
// 1. Simple Parameter (User Input)
param subnets array = [
  { name: 'frontend', ip: '10.10.0.0/24' }
  { name: 'backend', ip: '10.10.1.0/24' }
]

// 2. Variable Loop (Transformation Logic)
var subnetsProperty = [for subnet in subnets: {
  name: subnet.name
  properties: {
    addressPrefix: subnet.ip
  }
}]

// 3. Resource Definition (Usage)
resource vnet 'Microsoft.Network/virtualNetworks@2024-05-01' = {
  // ...
  properties: {
    subnets: subnetsProperty // Uses the transformed variable
  }
}
```

## Output loops

**Purpose:** Return information (like endpoints or connection strings) for multiple resources created via a loop.

**Syntax**

Use the `for` keyword inside an `output` definition.
```Bicep
output outputItems array = [for i in range(0, length(items)): items[i]]
```

**The "Indexer" Constraint**

Currently, Bicep does not support directly referencing the "iterator" of a resource loop inside an output loop. You must use Array Indexers combined with the `range()` and `length()` functions.

**Scenario:** You created multiple storage accounts using a loop. You need to output the Blob Endpoint for each of them.
```Bicep
// Assume 'locations' is an array parameter and 'storageAccounts' is a looped resource

output storageEndpoints array = [for i in range(0, length(locations)): {
  name: storageAccounts[i].name
  location: storageAccounts[i].location
  blobEndpoint: storageAccounts[i].properties.primaryEndpoints.blob
}]
```

* **Logic:** The loop runs from 0 to the length of the input array. It accesses the resource properties using the index `[i]`.

## Security warning

**Constraint:** Never use output loops (or any outputs) to return secrets, access keys, or passwords.

* **Reason:** Deployment outputs are logged in plain text in the Azure Portal deployment history and can be read by anyone with access to the Resource Group.
