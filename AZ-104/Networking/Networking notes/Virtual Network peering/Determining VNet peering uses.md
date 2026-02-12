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




