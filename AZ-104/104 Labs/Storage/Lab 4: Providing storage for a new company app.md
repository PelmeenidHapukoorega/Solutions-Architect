# Lab 4: Providing storage for a new company app

**Job Skills**

* Creating storage account and managed identity
* Securing access to the storage account with a key vault and key
* Configuring storage account to use customer managed key in the key vault
* Configuring time based retention policy and encryption scope

1. Created new storage account `encrypt89` and under `encryption` tab i checked the box for `enable infrastructure encryption`. Noted that this couldnt be changed after the creation of storage account.

Waited for resource to deploy. 

2. Went to `Managed identities` > `create` and selected my resource group `Encryption`. Named managed identity as `system`
3. Went to my storage account again > then `IAM` > `Add role assignment`.

Under `Job functions roles` searched for `storage data reader` and selected it.
