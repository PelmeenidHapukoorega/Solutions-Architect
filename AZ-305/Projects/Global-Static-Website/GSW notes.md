# Raw notes of creating global static website architecture (Azure storage + CDN + Monitoring + Logging)


Step 1, creating resource group for clean structure, easy deletion, cost control and RBAC **(pic 1)**

```
az group create \
  --name StaticWeb-RG \
  --location westeurope
```

Step 2: next i needed to create storage a storage account for static website hosting, global availability and logging + monitoring. For storage i used StorageV2 since its required for static sites and LRS for the cheapest replication.
```
az storage account create \
  --name statichermitweb \
  --resource-group StaticWeb-RG \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true
```
Ran into an error:
```
(RequestDisallowedByAzure) Resource 'statichermitweb' was disallowed by Azure: The selected region is currently not accepting new customers: https://aka.ms/locationineligible.
Code: RequestDisallowedByAzure
Message: Resource 'statichermitweb' was disallowed by Azure: The selected region is currently not accepting new customers: https://aka.ms/locationineligible.
Target: statichermitweb
```
Region full i.e it hit a capacity limit for free tier of certain resources, in this case storage

Switched region for storage to northeurope instead:
```
az storage account create \
  --name statichermitweb \
  --resource-group StaticWeb-RG \
  --location northeurope \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access true \
```
Success: **(Pic2)**

Step 3 Enabling static website hosting on the storage account:

```
az storage blob service-properties update \
  --account-name statichermitweb \
  --static-website \
  --index-document index.html \
  --404-document error.html
```
Azure created a special container for me called $web. This is now my public website root. It expects index.html and error.html

Step 4 Uploading files to website index.html and error.html

Uploading index.html
```

