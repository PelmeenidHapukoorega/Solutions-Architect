# Notes about creating NSG rules

1. Source and destination filters

* IP Addresses: You can use specific IPs or CIDR blocks (`192.168.1.0/24`).
* Service Tags: Predefined identifiers for Azure services (`Internet`, `VirtualNetwork`, `AzureLoadBalancer`). Use these to avoid managing long lists of IPs.
* Application Security Groups (ASG): Allows you to group VMs logically (eg "WebServers") and apply rules to the group rather than individual IP addresses.

2. Service (ports and protocols)

* Predefined Services: Azure provides shortcuts for common ports like RDP (3389), SSH (22), HTTP (80), and HTTPS (443).
* Custom Ports: You can define specific ports, ranges (eg 8080-8088) or multiple ports separated by commas.
* Protocols: You must choose between TCP, UDP, ICMP, or Any.

3. Rule priority

* The Logic: Rules are processed in order from lowest number to highest number.
* Match and Stop: As soon as traffic matches a rule, Azure applies that action (Allow/Deny) and stops checking any further rules.
* Gapping: Best practice is to leave gaps in numbering (100, 110, 120) so you can insert new rules later without re-ordering.

4. Direction and action

* Direction: Rules are either Inbound (traffic coming in) or Outbound (traffic going out).
* Action: You must explicitly set the rule to either Allow or Deny.

5. Default rules (hidden rules)

* Azure includes 3 default Inbound and 3 default Outbound rules in every NSG.
* They have the highest possible numbers (`65000+`) meaning any custom rule you create will override them.
* By default, they allow traffic within the VNet but block all inbound traffic from the Internet.
