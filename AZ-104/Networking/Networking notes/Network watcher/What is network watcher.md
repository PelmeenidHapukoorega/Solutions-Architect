# Notes on what network watcher is

1. Core purpose and scope

* IaaS Focused: Designed specifically for Infrastructure-as-a-Service resources (VMs, VNets, Load Balancers, Application Gateways).
* Exclusions: It is not intended for PaaS monitoring or high-level web analytics.
* Three Pillars: The toolset is divided into Monitoring, Diagnostics, and Traffic Analysis.

2. Monitoring tools

* Topology:
  * Provides an interactive visual map of your entire network.
  * Visualizes relationships between resources across multiple subscriptions, resource groups, and regions.
  * Essential for finding "hidden" configuration errors that arent obvious in a standard list view.

* Connection monitor
  * Provides end-to-end performance monitoring.
  * Works for both Azure-to-Azure and Hybrid scenarios (Azure to on-premises).
  * Useful for verifying that different tiers of a multi-tier app can actually communicate.

3. Diagnostic tool: IP flow verify

* Traffic Filtering Detection: Checks if a packet is allowed or denied at the Virtual Machine level.
* Rule Identification: It doesnt just say "denied", it tells you exactly which NSG rule is responsible for blocking the traffic.
* Protocol Support: Works for both IPv4 and IPv6 addresses.

```
+----------------------+----------------------+-------------------------------------------------------+
|       CATEGORY       |         TOOL         |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| Monitoring           | Topology             | Visualizes entire network/resource relationships.     |
+----------------------+----------------------+-------------------------------------------------------+
| Monitoring           | Connection Monitor   | End-to-end performance for Azure and Hybrid links.    |
+----------------------+----------------------+-------------------------------------------------------+
| Diagnostic           | IP Flow Verify       | Detects traffic filtering; names the specific NSG rule|
+----------------------+----------------------+-------------------------------------------------------+
| Traffic              | Flow Logs            | Records metadata about IP traffic passing through NSGs.|
+----------------------+----------------------+-------------------------------------------------------+
| Scope                | IaaS Only            | For VMs/VNets. Not for PaaS or Web Analytics.         |
+----------------------+----------------------+-------------------------------------------------------+
```

4. Advanced diagnostic tools

* NSG Diagnostics: Broader than IP Flow Verify; it works for VMs, Scale Sets, and Application Gateways. It pinpoints the exact rule causing a block and even suggests/creates a higher-priority rule to fix the issue.
* Next Hop: The primary tool for routing issues. It tells you exactly where a packet is going (the "Next Hop Type") and which Route Table ID is being followed.
* Effective Security Rules: Essential for "Security Math." It shows the final result of all rules combined from both the Subnet level and the NIC level.
* Connection Troubleshoot: A point-in-time test for connectivity. Use this to quickly check if a VM can reach a specific URL, FQDN, or IP address.
* Packet Capture: Remotely record actual traffic (PCAP files) from a VM for deep-dive analysis in tools like Wireshark.
* VPN Troubleshoot: Specifically designed to diagnose health and connection issues for Virtual Network Gateways.

5. Traffic analysis tools

* Flow Logs: These record the metadata (source, destination, port, allowed/denied) of IP traffic. This data is stored in Azure Storage for audit and compliance.
* Traffic Analytics: The "Visualizer." It takes the raw data from Flow Logs and turns it into a rich, interactive dashboard showing traffic patterns, hotspots, and security threats.

Technical summary table:
```
+----------------------+----------------------+-------------------------------------------------------+
|       CATEGORY       |         TOOL         |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| Diagnostics          | NSG Diagnostics      | Detects filtering issues; identifies specific rules.   |
+----------------------+----------------------+-------------------------------------------------------+
| Diagnostics          | Next Hop             | Troubleshoots routing; shows Next Hop & Route Table ID|
+----------------------+----------------------+-------------------------------------------------------+
| Diagnostics          | Effective Rules      | Aggregates Subnet + NIC rules into one final view.    |
+----------------------+----------------------+-------------------------------------------------------+
| Diagnostics          | Packet Capture       | Remotely records VM traffic for deep-dive analysis.   |
+----------------------+----------------------+-------------------------------------------------------+
| Traffic              | Flow Logs            | Records IP metadata passing through NSGs/VNets.       |
+----------------------+----------------------+-------------------------------------------------------+
| Traffic              | Traffic Analytics    | Visual dashboard for analyzing Flow Log data.         |
+----------------------+----------------------+-------------------------------------------------------+
```
