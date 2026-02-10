# Notes about subnets

Things to remember about subnets in general

* Each subnet contains a range of IP addresses that fall within the virtual network address space.
* The address range for a subnet has to be unique within the address space for the VNet.
* The range for 1 subnet cant overlap with other subnet IP address ranges in the same VNet.
* The IP address space for a subnet must be specified by using CIDR notation.
* VNet can be segmented into 1 or more subntes in the portal. Characteristics about the IP addresses for the subnets are listed:

<img width="1138" height="349" alt="image" src="https://github.com/user-attachments/assets/b4540072-0bbb-4408-a5af-d7f1b74176e8" />

## Reserved addresses

For each subnet Azure reserves 5 IP addresses. First 4 addresses and the last addresses are reserved.

**Example**

Reserved address in an IP address range of `192.168.1.0/24`.

<img width="700" height="231" alt="image" src="https://github.com/user-attachments/assets/61cb42fd-b234-4241-821d-47ed031dc02f" />


## Things to consider when using subnets

* **Service requirements:**

Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets. A service might require or create their own subnet. There must be enough unallocated space to meet the service requirements. Suppose you connect a virtual network to an on-premises network by using Azure VPN Gateway. The virtual network must have a dedicated subnet for the gateway.

* **Network virtual appliances:**

Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.

* **Network security groups:**

You can associate zero or 1 network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.

* **Private links:**

Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.
