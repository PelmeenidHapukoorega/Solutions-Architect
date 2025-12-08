# Describe Azure virtual private networks

## Table of contents
* [VPN gateway](#VPN-gateway)
* [High availability scenarios](#high-availability-scenarios)

## My interpretations

Virtual private network uses encrypted tunnel within another network. VPNs are typically deployed to connect 2 or more trusted private networks to one another over an untrusted network (typically public internet). Traffic is encrypted while traveling over the untrusted network to prevent eavesdropping or another attacks. VPNs can enable networks safely and securely share sensitive information.

### Why does it exist?

To provide secure connectivity over the public internet (without exposing traffic to the public internet)between your on premises network and your Azure VNet or between different VNets within Azure. It creates a secure encrypted tunnel to extend your corporate network into the cloud or within the cloud. 

### What does it do?

Establishes secure, encrypted connections to extend your on prem network securely across the public internet to your Azure VNet. Its a gateway to securely connect different networks.

### How does it work?

It works by using the VPN gateway service to create secure, encrypted tunnles through the public internet or private connections, effectively extending your private network. The process depends on the connection type, but both rely on the VPN gateway for management.

### Summary

VPN is so you could securely connect over the public internet without exposing traffic to the public internet and make yourself vulnerable to attacks. Its the secure way to connect and extend your network into the cloud or within the cloud.

## VPN gateway

VPN gateway is a type of VNet gateway. Azure VPN gateway instances are deployed in a dedicated subnet of the VNet and enables the following connectivity:

* Connect on prem data centers to VNets through site to site (S2S) connection.
* Connect individual devices to VNets through point to site (P2S) connection.
* Connect VNets to other VNets though a network to network connection.

### Why does it exist?

To provide a secure, encrypted and managable way to connect and extend private networks across the public internet. Its the cloud service equivalent of a physical router/firewall that creates VPN tunnels, making secure hybrid connectivity possible,

### What does it do?

It establishes secure, encrypted connections to extend your on prem network securely across the public internet to your Azure VNet or to connect different Azure VNets. It acts as a gateway to securely connect otherwise isolated networks.

### How does it work?

It works by using VPN gateway service to create secure, encrypted tunnles through public internet or private connections, effectively extending your private network. The process depends on the connection type (S2S or P2S) but both rely on the VPN gateway for management.

### Additional information about gateways

* All data is encrypted inside a private tunnel as it crosses the internet.
* You can deploy only 1 VPN gateway in each VNet.
* You can use 1 gateway to connect to multiple locations, including other VNets and on prem data centers.
* When setting up VPN gateway you must specify the type of VPN. Policy based or Route based.
* Difference between Policy based and Route based is which traffic needs encryption.
* In Azure, regardless of the VPN type, the method of authentication employed is a preshared key.

1. Policy based VPN gateway: Specifies statically the IP address of packets that should be encrypted through each tunnel
   * This type of device evaluates every data packet against those sets of IP addresses to choose the tunnel where that           packet is going to be sent through.
2. Route based gateway: IPSec tunnels (Internet protocol) are modeled as a network interface or virtual tunnel interface.
   * IP routing (static routes or dynamic routing protocols) decides which of these tunnel interfaces to use when sending         each packet.
   * Route based VPNs are the preferred connection method for on prem devices. These are more resilient to topology changes       such as creation of new subnets.

### Use cases

Use route based VPN gateway if you need:

* Connections between VNets.
* P2S connections.
* Multiple connections.
* Coexistance with Azure ExpressRoute gateway.

### Summary

VPN gateway is like the security guy inside the club who determines who gets into the VIP section and who doesnt. The clubs main area is like the public internet, and the security guy in front of the VIP section makes sure that only the expected guests in the VIP section are allowed and none of the randoms who werent invited. 
I.e it seperates untrusted public traffic from your trusted private networks.

## High availability scenarios

In case you are configuring VPN to keep information safe, you also want to be sure that its highly available and fault tolerant VPN configuration.

### Active/standby

By default VPN gateways are deployed as 2 instances in an active/standby configuration, even if you only see 1 VPN gateway resource within Azure. If unplanned maintenance or say disruption affects the active instance, the standby instance automatically assumes responsibility for connections without any user meddling. Connections are interrupted during this failover, but they typically restore within a few seconds or plannend maintenance and within 90 seconds for unplanned disruptions.

### What does it mean?

It means Azure VPN gateways are designed with automatic redundancy and high availability built in, even though you only manage a single resource in the portal.

### Active/active

By introducing BGP routing protocol you can deploy VPN gateways in an active/active configuration. In this you assign a unique public IP address to each instance. Then you create seperate tunnles from the on prem device to each IP address. You can extend the high availability by deploying additional VPN device on prem.

### What does it mean?

By leveraging BGP routing protocol you can upgrade the reliability and performance of your VPN connection from default configuration.

## ExpressRoute failover

Another option for high availability is to configure VPN gateway as a secure failover path for ExpressRoute connections. ExpressRoute circuits have resiliency built in, but they arent immune to physical problems that affect the cables delivering connectivity or even outages that affect the complete ExpressRoute location. In high availability scenario where there is risk associated with an outage of an ExpressRoute circuit, you can provision a VPN gateway that uses internet as alternative method for connection. This way you ensure there is always a connection to the VNet.

### What does it mean? 

It means you can use the VPN gateway as a backup plan for highly available but expensive ExpressRoute circuit, thus ensuring that your connection to Azure never completely fails.

## Zone redundant gateways

Regions that support Availability zones, VPN gateways and ExpressRoute gatways can be deployed in zone redundant configuration. This brings resiliency, scalability and higher availability to VNet gateways. Deploying gateways in availability zones physically and logically seperate gateways within a region while still protecting your on prem network connectivity to Azure from zone level failures (meteor strike!). These gateways require different gateway stock keeping units aka SKUs and use standard public IP addresses instead of basic public IP addresses.
