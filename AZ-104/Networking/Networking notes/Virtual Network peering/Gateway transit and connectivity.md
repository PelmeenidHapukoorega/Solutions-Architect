# Notes about determining gateway transit and connectivity.md

1. Gateway transit

* Definition: It allows "spoke" virtual networks to use a VPN Gateway located in a "hub" virtual network to reach external locations (like on-premises offices).
* Shared Resource: You only pay for and manage one gateway in the hub, rather than putting a gateway in every single spoke VNet.
* Hub-and-Spoke: This is the standard architecture for enterprise environments to centralize external connectivity.

2. Connectivity capabilities

When a spoke VNet uses the hubs transit gateway it can reach:

* On-Premises: via Site-to-Site (S2S) VPN or ExpressRoute.
* Other VNets: via VNet-to-VNet connections.
* Remote Users: via Point-to-Site (P2S) VPN clients.

3. Implementation rules

* One Gateway Limit: A virtual network can have only one VPN gateway.
* Peering Support: Gateway transit works for both Regional and Global VNet peering.
* Bi-directional Settings: For transit to work, you must configure both sides of the peering:
  * Hub side: Must be set to "Allow gateway transit".
  * Spoke side: Must be set to "Use remote virtual network's gateway".

4.  Portal configuration

Portal uses specific terminology in the peering settings:

* Allow forwarded traffic: Controls if traffic passing through the VNet (not destined for it) is allowed.
* Allow gateway transit: (In the Hub) Shares its gateway with the peering partner.
* Use the remote virtual networks gateway: (In the Spoke) Opts into using the Hub's gateway for external traffic.

5. Security with NSG

* Independent Security: Network Security Groups (NSGs) still work at the subnet and NIC levels even when peering is active.
* Traffic Control: You can use NSGs to block or allow specific traffic between peered VNets or between a spoke and the on-premises network.

6. Summary

* Hub VNet > `Allow gateway transit` > Acts as the "Exit/Entry" point for the whole network.
* Spoke VNet > `Use remote gateway` > Sends external traffic to the Hub instead of its own gateway.
* Cost benefit > Shared infrastructure > Eliminates the cost of redundant VPN Gateways.
