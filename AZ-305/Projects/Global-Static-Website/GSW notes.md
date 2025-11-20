Global Static Website – Build Journey & Troubleshooting Log

This document covers my entire process of building a global static website using Azure Storage, Azure CLI, Git Bash, and later fixing every single issue that popped up along the way. This includes region problems, missing folders, wrong shell, wrong login, missing roles, authentication failures, Azure CLI installation, and more.

This is written in a chronological “did this → hit this error → fixed it like this” style to show transparency and track my real workflow as I built the project.

1. Created Resource Group (CLI)

I began the project by creating a Resource Group for everything to live in.
I ran this:

az group create \
  --name StaticWeb-RG \
  --location westeurope


Why:
RG is the main container for all resources. Organized, easy to delete, clean structure.

This worked fine.

2. Created Storage Account (CLI)

Next step was creating a Storage Account for the static website:

az storage account create \
  --name statichermitweb \
  --resource-group StaticWeb-RG \
  --location westeurope \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true


Issue:
Got this lovely error:

The selected region is currently not accepting new customers.
RequestDisallowedByAzure


Fix:
West Europe was at capacity.
I switched region to North Europe (Ireland):

az storage account create \
  --name statichermitweb \
  --resource-group StaticWeb-RG \
  --location northeurope \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true


This succeeded.

3. Enabled Static Website Hosting

After creating the storage account, I enabled static site hosting:

az storage blob service-properties update \
  --account-name statichermitweb \
  --static-website \
  --index-document index.html \
  --404-document error.html


Output showed static website enabled.

4. Prepared index.html and error.html

I created my HTML files inside my GitHub repo folder:

AZ-305/Projects/Global-Static-Website/Source-website/index.html
AZ-305/Projects/Global-Static-Website/Source-website/error.html

5. Tried Uploading Files — Ran Into Path Errors

I tried this:

az storage blob upload \
  --account-name statichermitweb \
  --container-name '$web' \
  --name index.html \
  --file AZ-305/Projects/Global-Static-Website/source-website/index.html \
  --overwrite true


Error:

[Errno 2] No such file or directory


Why it happened:
I was running the commands in the wrong shell (WSL).
The repo only existed on my Windows Git Bash, not in the Linux shell (sander [ ~ ]$).

WSL couldn’t see my repo, so it had no idea where the HTML files were.

6. Confirmed Folder Names & Environment Confusion

I checked:

ls AZ-305/Projects/Global-Static-Website


WSL kept returning:

No such file or directory


That’s when I realized:

Git Bash → has repo

WSL → does NOT have repo

I was uploading from WSL

But all files existed only on Windows side

7. Solution: Use Git Bash As Main Environment

To avoid juggling multiple shells, I committed to using Git Bash only for this entire project.

Problem:
Git Bash didn’t have Azure CLI installed.

8. Installed Azure CLI in Windows

Opened PowerShell as Admin and ran:

winget install Microsoft.AzureCLI


Alternative (MSI installer):

msiexec /i https://aka.ms/installazurecliwindows


Restarted Git Bash → now I had az available locally:

az --version

9. Logged Into Azure

In Git Bash:

az login


Issue:
It logged into my Gmail account by accident → that account had no Azure subscription.

Got:

No subscriptions found
You must use MFA...


Fix:
Forced login using device code:

az login --use-device-code


Logged in with the correct Microsoft account (the one in the Azure Portal).
This time my subscription showed up.

Checked:

az account list --output table


Output showed:

Azure subscription 1   f37270cc-...   IsDefault=True

10. Upload Attempt — Now RBAC Permission Error

Tried upload again:

az storage blob upload \
  --account-name statichermitweb \
  --container-name '$web' \
  --name index.html \
  --file AZ-305/... \
  --overwrite true \
  --auth-mode login


Got hit with:

You do not have the required permissions to access this container


Fix:
I had to assign myself Storage Blob Data Contributor.

Went to Azure Portal → Storage Account → IAM → Add Role Assignment →
selected:

Storage Blob Data Contributor

Added myself as the member.

Waited ~20 seconds.

11. Upload Attempt — Success

After RBAC roles propagated, I reran the upload:

az storage blob upload \
  --account-name statichermitweb \
  --container-name '$web' \
  --name index.html \
  --file AZ-305/Projects/Global-Static-Website/Source-website/index.html \
  --overwrite true \
  --auth-mode login


Success.

Uploaded error.html:

az storage blob upload \
  --account-name statichermitweb \
  --container-name '$web' \
  --name error.html \
  --file AZ-305/Projects/Global-Static-Website/Source-website/error.html \
  --overwrite true \
  --auth-mode login

Success again.

Step: Started working on CDN for the static website
Did this: tried to create a CDN profile using Azure CLI in Git Bash.

Command:

az cdn profile create \
  --name StaticWeb-CDN \
  --resource-group StaticWeb-RG \
  --sku Standard_Microsoft


What happened:
Azure first said the resource provider Microsoft.Cdn was not registered and that it was registering it for me.

Message (part of it):

Resource provider 'Microsoft.Cdn' used by this operation is not registered. We are registering for you.
Registration succeeded.


Then immediately after that it failed with a BadRequest error about classic CDN:

(BadRequest) Azure CDN from Microsoft (classic) no longer support new profile creation.
Code: BadRequest
Message: Azure CDN from Microsoft (classic) no longer support new profile creation.


Realisation:
The SKU Standard_Microsoft is tied to the old “Azure CDN from Microsoft (classic)” and Microsoft no longer allows creating new classic CDN profiles. So I couldn’t use this path at all on my subscription.

Step: Switched approach from Classic CDN to Azure Front Door (modern CDN)
Did this: tried to create an Azure Front Door Standard profile instead of classic CDN, still in Git Bash.

Command:

az afd profile create \
  --resource-group StaticWeb-RG \
  --name StaticWeb-FD \
  --sku Standard_AzureFrontDoor


What happened:
Azure rejected it because of the subscription type (free trial / student).

Error:

(BadRequest) Free Trial and Student account is forbidden for Azure Frontdoor resources.
Code: BadRequest
Message: Free Trial and Student account is forbidden for Azure Frontdoor resources.


Realisation:
Even though Azure Front Door Standard is the modern replacement for CDN, my current Azure subscription type (Free Trial / Student) is not allowed to create any Azure Front Door resources. So:

– Classic CDN is blocked because it’s deprecated for new profiles.
– Front Door is blocked because free trial / student accounts are not allowed to use it.

Conclusion at this point:
On this subscription I can fully run a static website on Storage, but I cannot actually provision a CDN / Front Door layer. CDN for this project has to stay as a “future enhancement” for when I’m on a Pay-As-You-Go or normal paid subscription.

