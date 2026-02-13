# Notes about Load balancer

1. Core function

* Traffic Distribution: It distributes incoming network traffic equitably across a group of backend resources (VMs or Virtual Machine Scale Sets).
* Performance vs. Scale: It is more effective to use a "pool" of multiple smaller servers rather than one giant, expensive server.
* Protocol Support: Operates at Layer 4 of the OSI model, supporting millions of TCP and UDP flows.

2. Public VS internal load balancers

* Public Load Balancer:
  * Used for Internet-facing traffic.
  * Maps a Public IP to the private IPs of your backend VMs.
  * Can also provide outbound internet connectivity for VMs inside the VNet.

* Internal (Private) Load Balancer:
  * Used for internal traffic only (Private IP on the frontend).
  * Never exposed to the public internet.
  * Commonly used in multi-tier apps (e.g sending traffic from web servers to internal database servers).

3. Key components

* Load-Balancing Rules: Define exactly how traffic is mapped from the frontend (incoming port) to the backend (server port).
* Health Probes: These are "heartbeat" checks. If a VM fails a health probe, the Load Balancer stops sending traffic to it until it becomes healthy again.
* Backend Pool: The group of VMs or instances that receive the distributed traffic.

4. Common use cases

* High Availability: Ensures your application stays up even if 1 or 2 servers fail.
* Multi-Tier Apps: Balancing traffic between an internet-facing frontend and a private backend tier.
* LOB Applications: Scaling internal Line-of-Business apps accessed via VPN from on-premises.

## Summary
```
+----------------+--------------------------+--------------------------+
|    FEATURE     |   PUBLIC LOAD BALANCER   |  INTERNAL LOAD BALANCER  |
+================+==========================+==========================+
| Source         | Public Internet          | VNet or VPN/ExpressRoute |
+----------------+--------------------------+--------------------------+
| Frontend IP    | Public IP address        | Private IP address       |
+----------------+--------------------------+--------------------------+
| Typical Target | Web Servers              | Database or App Tiers    |
+----------------+--------------------------+--------------------------+
| Visibility     | Exposed to Internet      | Hidden from Internet     |
+----------------+--------------------------+--------------------------+
```
