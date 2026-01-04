# Bicep Parameter Files

## Automation and State
To move away from manual command-line arguments use **Parameter Files** to define specific configuration sets for different environments (e.g., `dev`, `prod`) and enable automated deployments.

*   **Problem:** Providing values via CLI (`-param value`) works for testing but becomes unmanageable with many parameters.
*   **Solution:** Store values in a JSON or Bicep-specific file.
*   **Naming Convention:** A common practice is `templateName.parameters.environment.json` (e.g., `main.parameters.dev.json`).

---

## 2. File Structure (JSON Format)
While `.bicepparam` files exist, the standard format is often **JSON**. The structure requires specific headers and a nested value object.

### The Schema
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "appServicePlanInstanceCount": {
      "value": 3
    },
    "appServicePlanSku": {
      "value": {
        "name": "P1v3",
        "tier": "PremiumV3"
      }
    }
  }
}
```

## Key components

* `%schema` - Required, it tells Azure ARM how to validate the file.
* `parameters` - The root object containing your inputs.
* **Matching names** - Parameter names (e.g., `appServicePlanInstanceCount`) must match the Bicep template exactly.
* `value` object - You cant simply write `"param": 3`. You must nest it: `"param": { "value": 3 }`.

## Deployment Operations

To use a parameter file during deployment, use the `-TemplateParameterFile` switch in PowerShell.
```PWSL
New-AzResourceGroupDeployment `
  -Name main `
  -TemplateFile main.bicep `
  -TemplateParameterFile main.parameters.json
```

## Order of operations (Value presedence)

Its possible to define a value in three places simultaneously. Azure follows a strict hierarchy to determine which value "wins."

**The hierarchy (highest to lowest)**

1. **Command Line Argument** (Highest Priority - Overrides everything)
2. **Parameter File** (Overrides defaults)
3. **Default Value** (Used only if nothing else is provided)

**Scenario example**

* **Default in Bicep:** `Count = 1`
* **Value in Parameter File:** `Count = 3`
* **Value in CLI Command:** `-appServicePlanInstanceCount 5`

**The result:** Deployed count will be 5.

You can use a parameter file and override specific values in the same command:
```PWSL
New-AzResourceGroupDeployment `
  -Name main `
  -TemplateFile main.bicep `
  -TemplateParameterFile main.parameters.json `
  -appServicePlanInstanceCount 5
```

* `location` - Default value (not in a file or CLI) = Resource group location
* `appServicePlanSku` - parameter file (not in CLI) = `P1v3` (from JSON)
* `appServicePlanInstanceCount` - CLI arguement (overrides JSON and Default) = 5
