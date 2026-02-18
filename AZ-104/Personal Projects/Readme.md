# Project 01: Secure Hub-and-Spoke Hybrid Network

Goal: Architect and deploy a compliant Azure environment for a medical company, centralizing security and establishing secure hybrid connectivity.

**Tech Stack**

* Tools: Azure CLI (Scripted Deployment)
* Security: Azure Firewall (Standard), App Gateway + WAF v2, Azure Bastion
* Networking: Hub-and-Spoke Topology, VNet Peering, S2S VPN, User-Defined Routes (UDR)
* Identity & Management: Application Security Groups (ASG), Network Security Groups (NSG)

**Security and Compliance**

* Forced Tunneling: Configured UDRs to ensure 100% of spoke traffic (internal and internet-bound) is inspected by a central Azure Firewall.
* Zero Public Exposure: Eliminated Public IPs on all backend VMs; management is strictly handled via Azure Bastion.
* WAF Protection: Secured the public-facing Pharmacy portal using an Application Gateway with Web Application Firewall (WAF) policies.
* Identity-Based Rules: Utilized ASGs to manage traffic flow by server function rather than static IP addresses, reducing manual overhead and risk.

**Key Features**
* Hybrid Connectivity: Simulated an "On-Premises" environment using a cross-region VNet connected via a Site-to-Site VPN tunnel.
* Inter-Spoke Inspection: Implemented strict firewall rules to control and log communication between the Pharmacy and Research environments.
* Centralized Shared Services: Deployed management and connectivity tools (VPN, Bastion, Firewall) in a central Hub to optimize costs and security posture.
* Network Virtualization: Successfully managed complex routing logic and ARM dependency challenges across multiple resource groups and regions.

[View Project 01: Secure Hub-and-Spoke Hybrid Network](./Project%2001)
