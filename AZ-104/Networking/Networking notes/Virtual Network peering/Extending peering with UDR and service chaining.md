# Notes about extending peering with UDR and service chaining

1. Peering is non-transitive (Most important rule)

* The Logic: If VNet-A is peered with VNet-B, and VNet-B is peered with VNet-C, VNet-A and VNet-C cannot communicate through VNet-B.
* The Fix: To get A to talk to C, you must either peer them directly or use User-Defined Routes (UDRs) and a Network Virtual Appliance (NVA) in the middle.

2. Hub-and-Spoke architecture

* The Hub: A central VNet that hosts shared resources like VPN Gateways, Azure Firewall, or Network Virtual Appliances (NVAs).
* The Spokes: Multiple VNets that peer only with the Hub. They use the Hubs resources to reach the internet or on-premises networks.

3. User-defined routes (UDR)

* Custom Routing: UDRs allow you to override Azures default system routes.
* Next Hop: You can create a route that tells a subnet: "To reach the Internet, your Next Hop is the private IP of the Firewall in the peered Hub VNet."
* Peering Support: A UDR can point to an IP address or a gateway located in a peered virtual network.

4. Service chaining

* The Definition: This is the process of "chaining" network traffic through a series of services (like a firewall or load balancer) before it reaches its final destination.
* How its done: You implement service chaining by configuring UDRs in your spoke VNets that point to an NVA or Gateway in the Hub VNet as the "Next Hop."

5. Why use service chaining?

* Security: Forcing all traffic from spokes through a central Hub firewall for inspection.
* Compliance: Ensuring all outbound internet traffic is logged and filtered in one central location.
* Efficiency: Centralizing expensive resources (like 10Gbps Firewalls) so all spokes can share them rather than buying one for every VNet.

6. Summary

* Non-transitive > Peering doesn't "pass through" (A > B > C is blocked)
* Hub-and-Spoke > Best way to centralize management and shared resources.
* UDR > Manually tells traffic where to go (overrides Azure defaults).
* Service chaining > Using UDRs to force traffic through a security appliance (NVA).
