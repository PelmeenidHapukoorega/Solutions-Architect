# Module: Azure compute and networking services
# Virtual machine creation and Nginx installation lab notes

To build anything meaningful in Azure compute you gotta master VM deployment and extension configuration. VM + networking + extensions is the foundation for real application hosting. 

**Bottom line:** If you can deploy a VM and automate setup with extensions, you’re already thinking like an architect or like Megamind after three energy drinks, which is honestly the same thing.

## Task 1 Create a Resource Group (Cloud Shell)

**Steps I took**

1. Logged into Azure and opened Cloud Shell (Azure CLI).
2. In top-right corner → opened Cloud Shell.
3. Ran the following command to create a resource group:

```bash
az group create \
--name MinuVirtukas \
--location eastus
```
4. Resource group MinuVirtukas created successfully.
[![Resource Group](../screenshots/resourcegrp.PNG)](../screenshots/resourcegrp.PNG)

## Task 2 Create a Linux Virtual Machine

1. Tried to create the VM using:
```bash
az vm create \
--resource-group MinuVirtukas \
--name my-vm \
--size Standard_D2s_v5 \
--public-ip-sku Standard \
--image Ubuntu2204 \
--admin-username virtualhermit \
--generate-ssh-keys
```

2. Hit the following SKU availability error:
```bash
Message: The requested VM size for resource 
'Following SKUs have failed for Capacity Restrictions: Standard_D2s_v5'
is currently not available in location 'eastus'.
```
What I understood:
SKU Standard_D2s_v5 wasn’t available in eastus. Needed to try another region.

3. Deleted the original RG to restart clean:
```bash
az group delete --name MinuVirtukas
```
### Checking SKU availability in another region

Needed to verify which regions had the correct VM SKU.

1. Listed SKUs for norwayeast:
```bash
az vm list-skus --location norwayeast --output table
```
[![Resource Group](../screenshots/skulist.PNG)](../screenshots/skulist.PNG)

2. Filtered SKUs down to exactly what I needed:
```bash
az vm list-skus \
--location norwayeast \
--size Standard_D2s \
--all false \
--output table
```

This reduced the results drastically:
[![Resource Group](../screenshots/filteredskus.PNG)](../screenshots/filteredskus.PNG)

**Outcome:**
Standard_D2s_v5 was available in norwayeast.

### Attempt 2 Successful VM Creation

1. Recreated the resource group in correct region:
```bash
az group create \
--name MinuVirtukas \
--location norwayeast
```

2. Created the VM again:
```bash
az vm create \
--resource-group MinuVirtukas \
--name minu-virtukas \
--size Standard_D2s_v5 \
--public-ip-sku Standard \
--image Ubuntu2204 \
--admin-username virtualhermit \
--generate-ssh-keys
```
VM successfully deployed.
[![Resource Group](../screenshots/VMcreation.PNG)](../screenshots/VMcreation.PNG)

## Task 3 Install Nginx Using Custom Script Extension

Used the Custom Script Extension to automatically install Nginx on the VM:
```bash
az vm extension set \
--resource-group MinuVirtukas \
--vm-name minu-virtukas \
--name customScript \
--publisher Microsoft.Azure.Extensions \
--version 2.1 \
--settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
--protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```
Nginx installed successfully.
[![Resource Group](../screenshots/Nginx.PNG)](../screenshots/Nginx.PNG)

## What I learned
### Key points
* Cloud Shell is perfect for quick deployments without local setup
* VM SKUs vary by region → always verify before creating
* Deleting/recreating RGs keeps lab environments clean
* Using list-skus filtering saves tons of time
* Custom Script Extension = clean automation for config
* Understanding region and SKU issues is part of learning how Azure really works in practice.

## Takeaway
This lab teaches a core architect mindset: Region matters, availability matters, automation matters. Deploying the VM is simple but troubleshooting capacity, picking the right SKU, and automating configuration is the skillset.

## References
https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/

https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-sku-not-available?tabs=azure-cli
