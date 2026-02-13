# Notes on when to use load balancer

1. When to use it

* Ultra-Low Latency: The primary choice when your application requires high performance and the lowest possible latency.
* Layer 4 Functionality: Best for replicating the behavior of traditional on-premises network hardware devices.
* TCP/UDP Protocols: Handles all UDP and TCP traffic, capable of scaling to millions of requests per second.
* Multi-Tier Apps: Ideal for balancing traffic between different internal tiers (e.g between a Web tier and a Data Analysis tier).
* Administrative Access: Use Inbound NAT rules to provide RDP or SSH access to specific VMs for management.
* High Availability: Supports Zone-redundancy ensuring traffic is distributed across different Availability Zones.

2. When NOT to use load balancer

* Single VM Instances: If an application runs on a single VM with low traffic, a load balancer adds unnecessary cost and complexity.
* Layer 7 Requirements: It cannot perform URL path-based routing, SSL/TLS offloading, or cookie-based affinity.
* Web Application Firewall (WAF): If you need built-in protection against web vulnerabilities (SQL injection, etc), Azure Load Balancer is not appropriate.

3. Comparison with other solutions

* Azure Application Gateway: A Regional Layer 7 solution. Use this for SSL termination and path-based routing within a single region.
* Azure Front Door: A Global Layer 7 solution. Use this for cross-region web applications, site acceleration, and global failover.
* Azure Traffic Manager: A DNS-based global load balancer. It operates at the domain level: it is slower than Front Door because it relies on DNS TTLs and caching.

Traffic services comparison table:
```
+-------------------+-------+----------+---------------------------------------+
|      SERVICE      | LAYER |  SCOPE   |             KEY USE CASE              |
+===================+=======+==========+=======================================+
| Load Balancer     | L4    | Regional | Ultra-low latency TCP/UDP traffic.    |
+-------------------+-------+----------+---------------------------------------+
| App Gateway       | L7    | Regional | SSL offload, WAF, Path-based routing. |
+-------------------+-------+----------+---------------------------------------+
| Front Door        | L7    | Global   | Global web apps, Caching, WAF.        |
+-------------------+-------+----------+---------------------------------------+
| Traffic Manager   | DNS   | Global   | Domain-level routing, Global DR.      |
+-------------------+-------+----------+---------------------------------------+
```

Key differences recap:

* **Load Balancer** is about raw speed and network-level protocols (TCP/UDP).
* **Application Gateway** is about web-specific intelligence (HTTPS, URLs) inside one region.
* **Front Door** is the "supercharged" global version of Application Gateway.
* **Traffic Manager** is just a "switchboard" that tells your browser which IP to go to based on your location.
