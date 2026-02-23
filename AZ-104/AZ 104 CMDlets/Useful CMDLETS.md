## Identity and governance

### Resources
1. `Get-AzResource` > retrtieves or gets resources.
2. `Get-AzResourceGroup` > gets azure RGs in the current subscription.
3. `Get-AzResourceLock` > Calls on any existing resource locks or specific lock if relevant name and RG details are entered.
4. `New-AzResource` > creates new resource.
5. `New-AzResourceLock` > Creates new resource lock.

### Tags

1. `New-AzTag` > replaces all tags on resource, RG or subscription or creates new tag.
2. `Update-AzTag` > Adds tags to resources that have tags
  * `-Operation Merge` > Ensures that existing tag is preserved.
  * `-Operation Replace` > replaces existing set of tags and the older values will be lost.

### Policy

1. `az provider register --namespace 'Microsoft.PolicyInsights'` > Needs to be registered before creating Azure policies.

**JSON**

1. `Indexed` > Mode property checks whether resource supports feature before checking it.
2. `All` > Tries to apply policy to all resources even when resource is not supporting tagging.
3. `[tags[Environment]]` > acts as the field value for tags.

### Management groups

1. `New-AzManagementGroupSubscription` > used to add a subscription to a specificied management group.
2. `Remove-AzManagementGroupSubscription` > Used to remove subscription from management group. 
3. `Update-AzManagementGroup` > Used to update supported parameters like MGs display name or change MG parent.
4. `Remove-AzManagementGroup` > Used to delete management group.

### RBAC

1. 

### Storage

**Regenerating keys access without interrupting app**

1. `az storage account keys list` with `--query [1]` > To retrieve secondary key.
2. `az storage account keys renew` with `--key primary` >  To regenerate primary key.
   * `--key secondary` > To regenerate secondary key.
3. `az storage account keys list` with `--query [0]` > To retrieve primary key.
4. `EnableShareDeleteRetentionPolicy` with `$false` > Only disables soft delete on the storage account.

**PWSH script to upload blob to specific tier**

1. `New-AzStorageContext` > Creates new AZ storage context. Context allows to authenticate ST account and it is generally the ST account key and connection string used to create it.
2. `New-AzStorageContainer` > Creates new container.
3. `Set-AzStorageBlobContent` > Used to upload local file to blob container.

**Note**

To permanently delete file share that has been soft deleted, you must undelete it, disable soft delete and then delete it again.

**Storage Lifecycle management policy via PWSH**

1. `$action` with `Add-AzStorageAccountManagementPolicyAction` > Adds an action to create `ManagementPolicy Action Group` object.
2. `$filter` with `New-AzStorageAccountManagementPolicyFilter` > Creates a management policy filter.
   * Used in conjunction with `Add-AzStorageAccountManagementPolicy`.
   * Needs to set the `PrefixMatch` parameter and `BlobType` parameter.
3. `$rule2` with `New-AzStorageAccountManagementPolicyRule` > Creates new action object.
   * Used in conjunction with `Set-AzStorageAccountManagementPolicy`.
4. `Set-AzStorageAccountManagementPolicy` > Once new object, filter object and new rule object have been created in PWSH script, then this cmdlet creates the policy for the RG and ST account.
5. `New-AzStorageBlobInventoryPolicyRule` > Used to create blob inventory rule object.
   * Used together with `Set-AzStorageBlobInventoryPolicy` granted there is an existing blob container.
6. `Set-AzStorageBlobInventoryPolicy` > Used to create or update blob inventory policies.
7. `Set-AzStorageBlobContent` > Used when uploading a file from local source to AZ storage blob. 
