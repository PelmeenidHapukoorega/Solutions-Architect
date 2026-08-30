## Notes about network virtual appliance or NVA

1. Definition and functions

* What it is: An NVA is essentially a virtual machine that performs specialized network functions instead of just running applications.
* Common Roles: Firewalls (most common), WAN optimizers, Load Balancers, IDS/IPS (Intrusion Detection/Prevention), and Proxies.
* Vendors: You can get these directly from the Azure Marketplace (e.g Cisco, Barracuda, Check Point, SonicWall).

2. Traffic control logic

* Routing Control: NVAs work by intercepting and controlling the flow of network traffic through custom routing.
* Perimeter Network (DMZ): They are typically placed in a "DMZ" subnet to act as a security gatekeeper between the internet and your private internal subnets.
* Microsegmentation: A security strategy where you create dedicated subnets for firewalls and route all traffic through the NVA for inspection, ensuring subnets cannot talk directly to each other without being "scrubbed."

3. Deep packet inspection

* Layer 4: NVAs can inspect packets based on ports and protocols (standard networking).
* Layer 7: Application-aware NVAs can look at the actual content of the data (e.g identifying a specific website or a malicious SQL injection attempt).

4. Technical requirements

* IP Forwarding: To work as an NVA, you must explicitly enable IP Forwarding on the VMs Network Interface (NIC). Without this, the VM will drop any traffic not destined for its own IP.
* Multiple NICs: Most NVAs use at least two interfaces:
  * Management NIC: For the administrator to log in and configure the device.
  * Traffic NIC: For processing and routing the actual data flow.

5. High availability

* Criticality: Since the NVA is the "middleman" for all traffic, if it fails, the entire network goes down.
* Architecture: You must design for High Availability (using multiple NVAs behind a Load Balancer) to ensure there is no single point of failure.

6. Interaction with UDRs

* The Trigger: An NVA does nothing by itself. You must create User-Defined Routes (UDRs) on your subnets to tell Azure: "If you want to go to the internet (or another subnet) send the traffic to the NVAs private IP first."

## Summary

* Marketplace > Allows you to bring your own familiar firewall (like Cisco) to Azure.
* Microsegmentation > Forces every single packet to be inspected before it moves subnets.
* IP forwarding > Must be enabled or the NVA wont route traffic.
* UDR pairing > UDRs are the "steering wheel" that drives traffic into the NVA.
