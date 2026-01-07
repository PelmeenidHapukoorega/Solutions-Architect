# Bicep Advanced Looping Control & Nesting

## Controlling Loop Execution (@batchSize)
By default, Azure Resource Manager executes loops in **parallel** and in a **non-deterministic** order to minimize deployment time. However, specific scenarios (e.g., preventing simultaneous restarts of all App Services) require sequential or batched execution.

### The `@batchSize` Decorator
This decorator controls the number of resources deployed concurrently within a loop. Bicep waits for a batch to complete entirely before starting the next batch.

**Syntax:**
```bicep
@batchSize(<number>)
resource symbolicName 'Microsoft.Provider/type' = [for i in range(0,10): { ... }]
```

**Execution Modes**

* Parallel (Default) - No decorator - All resources deploy simultaneously.
* Batched - `@batchsize(2)` - Deploys 2 resources. Waits for completion. Deploys next 2.
* Sequential - `@batchSize(1)` - Deploys 1 resource at a time. Purely serial execution.

## Loops within Resource Properties

Loops are not limited to creating top-level resources. They can also be used to generate arrays within a resource's properties. A common use case is defining multiple Subnets within a single Virtual Network resource.

**Syntax:**

Place the `for` loop inside the property array definition.
```Bicep
param subnetNames array = ['api', 'worker']

resource virtualNetwork 'Microsoft.Network/virtualNetworks@2024-05-01' = {
  name: 'example-vnet'
  properties: {
    // Loop runs inside the 'subnets' property
    subnets: [for (subnetName, i) in subnetNames: {
      name: subnetName
      properties: {
        // Use index 'i' to create unique CIDR blocks
        addressPrefix: '10.0.${i}.0/24' 
      }
    }]
  }
}
```

## Nested loops

Bicep supports loops inside other loops. This is used for complex deployments, such as deploying Multiple VNETs (Outer Loop), where each VNET contains Multiple Subnets (Inner Loop).

**Logic and scope**

* **Outer Loop:** Iterates through locations to create VNETs.
* **Inner Loop:** Iterates through a count/array to create subnets inside those VNETs.
* **Variable Access:** The inner loop can access variables (like the index i) from the outer loop.

**Example implementation**

**Scenario:** Deploy a VNET to 3 different regions. Inside each VNET, create 2 subnets.
```Bicep
param locations array = ['westeurope', 'eastus2', 'eastasia']
var subnetCount = 2

// Outer Loop: Iterates through Locations (index 'i')
resource virtualNetworks 'Microsoft.Network/virtualNetworks@2024-05-01' = [for (location, i) in locations : {
  name: 'vnet-${location}'
  location: location
  properties: {
    addressSpace:{
      addressPrefixes:[
        '10.${i}.0.0/16' // Uses Outer Loop index 'i'
      ]
    }
    // Inner Loop: Iterates through Subnet Count (index 'j')
    subnets: [for j in range(1, subnetCount): {
      name: 'subnet-${j}'
      properties: {
        // Uses BOTH 'i' (Outer) and 'j' (Inner) to ensure uniqueness
        addressPrefix: '10.${i}.${j}.0/24'
      }
    }]
  }
}]
```

### Resulting architecture

| VNET Name | Location | Address Space | Subnets Generated |
| :--- | :--- | :--- | :--- |
| `vnet-westeurope` | westeurope | `10.0.0.0/16` | `10.0.1.0/24`, `10.0.2.0/24` |
| `vnet-eastus2` | eastus2 | `10.1.0.0/16` | `10.1.1.0/24`, `10.1.2.0/24` |
| `vnet-eastasia` | eastasia | `10.2.0.0/16` | `10.2.1.0/24`, `10.2.2.0/24` |
