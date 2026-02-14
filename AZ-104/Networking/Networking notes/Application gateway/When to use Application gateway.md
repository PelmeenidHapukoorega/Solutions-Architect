# Notes on when to use Application gateway

1. When to use Application gateway

* Web-Specific Security (WAF): Essential if you need built-in protection against SQL injection and Cross-site scripting (XSS) before traffic hits your servers.
* TLS/SSL Performance: Use it to offload encryption tasks from your backend servers, freeing up their CPU for application logic.
* Stateful Applications: Necessary for apps that store session data locally on servers: its Session Affinity ensures users stay on the same server throughout their visit.
* Hybrid Connectivity: Capable of routing traffic from an Azure endpoint to backend servers sitting in your on-premises datacenter.
* Intelligent Routing: Ideal for complex web apps needing Path-based (/video vs /images) or Multi-site (host1.com vs host2.com) routing.

2. When NOT to Use Azure Application Gateway

* Low Traffic Apps: If your app runs on a single server and handles its current load fine, a gateway is unnecessary cost and complexity.
* Non-HTTP/S Traffic: If you are balancing raw TCP or UDP (like gaming or database traffic), use Azure Load Balancer instead.
* Global Traffic Needs: Application Gateway is regional. If you need to load balance web apps across multiple global regions, Azure Front Door is the better choice.

3. Quick comparison: The big 4 traffic services

* Azure Front Door (Global L7): Think of this as a "Global Application Gateway" with added site acceleration and caching.
* Azure Traffic Manager (Global DNS): Distributes traffic at the domain level. It is slower to failover than Front Door because it relies on DNS caching/TTL.
* Azure Load Balancer (Regional L4): The "Speed King." Ultra-low latency for TCP/UDP traffic, capable of millions of requests per second.
* Azure Application Gateway (Regional L7): The "Smart Hub." Understands URLs, manages cookies, and provides WAF security within a single region.

Traffic solutions summary table:
```
+-----------------------+-------+----------+---------------------------------------+
|        SERVICE        | LAYER |  SCOPE   |             BEST USE CASE             |
+=======================+=======+==========+=======================================+
| Application Gateway   | L7    | Regional | WAF, SSL Offload, Cookie-Affinity.    |
+-----------------------+-------+----------+---------------------------------------+
| Front Door            | L7    | Global   | Global Web Apps, Caching, Fast Fail.  |
+-----------------------+-------+----------+---------------------------------------+
| Load Balancer         | L4    | Regional | Ultra-low latency, TCP/UDP, High-Vol. |
+-----------------------+-------+----------+---------------------------------------+
| Traffic Manager       | DNS   | Global   | Domain-level routing, Legacy/DR.      |
+-----------------------+-------+----------+---------------------------------------+
```

Key decision logic recap:

1. Is it Web traffic (HTTP/S)? Yes > Layer 7. No > Load balancer.
2. Is it global? Yes > Front Door. No > Application gateway.
3. Does it need WAF or SSL offload? Yes > Application gateway or Front Door.
