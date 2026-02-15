# Notes on when to use network watcher

## Scenario: Resolving VM connectivity issues

* The Problem: Developers cannot establish a connection (e.g. Remote PowerShell on port 5986) between two VMs on the same VNet.
* The Tool: IP Flow Verify.
* The Logic: You run a logical test by entering the source/destination IPs, ports, and protocol.
* The Result: It identifies exactly which NSG rule (inbound or outbound) is dropping the traffic, allowing you to fix the specific security bottleneck.

## Scenario: Troubleshooting VPN connections

* The Problem: On-premises hosts cannot reach Azure IaaS VMs over a Site-to-Site VPN.
* The Tool: VPN Troubleshoot.
* The Logic: Performs a deep diagnostic scan of the Virtual Network Gateway and the specific connection tunnel.
* The Result: Provides a health status, detailed log files, and suggested resolutions to get the tunnel back online.

## Scenario: Determining network latency

* Global Placement: Use monitoring tools to check latency between different Azure regions. This helps decide if an app can be geographically distributed or if it must stay in a single region for performance.
* Migration Decisions: Compare the latency of an on-premises hybrid application vs. an Azure VM connecting to the same endpoint. High on-premises latency often builds the business case for migrating that application to Azure.

## When NOT to use network watcher

* PaaS and Web Analytics: It is not designed for Platform-as-a-Service (PaaS) or high-level web traffic analytics. If a PaaS service is down, check Azure Service Health instead.
* Advanced Proprietary Needs: It provides intermediate-level diagnostics. If your organization requires highly specialized, deep-packet inspection features not found in Azure, you may need a 3rd party NVA (Network Virtual Appliance).
* Non-IaaS Resources: If the issue is with a managed service (like Azure SQL or App Service) rather than a Virtual Network/VM, Network Watcher is not the correct tool.

Technical summmary:
```
+----------------------+----------------------+-------------------------------------------------------+
|       SCENARIO       |      BEST TOOL       |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| VM-to-VM Blocked     | IP Flow Verify       | Confirms if an NSG rule is the culprit.               |
+----------------------+----------------------+-------------------------------------------------------+
| Broken VPN Tunnel    | VPN Troubleshoot     | Diagnoses Gateway health & provides fix-it steps.     |
+----------------------+----------------------+-------------------------------------------------------+
| Performance Issues   | Connection Monitor   | Measures latency for cross-region/migration planning. |
+----------------------+----------------------+-------------------------------------------------------+
| PaaS Outage          | Service Health       | Network Watcher CANNOT diagnose PaaS/Web Analytics.   |
+----------------------+----------------------+-------------------------------------------------------+
| Deep Packet Audit    | Packet Capture       | Use for pcap files; may need 3rd party for analysis.  |
+----------------------+----------------------+-------------------------------------------------------+
```
