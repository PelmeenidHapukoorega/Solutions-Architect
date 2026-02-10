# Notes about implementing VNet traffic routing

* Azure creates automatically route table for each subnet within VNet.
* Route table has default system routes and any user defined routes required.

## System routes

* System routes are automatically created and assigned to each subnet within the VNet.
* System routes cant be created or removed but some system routes can be overrided with custom routes.
* AZ creates default system routes for each subnet and adds other optional default routes to specific subnets or every subnet when using specific AZ capabilities.

## Default system routes

* When a VNet is created default system routes are created for each subnet within the VNet.

Each system route contains an address prefix and next hop type:

<img width="701" height="317" alt="image" src="https://github.com/user-attachments/assets/1a4be6bb-5d52-4677-b212-8341f8a9e86d" />

* In routing terms a hop is a waypoint on the overall route.
* I.e next hop is the next waypoint that the traffic is directed to on its way to its ultimate destination.

Hop types are defined as follows:

* **VNet**
  * Routes traffic between address ranges within address space of a VNet.
  * AZ creates route with an address prefix that corresponds to each address range defined within the address space of a VNet.
  * AZ automatically routes traffic between subnets using the routes created for each address range.

* **Internet**
  * Routes traffic specified by the address prefix to the internet.
  * System default route specifies `0.0.0.0/0` address prefix.
  * AZ routes traffic for any address NOT specified by an address range within a VNet to the internet.
    * Unless the destination address if for AZ service.
  * AZ routes any traffic destined for its service directly to the service over backbone network, instead of routing the traffic to the internet.
  * AZ default system route for the `0.0.0.0/0` prefix can be overridden with a custom route.

* **None**
  * Traffic routed to the None next hop type is dropped, rather than routed outside the subnet.

## Optional default system routes

* AZ adds default system routes for any AZ capabilities that are being enabled.
* Depending on the capability AZ adds optional default routes to either specific subnets within the VNet or to all subnets within a VNet.

<img width="710" height="297" alt="image" src="https://github.com/user-attachments/assets/3099e0e6-029e-446d-a977-55078504a821" />

* **VNet peering**
  * When creating peering between VNets a route is added for each address range within the address space of each VNet.

* **VNet gateway**
  * If gateway is added to VNet, AZ then add 1 or more routes with VNet gateway as the next hop type.
  * Source is listed as VNet gateway becayse the gateway adds the routes to the subnet.

* **VNet service endpoint**
  * AZ adds public IP address for certain services to the route table when service endpoint is enabled to the service.
  * Service endpoints are enabled for individual subnets within a VNet.
    * Meaning that route is only added to the route table of a subnet a service endpoint is enabled for.
  * Public IP address of AZ services change periodically and AZ manages the updates to the routing tables when necessary.

## User defined routes

* UDR is useful when you want to ensure traffic between 2 subnnets passes through a firewall appliance.
* Custom routes override AZ default system routes.
* Each subnet can have 0 or 1 associated route table.
* When creating route table and associating it to a subnet, the routes within it are combined with or override the default routes AZ adds to a subnet.

You can specify following hop types when creating UDRs:

* **Virtual appliance**
  * VA is a VM that usually runs a network application like firewall.
  * When creating route with the VA hop type you also specify next hop IP address.

* **VNet gateway**
  * Specify when you want traffic destined for specific address prefixes routed to a VNet gateway.
  * VNet gateway has to be created with type VPN

* **None**
  * Specify when you want to drop traffic to address prefix, rather than forwarding traffic to a destination.

* **VNet**
  * Specify when you want to override the default routing within a VNet.

* **Internet**
  * Specify when you want to explicitly route traffic destined to an address prefix to the internet.

## Considering AZ route server

AZ route server simplifies dynamic routing between network virtual appliance aka NVA and the VNet.

It simplifies:

* Configuration
* Management
* Deployment of NVA in a VNet

## Troubleshooting with effective routes

Routing problems can be diagnosed by viewing the effective routes for a VM network interface.
