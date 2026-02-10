# M01 Unit 8: Connecting 2 VNets using Global VNet peering

### Goal:

Configuring connectivity between `CoreServicesVnet` and `ManufacturingVnet` by adding peerings to allow traffic flow between them.

**Job Skills**

* Creating VM to test configuration
* Connecting to the Test VMs using RDP
* Testing connection between the VMs
* Creating VNet peerings between `CoreServicesVnets` and `ManufacturingVnet`
* Testing connection between VMs

Diagram:
```ascii
______________________________________________________________________________________________
| [ DragonEggs ]                                                                             |
|                                                                                            |
|          [ East US ]                            [ West Europe ]                            |
|  +-------------------------+          +------------------------------------------+         |
|  | CoreServicesVnet        |          | ManufacturingVnet (10.30.0.0/16)         |         |
|  | (10.20.0.0/16)          |          |  +------------------------------------+  |         |
|  |                         |   VNet   |  | ManufacturingSystemsSubnet         |  |         |
|  |      [ TestVM1 ]        |<-------->|  | (10.30.10.0/24)                    |  |         |
|  |      (10.20.20.4)       | Peering  |  | [NSG: ManufacturingVM-nsg]         |  |         |
|  |           ^             |          |  | [ ManufacturingVM ] <--------------|--|---[ User ]
|  +-----------|-------------+          |  | (10.30.10.4)                       |  |   (Laptop)
|              |                        |  +------------------------------------+  |     RDP
|              └------- Test RDP Session ------------------------------------------┘   Session
|____________________________________________________________________________________________|
```
