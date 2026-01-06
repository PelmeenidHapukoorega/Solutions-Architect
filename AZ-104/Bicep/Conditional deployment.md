# Bicep Conditional Deployment

## Infrastructure Logic
Purpose: To use a single Bicep template for all environments (Dev, Prod), but only deploy specific resources (like Auditing or Premium Storage) when specific criteria are met.

**Key Syntax:**
The `if` keyword placed immediately after the resource assignment.
```Bicep
resource symbolicName 'Microsoft.Example/type' = if (condition) {
  ...
}
```

## Implementation strategies

**Strategy: Direct boolean parameter**

The simplest method. The user explicitly says "True" or "False".
```Bicep
param deployStorageAccount bool

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = if (deployStorageAccount) {
  name: 'teddybearstorage'
  // ...
}
```

**Strategy: Logical expressions (variables)**

Instead of asking the user "Deploy Auditing?", you ask "Is this Prod?" and let the code decide.
Best Practice: Define the logic in a `var` for readability.
```Bicep
param environmentName string

// Logic: specific boolean check
var auditingEnabled = environmentName == 'Production'

resource auditingSettings 'Microsoft.Sql/servers/auditingSettings@2024-05-01-preview' = if (auditingEnabled) {
  // This resource only deploys if auditingEnabled is true
  name: 'default'
}
```

## The dependency trap (ResourceNotFound error)

**Scenario:**
You have two resources that are both conditional:

1. **Storage account** (Only created in Prod).
2. **SQL auditing** (Only created in Prod, and depends on the Storage Account keys).

**The problem:**

Even if you set the condition to `False` (Dev environment), the deployment might fail with ResourceNotFound.

* **Why?**
  * Azure Resource Manager evaluates the properties inside the resource before it evaluates the `if` condition on the resource itself. It tries to fetch the Storage Keys, cant find the storage (because it's not being created), and throws an error.

**The fix: Ternary checks in properties**

You must use the ternary operator (`? :`) inside the properties to provide a "dummy value" (like an empty string) when the condition is false.
```Bicep
resource auditingSettings 'Microsoft.Sql/servers/auditingSettings@2024-05-01-preview' = if (auditingEnabled) {
  name: 'default'
  properties: {
    state: 'Enabled'
    // SAFE WAY: If Prod, get the blob endpoint. If NOT, pass empty string ''.
    storageEndpoint: environmentName == 'Production' ? auditStorageAccount.properties.primaryEndpoints.blob : ''
    
    // SAFE WAY: If Prod, get the keys. If NOT, pass empty string ''.
    storageAccountAccessKey: environmentName == 'Production' ? listKeys(auditStorageAccount.id, auditStorageAccount.apiVersion).keys[0].value : ''
  }
}
```

## Constraints and gotchas

* **Unique Naming Conflict:** You cannot define two resources with the exact same symbolic name in the same file and try to toggle between them using conditions. Bicep views this as a duplicate identifier.
* **Module Strategy:** If you have a large group of resources that all share the same condition (e.g., a whole monitoring stack), it is cleaner to put them in a **Module** and place the `if` condition on the Module deployment itself.
