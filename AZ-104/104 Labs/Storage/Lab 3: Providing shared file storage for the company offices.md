# Lab 3: Providing shared file storage for the company offices

**Job Skills**

* Creating storage account specifically for file shares.
* Configuring file share and directory.
* Configuring snapshots and practicing restoring files.
* Restricting access to specific VNet and subnet.

## Part 1: Creating storage account for Azure files

1. Went to `Storage accounts` and created a new one to my already existing resource group `FilesOnly`. Named the storage account `azfiles27`. For performance i chose premiumd due to low latency. For redundancy i picked `ZRS` since it protect agains datacentre level failures and i used for high availability scenarios. Since im working with Azure files high availability is a must because files need to be accessed from multitude of places. For account type i chose `file shares`.

Final config:

<img width="801" height="616" alt="image" src="https://github.com/user-attachments/assets/93af5d9a-9e1b-493c-a081-aa00ae2f273d" />

## Part 2: Configuring file share with directory

1. In the `azfiles27` storage account, went under `data storage` > `file shares` > `+ file share`. Added a name `sharing1` and left everything default but did however view other options. In the basics tab i could define protocol whether `SMB` or `NFS`, define the provisioned storage size and i could enable backup too.

<img width="461" height="332" alt="image" src="https://github.com/user-attachments/assets/26a4b0d1-fe76-410c-acbd-8e22b1c1a9ab" />

Selected `create`. Then went to file share resource and chose `add directory` from the top. Named the new directory as `finance`. Then from the left panel chose `Browse` then > selected `finance` directory i had created.

I noticed that when i opened the directory i could add subsequent directorys to it if needed. For now i uploaded a file by selecting `upload` from the top:

<img width="900" height="291" alt="image" src="https://github.com/user-attachments/assets/f1fda6cb-8a46-4d7c-b95c-afaa9b68ffd3" />

## Part 3: Configuring and testing snapshots

1. Selected my file share, then from left panel `Operations` > `Snapshots`. Selected `+ add snapshots`. Comment was optional so added comment `test` and verified my directory `finance` and the uploaded file were in the snapshot:

<img width="688" height="239" alt="image" src="https://github.com/user-attachments/assets/71647b13-9f0d-40c7-8dc8-3c221dbbd870" />

File itself:

<img width="653" height="290" alt="image" src="https://github.com/user-attachments/assets/1d2f3cf2-a4ed-4063-9599-d7093f63719a" />

2. Returned to my file shared > `browse` > `finance` and the the uploaded file, selected it and the selected `delete` from the top, although you can delete files by selecting `...` at the end of each file and quick delete from there.

<img width="699" height="242" alt="image" src="https://github.com/user-attachments/assets/f89c35cb-9105-45ca-9a91-271f4a025b22" />

Selected `yes`.

Went to `snapshots` again and selected my created snapshot. Navigated to the file i wanted to restore, selected it and then used `restore` option from the top:

<img width="436" height="547" alt="image" src="https://github.com/user-attachments/assets/be15ecfe-8cc7-486f-a654-38d072ef22a5" />

When restoring files you need to provide a restored file name.

Went back to `browse` and `finance` directory to verify that file was now restored with provided name:

<img width="918" height="355" alt="image" src="https://github.com/user-attachments/assets/eb7313d4-e6b0-472e-863b-2aadcb589112" />

Success

## Part 4: Configuring sotrage access restriction to selected VNets.

1. Went to `virtual networks` and created new one, named it `azfiles` and placed it in the same RG group where files existed. Left parameters at default:

<img width="739" height="700" alt="image" src="https://github.com/user-attachments/assets/1f9dd216-f4da-43f7-a378-a569f867995b" />

Went to resource it self then from `settings` went to > `subnets`. Selected default subnet, then from `service endpoints` section went to `microsoft.storage` from the drop down of `services` and saved.

The core idea is to access storage account only through the created VNet.

2. Went back to storage account and went to `networking` from `security + networking` section.

Then changed `public network access` to `enabled from selected virtual networks and ip addresses`.

In the `virtual network` section selected `add a virtual network` and from dropdown chose `add existing virtual network` instead of creating a new one from there.

Filled out necessary values, picked my `azfiles` network and under subnets chose `default`:

<img width="291" height="321" alt="image" src="https://github.com/user-attachments/assets/760e50cd-8379-450b-9d26-d9a2c93b606f" />

Selected `add` at the bottom. 

<img width="932" height="267" alt="image" src="https://github.com/user-attachments/assets/b5b09c31-63e0-4012-a011-92985fb47e0a" />

Saved changes and navigated to my file share to hopefully see an error or cant access message:

<img width="941" height="604" alt="image" src="https://github.com/user-attachments/assets/1b75f56e-ebf6-4f1c-b66b-80ff6e0fe7cf" />

I couldnt access the file share anymore because now the file share could only be accessed through the vnet.

### Summary

For this lab i started by creating a storage account named `azfiles27` specifically for file shares. I chose Premium performance because i wanted that low latency, and picked ZRS (Zone Redundant Storage) for redundancy. Since these files need to be accessed from multiple places, ZRS is crucial to protect against datacenter-level failures.

Once the account was ready i created a file share called `sharing1` and added a directory named `finance` inside it. Uploaded a file so i could test out the snapshot capabilities. Took a manual snapshot of the share and then to test the recovery, i deleted the file from the main directory. I was able to go back into the snapshots find the file, and restore it by providing a new name for the recovered version.

Finally i worked on network security. Created a virtual network called `azfiles` and enabled the `Microsoft.Storage` service endpoint on the default subnet. Then went to the storage accounts networking settings and switched public access to only allow "selected virtual networks." After adding my new VNet to the allow list, i tried accessing the share from the browser again and got an error message which confirmed that the storage was properly locked down and only accessible through that specific private network.

### Key Takeaways

* Azure files offers fully managed file shared in the cloud that are accessible via the industry standard server message block SMB and network file system NFS protocols as well as AZ files REST API.
* Files provides the capability to take snapshots of SMB and NFS file shares. Share snapshots capture the share state at the point of time when snapshot was taken. Share snapshots provides only file level protection.
* Storage account can be configured with an endpoint for accessing AZ file share directly. Endpoints restrict network access to the storage account.
