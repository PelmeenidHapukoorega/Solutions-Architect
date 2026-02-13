# Notes about how load balancer works

1. Core networking fundamentals

* OSI Layer 4: Operates at the Transport Layer. It manages traffic based on network properties (IPs and Ports), not application content.
* Decision Criteria: Traffic is managed based on the source/destination address, protocol type (TCP or UDP), and port number.
* Layer 4 vs. Layer 7: It cannot inspect the "content" of a packet (like a URL or a cookie). For application-layer routing, Azure Application Gateway is required.

2. Front-end IP configurations

* Purpose: This is the single IP address that clients use to connect to the balanced service.
* Public Load Balancer:
  * Used for internet-facing traffic.
  * Maps a Public IP and Port to the Private IP and Port of a backend VM.
  * Handles the "Return Path": Maps VM responses back to the Public IP to transmit them to the original client.

* Internal Load Balancer:
  * Used for resources inside a virtual network (VNet).
  * Private IP only: Never directly exposed to the internet.
  * Commonly used for Line-of-Business (LOB) apps or balancing traffic between internal tiers (e.g Web tier to Database tier).

3. Load balancer rules and traffic logic

* Mapping: A rule defines how a specific Front-end IP + Port combination connects to a set of Back-end IPs + Ports.
* The Five-Tuple Hash: The mathematical formula used to distribute traffic:
  1. Source IP
  2. Source port
  3. Destination IP
  4. Destination port
  5. Protocol type (TCP/UDP)
* Session Affinity: A configuration that ensures the same client is always handled by the same backend pool node.
* Flexibility: Can support multiple ports and multiple IP addresses (Multi-front-end) specifically for IaaS VMs.

4. Back-end pool and dynamic scaling

* Membership: Consists of a group of Virtual Machines or Virtual Machine Scale Set instances.
* Scalability: To meet high traffic, it is recommended to add more instances to this pool rather than increasing the size of a single server.
* Automatic Reconfiguration: The Load Balancer automatically detects when instances are added or removed (scale up/down) and redistributes the load immediately based on existing rules.

```
+----------------------+-------------------------------------------------------+
|      COMPONENT       |                   KEY FUNCTIONALITY                   |
+======================+=======================================================+
| OSI Layer            | Layer 4 (Transport). No URL/Content-based routing.    |
+----------------------+-------------------------------------------------------+
| Five-Tuple Hash      | Logic: Src IP/Port, Dst IP/Port, Protocol Type.       |
+----------------------+-------------------------------------------------------+
| Public LB            | Maps Public IP to Private VM IP for Internet traffic. |
+----------------------+-------------------------------------------------------+
| Internal LB          | Private IP only. Restricts access to VNet/VPN/ExR.    |
+----------------------+-------------------------------------------------------+
| Back-end Pool        | Group of VMs/Scale Sets. Auto-scales traffic flow.    |
+----------------------+-------------------------------------------------------+
| Session Affinity     | Ensures a client "sticks" to the same backend node.   |
+----------------------+-------------------------------------------------------+
```

5. Health probes (traffic filter)

* Purpose: Determines which backend instances are "Healthy" and capable of receiving traffic.
* Failure Behavior: When a probe fails, the Load Balancer stops sending new connections to that instance.
* Existing Connections: Crucially, a probe failure does not kill existing connections. They continue until:
  * The application ends the flow.
  * An idle timeout occurs.
  * The VM is shut down.

* Probe Types:
  * TCP: Succeeds if a TCP session is successfully established on the defined port.
  * HTTP/HTTPS: Probes a specific URI (default every 15s). It only succeeds if the VM responds with an HTTP 200 OK. Any other status (404, 500, etc.) is a failure.

6. Session persistence (stickiness)

Determines how the load balancer handles successive requests from the same client:

* None (Default): Any healthy VM can handle the next request.
* Client IP (2-tuple): Successive requests from the same Client IP are routed to the same backend instance.
* Client IP and Protocol (3-tuple): Successive requests from the same Client IP and Protocol (TCP/UDP) go to the same backend instance.
* Context: Also known as Session Affinity or Source IP Affinity.

7. High availability

* Configuration: A rule set to Protocol: All and Port: 0.
* Function: Load balances all TCP and UDP flows on all ports simultaneously for an internal standard load balancer.
* Critical Use Case: Essential for Network Virtual Appliances (NVAs) where you need to scale and provide high availability for a large range of ports.

8. Inbound NAT rules

* Purpose: Provides a 1-to-1 connection to a specific VM in the backend pool.
* Mechanism: Maps a public port on the Load Balancer to a specific private port on a specific VM.
* Example: Mapping Load Balancer Port 5000 to VM1 Port 3389 to allow direct RDP access to that specific machine from the internet.

9. Outbound rules

* Purpose: Configures Source Network Address Translation (SNAT) for backend instances.
* Function: Enables VMs in the private backend pool (which typically have no public IP) to communicate outbound to the internet or other public endpoints.

Summary:
```
+----------------------+-------------------------------------------------------+
|      COMPONENT       |                   KEY FUNCTIONALITY                   |
+======================+=======================================================+
| Health Probes        | TCP handshake or HTTP 200. Fail = No NEW connections. |
+----------------------+-------------------------------------------------------+
| Session Persistence  | Client IP (2-tuple) or Client IP + Protocol (3-tuple).|
+----------------------+-------------------------------------------------------+
| HA Ports (Port 0)    | Balances ALL ports/protocols. Required for NVAs.      |
+----------------------+-------------------------------------------------------+
| Inbound NAT          | 1-to-1 mapping to manage a SPECIFIC backend VM.       |
+----------------------+-------------------------------------------------------+
| Outbound Rules       | Provides SNAT for private VMs to access the internet. |
+----------------------+-------------------------------------------------------+
```
