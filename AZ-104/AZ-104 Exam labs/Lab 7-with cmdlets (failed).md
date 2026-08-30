# Lab 7: Managing Azure storage

## Goal:

How to create storage accounts for Blobs and Files, how to configure and secure blob containers. Using storage browser to configure and secure File shares.

**Job Skills**

* Creating and configuring storage account via cmdlets.
* Creating and configuring secure blob storage via cmdlets.
* Creating and configuring secure Azure file storage via cmdlets.

## Part 1: Creating and configuring storage account

1. Created new resource group:
```PWSH
New-AzResourceGroup -Name "Storages" `
-Location eastus
```

2. Set a name for storage account:
```PWSH
$saName = "mainstorage670"
```

Created storage account:
```PWSH
New-AzStorageAccount `
-ResourceGroupName "Storages" `
-Name mainstorage670 `
-Location eastus `
-SkuName Standard_RAGRS `
-Kind StorageV2 `
-AllowBlobPublicAcces $false `
-EnableHttpsTrafficOnly $true `
-PublicNetworkAccess Disabled
```

Output:

<img width="937" height="145" alt="image" src="https://github.com/user-attachments/assets/82a3a4a8-1d0e-4610-b556-05198a5e59b8" />

3. Enabled public network access:
```PWSH
Set-AzStorageAccount `
-ResourceGroupName "Storages" `
-Name mainstorage670 `
-PublicNetworkAccess Enabled
```

4. Next i added my IP address, but needed to first get it:
```PWSH
$myIP = (Invoke-RestMethod -Uri "https://api.ipify.org?format=json").ip
```

Verified my ip:
```PWSH
$myIP
```

Copied output: `108.142.162.20`

Then added network rule using my ip:
```PWSH
Add-AzStorageAccountNetworkRule `
-ResourceGroupName Storages `
-Name mainstorage670 `
-IPAddressOrRange 108.142.162.20/32
```

I ran into an error:
```
Add-AzStorageAccountNetworkRule: Operation returned an invalid status code 'BadRequest'
```

So i checked if public network access was enabled for storage account with:
```PWSH
(Get-AzStorageAccount -ResourceGroupName "Storages" -Name "mainstorage670").PublicNetworkAccess
```

Public access was enabled.

Checked provisioning state just in case:
```PWSH
(Get-AzStorageAccount -ResourceGroupName "Storages" -Name "mainstorage670").ProvisioningState
```

Provisioning was succeeded.

So the only logical reason was that the storage account already had its own network rule set and azure was rejecting the incremental update.

I needed to update the entire rule set in 1 operation instead of adding a single rule.

First i created an empty list to hold IP rules:
```PWSH
$rules = New-Object "System.Collections.Generic.List[Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule]"
```

Then i added my ip to that list:
```PWSH
$rules.Add([Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule]::new("108.142.162.20/32"))
```

But ran into another error:
```
MethodException: Cannot find an overload for "new" and the argument count: "1".
```

This meant that PSIprule class in my az storage version did not expose constructor that would accept strings and since i had created empty rule container i now needed to create IP rule object aka making an empty rule:
```PWSH
$rule = New-Object Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule
```

Then i assigned my ip to the rule:
```PWSH
$rule.IPAddressOrRange = "108.142.162.20/32"
```

Then i added the rule to the container:
```PWSH
$rules.Add($rule)
```

Now i needed to update the entire rule set:
```PWSH
Update-AzStorageAccountNetworkRuleSet `
-ResourceGroupName Storages `
-Name mainstorage670 `
-DefaultAction Deny `
-Bypass AzureServices `
-IpRule $rules
```

After all this i still got met with bad request error so i expanded the error with:
```PWSH
$Error[0].Exception.Response.Content
```

Output:
```
{"error":{"code":"InvalidValuesForRequestParameters","message":"Values for request parameters are invalid: networkAcls.ipRule[*].value. For more information, see - https://aka.ms/storagenetworkruleset"}}
```

Azure was rejecting the IP rule because property name was wrong for the az.storage module version. Backend for storage account expected the property to be named `value`.

Knowing that i created new object again:
```PWSH
$rule = New-Object Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule
```

Then ran `$rule` with `Value`:
```PWSH
$rule.Value = "108.142.162.20/32"
```

But even that didnt work.

Decided to inspect the object:
```PWSH
$rule | Get-Member
```

Output:
```
   TypeName: Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule

Name             MemberType Definition
----             ---------- ----------
Equals           Method     bool Equals(System.Object obj)
GetHashCode      Method     int GetHashCode()
GetType          Method     type GetType()
ToString         Method     string ToString()
Action           Property   System.Nullable[Microsoft.Azure.Commands.Management.Storage.Models.PSNetworkRuleActionE…
IPAddressOrRange Property   string IPAddressOrRange {get;set;}
```

So this told me that my `PSIpRule` did support `IPAddressOrRange` and meant that earlier failure wasnt because the property was wrong but because Azure just rejected it.

`networkAcls.ipRule[*].value is invalid` basically said that the IP address i sent was invalid for the storage accounts network rule schema.

Knowing that i got the new IP rule object again:
```PWSH
$rule = New-Object Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule
```

Set the IP:
```PWSH
$rule.IPAddressOrRange = "108.142.162.20/32"
```

Then i set the rule to `allow`
```PWSH
$rule.Action = "Allow"
```

Created a list to hold the rule:
```PWSH
$rules = New-Object "System.Collections.Generic.List[Microsoft.Azure.Commands.Management.Storage.Models.PSIpRule]"
```

Added the rule to the list:
```
$rules.Add($rule)
```

Replaced the entire network rule set:
```PWSH
Update-AzStorageAccountNetworkRuleSet `
-ResourceGroupName Storages `
-Name mainstorage670 `
-DefaultAction Deny `
-Bypass AzureServices `
-IpRule $rules
```

And i still... got met with the same error and i learned that if the storage account is using the following SKUs:

* Standard_GRS
* Standard_RAGRS
* Standard_LRS (Older versions)
* Premium_LRS (Older versions)
* BlobStorage

Do not support IP rules and Azure will always return badrequest.

The storage accounts that dont support network rules are:

* BlobStorage
* Storage (classic)
* FileStorage

Only the following support IP rules:

*StorageV2
* Storage (modern)
* BlockBlobStorage (some regions)

In short this lab couldnt be done with cmdlets only.

Portal uses newer API which means that it:

* Supports RA-GRS (which the lab requires)
* Supports newer `networkAcls` schema.
* Accepts IP rules on RA-GRS accounts.
* Automatically fills in missing fields.
* Automatically handles replication layer quirks.

Now powershell uses older API for storage network rules which means that it:

* Does not fully support RA-GRS.
* Does not accept IP rules for RA-GRS accounts.
* Requires fields that RA-GRS accounts dont expose.
* Rejects the rule with generic error:
```
InvalidValuesForRequestParameters: networkAcls.ipRule[*].value
````

In short this lab works in portal because it was written for portal workflow, not powershell automation which is a tough pill to swallow.
