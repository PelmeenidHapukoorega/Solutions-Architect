# Lab 4: Implementing Virtual networking

**Job Skills**

* Creating virtual networks with subnets using portal.
* Creating virtual networks and subnets using templates.
* Creating and configuring communication between Application security group and a Network security group.
* Configuring public and private Azure DNS zones. 

## Part 1: Creating virtual network with subnets using portal

1. Searched for `Virtual networks` in the portal, on the landing page selected `Create`.
2. Filled out the basics tab and added subnets, then reviewed settings:

<img width="725" height="688" alt="image" src="https://github.com/user-attachments/assets/ad7658ee-d45b-4987-8a66-022429f955e6" />

3. Selected `create`, went to resource and verified address space as well as subnets:

<img width="150" height="47" alt="image" src="https://github.com/user-attachments/assets/77cf140a-fe41-45fd-b564-37bb5a56819e" />

<img width="516" height="130" alt="image" src="https://github.com/user-attachments/assets/572c2c47-4130-4b2e-809e-93f65cbcc63a" />

4. Went to automation blade within the Vnet and exported the template to my pc and renamed it to `vnet.json`.

## Part 2: Creating Vnet and subnets using a template

1. Opened up `vnet.json` in VSC for editing and replace the following:

* `VnetForCoreServices` > `ManufacturingVnet`
* Replaced all `10.20.0.0` with `10.30.0.0`
* Changed `SharedServicesSubnet` to `SensorSubnet1`
* Changed all occurences of `10.20.10.0/24` to `10.30.20.0/24`
* Changed all occurences of `DatabaseSubnet` to `SensorSubnet2`
* Chaned all occurences of `10.20.20.0/24` to `10.30.21.0/24`

Read through the file:
```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_VnetForCoreServices_name": {
            "defaultValue": "ManufacturingVnet",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2024-07-01",
            "name": "[parameters('virtualNetworks_ManufacturingVnet_name')]",
            "location": "eastus",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.30.0.0/16"
                    ]
                },
                "encryption": {
                    "enabled": false,
                    "enforcement": "AllowUnencrypted"
                },
                "privateEndpointVNetPolicies": "Disabled",
                "subnets": [
                    {
                        "name": "SensorSubnet1",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VnetForCoreServices_name'), 'SharedServicesSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.30.20.0/24"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "SensorSubnet2",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VnetForCoreServices_name'), 'DatabaseSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.30.21.0/24"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-07-01",
            "name": "[concat(parameters('virtualNetworks_ManufacturingVnet_name'), '/DatabaseSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_ManufacturingVnet_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.30.21.0/24"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-07-01",
            "name": "[concat(parameters('virtualNetworks_ManufacturingVnet_name'), '/SharedServicesSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_ManufacturingVnet_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.30.20.0/24"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        }
    ]
}
```

2. Saved changes since i had downloaded full file with parameters.
3. Went to portal and looked up `Custom deployment template` and uploaded my `vnet.json`, selected my virtualnetworks resource group
