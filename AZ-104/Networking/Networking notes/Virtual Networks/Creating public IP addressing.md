# Notes on creating public IP addressing

Public IP address can be created in Portal. You could create public IP address for a virtual machine:

<img width="610" height="408" alt="image" src="https://github.com/user-attachments/assets/5f440450-1915-4a4b-b8da-a35002dae8f7" />

## Creating public IP via PWSH
```PWSH
$ip = @{
    Name = 'myStandardPublicIP'
    ResourceGroupName = 'QuickStartCreateIP-rg'
    Location = 'eastus2'
    Sku = 'Standard'
    AllocationMethod = 'Static'
    IpAddressVersion = 'IPv4'
    Zone = 1,2,3
}
New-AzPublicIpAddress @ip
```

**Note**

For `Az.Network` modules older than 4.5.0 the command above needs to be ran without specifying zone parameter to create zone-redundant IP address.

## Creating public IP via CLI
```CLI
az network public-ip create \
--resource-group QuickStartCreateIP-rg \
--name myStandardPublicIP \
--version IPv4 \
--sku Standard \
--zone 1 2 3
```

* **Note**
  * For versions of the API older than 2020-08-01 the command needs to be executed without specifying `--zone` parameter to create a zone-redundant IP address.

## Things to consider when creating public IP address

* **IP Version**
  * Public IPv4 addresses can be attached to a load balancer or to a network interface. Public IPv6 addresses can only be attached to load balancers. IPv4 and IPv6 addresses are charged at the same rate.

* **SKU**
  * Select SKU for the public IP address. A public IPs SKU must match the SKU of the load balancer with which it is used.

* **Tier**
  * Must match the load balancer tier.
  * Cross-region load balancer distributes traffic across regional backends.
  * Regional distributes traffic within a virtual network

* **IP address assignment**
  * Static addresses are assigned when a public IP address is created.
  * Static addresses arent released until a public IP address resource is deleted.
