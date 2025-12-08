# Describe Azure virtual networking

## Table of contents
* [Isolation and segmentation](#isolation-and-segmentation)
* [Communication between resources](#communication-between-resources)
* [Communication between on prem resources](#communication-between-on-prem-resources)
* [Route network traffic](#route-network-traffic)
* [Filter network traffic](#filter-network-traffic)
* [Connect virtual networks](#connect-virtual-networks)

## My interpretations

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

### Why does it exist?

It exists as a fundamental principle in computing, networking and security to achieve the following goals: Security, stability and control. Its a mechanism that prevents different systems, applications and network zones from interfering with each other, ensuring that a problem in one area doesnt spread to others.

[Isolation and segmentation](./Diagrams/Isolation%20and%20segmentation.md)

### What does it do?

It works to seperate, contain and control different parts of a system or network to enhance security, stability and management.

### How does it work?

By enforcing boundaries and rules that seperate resources, applications and network traffic into discrete, manageable and protected units. Isolation focuses on seperating individual compute resources, segmentation focuses on seperating and controlling traffic flow across a network.

### Summary

Basically isolation and segmentation is used to clearly define boundaries so resources cant get conflicted with each other, because if they do it may affect the entire system instead of 1 individual aspect. Its used to better define what does what and why and how it all communicates within the system.

## Communication between resources

* Virtual networks can connect not only VMs, but other resources such as the App service environment for power apps, AKS and azure virtual machine scale sets.
* Service endpoints can connect to other Azure resource types such as Azure SQL databases and storage acconts. This enables you to link multiple Azure resources to virtual networks in order to improve security and provide optimal routing between resources.

[Comms between resources (same VNet)](./Diagrams/Comms%20between%20resources%20(same%20VNet).md)

## Communication between on prem resources

Azure VNet allows you to link resources together in your on prem environment and withing your Azure subscription. You can create a network that spans both your local and cloud environments. To achieve this there are 3 mechanisms for it:

* Point to site VPN or virtual private network connections are from computer outside your organization back into your corporate network. In this case the client computer initiates and encrypted VPN connection to connect to the Azure VNet.
* Site to site VPN links your on prem VPN device or gateway to the Azure VPN gateway in a VNet. In effect the devices in Azure can appear as being on the local network. The connection is encrypted and works over the internet.
* Express route provides a dedicated private connectivity to Azure that doesnt travel over the internet. Its useful for environments where you need greater bandwidth and even higher levels of security.

[Comms with on prem (VPN)](./Diagrams/Comms%20with%20on%20prem%20(VPN).md)

### Route network traffic

Azure routes traffic between subnets or any connected VNets, on prem networks and the internet by default. You also can control routing and override those settings as follows:

* Route tables allow you to define rules about how traffic should be directed. You can create custom route tables that control how packets are routed between subnets.
* Border gateway protoco or BGP works with Azure VPN gateways, route server or express route to propagate on prem BGP routes to Azure VNets.

[Route network traffic (UDR)](./Diagrams/Route%20network%20traffic%20(UDR).md)

### Why does it exist?

To provide control, security and efficiency over how data travels to, and withing your private virtual networks in the cloud. It ensures that traffic is delivered securely to the correct destination and that you can enforce your network architecture and security policies.

### What does it do?

Since its the orchestration and direction of all IP based traffic to, from and withing the cloud, it uses combination of automatic rules and user defined configurations to ensure data packets reach their intended destinations securely and efficiently.

### How does it work?

It works by using a combination of default system rules and user defined rules to direct data packets to the correct destination within Azure environment.

### Summar

Route network traffic is like a system for trains that regulates the trains path to avoid collision with other trains and to make sure the train reaches its destination securely and on point.

## Filter network traffic

Enables you to filter traffic between subnets by using network security groups or NSG to define inbound and outbound security rules based on factors like source, destination IP address, port and protocol. Additionally use specialized VMs called network vritual appliances which then carries out a particular network function, such as running a firewall or performing wide area network aka WAN for optimization.

[Filter network traffic (NSG)](./Diagrams/Filter%20network%20traffic%20(NSG).md)

### Why does it exist?

To serve the goals of security, compliance and performance by creating a controlled boundary for all data entering, leaving and moving within a network. It works as the first line of defense for any organization connected to the internet.

### What does it do?

It inspects and controls every data packet that attempts to move across a network boundary like firewall or NSG. Its an enforcement mechanism based on pre defined security policies.

### How does it work?

It uses a specialized software or hardware like firewall or NSG to inspect data packets against a predefined set of sequential rules to decide whether to allow, block or redirect the data.

### Summary

Filtering network traffic is like the guy who inspects the trains before sending them out the guy being in this case either the firewall or NSG. It controls the cargo (aka data packets), makes sure that destination the train is supposed to go is correct before giving it a green light or a red light.

## Connect virtual networks

You can link VNets together by VNet peering. Peering allows 2 seperate virtual networks connect directly to each other. Network traffick between peered networks is private and travels on the microsoft backbone network, never entering the public internet. Peering enables resources in each VNet to communicate with each other. These VNets can be in seperate regions, this allows you to create a global interconnected network through Azure.

User defined routes or UDR allows you to control routing tables between subnest within a virtual network or between virtual networks which in turn allows for greater control over the network traffic flow.

[Connect virtual networks (VNet peering)](./Diagrams/Connect%20virtual%20networks%20(VNet%20peering).md)
