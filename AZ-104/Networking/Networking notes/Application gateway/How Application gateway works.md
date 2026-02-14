# Notes on how application gateway works

1. Front-end IP and entry point

* IP Configuration: Can have a Public IP, a Private IP, or both.
* Constraint: You are limited to a maximum of 1 public and 1 private IP address per gateway.
* Listener Logic: This is the "gatekeeper." It listens for specific combinations of Protocol, Port, Host, and IP.
  * Basic Listener: Routes traffic based only on the URL path.
  * Multi-site Listener: Routes traffic based on the Hostname (e.g. `shop.contoso.com` vs. `blog.contoso.com`).

2. Layer 7 routing and rules

* Application Aware: Unlike a standard Load Balancer (Layer 4), it makes decisions based on the HTTP request content (URL path and host header).
