# Notes about how network watcher works

## 1. Activaion and accessibility

* Automatic Availability: Network Watcher is automatically enabled in a region as soon as you create a Virtual Network (VNet) in your subscription for that region.
* Access: It is reached directly via the Azure Portal search bar. No complex "install" is needed for the main interface.

## 2. Topology tool

* Comprehensive Visualization: It maps out the entire structure of a VNet, including Subnets, NICs, NSGs, Load Balancers (and their health probes), Public IPs, Peering, Gateways, and VMs.
* Relationship Logic: It doesnt just list resources; it shows how they are linked.
* Association types:
  * Contains: Indicates a parent-child relationship (e.g. a VNet contains a Subnet).
  * Associated: Indicates a logical link (e.g., an NSG is associated with a NIC).
* Resource Metadata: Every icon in the topology map provides the resource Name, Location (Region), and its unique Azure URI (ID).

## 3. Connection monitor tool

* Unified Monitoring: Supports end-to-end monitoring for Azure-to-Azure, Azure-to-Hybrid, and Hybrid-to-On-premises deployments.
* Performance Metrics: Primarily used to measure latency and connectivity over time.
* Change Detection: It can automatically identify when a network change (like an NSG rule update) breaks a connection.
* Diagnostic Guidance: It doesnt just report "Down"; it provides an explanation of why the failure happened and lists steps to fix it.

## 4. Monitoring agent requirement (CRITICAL)

* Agents are Required: To monitor connectivity, you must install a monitoring agent on the host.
* Azure VMs: Requires the Network Watcher Agent extension.
* Non-Azure/On-premises: Requires a lightweight executable file to be installed on the host.

Technical summary so far:
```
+----------------------+----------------------+-------------------------------------------------------+
|       FEATURE        |      REQUIREMENT     |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| Activation           | Automatic            | Enabled per region upon VNet creation.                |
+----------------------+----------------------+-------------------------------------------------------+
| Topology             | None                 | Visualizes "Contains" vs "Associated" relationships.  |
+----------------------+----------------------+-------------------------------------------------------+
| Connection Monitor   | Monitoring Agents    | Measures latency; detects NSG/Routing changes.        |
+----------------------+----------------------+-------------------------------------------------------+
| Agent Type (Azure)   | NW Agent Extension   | Must be installed on VMs to use Connection Monitor.   |
+----------------------+----------------------+-------------------------------------------------------+
| Diagnosis            | Automated            | Provides specific "how-to-fix" steps for outages.     |
+----------------------+----------------------+-------------------------------------------------------+
```

## 5. IP flow verify

* 5-Tuple Check: Uses Source IP, Source Port, Destination IP, Destination Port, and Protocol (TCP/UDP) to verify traffic.
* VM Level: Specifically detects filtering issues (Inbound/Outbound) for a specific Virtual Machine and its network adapter.
* Quick Identification: Instantly tells you if a packet is `Allowed` or `Denied`, identifying the specific security rule responsible.

## 6. Next hop

* Routing Analysis: Identifies if traffic is being sent to the correct destination or dropped ("Black holed").
* Hop Details: Provides the Next Hop Type (e.g. Internet, Virtual Appliance) and the IP address.
* Route Table ID: Identifies whether the traffic is following a System Route or a User-Defined Route (UDR). Essential for troubleshooting connectivity in Hub-and-Spoke or NVA environments.

## 7. Effective security rule

* Rule Aggregation: Since multiple NSGs can be applied to one resource (one at the Subnet and one at the NIC), this tool does the "math" for you.
* Final Result: Displays the absolute final set of rules that govern the traffic, making it easy to see why a "Deny" is happening even if you have an "Allow" elsewhere.

## 8. Packet capture

* Remote Extension: Operates as a VM extension that you can start remotely without logging into the OS.
* Deep-Dive Analysis: Records actual traffic (pcap files) for analysis in tools like Wireshark.
* Targeted Capture: Uses 5-tuple filters to ensure you only capture the specific traffic you need, saving storage space.
* Storage Options: Can save captured data directly to a Storage Blob or the Local Disk of the VM.

## 9. Connection troubleshoot

* Point-in-Time Test: Checks TCP connectivity from a source VM to a destination (VM, FQDN, URI, or IP).
* Performance Data: Returns latency (ms), the number of hops, and the number of probe packets sent.
* Specific Fault Reasons: If a connection fails, it identifies the "Why":
  * Resource Issues: High CPU or Memory on the VM.
  * Networking: NSG rules or incorrect User-Defined Routes (UDR).
  * External: DNS resolution failures or Guest Firewall (OS-level firewall) blocks.

```
+----------------------+----------------------+-------------------------------------------------------+
|         TOOL         |        FOCUS         |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| IP Flow Verify       | NSG Filtering        | Uses 5-tuple check to see if a packet is blocked.     |
+----------------------+----------------------+-------------------------------------------------------+
| Next Hop             | Routing              | Shows where traffic goes next & identifies UDR vs Sys.|
+----------------------+----------------------+-------------------------------------------------------+
| Effective Rules      | Multi-Level Security | Combines Subnet + NIC NSGs into a single final view.  |
+----------------------+----------------------+-------------------------------------------------------+
| Packet Capture       | Traffic Recording    | Remotely captures pcap files; stores to Blob or Disk. |
+----------------------+----------------------+-------------------------------------------------------+
| Connection Troublesh.| End-to-End Test      | Diagnoses specific faults (DNS, CPU, NSG, UDR, etc).  |
+----------------------+----------------------+-------------------------------------------------------+
```

## 10. VPN trouble shoot

**Purpose and Scope**

* Target Resources: Specifically designed to diagnose health issues for Virtual Network Gateways and their active Connections (S2S VPN, P2S VPN, or ExpressRoute).
* Availability: Can be triggered via the Azure Portal, PowerShell, Azure CLI, or REST API.

**Operational logic**

* Long-Running Transaction: Unlike simple ping tests, this is a deep diagnostic process that takes time to complete.
* Preliminary Results: While the test is running, it provides an initial high-level view of the resource health before the final detailed report is generated.

** Diagnostic outputs (results)

When a test is completed, the tool returns specific data field to help identify root cause:

* Status Code: Returns UnHealthy if even a single diagnosis failure is detected.
* Fault Identification: Provides an ID (the fault type) and a Summary of the problem.
* Deep Dive: Includes a Detailed Description of exactly what went wrong within the gateway or the tunnel.

**Remediation guidance**

* Recommended Actions: One of the most vital features; it doesn't just find the problem, it provides a collection of specific steps to fix it.
* Actionable Documentation: Provides an Action URI (link to Microsoft documentation) and Action Text explaining exactly what you need to do to restore the connection.

```
+----------------------+----------------------+-------------------------------------------------------+
|       PROPERTY       |         FIELD        |                   VITAL INFORMATION                   |
+======================+======================+=======================================================+
| Target               | Gateway/Connections  | Diagnoses VPN Gateways and their linked tunnels.      |
+----------------------+----------------------+-------------------------------------------------------+
| Nature of Task       | Long-Running         | Deep transaction; provides preliminary health view.   |
+----------------------+----------------------+-------------------------------------------------------+
| Health Indicator     | "code"               | Returns "UnHealthy" if any single fault is found.     |
+----------------------+----------------------+-------------------------------------------------------+
| Troubleshooting Info | Results & Summary    | Lists specific Fault IDs and detailed descriptions.   |
+----------------------+----------------------+-------------------------------------------------------+
| Remediation          | Recommended Actions  | Provides "how-to" text and links to fix-it docs.      |
+----------------------+----------------------+-------------------------------------------------------+
```
