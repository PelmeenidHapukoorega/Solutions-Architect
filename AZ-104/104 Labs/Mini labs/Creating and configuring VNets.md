# Mini lab: Creating and configuring virtual networks

Scenario:

Your organization is migrating a web-based application to Azure. Your first task is to put in place the virtual networks and subnets. You also need to securely peer the virtual networks. You identify these requirements.

* Two virtual networks are required, app-vnet and hub-vnet. The virtual networks simulate a hub and spoke network architecture.
* The app-vnet hosts the application. The app-vnet virtual network requires two subnets. The frontend subnet hosts the web servers. The backend subnet hosts the database servers.
* The hub-vnet only requires a subnet for the firewall.
* The two virtual networks must be able to communicate with each other securely and privately through virtual network peering.
* Both virtual networks should be in the same region.

Diagram:
```ascii
[ East US (RG1) ]                              [ East US (RG1) ]
+-------------------------------+              +-------------------------------+
| app-vnet (10.1.0.0/16)        |              | hub-vnet (10.0.0.0/16)        |
|                               |   VNET       |                               |
|  +-------------------------+  |  Peering     |  +-------------------------+  |
|  | frontend (10.1.0.0/24)  |  |<============>|  | AzureFirewallSubnet     |  |
|  | [ VM1 ]                 |  |              |  | (10.0.0.0/26)           |  |
|  +-------------------------+  |              |  +-------------------------+  |
|                               |              +-------------------------------+
|  +-------------------------+  |      VNET Link      +--------------------+
|  | backend (10.1.1.0/24)   |  | <---------------->  |  Private DNS Zone  |
|  | [ VM2 ]                 |  |                     | (private.contoso)  |
|  +-------------------------+  |                     +--------------------+
|                               |
|  +-------------------------+  |              +---------------------------+
|  | AzureFirewallSubnet     |  |              |  Azure Devops Pipelines   |
|  | (10.1.63.0/26)          |  |              +---------------------------+
|  | [ Firewall ]            |  |
|  +-------------------------+  |              +---------------------------+
+-------------------------------+              |  Azure DNS Resolution     |
                                               +---------------------------+
```
