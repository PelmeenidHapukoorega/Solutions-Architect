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

### Management groups

1. `New-AzManagementGroupSubscription` > used to add a subscription to a specificied management group.
2. `Remove-AzManagementGroupSubscription` > Used to remove subscription from management group. 
3. `Update-AzManagementGroup` > Used to update supported parameters like MGs display name or change MG parent.
4. `Remove-AzManagementGroup` > Used to delete management group.

### RBAC

1. 
