* Problem: Company is overspending

* Task:
  * Resource group
  * Azure policy that allows B-Series VMs
  * Custom RBAC role that allows users to `read` everything but only `write` to storage
  * Applying lock to the resource group

1. Creating resource group
```bash
az group create \
--name Guardrail \
--location westeurope
```

2. Creating AZ policy

First got the subscription ID to later use it to apply the policy to resource group scope where it would apply
```bash
az account show --query id -o tsv
```

The policy itself already exists under name `allowed virtual machine size Skus` all i had to do was define which series it would apply to and the scope where it would apply to
