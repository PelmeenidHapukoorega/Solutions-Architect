# Bicep Module Contracts (Inputs and Outputs)

## The Module Contract
**Goal:** Treat modules as "Black Boxes" with a strict contract. The Parent template shouldn't care *how* the module works, only what it needs (Parameters) and what it gives back (Outputs).

*   **Inputs:** Parameters (Configuration, SKUs, Names).
*   **Process:** Resource creation & logic.
*   **Outputs:** Data returned to the parent (Resource IDs, Endpoints).

## Module Parameters (Inputs)
Designing parameters for modules requires different thinking than standalone templates.

### Best Practices
1.  **Defaults:** Avoid default values in modules if possible.
    *   *Why?* If Parent has a default and Module has a different default, it causes confusion. Let the **Parent** control the default value.
2.  **Business Logic (Generic vs. Specific):**
    *   *Generic Module (Reusable):* Do **not** hardcode business rules (e.g., "Prod always equals GRS"). Pass the SKU as a parameter so the module remains flexible for any project.
    *   *Organization Module (Specific):* If the module is strictly for internal use, you **can** hardcode rules to simplify the parent template.
3.  **Documentation:** Always use `@description`. This is the "manual" for the person consuming your module.

```bicep
@description('The name of the storage account to deploy.')
param storageAccountName string
```

## Conditional logic in modules

You can make resources inside a module optional by combining default empty parameters with `if` conditions.

**Scenario:** Only deploy Diagnostic Settings if a Log Analytics Workspace ID is provided.
```Bicep
// 1. Default to empty string (Optional Parameter)
param logAnalyticsWorkspaceId string = ''

resource cosmosDBAccount 'Microsoft.DocumentDB/databaseAccounts@2022-08-15' = {
  // ... account definition
}

// 2. Check if the parameter is NOT empty
resource cosmosDBAccountDiagnostics 'Microsoft.Insights/diagnosticSettings@2021-05-01-preview' =  if (logAnalyticsWorkspaceId != '') {
  scope: cosmosDBAccount
  name: 'route-logs-to-log-analytics'
  properties: {
    workspaceId: logAnalyticsWorkspaceId // Only runs if ID was provided
    // ...
  }
}
```

## Module outputs

Outputs allow the Parent template to retrieve properties of resources created inside the module.

**Syntax**
```Bicep
@description('The Resource ID of the blob container.')
output blobContainerResourceId string = storageAccount::blobService::container.id
```

**Security warning**

* **The Rule:** NEVER use outputs for secrets (Passwords, Keys, Connection Strings).
* **The Risk:** Outputs are stored in clear text in the Azure Deployment History.
* **The Workaround:**
  1. Output the Resource Name instead. The parent can then use the `existing` keyword to look up the resource and retrieve the secret securely.
  2. Have the module write the secret directly to a Key Vault.

## Chaining modules

You can connect modules together by passing the Output of Module A into the Parameter of Module B.

**Scenario:** Create a VNET in Module 1, pass the Subnet ID to a VM in Module 2.
```Bicep
// Module 1: Network
module virtualNetwork 'modules/vnet.bicep' = {
  name: 'virtual-network'
}

// Module 2: Compute
module virtualMachine 'modules/vm.bicep' = {
  name: 'virtual-machine'
  params: {
    adminUsername: adminUsername
    // Chaining: Accessing the output of the previous module
    subnetResourceId: virtualNetwork.outputs.subnetResourceId
  }
}
```

### Dependency management

* **Implicit Dependency:** Bicep automatically detects that `virtualMachine` depends on `virtualNetwork`.
* **Execution Order:** Bicep will wait for `virtualNetwork` to finish completely before starting `virtualMachine`.
