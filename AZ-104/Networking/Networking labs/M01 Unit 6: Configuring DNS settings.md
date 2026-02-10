# M01 Unit 6: Configuring DNS settings in Azure

### Goal:

Configuring DNS name resolutioon, create private DNS zone then link VNets for registration and resolution and then create 2 VMs and test the configuration

**Job Skills**

* Creating private DNS zone
* Linking subnets for auto registration
* Creating VMs to test the configuration
* Verifying records are present in the DNS zone

Diagram:
```ascii
_______________________________________________________________________________
| [ ContosoResourceGroup ]                                                    |
|                                                                             |
|                                       [ East US ]                           |
|  +--------------------+        +-----------------------------------------+  |
|  |  Private DNS Zone  |        | CoreServicesVnet (10.20.0.0/16)         |  |
|  |   (contoso.com)    |        |                                         |  |
|  |                    |VNetLink|  +-----------------------------------+  |  |
|  |       (DNS)        |<------>|  | DatabaseSubnet (10.20.20.0/24)    |  |  |
|  +--------------------+        |  |                                   |  |  |
|                                |  |  [ TestVM1 ]         [ TestVM2 ]  |  |  |
|                                |  |  (10.20.20.4)       (10.20.20.5)  |  |  |
|                                |  +-----------------------------------+  |  |
|                                +-----------------------------------------+  |
|_____________________________________________________________________________|
```

1. Went to `Private DNS zones` page in the portal and created a new one.
2. In the `Basics` created new resource group, added zone name, then reviewed settings:

<img width="701" height="521" alt="image" src="https://github.com/user-attachments/assets/9189f93f-9581-4c1e-89d3-5662594e3fd3" />

Hit create then went to resource.

3. The went under `DNS management` > `Virtual network links` and added new one.
4. Used `CoreServicesVnet` i had created in the previous lab to create the link. Then enabled auto registration:

<img width="792" height="616" alt="image" src="https://github.com/user-attachments/assets/8bf315f3-fc8a-4630-8469-513ad6831ef1" />

Hit create.

Verified:

<img width="585" height="151" alt="image" src="https://github.com/user-attachments/assets/75102868-41af-4b02-944e-b8cf701028ec" />

<img width="585" height="151" alt="image" src="https://github.com/user-attachments/assets/bf0838b9-e2b7-4d65-853d-c1c70804e2e2" />

## Creating VMs to test configuration

1. Opened up terminal in PWSH and uploaded template files `azuredeploy.json` and `azuredeploy.parameters.json` i downloaded prior from MS repo.

2. Set resource group variable:
```PWSH
$RGName = "DragonEggs"
```
3. Then i deployed the files:
```PWSH
New-AzResourceGroupDeployment `
-ResourceGroupName $RGName `
-TemplateFile azuredeploy.json `
-TemplateParameterFile azuredeploy.parameters.json
```

<img width="772" height="477" alt="image" src="https://github.com/user-attachments/assets/ea02340a-0d87-463f-a6a5-33f131176cce" />

Verified in portal that both VMs now existed:

<img width="588" height="412" alt="image" src="https://github.com/user-attachments/assets/90d38a77-0a85-4ae3-862e-d22e42819424" />

## Verified that records were present in the DNS zone

1. Went to my private DNS zone `DragonEggs.com`:

<img width="666" height="87" alt="image" src="https://github.com/user-attachments/assets/8626a1f2-c55d-4d25-b715-01bbf0205fa5" />

2. Then connected VM to test the name resolution by selecting `TestVM1` > `Connect` > `connect` and dowloaded RDP file.

<img width="434" height="66" alt="image" src="https://github.com/user-attachments/assets/0a40fd4a-4bfa-465f-80be-86fb6e0c79e1" />

3. When RDP was downloade and located i executed the file > selected `connect` and provided password to `TestUser` during templates deployment.

<img width="440" height="345" alt="image" src="https://github.com/user-attachments/assets/7a580ac7-d02e-43c5-b64c-945878555a0a" />

Warning page came up, selected okay and yes.

Got into the VM and opened up PWSH and ran `ipconfig /all` command.

<img width="664" height="466" alt="image" src="https://github.com/user-attachments/assets/c915c582-9b2e-4f64-aa5f-279b72729b23" />

Noticed that IP address was the same as in the DNS zone.

Ran command `ping TestVM2.DragonEggs.com`

<img width="495" height="157" alt="image" src="https://github.com/user-attachments/assets/3a9a5753-f15a-4224-ae6e-81b730740bb2" />

The connection timed out because windows firewall was enabled on the VMs

Used `nslookup TestVM2.DragonEggs.com` instead:

<img width="406" height="133" alt="image" src="https://github.com/user-attachments/assets/1a65f0d5-5a6f-44e8-b4f3-2d2e76d6247e" />

This succeeded and clearly demonstrated private zone name resolution.

Performed cleanup.

## Summary

For this unit i set up a private way for my Azure VMs to talk to each other using names instead of just messy IP addresses. I started by making a private DNS zone called DragonEggs.com and then linked it up to my `CoreServicesVnet`. 

The main part was checking that `auto-registration` box so the VMs would talk to the DNS zone on their own. I used some JSON templates and Powershell to deploy two VMs quickly.

After they were up I jumped into one of them with RDP to test it out. Ping didnt work at first because of the default Windows firewall but `nslookup` proved the DNS was working perfectly because it resolved the name to the right IP address.

## What i learned

* How to set up a Private DNS zone from scratch and link it to a VNet.
* Auto-registration is awesome because it automatically creates the DNS records for you when you spin up new VMs.
* Using `New-AzResourceGroupDeployment` with JSON templates is way faster for deploying stuff than clicking through the portal every time.
* Just because a `ping` fails doesnt mean the DNS is broken its usually just the Windows firewall blocking ICMP, so nslookup is the real way to check if your DNS settings are actually solid.
* Learned how to verify that the IP addresses in the `ipconfig` match exactly what showed up in the Azure DNS records.

## Key Takeaways from MS learn

* Azure DNS is a cloud service that allows you to host and manage domain name system (DNS) domains, also known as DNS zones.
* Azure DNS public zones host domain name zone data for records that you intend to be resolved by any host on the internet.
* Azure Private DNS zones allow you to configure a private DNS zone namespace for private Azure resources.
* A DNS zone is a collection of DNS records. DNS records provide information about the domain.
