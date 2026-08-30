# Lab 4: Implementing Virtual networking

**Job Skills**

* Creating virtual networks with subnets using portal.
* Creating virtual networks and subnets using templates.
* Creating and configuring communication between Application security group and a Network security group.
* Configuring public and private Azure DNS zones. 

## Part 1: Creating virtual network with subnets using portal

1. Searched for `Virtual networks` in the portal, on the landing page selected `Create`.
2. Filled out the basics tab and added subnets, then reviewed settings:

<img width="677" height="773" alt="image" src="https://github.com/user-attachments/assets/88d6259c-1890-4d56-9af2-41e52efaac94" />

3. Selected `create`, went to resource and verified address space as well as subnets:

<img width="670" height="167" alt="image" src="https://github.com/user-attachments/assets/7a53db07-1efc-4e3b-af27-245a36652ebe" />

<img width="622" height="141" alt="image" src="https://github.com/user-attachments/assets/45b9bd37-aa6f-4f3f-bc7c-28723c121fac" />

4. Went to automation blade within the Vnet and exported the template to my pc.

## Part 2: Creating Vnet and subnets using a template

1. Opened up `template.json` in VSC for editing and replaced the following:

* `CoreServicesVnet` > `ManufacturingVnet`
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
        "virtualNetworks_ManufacturingVnet_name": {
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
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_ManufacturingVnet_name'), 'SensorSubnet1')]",
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
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_ManufacturingVnet_name'), 'SensorSubnet2')]",
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
            "name": "[concat(parameters('virtualNetworks_ManufacturingVnet_name'), '/SensorSubnet2')]",
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
            "name": "[concat(parameters('virtualNetworks_ManufacturingVnet_name'), '/SensorSubnet1')]",
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

Selected `edit parameters` and added `parameters.json` file:
```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_ManufacturingVnet_name": {
            "value": null
        }
    }
}
```
2. Saved changes.
3. Went to portal and looked up `Custom deployment template` and uploaded my `template.json`, selected my virtualnetworks resource group and then `create`

<img width="832" height="383" alt="image" src="https://github.com/user-attachments/assets/b01be88d-c422-47aa-9975-24d1a4a3f7c8" />

Manufacturing Vnet and subnets were created.

Address space:

<img width="608" height="149" alt="image" src="https://github.com/user-attachments/assets/b1332fa4-62bf-41d0-b78f-9604789525af" />

Subnets:

<img width="542" height="134" alt="image" src="https://github.com/user-attachments/assets/819c635a-c07b-494d-80f2-438cf9c0afb6" />


## Part 3: Creating and configuring communication between app security group and network security group

1. Looked up `Application security groups` in the portal, selected `create` and filled it with info:

<img width="472" height="186" alt="image" src="https://github.com/user-attachments/assets/8ba72d7e-1c7e-4aa4-af44-65ad1bc6a1a9" />

2. Went to `network security groups` and created a new one:

<img width="506" height="440" alt="image" src="https://github.com/user-attachments/assets/84541676-252c-4148-97df-941b171b10ec" />

Selected `create`

3. Went to NSG resource, selected `subnets` from `settings` blade and then `+ associate` to associate my Vnet with NSG security group:

<img width="580" height="164" alt="image" src="https://github.com/user-attachments/assets/73bbab11-51c3-4e81-a66e-b302ae3c5593" />

Selected `OK`:

<img width="656" height="196" alt="image" src="https://github.com/user-attachments/assets/b6b61bf1-f368-4732-8db1-7a40aa429c51" />

4. Now that i had associated shared services subnet with my NSG security group i needed to configure inbound security rule to allow ASG traffic.

5. In the settings of `myNSGSecure` group i went to `inbound security rules` to add inbound port rule which would allow ASG traffic. Selected `+ add`.

In the follow up tab added following settings:

1. Source > Application security group
2. Source application security groups > asg-web
3. source port ranges > *
4. Destination > any
5. Service > custom
6. Destination port ranges > 80,443
7. Protocol > TCP
8. Action > Allow
9. Priority > 100
10. Name > AllowASG

6. Next i needed to configure outbound NSG rule that would deny internet access.
7. Selected `Outbound security rules` from the blade.


Noticed that `AllowInternetOutBound` rule could not be deleted and priority was 65001.


8. Selected `+ add` and configured outbound rule that denied access to the internet:

1. Source > any
2. Source port ranges > *
3. Destination > Service tag
4. Destination service tag > Internet
5. Service > Custom
6. Destination port ranges > *
7. Protocol > any
8. Action > deny
9. Priority > 4096
10. Name > DenyInternetOutbound

<img width="1236" height="26" alt="image" src="https://github.com/user-attachments/assets/ae8087dd-84e6-498d-a353-850fdf45dff7" />

## Part 4: Configuring public and private Azure DNS zones

### Configuring Public DNS zone

1. Went to `DNS zones` landing page and selected `+ create`

Filled out basics tab:

<img width="491" height="497" alt="image" src="https://github.com/user-attachments/assets/08fd58a5-c8a4-4dd6-88c4-4c81abec322e" />

Went ahead and created it.

2. On the overview page of DNS zone resource i could see 4 name servers. Copied `ns2-07.azure-dns.net.` to make sure name resolution is working properly for later use.
3. Expanded `DNS management` blade and went to `recordsets` then > `+ add` and filled it as follows:

<img width="544" height="606" alt="image" src="https://github.com/user-attachments/assets/1a5024f5-bac7-41fc-b041-c08981ccdc80" />

Verified that my domain now had A record set named `wwww`:

<img width="619" height="73" alt="image" src="https://github.com/user-attachments/assets/ab0ab19e-4a10-400a-921a-59bc5038396f" />

Opened up terminal and ran the following command:
```PWSL
nslookup www.virtual.hermit ns2-07.azure-dns.net.
```

<img width="590" height="163" alt="image" src="https://github.com/user-attachments/assets/d33c1ce0-b80d-44c0-b7e9-69741e78c82b" />

From the result i could now confirm that name resolution was working properly because `www.virtual.hermit` resolved to IP address i provided earlier which was `10.1.1.4`.

### Configuring private DNS zone

1. Went to `private dns zones` landing page by searching for it, selected `+ create`. Filled the basics:

<img width="697" height="556" alt="image" src="https://github.com/user-attachments/assets/80c167d2-5a2c-4cb5-8e97-9b91a1cbfd29" />

Went ahead and created it, then went to resource itself. Noticed there were no name server records this time around on the overview page.

2. Expanded `DNS management` blade and selected `Virtual network links` and then configured the link:

<img width="803" height="680" alt="image" src="https://github.com/user-attachments/assets/49e76afb-9821-4633-b60c-3383ccf892e9" />

Created that, then went back and selected `+ recordsets`. I would now add record for each VM that would need private name resolution support:

<img width="646" height="88" alt="image" src="https://github.com/user-attachments/assets/360f9315-199d-4b25-845a-d48dd43c6b31" />

Performed cleanup.

### Summary

Learned how to create a virtual network named `VnetForCoreServices` with two subnets, SharedServicesSubnet and DatabaseSubnet, and made sure the IP ranges didnt overlap so they could communicate properly.

Additionally noticed that when i wanted to scale this out with a template, i had to be careful with how i defined the subnets. I practiced refactoring my ARM template for the `ManufacturingVnet` to use external subnet resources for SensorSubnet1 and SensorSubnet2 instead of inline ones, which fixed that NetcfgSubnetRangesOverlap error i kept getting when the template was fighting itself for IP space.

I also practiced setting up security by creating an Application Security Group (ASG) named asg-web and a Network Security Group (NSG) called myNSGSecure. It was cool to see how i could associate the NSG with the SharedServicesSubnet and then create an inbound rule AllowASG to let traffic through on ports 80 and 443, while completely blocking internet access with a high priority DenyInternetOutbound rule.

Finally i set up both public and private DNS zones, which taught me how to resolve hostnames like www.virtual.hermit to a specific IP like 10.1.1.4 and link my VNets for private name resolution. This lab showed me that keeping your network segments organized and secured with NSGs and proper DNS is the way to go for a solid cloud foundation.

### Key Takeaways

* Virtual network is a representation of your own network in the cloud
* When designing VNets its a good practice to avoid overlapping IP address ranges. Tis reduces issues and simplifies troubleshooting.
* Subnet is a range of IP addressess in the virtual network. You can divide a virtual network into multiple subnets for organization and security.
* NSG contains security rules that allow or deny network traffic. There are default incoming and outgoing rules which can be customized to your needs.
* Application security groups are used to protect groups of servers with a common function, such as web servers or database servers.
* Azure DNS is a hosting service for DNS domains that provides name resolution. Azure DNS can be configured to resolve host name in your public domain. You can also use private DNS zones to assign DNS names to VMs in Azure virtual networks.
