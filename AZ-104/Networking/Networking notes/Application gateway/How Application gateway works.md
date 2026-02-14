# Notes on how application gateway works

1. Front-end IP and entry point

* IP Configuration: Can have a Public IP, a Private IP, or both.
* Constraint: You are limited to a maximum of 1 public and 1 private IP address per gateway.
* Listener Logic: This is the "gatekeeper." It listens for specific combinations of Protocol, Port, Host, and IP.
  * Basic Listener: Routes traffic based only on the URL path.
  * Multi-site Listener: Routes traffic based on the Hostname (e.g. `shop.contoso.com` vs. `blog.contoso.com`).

2. Layer 7 routing and rules

* Application Aware: Unlike a standard Load Balancer (Layer 4), it makes decisions based on the HTTP request content (URL path and host header).
* Routing Rules: These bind a Listener to a Back-end pool. They interpret the URL and decide which pool should handle the request.
* HTTP Settings: These define how the gateway talks to the backend. This includes:
 * Encryption (HTTPS) settings.
 * Session Stickiness: Keeping a user on the same server.
 * Connection Draining: Safely removing servers for maintenance without dropping active sessions.

3. Web application firewall (WAF)

* Pre-emptive Security: WAF inspects traffic before it even reaches a listener.
* OWASP Standard: Based on the Open Web Application Security Project (OWASP) Core Rule Sets (CRS).
* Protection Types: Blocks common attacks like SQL Injection (SQLi), Cross-Site Scripting (XSS), and malicious bots/scanners.
* Customization: You can select specific rule sets (default is CRS 3.1) or exclude specific rules that cause false positives.

4. Back-end pools and encryption

* Target Variety: Can route to VMs, Scale Sets, Azure App Services, or even on-premises servers.
* Back-end Re-encryption: For full security, the gateway can terminate SSL from the user, then re-encrypt it using a backend certificate before sending it to the servers.
* App Service Advantage: If your backend is Azure App Service, you dont need to manage certificates in Application Gateway; the encryption is trusted and handled automatically by Azure.

5. Load balancing mechanism

* Round-Robin: Automatically distributes requests across healthy members of a pool.
* Health Probes: Continuously monitors backends. If a server stops responding, it is automatically removed from the rotation.

Recap:
```
+----------------------+-------------------------------------------------------+
|      COMPONENT       |                   KEY FUNCTIONALITY                   |
+======================+=======================================================+
| Frontend IP          | Max 1 Public + 1 Private. Gateway entry point.        |
+----------------------+-------------------------------------------------------+
| Listeners            | Basic (URL Path) or Multi-site (Hostname).            |
+----------------------+-------------------------------------------------------+
| Routing Rules        | The "Traffic Director." Binds Listeners to Pools.     |
+----------------------+-------------------------------------------------------+
| WAF (Optional)       | OWASP-based security. Inspects BEFORE Listeners.      |
+----------------------+-------------------------------------------------------+
| HTTP Settings        | Manages Stickiness, Draining, and Backend Encryption. |
+----------------------+-------------------------------------------------------+
| Backend Pools        | VMs, Scale Sets, App Service, or On-premises.         |
+----------------------+-------------------------------------------------------+
```

6. Advance routing methods

Application gateway makes intelligent routing decisions based on the specific content of the HTTP request:

* Path-Based Routing:
 * Routes traffic to different backend pools based on the URL path.
 * Example: `/video/*` goes to a video-optimized pool, while `/images/*` goes to an image-handling pool.

* Multi-site routing
 * Hosts multiple web applications (different domains) on a single gateway instance.
 * Uses Host Headers to distinguish traffic.
 * Example: Requests for `contoso.com` and `fabrikam.com` are received on the same IP but sent to entirely different backend pools.

7. Additional routing features

* Redirection: Automatically redirects traffic (e.g from an old site to a new one, or the common HTTP to HTTPS redirect).
* Rewrite HTTP Headers: Allows the gateway to add, remove, or modify HTTP request/response headers to pass information between client and server.
* Custom Error Pages: Allows you to replace default "404" or "502" errors with branded, custom-designed HTML pages.

8. TLS/SSL management

* SSL Termination (Offloading):
 * The gateway handles the decryption of traffic.
 * Benefit: Saves CPU resources on backend servers and simplifies certificate management (you only install the cert on the gateway, not every VM).

* End-to-End Encryption:
 * Required for high-security environments.
 * The gateway decrypts traffic to inspect it (for WAF), then re-encrypts it before sending it to the backend pool.
* Front-end Security: Only ports 80 (HTTP) and 443 (HTTPS) are exposed to the internet.

9. Security and attack surface reduction

* Isolation: Backend servers are kept in a private subnet and are not directly accessible from the internet.
* Proxy Logic: The gateway acts as a middleman: the internet only "sees" the gateway, which significantly reduces the attack surface of your web infrastructure.

Summary:
```
+----------------------+-------------------------------------------------------+
|      ROUTING TYPE    |                   VITAL INFORMATION                   |
+======================+=======================================================+
| Path-Based           | Uses URL folders (/video, /images) to select pools.   |
+----------------------+-------------------------------------------------------+
| Multi-Site           | Uses Hostnames (domain.com) to host multiple apps.    |
+----------------------+-------------------------------------------------------+
| SSL Termination      | Offloads decryption to Gateway: saves backend CPU.    |
+----------------------+-------------------------------------------------------+
| End-to-End SSL       | Decrypts for inspection, then re-encrypts for VM.     |
+----------------------+-------------------------------------------------------+
| Redirects            | Supports site-to-site and HTTP-to-HTTPS logic.        |
+----------------------+-------------------------------------------------------+
| Attack Surface       | Only ports 80/443 exposed; VMs remain private.        |
+----------------------+-------------------------------------------------------+
