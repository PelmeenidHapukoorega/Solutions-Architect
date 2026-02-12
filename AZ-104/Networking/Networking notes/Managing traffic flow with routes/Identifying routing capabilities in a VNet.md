# Notes about identifying routing capabilities in a VNet

1. System routes (defaults)

* Automatic: Azure creates these for every subnet by default. You cannot create or delete them.
* Connectivity: By default, all subnets in a VNet can communicate with each other, and all can reach the Internet (0.0.0.0/0).
* The "None" Hop: Azure automatically creates "None" routes for private IP ranges (like 10.0.0.0/8). This means traffic to these ranges is dropped unless they are part of your VNet address space.

2. User-defined routes (UDR)

* Overriding Power: UDRs allow you to manually override Azures system routes.
* Common Use Case: Forcing traffic through a Network Virtual Appliance (NVA) or Firewall for inspection instead of letting subnets talk directly.
* Service Tags: You can now use Service Tags (like `Storage` or `SQL`) as the destination prefix in a UDR, making it easier to route traffic to Azure services.

3. Next hop types

When creating custom route, you must tell azure where to send the traffic next:

* Virtual Appliance: Sends traffic to a VMs private IP (usually a Firewall or NVA).
* Virtual Network Gateway: Sends traffic to a VPN or ExpressRoute gateway.
* Internet: Forces traffic out through the Azure infrastructure to the public web.
* None: Acts as a "black hole" to drop traffic.

4. Border Gateway Protocol (BGP)

* Hybrid Networking: Standard protocol used to exchange routing info between your on-premises network and Azure.
* Usage: Typically used with ExpressRoute or Site-to-Site VPN.
* Resiliency: BGP allows the network to automatically re-route traffic if a specific path goes down.

5. Route selection logic (Priority rules)

If multiple routes exist for the same destination, Azure follows two strict rules to decide which one to use:

**Rule A: Longest Prefix Match (Specificity wins)**

* Azure chooses the most specific route.
* Example: If one route is for `10.0.0.0/16` and another is for `10.0.0.0/24`, Azure will pick the /24 because it is a smaller, more specific range.

**Rule B: The Priority Order (If prefixes are identical)**

If two routes have the exact same address prefix, Azure picks based on this hierarchy:

1. **User-Defined Routes (UDR)** > Highest Priority
2. **BGP Routes**
3. **System Routes** > Lowest Priority

6. Summary

* UDR > Manual (User) > `Priority` 1 (highest) > Security/firewalling (NVA).
* BGP > Automatic (hybrid) > `Priority` 2 > Connecting on-prem to azure.
* System > Automatic (hybrid) > `Priority` 3 (lowest) > Basic VNet and internet connectivity
