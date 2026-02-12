# Mini lab: Creating custom routes

Here i created route table, custom route and subnets. Then i associated the route table with the subnets to further my understanding on how it all works and how is it beneficial.

1. Created resource group:
```bash
az group create --name customroutestest --location norwayeast
```

Output:

<img width="792" height="268" alt="image" src="https://github.com/user-attachments/assets/6b8b0233-8649-4cae-a3b7-349f82471c16" />

2. Then i created route table in the resource group:
```bash
az network route-table create \
--name publictable \
--resource-group "customroutestest" \
--disable-bgp-route-propagation false
```

Output:

<img width="939" height="347" alt="image" src="https://github.com/user-attachments/assets/e0f1f9dd-667e-4380-9e16-3da2dd343127" />

I used `--disable-bgp-route-propagation false` so that route table would accept and use routes learned dynamically from BGP peers.

If the statement were set to `true` it would block BGP routes from entering the route table and only the static UDRs would apply.

3. Next i created custom route with hop type and next hops IP address:
```bash
az network route-table create \
--route-table-name publictable \
--resource-group "customroutestest \
--name productionsubnet \
--address-prefix 10.0.1.0/24 \
--next-hop-type VirtualAppliance \
--next-hop-ip-address 10.0.2.4
```

Output:

<img width="939" height="398" alt="image" src="https://github.com/user-attachments/assets/15c38672-cce6-4e6b-9e1f-c2cec81ee6ee" />

## Creating VNets and subnets

I needed to create 3 subnets: publicsubnet, privatesubnet and dmzsubnet

1. VNet and Publicsubnet creation:
```bash
az network vnet create \
--name vnet \
--resource-group "customroutestest" \
--address-prefixes 10.0.0.0/16 \
--subnet-name publicsubnet \
--subnet-prefixes 10.0.0.0/24
```

2. Privatesubnet creation:
```bash
az network vnet subnet create \
--name privatesubnet \
--vnet-name vnet \
--resource-group "customroutestest" \
--address-prefixes 10.0.1.0/24
```

3. Created dmzsubnet:
```bash
az network vnet subnet create \
--name dmzsubnet \
--vnet-name vnet \
--resource-group "customroutestest" \
--address-prefixes 10.0.2.0/24
```

4. Ran VNet subnet list to confirm that all 3 subnets now existed in the VNet:
```bash
az network vnet subnet list \
--resource-group "customroutestest" \
--vnet-name vnet \
--output table
```

Output:

<img width="942" height="287" alt="image" src="https://github.com/user-attachments/assets/920bd10b-f2a3-4af8-aaf5-6073de164b4e" />

## Associating route table with public subnet

Associated the route table with public subnet to force outbound traffic through NVA.
``` bash
az network vnet subnet update \
--name publicsubnet \
--vnet-name vnet \
--resource-group "customroutestest \
--route-table publictable
```

Output:

<img width="944" height="455" alt="image" src="https://github.com/user-attachments/assets/f62aa3da-17a0-4adc-8fa4-bf0e74a8ca31" />

**Note**

Route table is associated only when you want to change how the specific subnet routes outbound traffic.

Performed cleanup:
```bash
az group delete --name customroutestest
```

## Summary

## What i learned
