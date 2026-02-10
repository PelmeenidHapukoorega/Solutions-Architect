# Main takeaways from "Introduction to AZ Vnets" module

* Azure virtual networks enable resources in Azure to securely communicate with each other, the internet, and on-premises networks.
* A subnet is a range of IP address in a virtual network. Each subnet must have a unique address range.
* Public networks like the Internet communicate by using public IP addresses. Private networks like your Azure Virtual Network use private IP addresses.
* A dynamic public IP address can change over the lifespan of the Azure resource. A static public IP address doesn't change over the lifespan of the Azure resource.
* Azure provides both public and private DNS resolution.
* Virtual network peering enables you to seamlessly connect two Azure virtual networks. Peering can be either global or regional.
* Azure automatically creates system routes and assigns the routes to each subnet in a virtual network. Each route contains an address prefix and next hop type.
* You can create custom, or user-defined(static), routes. To troubleshoot routing issues, you can view the effective routes for each network interface.
* NAT enables you to share a single public IPv4 address among multiple internal resources.

# Main takeaways from "Configuring VNets" module

* Azure virtual networks allow different Azure resources to securely communicate with each other, the internet, and on-premises networks.
* Subnets within virtual networks provide logical divisions, improving security, performance, and management.
* When creating virtual networks, ensure that the IP address space is unique and doesn't overlap with other subnets.
* IP addresses can provide public or private access to resources.
