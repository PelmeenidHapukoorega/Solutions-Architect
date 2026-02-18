# Project: Secure Hub-and-Spoke Hybrid network

## Architecture scenario

Scenario: Medical company is migrating its legacy patient management system to Azure.

They have strict compliance requirement: All traffic leaving the application environments must be inspected by a central security appliance. In additon their `on-premises` staff needs to access AZ resouces securely as if they were on the local network.

* **Architecture:** Implementing Hub and Spoke topology
  * Hub: Has to contain shared infra (security and connectivity)
  * Spokes: 2 Seperate VNets (Pharmacy and Research)

* **On prem simulation:** 4th VNet in a different region. Establishing `site-to-site VPN` between the mock on prem VNet and the Hub to simulate hybrid connection.

* **Security and inspection:**
  * Deploying AZ FW in the hub
  * Configuring UDRs so traffic between Pharmacy and Research spokes is forced through the FW in the hub (Inter-spoke inspection)
  * Directing all internet-bound traffic from the spokes through the firewall

* **Application delivery:**
  * Pharmacy portal (simulated by a VM) has to be accessible from the public internet, but only via Application gateway with WAF enabled

* **Management:**
  * No VMs should have public IP addresses
  * Access for troubleshooting has to be handled via AZ Bastion deployed in the Hub

* **Identity and naming:**
  * Using ASGs to manage traffic rules for the web servers in the Pharmacy spoke, rather than relying solely on individual IP addresses in the NSGs

