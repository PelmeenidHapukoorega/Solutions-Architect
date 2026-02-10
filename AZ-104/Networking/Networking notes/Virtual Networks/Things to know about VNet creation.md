# Notes about creating VNets

Things to know about creating virtual networks

* When creating VNet, IP address space for the network needs to be defined.
* Plan to use an IP address space thats not already in use in the organization.
* The address space for the VNet can be either on premises or in the cloud but never both.
* To create a VNet you need to define at least 1 subnet:
  * Each subnet contains a range of IP addresses that fall within the VNet address space.
  * Address range for each subnet must be unique withing the address space for the VNet.
  * Range for 1 subnet cant overlap with other subnet IP address ranges in the same VNet.

* When creating VNet in the portal you have to provide `subscription`, `resource group` where its going to be stationed in, `virtual network name`, and service `region` for the network.

**Note**

Default limits on Azure networking resource can change periodically.

Resource to be used: https://learn.microsoft.com/en-us/azure/networking/

## Creating VNet using PWSH

1. Creation:
```PWSH
$vnet = @{
    Name = 'vnet-1'
    ResourceGroupName = 'test-rg'
    Location = 'eastus2'
    AddressPrefix = '10.0.0.0/16'
}
$virtualNetwork = New-AzVirtualNetwork @vnet
```

2. Creating subnet configuration
```PWSH
$subnet = @{
    Name = 'subnet-1'
    VirtualNetwork = $virtualNetwork
    AddressPrefix = '10.0.0.0/24'
}
$subnetConfig = Add-AzVirtualNetworkSubnetConfig @subnet
```

3. Associating subnet configuration to the VNet
```PWSH
$virtualNetwork | Set-AzVirtualNetwork
```

## Creating VNet with CLI

Creating VNet and subnet
```PWSH
az network vnet create \
--name vnet-1 \
--resource-group test-rg \
--address-prefix 10.0.0.0/16 \
--subnet-name subnet-1 \
--subnet-prefixes 10.0.0.0/24
```
