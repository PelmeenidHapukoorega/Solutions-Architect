# Notes about enabling cross Vnet connectivity with peering

* VNet peering enables to seamlessly connect separate VNets with optimal network peformance regardless of being in the same region (VNet peering) or in different regions (global VNet peering).
* Traffic between peered VNets is private.
* VNets would appear as 1 for connectivity purposes.
* Traffic between VMs in peered VNet uses MS backbone infra meaning:
  * No public interner, gateways or encruyption is required in the communication between VNets.


## Types of VNet peerings

* **Regional VNet peering**
  * Connects VNets in the same region.

* **Global VNet peering**
  * Connects VNets in different regions.
  * Peered VNets can exist in any Azure public cloud or China cloud regions. But not in Government cloud regions.
  * You can only peer VNet in the same region in Government cloud regions.

  Diagram:
  ```ascii
  +--------------+                      +------------------------------------------+
  |   Region 1   |                      |                 Region 2                 |
  |              |                      |                                          |
  |  [ VNet1 ]   |<-------------------->|  [ VNet2 ] <----------------------> [ VNet3 ]
  |              |        Global        |                 Regional                 |
  +--------------+     VNet Peering     |               VNet Peering               |
                                        +------------------------------------------+
  ```

  ## Benefits of VNet peering

  * Low-latency, high-bandwidth connection between resources in different VNets.
  * Ability to apply NSG in either VNet to block access to other VNets or subnets.
  * Ability to transfer data between VNets across:
    * Subscriptions.
    * Entra tenants.
    * Deployment models
    * Regions.
  * Ability to peer VNets created through ARM
  * Ability to peer VNets created through resource manager to one created through classic deployment model.
  * No downtime to resources in either VNet is required to create peering or after peering creation.

## Configuring VNet peering

Steps to configure VNet peering.

1. Creation of 2 VNets
2. Peer the VNets
3. Create VMs in each VNet
4. Test communication between VMs

## Gateway transit and connectivity

* VPN gateway can be configured in peered VNet as gateway transit point.
* Gateway transit is peering property that lets 1 VNet use the VPN gateway in peered VNet for cross-premises or VNet-to-VNet connectivity.
* VNet can have only 1 gateway.
* Peered VNet uses remote gateway to gain access to resources.
* Gateway transit is supported for both VNet peering and Global VNet peering.

Gateway transit allows VNets to communicate to resources outside the peering.

Example: Subnet gateway could:

* Use site-to-site VPN to connect to on-premises network.
* Use VNet-to-VNet connection to another VNet.
* Use point-to0site VPN to connect to a client.

In these cases transit allows peered VNets to share gateway and get access to resources meaning that there is no need to deploy a VPN gateway in the peer VNet.

**Note**

NSG can be applied in either VNet to block access to other VNets or subnets.
