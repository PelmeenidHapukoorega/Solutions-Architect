# Describe Azure virtual networking

Azure virtual networks and virtual subnets enable Azure resources to communicate with each other, with users over the iternet
and with your on prem client computers. Think of it as an extension of your on prem network with resources that link other Azure resources.

### Why does it exist?

To provide logically isolated private network environment in the cloud allowing users to securily connect their Azure resources,
enable communication with the internet and connect back to their on premises data centers.

### What does it do?

Its the fundamental networking service in Azure that provides secure, private and isolated network for all your cloud resources.
It essentially gives you your own customizable virtual data center network in the cloud.

### How does it work?

It works by using software defined networking SDN to create private, isolated and highly customizable network structure on top of Microsoft physical cloud infra. 

* When you create a VNet you define a large, private IP address space.
* Any resource deployed into this VNet is automatically assigned a private IP address from 10.0.0.0/16 range for internal communication.
* You divide the VNets IP space into smaller specific blocks aka Submets. Subnets serve the group resources by function or security level. Resources in one subnet can only talk to resources in another subnet based on security rules you set.
* You are gonna have to set network security groups NSG, these are virtual firewalls that you attach to the subnets or individual resources like VMs. They contain the rules that explicitly allow or deny inbound and outbound traffic based on source/destination IP, port and protocol. Filters communication.
* Azure automatically provides system routes so resources in the same VNet can communicate by default.
* You can define user defined routes or UDRs to force traffic to go through specific network appliances before reaching its destination, which in turn allows for centralized security inspection.
* Specialized services use the VNet as a hook:
  * VNET peering connects 2 VNets together directly even in different regions.
  * VPN gateway or ExpressRoute securily connects the VNet back to your office or data center.

### Summary

VNet enables communication between resources which is essential if you want to direct your traffic. Its the product manager of Azure, every bit of communication goes through it, and then it decides where to send the provided information based on clear rules set. 

## Isolation and segmentation

Azure virtual network allows you to create multiple isolated virtual networks.
