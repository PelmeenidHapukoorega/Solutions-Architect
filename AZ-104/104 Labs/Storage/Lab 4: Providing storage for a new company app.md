# Lab 4: Providing storage for a new company app

**Job Skills**

* Creating storage account and managed identity
* Securing access to the storage account with a key vault and key
* Configuring storage account to use customer managed key in the key vault
* Configuring time based retention policy and encryption scope

## Part 1: Creating storage account and managed identity

1. Created new storage account `encrypt89` and under `encryption` tab i checked the box for `enable infrastructure encryption`. Noted that this couldnt be changed after the creation of storage account.

Waited for resource to deploy. 

2. Went to `Managed identities` > `create` and selected my resource group `Encryption`. Named managed identity as `system`
3. Went to my storage account again > then `IAM` > `Add role assignment`.

Under `Job functions roles` searched for `storage blob data reader` and selected it. Then under members assigned access to managed identity > selected members and added my `system` managed identity:

<img width="569" height="809" alt="image" src="https://github.com/user-attachments/assets/f795e847-dbcb-4186-958f-1c1d4c19b5b0" />

Final review:

<img width="933" height="600" alt="image" src="https://github.com/user-attachments/assets/304c359e-27a3-4fdd-b396-3480e2b0ab3f" />

Went ahead and added role. Now my storage account could be accessed by the managed identity i created. However its a read role so managed identity couldnt actually do anything except viewing it.

## Part 2: Securing access to the storage account with a key vault and key

For the key vault and key my account needed key vault admin permissions.

1. Selected my resource group then > `IAM` > `Add role assignment` in the centre, then > searched for `key vault administrator` role. Then under `members` added my user:

<img width="845" height="481" alt="image" src="https://github.com/user-attachments/assets/540d95a1-d197-4b09-acba-6ca26249f564" />

Went ahead and added role.

2. Then i went to `key vaults` and created a new one `rufus`. In the `access configuration` tab made sure `azure RBAC` was selected.

Final review:

<img width="770" height="810" alt="image" src="https://github.com/user-attachments/assets/ffb302c0-4c7e-4f57-ac29-82f949697fd6" />

Went to resource itself and made sure on the overview both `soft delete` and `purge protection` were enabled:

<img width="172" height="116" alt="image" src="https://github.com/user-attachments/assets/b4427be9-00c7-4564-9a10-c74c3ae9165d" />

Purge protection was disabled by default so i quickly enabled it after.

Went to key vault > `objects` > `Keys` and generated a new key which i named `silver` and left the rest default:

<img width="828" height="742" alt="image" src="https://github.com/user-attachments/assets/bd9af870-be34-46e1-9fe1-37ee4265ddd8" />

## Part 3: Configuring storage account to use the customer managed key in the key vault

1. Went back to RG group and `IAM`. Added a new role `Key vault crypto service encryption user` to the managed identity.

Reviewed before creation:

<img width="889" height="554" alt="image" src="https://github.com/user-attachments/assets/49f4c447-9678-46f6-aa42-26ab25a86b2a" />

2. Next i configured storage account to use the customer managed key in the key vault.

Returned to my storage account and went to `Encryption` blade > selected `customer managed keys` > `select a key vault and key`:

<img width="852" height="433" alt="image" src="https://github.com/user-attachments/assets/295b1366-b38d-48c7-9640-1da171be2ff3" />

After doing that i made sure identity type was set to `user assigned` and added my managed identity `system`. Saved changes. 

<img width="330" height="70" alt="image" src="https://github.com/user-attachments/assets/72d64dca-3fdd-4ff7-84ac-4054320f621b" />

## Part 4: Configuring time based retention policy and encryption scope

1. In the storage account created a new container `hold` with default settings. Uploaded a file to it. 

Then in `settings` > `access policy` and under `immutable blob storage` added new policy `time based retention` and for retention period set it to 5 days. Saved changes:

<img width="572" height="470" alt="image" src="https://github.com/user-attachments/assets/f78b39c8-e42e-4273-bf08-c7021afe21af" />

Then i tried to delete the file and expected the policy to block it:

<img width="360" height="192" alt="image" src="https://github.com/user-attachments/assets/1f9b0d35-faec-4233-8499-24b0c1a3ac23" />

2. In the storage account went back to `encryption` and under `encryption scopes` tab added a new one `secretenc` and left the rest default:

<img width="573" height="449" alt="image" src="https://github.com/user-attachments/assets/495fd358-d686-42b6-9af4-8e77d54010a5" />

Then i created a new container `pause`
