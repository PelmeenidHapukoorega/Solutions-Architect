# Lab 4: Providing storage for a new company app

**Job Skills**

* Creating storage account and managed identity
* Securing access to the storage account with a key vault and key
* Configuring storage account to use customer managed key in the key vault
* Configuring time based retention policy and encryption scope

## Goal:

To create protected immutable storage that would use RBAC and would be accessed using keys and managed identity.

## Part 1: Creating storage account and managed identity

1. Created a new storage account `devsonly` and placed in the resource group `devstorage`.
2. In the encryption tab made sure `infrastructure encryption` was checked and enabled. This needs to be created at the same time as the storage account and cannot be changed later on.

Review:

<img width="689" height="255" alt="image" src="https://github.com/user-attachments/assets/497c3b79-b8b2-4164-bbda-e754d9ceda3c" />

<img width="525" height="149" alt="image" src="https://github.com/user-attachments/assets/fcaed788-ea87-4d6e-9c20-48dbb9fdcd9e" />

3. Created a new managed identity `developers` and added it `devstorage` resource group:

<img width="385" height="226" alt="image" src="https://github.com/user-attachments/assets/17c275a2-c08c-4a65-b730-f702631397fb" />

4. In the `IAM` of the storage account added a new role `Storage blob data reader` since the identity only needs to read and list containers and blobs. Added my managed identity and created a role.

Review:

<img width="887" height="437" alt="image" src="https://github.com/user-attachments/assets/7f47f8b2-5579-4fcd-a1d8-4d1e25ada47b" />

With that storage account could be accessed by `developers` with the added role.

## Part 2: Securing access to the storage account with a key vault and key

1. Added a new role for the resource group `key vault administrator` since my user needed that permissions to create key vault and the key needed through `IAM`. Added my user and made a final review:

<img width="710" height="360" alt="image" src="https://github.com/user-attachments/assets/cde6a56b-43d2-42d8-b480-060d5dc5992e" />

2. Next i went to `key vaults` and created a new one to the resource group to store access keys. In the `access configuration` tab made sure Azure RBAC was checked.
3. In the the basic tab i enabled `purge protection`. `Soft delete` is enabled by default.

Review:

<img width="563" height="632" alt="image" src="https://github.com/user-attachments/assets/92120595-8acb-4714-b374-6f2d66714c4d" />

4. Generated new key `devkeys` under `Objects` > `keys` and took default settings.

## Part 3: Configuring storage account to use the customer managed key in the key vault

1. In the resource group > `IAM` added new role `Key vault crypto service encryption user` role since i needed to assign it to the managed identity.

2. Under members added my managed identity then went for final review before creation:

<img width="898" height="403" alt="image" src="https://github.com/user-attachments/assets/b0748f47-f31b-4f54-bd6c-01c13cc39455" />

3. Next i configured storage account to use customer managed key in the key vault. Under `Encryption` in the storage account settings i added my key vault and key by using `customer managed key` option. Added my managed identity and before adding went for review:

<img width="761" height="248" alt="image" src="https://github.com/user-attachments/assets/545e9dc3-3bae-49a0-b993-74fb0d083c86" />

<img width="664" height="305" alt="image" src="https://github.com/user-attachments/assets/84698ae7-151f-4e8e-b341-55bec9af5727" />

Since i had added correct permissions i wasnt met with an error:

<img width="337" height="72" alt="image" src="https://github.com/user-attachments/assets/58440259-380b-424e-b414-9d21e3fe20a0" />

## Part 4: Configuring time based retention policy and encryption scope

1. Created new container `hold` in the storage account and left default, then uploaded a file to it.
2. Under `access policy` > `Immutable blob storage` section added new policy and used `time based retention` type, set the retention period to 5 days, saved changed then tried to delete the uploaded file:

<img width="336" height="172" alt="image" src="https://github.com/user-attachments/assets/052486e6-02a6-487a-86cf-04e987450f0a" />

This meant the policy was working.

3. In the storage account and under `Encryption` > `encryption scopes` tab added a new one `devscope`. For type selected `microsoft managed key`. Infrastructure encryption was enabled by default.

<img width="563" height="331" alt="image" src="https://github.com/user-attachments/assets/5e5d4fe7-16a9-4df6-8d7d-af79773d935c" />

Now after creation of the scope i went to create a new container and was meant to see public access level and under advance section i couldnt select my encryption scope:

<img width="426" height="510" alt="image" src="https://github.com/user-attachments/assets/0d6b5745-f321-43c7-8668-bab2cc3db919" />

Basically everything i had done felt like i did it all for nothing. So after reading forums and looking for answers i found the answer.

The reason i wasnt able to select anything was because blob versioning was disabled and you also needed to enable anonymous access which the guided lab failed to mention. In hindsight it was actually a good thing, since it made me look for the correct way to do things.

So i went to `data management` > `data protection` and in the overview page i enabled versioning for all blobs. And in the storage account overview page under `blob service` i enabled `Allow Blob anonymous access`

Went back to storage creation and:

<img width="417" height="434" alt="image" src="https://github.com/user-attachments/assets/ed714272-fd4f-4740-a6b1-d763d0de8991" />

With that public access was enabled, scope could be used.

### Summary

### Key Takeaways
