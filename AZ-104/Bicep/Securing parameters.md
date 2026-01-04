# Bicep Parameter Security

## Security Principles
When deploying resources requiring sensitive inputs (passwords, API keys), standard parameters are insufficient as they may be logged or visible in clear text.

*   **Primary Recommendation:** Use **Managed Identities** wherever possible to avoid handling credentials entirely.
*   **Secondary Approach:** If credentials are required, they must be secured to prevent exposure in deployment logs, terminal history, or version control systems.

---

## 2. The `@secure` Decorator
The `@secure` decorator is used to mask sensitive values during deployment.

### Capabilities
*   **Logging:** Prevents Azure from saving the parameter value in the deployment activity logs.
*   **Interactive Deployment:** Masks the input (hides text) if the user is prompted to type the value in the terminal.

### Supported Types
*   `string`
*   `object`

### Syntax
```bicep
@secure()
param sqlServerAdministratorLogin string

@secure()
param sqlServerAdministratorPassword string
```

**Best practices**

1. **No Default Values:** Never assign default values to secure parameters. Doing so risks users deploying with weak, known defaults.
2. **No Outputs:** Never return secure parameters as `outputs`. Deployment history is accessible to many users, and outputs are stored in plain text.

## Integration with Azure Key Vault (parameter files)

Storing actual secrets in `parameters.json` files is a security risk, especially if those files are committed to version control systems like Git. Instead, use **Key Vault References**.

**Mechanism:** The parameter file contains a reference ID to a secret stored in Azure Key Vault, rather than the secret value itself. Azure Resource Manager retrieves the value during deployment.

**JSON syntax (parameter file)
```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "sqlServerAdministratorPassword": {
      "reference": {
        "keyVault": {
          "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/toysecrets"
        },
        "secretName": "sqlAdminLoginPassword"
      }
    }
  }
}
```

**Requirements**

1. **Key Vault Configuration:** The Vault must be configured to allow "Azure Resource Manager for template deployment" access.
2. **User Permissions:** The user executing the deployment must have permission to access the Key Vault.

## Integration with Azure Key Vault (Modules)

When using Bicep Modules, you may need to pass a secret from a Key Vault into a module parameter.

**The `getSecret()` Function**

You can retrieve a secret directly within the Bicep code using the `existing` keyword (to reference the Vault) and the `getSecret()` method.

```Bicep
// 1. Reference the existing Key Vault
resource keyVault 'Microsoft.KeyVault/vaults@2023-07-01' existing = {
  name: keyVaultName
}

// 2. Pass the secret into the module
module applicationModule 'application.bicep' = {
  name: 'application-module'
  params: {
    // getSecret is only valid for secure module parameters
    apiKey: keyVault.getSecret('ApiKey')
  }
}
```

**Note!**

The `getSecret()` function can only be used to pass values to module parameters that are marked with `@secure()`.
