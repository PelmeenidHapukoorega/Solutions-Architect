# Notes abut Azure virtual networks in general

* **Communication with the internet**

All resources in a VNet can communicate outbound to the internet, by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer. You can also use public IP or public Load Balancer to manage your outbound connections.

* **Communication between Azure resources**

There are three key mechanisms through which Azure resource can communicate: 

* VNets
* VNet service endpoints
* VNet peering

Virtual Networks can connect not only virtual machines (VMs), but other Azure Resources, such as the App Service Environment, Azure Kubernetes Service, and Azure Virtual Machine Scale Sets. You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts. When you create a VNet, your services and VMs within your VNet can communicate directly and securely with each other in the cloud.

* **Communication between on premises resources**

Securely extend your data center. You can connect your on-premises computers and networks to a virtual network using any of the following options: Point-to-site virtual private network (VPN), Site-to-site VPN, Azure ExpressRoute.

* **Filtering network traffic**

You can filter network traffic between subnets using any combination of network security groups and network virtual appliances.

* **Routing network traffic**

Azure routes traffic between subnets, connected virtual networks, on-premises networks, and the Internet, by default. You can implement route tables or border gateway protocol (BGP) routes to override the default routes Azure creates.

## Design considerations

### Address space and subnets

Multiple VNets can be create per region and per subscription. 

Multiple subnets can be created withing each VNet

**Virtual networks**

When creating a VNet use address ranges enumared in RFC 1918. These address are for private, nonroutable address spaces:

* 10.0.0.0 - 10.255.255.255 (10/8 prefix)
* 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
* 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)

In addtion these address ranges are reserved:

* 224.0.0.0/4 (Multicast)
* 255.255.255.255/32 (Broadcast)
* 127.0.0.0/8 (Loopback)
* 169.254.0.0/16 (Link-local)
* 168.63.129.16/32 (Internal DNS)

**Subnets**

Subnet is a range of IP address in the VNet.

You segment (seperate) VNets into different size subnets and then deploy resources in a specific subnet.

* Smallest supported IPv4 subnet is /29
* Largest supported IPv4 subnet is /2

These are defined by CIDR subnet definitions.

* IPv6 subnets must be exactly /64 

**When implementing subnets consider:**

* Each subnet has to have unique address range in `Classless Inter-Domain Routing` CIDR format.
* Certain services in Azure require their own subnet.
* Subnets can be used for traffic management.
  * **Example:**
    * You can create subnets to route traffic through a network virtual appliance (NVA)
* Access can be limited to AZ resources to specific subents with a VNet service endpoint.
* Multiple subnets can be created.
* Service endpoint can be enabled for some subnets but not others.

**Considerations for virtual networks:**

* Ensure nonoverlapping address spaces. Ensure CIDR block doesnt overlap with organizations other network ranges.
* Is any security isolation required?
* Is there a need to mitigate any IP addressing limitations?
* Are there connections between VNets and on-premises networks?
* Is there any isolation required for administrative purposes?
* Am i using and services that create their own VNets?
