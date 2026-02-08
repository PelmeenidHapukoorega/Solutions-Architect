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
