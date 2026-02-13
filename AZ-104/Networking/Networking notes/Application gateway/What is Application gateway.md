# Notes about what application gateway is

1. Core definition and targets

* Layer 7 Load Balancer: Unlike the standard Load Balancer, this works at the Application Layer, specifically managing web traffic (HTTP/HTTPS).
* Diverse Back-end Pools: It can route traffic to various resources, including:
  * Azure Virtual Machines
  * Azure Virtual Machine Scale Sets
  * Azure App Service
  * On-premises servers

2. Supported protocols

* Web Standard Support: Fully supports HTTP, HTTPS, HTTP/2, and WebSocket protocols.
* End-to-End Encryption: Supports TLS/SSL encryption from the user to the gateway, and from the gateway to the back-end servers.

3. Traffic management logic

* Round-Robin: Uses a round-robin process to distribute requests evenly across the back-end pool.
* Session Stickiness (Cookie-based Affinity): Ensures a clients requests stay on the same back-end server. This is vital for applications like e-commerce where session data must persist.
* Connection Draining: Allows for "graceful removal" of servers. Existing sessions are finished before the server is taken out of the pool for maintenance.

4. Security and performance

* Web Application Firewall (WAF): Includes built-in protection against common web vulnerabilities and exploits (like SQL injection or cross-site scripting).
* TLS/SSL Offloading: Can terminate the SSL connection at the gateway to reduce the processing burden on the back-end servers.
* Autoscaling: Dynamically adjusts capacity up or down based on the actual volume of web traffic.

Summary:
```
+----------------------+-------------------------------------------------------+
|      FEATURE         |                   KEY FUNCTIONALITY                   |
+======================+=======================================================+
| OSI Layer            | Layer 7 (Application). Routes based on HTTP/HTTPS.   |
+----------------------+-------------------------------------------------------+
| WAF                  | Protects against web attacks (SQLi, XSS, etc.).       |
+----------------------+-------------------------------------------------------+
| Session Stickiness   | Cookie-based affinity keeps users on the same server. |
+----------------------+-------------------------------------------------------+
| SSL Offloading       | Gateway handles encryption to save back-end CPU.      |
+----------------------+-------------------------------------------------------+
| Backend Variety      | Supports VMs, App Service, and On-premises.           |
+----------------------+-------------------------------------------------------+
| Protocol Support     | HTTP, HTTPS, HTTP/2, and WebSockets.                  |
+----------------------+-------------------------------------------------------+
```
