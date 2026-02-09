# Notes about Vnets

* AZ VNet is a logical isolation of cloud resources
* VNets can be used to provision and manage VPNs in Azure
* Each VNet has its own classless Inter-Domain Routing (CIDR) block and can be linked to other VNets and on premises networks.
* VNets can be linked with an on-premises IT infrastructure to create hybrid or cross-premises solutions, when the CIDR blocks of the connecting networks dont overlap.
* You control the DNS server settings for virtual networks and segmentation of the VNet into subnets.

**VNet that has subnet with 2 VMs diagram:
```ascii
+---------------------------------------+
| Virtual network                       |          +-----------------------+
|                                       |          |      On-premises      |
|  +---------------------------------+  |     .----|      (Building)       |
|  | Subnet                          |  |     |    +-----------------------+
|  |                                 |  |     |
|  | [ VM ]--[NIC]     [ VM ]--[NIC] |--|-----+
|  |       Virtual machines          |  |     |    +-----------------------+
|  +---------------------------------+  |     |    |  Virtual network(s)   |
|                                       |     '--->|-----------------------|
+---------------------------------------+          |  Virtual network(s)   |
                                                   +-----------------------+
```

## Things to consider when using VNets

When you think about configuration plan for VNets and subnets consider following:

```
+----------------------------+---------------------------------------------------+
|          SCENARIO          |                    DESCRIPTION                    |
+============================+===================================================+
| Create a dedicated private | - No cross-premises configuration required.       |
| cloud-only virtual network | - VMs/Services communicate securely in the cloud. |
|                            | - Internet endpoints can still be configured.     |
+----------------------------+---------------------------------------------------+
| Securely extend your data  | - Build site-to-site VPNs.                        |
| center with virtual        | - Uses IPSEC for secure corporate connections.   |
| networks                   | - Scales datacenter capacity to Azure.            |
+----------------------------+---------------------------------------------------+
| Enable hybrid cloud        | - Supports a wide range of hybrid scenarios.      |
| scenarios                  | - Connects cloud apps to on-premises systems.     |
|                            | - Works with mainframes and Unix systems.         |
+----------------------------+---------------------------------------------------+
```
