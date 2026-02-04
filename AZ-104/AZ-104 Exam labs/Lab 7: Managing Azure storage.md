# Lab 7: Managing Azure storage

## Goal:

How to create storage accounts for Blobs and Files, how to configure and secure blob containers. Using storage browser to configure and secure File shares.

**Job Skills**

* Creating and configuring storage account via cmdlets.
* Creating and configuring secure blob storage via cmdlets.
* Creating and configuring secure Azure file storage via cmdlets.

## Part 1: Creating and configuring storage account

1. In the portal went to `Storage accounts` and created a new one.
2. Filled out the basics tab:
<img width="837" height="637" alt="image" src="https://github.com/user-attachments/assets/d8f9a175-dd0c-415e-bd4b-7b57fcfe21ae" />
3. Went to `networking` tab and in the `public network access` set it to `disabled`. In the `data protection` noticed that 7 days is the default soft delete retention policy. Accepted defaults.
4. Went to final review:

<img width="663" height="489" alt="image" src="https://github.com/user-attachments/assets/b91d2ac2-0b9f-4fe7-bff1-82a0d3485a89" />

<img width="413" height="72" alt="image" src="https://github.com/user-attachments/assets/1d227842-ed23-444b-a6bf-d9b7bd8a6207" />

5. After creation went to the resource then `security + networking` > `networking`.

Noticed the public network was disabled. Selected `manage` and changed the public network access to `enabled`.

Then changed `public network access scope` to `enable from selected networks`.

Under `IPv4 addresses` section selected `add your client IPv4 address`:

<img width="517" height="126" alt="image" src="https://github.com/user-attachments/assets/778847ac-dcd9-4c6a-b86d-7f027142ceba" />

Saved changes.

Went to `data management` > `lifecycle management` and `add a rule` named it `Movetocool`.

In the next tab set the rule that if blobs last modified then in 30 days move to cool storage:

<img width="747" height="386" alt="image" src="https://github.com/user-attachments/assets/57c34d0f-79ec-4d04-bfac-656f9e7846be" />

Added the rule.

## Part 2: Creating and configuring secure blob storage

1. Created new container `data` then selected ellipsis `...` at the end of the container and `access policy`.
2. Added new policy under `immutable blob storage` area, set it to `time based retention` and the retention period to 180 days and hit save:

<img width="548" height="83" alt="image" src="https://github.com/user-attachments/assets/0cda874b-ff12-4930-a9b6-b5de9e958b3e" />

3. In the `data` container selected  upload and under `advanced` made sure that:

* Block type was > Block blob
* Block size > 4 Mib
* Access tier > Hot
* Upload to folder > and gave it a folder to upload to `securitytest`

For encryption scope > use exisiting default container scope.

Added an image i wanted to upload and hit upload.

Then selected my uploaded file in the `securitytest` folder and its ellipsis `...` then > `Copy url`, pasted it into private browser and was met with access denied:

<img width="945" height="147" alt="image" src="https://github.com/user-attachments/assets/cbf777a9-bda0-432f-933b-096745137111" />

Because the access level was set to private.

Went back to the files ellipsis and selected `generate SAS` and used default settings with the exception of changing start date to yesterday for it to take effect instantly.

Copied `blob SAS url` after generation: `https://storages670.blob.core.windows.net/data/securitytest/82636f73-d74c-4f29-9aed-c92abc5fb91f.jfif?sp=r&st=2026-02-03T22:22:27Z&se=2026-02-05T06:37:27Z&spr=https&sv=2024-11-04&sr=b&sig=VtZ0BbVAj2buBmMyFBscfJzSRVdaSykFYss3fG7Ff%2FU%3D`

Now the image would be visible because i had created a token that uses specific URL for viewing regardless of access level:

<img width="957" height="877" alt="image" src="https://github.com/user-attachments/assets/1adcb2d2-d74d-49b8-ae13-ea79eef03553" />

## Part 3: Creating and configuring Azure file storage

1. In the storage account > `data storage` > `file shares` created new file share `share1` and kept the default `Transaction optimized`.

In the `backup` tab made sure `enable backup` was unchecked in order to simplify lab configuration.

Review:

<img width="490" height="313" alt="image" src="https://github.com/user-attachments/assets/8ea3e257-8714-4be5-893d-a18210ff5151" />

Hit create.

2. Went to storage account > `storage browser` which helps view all storage services under the account, then > `file shares` selected my `share1` directory and `+add directory`. This meant that i could create more folder structures.

Created a subsequent directory `memes` and uploaded a file to it.

Wanted to restrict access to the storage account so went to `virtual networks` and created a new one `vnet1` with default settings.

After deployment went to vnet resource > `service endpoints` > `add`.

In the `service` drop down took `Microsoft.Storage`. Then under `subnets` checked `default` subnet > add.

Went back to storage account > `Networking` > `Public network access` > `manage` > `add virtual network` > `add existing network` > selected `vnet1` and `default` subnet then add.

In the `IPv4 Addresses` section deleted my machines ip address to allow traffic to only come from the vnet, hit save.

Went to my container `data` and tried to access it:

<img width="418" height="763" alt="image" src="https://github.com/user-attachments/assets/51663b09-3692-4552-bfbc-73f95a41725f" />

Lab success, because i restricted access to the storage.

### Summary

Kicked off by spinning up a new storage account, nothing fancy just the usual basics tab → networking → data protection dance. Played with public network access first disabled it, then re enabled it but only for my own IP. Basically proving to myself how the access model behaves.

Set up lifecycle mgmt too simple rule: “after 30 days, shove blobs to cool tier.” Nothing deep, just getting hands on with automation.

Then moved into blob storage, made a container, slapped an immutable policy on it (time based, 180 days). Uploaded a file with all the advanced settings checked so i actually know whats happening under the hood (block blob, block size, hot tier, folder path, encryption scope). Tried accessing the blob directly → denied, as expected. Generated a SAS token → boom access works. Learned the difference between container ACLs and SAS‑based scoped access in a very real way.

After that jumped into Azure Files. Created a share, made subfolders through Storage Browser, uploaded new image. Then locked the whole storage account down by using a VNet + service endpoints. Removed my public IP from the allow list and confirmed i locked myself out. Perfect that was the end goal.

### Key Takeaways

* An Azure storage account contains all your Azure Storage data objects: blobs, files, queues, and tables. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS.
* Azure storage provides several redundancy models including Locally redundant storage (LRS), Zone-redundant storage (ZRS), and Geo-redundant storage (GRS).
* Azure blob storage allows you to store large amounts of unstructured data on Microsoft’s data storage platform. Blob stands for Binary Large Object, which includes objects such as images and multimedia files.
* Azure file Storage provides shared storage for structured data. The data can be organized in folders.
* Immutable storage provides the capability to store data in a write once, read many (WORM) state. Immutable storage policies can be time-based or legal-hold.
