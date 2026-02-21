## Identity and governance

1. `Get-AzResource` > retrtieves or gets resources.
2. `Update-AzTag` > Adds tags to resources that have tags
  * `-Operation Merge` > Ensures that existing tag is preserved.
  * `-Operation Replace` > replaces existing set of tags and the older values will be lost.
3. `New-AzTag` > replaces all tags on resource, RG or subscription.
4. `Get-AzResourceGroup` > gets azure RGs in the current subscription.

