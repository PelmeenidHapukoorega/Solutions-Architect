# Notes about AZ Vnet peering uses

1. The 2 types

* **Regional VNet Peering:** Connects VNets within the same Azure region.
* **Global VNet Peering:** Connects VNets across different Azure regions (eg East US to West Europe).

2. Network path and security

* **Azure Backbone:** Traffic stays entirely on Microsofts private global network. It never goes over the public internet.
* **No Gateways:** You do not need a VPN gateway or any extra hardware to make peering work.
* **Private IPs:** Resources in peered VNets communicate directly using their private IP addresses.

3. Management and scope

* **Separate Resources:** Even after peering, the VNets are managed as individual resources. Deleting one does not automatically delete the other (though it breaks the link).
* **Cross-Boundary:** You can peer VNets across different Subscriptions and even different Azure AD Tenants.
* **Cloud Regions:** Works across all Public and China regions. **Constraint:** Global peering between different Azure Government cloud regions is not allowed.

4. Performance and availability

* **Low Latency:** Because it uses Azures internal infrastructure, it provides the highest possible bandwidth and lowest latency.
* **Zero Downtime:** You can create or update peering connection without any service interruption to the VMs or resources inside the networks.

5. Benefits summarized

* Privacy: No public internet exposure: 100% private communication.
* Performance: High-speed connection that feels like the VMs are in the same network.
* Simplicity: Easy to set up: no encryption or complex routing tables required for basic connectivity.
* Flexibility: Allows for "Hub and Spoke" architectures where many VNets share one central hub.

6. Critical implementation rule

* Non-Overlapping IP Spaces: You **cannot** peer two VNets if they have overlapping IP address ranges (e.g. if both use 10.0.0.0/16). If the IPs overlap, the peering will fail.
