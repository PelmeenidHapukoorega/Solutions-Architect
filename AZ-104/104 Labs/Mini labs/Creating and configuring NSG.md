# Mini lab: Creating and configuring network security groups

Scenario:

Your organization requires the network traffic in the app-vnet to be tightly controlled. You identify these requirements.

* The frontend subnet has web servers that can be accessed from the internet. An application security group (ASG) is required for those servers. The ASG should be associated with any virtual machine interface that is part of the group. This will allow the web servers to be easily managed.
* The backend subnet has database servers used by the frontend web servers. A network security group (NSG) is required to control this traffic. The NSG should be associated with any virtual machine interface that will be accessed by the web servers.
* For testing, a virtual machine should be installed in the frontend subnet (VM1) and the backend subnet (VM2). The IT group has provided an Azure resource manager template to deploy these Ubuntu servers.

**Job Skills**

* Creating NSG
* Creating NSG rules
* Associating NSG to a subnet
* Creating and using ASG (application security groups) in NSG rules.

Diagram:
```ascii
[ Norway East (RG1) ]                                 [ Norway East (RG1) ]
+---------------------------------+              +---------------------------------+
| app-vnet (10.1.0.0/16)          |              | hub-vnet (10.0.0.0/16)          |
|                                 |    VNET      |                                 |
|  +---------------------------+  |   Peering    |  +---------------------------+  |
|  | frontend (10.1.0.0/24)    |  |<============>|  | AzureFirewallSubnet      |   |
|  | [app-frontend-asg]        |  |              |  | (10.0.0.0/26)            |   |
|  | [ VM1 ]                   |  |              |  | [FW] <--- [ Public IP ]  |   |
|  +-------------^-------------+  |              |  +---------------------------+  |
|                |                |              +---------------------------------+
|  +-------------|-------------+  |      VNET Link      +--------------------------+
|  | backend (10.1.1.0/24)     |  | <---------------->  | Private DNS Zone         |
|  | [app-vnet-nsg]            |  |                     | (private.contoso.com)    |
|  | [ VM2 ]                   |  |                     +-------------^------------+
|  +-------------^-------------+  |                                   |
|                |                |                                [ Users ]
|  +-------------|-------------+  |              +---------------------------------+
|  | AzureFirewallSubnet       |  |              | Azure Devops Pipelines          |
|  | (10.1.63.0/26)            |  |              +---------------------------------+
|  | [app-vnet-firewall]       |  |              +---------------------------------+
|  +-------------^-------------+  |              | Azure DNS Resolution            |
+----------------|----------------+              +---------------------------------+
                 |
    [ app-vnet-firewall-rt ]
```

1. Opened up terminal in the portal, stored "RG1" name by running:
```PWSH
$RGName = "RG1"
```

2. Created new resource group:
```PWSH
New-AzResourceGroup `
-Name "RG1" `
-Location norwayeast
```

<img width="853" height="197" alt="image" src="https://github.com/user-attachments/assets/8c6b4202-700c-4e7e-898d-017834b08c4d" />

3. Then deployed 2 VMs using template from microsofts Github repo to the resource group:
```PWSH
New-AzResourceGroupDeployment `
-ResourceGroupName $RGName `
-TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
```
