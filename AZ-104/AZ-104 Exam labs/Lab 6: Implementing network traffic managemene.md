# Lab 6: Implementing network traffic managemene

## Goal

To learn how to configure and test a public load balancer and application gateway.

* Using template to provision infrastructure.
* Configuring load balancer.
* Configuring application gateway

## Part 1: Using a template to provision infra

1. On the portal went opened up the cloud shell and selected `manage files` and uploaded lab files `az104-06-vms-template.json` then edited parameters and uploaded `az104-06-vms-parameters.json`.

In the basics tab selected my region for eastus, resource group as `deploymenttest` and provided admin password.

<img width="388" height="344" alt="image" src="https://github.com/user-attachments/assets/f544491b-d841-47ce-ba06-2dbd7d627e3a" />

Verified that i had 1 Vnet with 3 subnets and each subnet had virtual machine from deployment details:

<img width="576" height="457" alt="image" src="https://github.com/user-attachments/assets/2143ad10-51ad-4d82-b29a-18689cf4b238" />

## Part 2: Creating a load balancer with a frontend IP
```PWSL
$publicIp = New-AzPublicIpAddress `
-Name "az104-lbpip" `
-ResourceGroupName "az104-rg6" `
-Location eastus `
-AllocationMethod Static `
-Sku Standard

$frontend = New-AzLoadBalancerFrontendIpConfig `
-Name "az104-fe" `
-PublicIpAddress $publicIp

$lb = New-AzLoadBalancer `
-Name "az104-lb" `
-ResourceGroupName"az104-rg6" `
-Location eastus `
-Sku Standard `
-FrontendIpConfiguration $frontend
```
Now i had load balancer with a frontend IP which meant i could point all my traffic to 1 single address. 


Added backend pool:
```PWSL
$backendPool = Add-AzLoadBalancerBackendAddressPoolConfig `
-Name "az104-be" `
-LoadBalancer $lb
```

Then applied it:
```PWSL
Set-AzLoadBalancer -LoadBalancer $lb
````

Output:

<img width="562" height="103" alt="image" src="https://github.com/user-attachments/assets/5059ab31-7dca-48e9-90b5-ec1759149e93" />

Retrieved network interfaces aka NICs:
```PWSL
$nic0 = Get-AzNetworkInterface -Name "az104-06-nic0" -ResourceGroupName "az104-rg6"
$nic1 = Get-AzNetworkInterface -Name "az104-06-nic1" -ResourceGroupName "az104-rg6"
```

Then  attached them to the backend pool:
```PWSL
$nic0.IpConfigurations[0].LoadBalancerBackendAddressPools = $lb.BackendAddressPools[0]
$nic1.IpConfigurations[0].LoadBalancerBackendAddressPools = $lb.BackendAddressPools[0]
```

Updated interfaces:
```PWSL
Set-AzNetworkInterface -NetworkInterface $nic0
Set-AzNetworkInterface -NetworkInterface $nic1
```

Output for nic0:

<img width="847" height="95" alt="image" src="https://github.com/user-attachments/assets/0f71a958-20b8-4756-838d-02ab1522f5d5" />

Output for nic1:

<img width="834" height="89" alt="image" src="https://github.com/user-attachments/assets/545e1748-970b-4530-8d72-7496b01466f7" />

Added health probe so load balancer would know if VM0 and VM1 are healthy:
```PWSL
$lb = Get-AzLoadBalancer -Name "az104-lb" -ResourceGroupName "az104-rg6"
```

```PWSL
$probe = Add-AzLoadBalancerPropeConfig `
-Name "az104-hp" `
-LoadBalancer $lb `
-Protocol Tcp `
-Port 80 `
-IntervalInSeconds 5 `
-ProbeCount 2
```

```PWSL
Set-AzLoadBalancer -LoadBalancer $lb
```

<img width="624" height="330" alt="image" src="https://github.com/user-attachments/assets/f66b16e1-666e-4aa3-8017-5280c2099c1b" />

Created load balancing rule that would forward traffic from the public IP to > backend pool.
```PWSL
$lb = Get-AzLoadBalancer -Name "az104-lb" -ResourceGroupName "az104-rg6"
```

```PWSL
Add-AzLoadBalancerRuleConfig `
-Name "az104-lbrule" `
-LoadBalancer $lb `
-FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
-BackendAddressPool $lb.BackendAddressPools[0] `
-Probe $lb.Probes[0] `
-Protocol Tcp `
-FrontendPort 80 `
-BackendPort 80 `
-IdleTimeoutInMinutes 4 
-EnableFloatingIP $false
```

Ran into error here:

<img width="920" height="28" alt="image" src="https://github.com/user-attachments/assets/9ff1f3e1-6875-4054-bc13-160b7953fca7" />

Error occured because `Add-AzLoadBalancerRuleConfig` apparently doesnt have a parameter `-EnableFloatingIP` on the newer az network module, therefore powershell thinks its being passed as positional argument.

Removed `-EnableFloatingIP` parameter entirely and tried again:

<img width="635" height="282" alt="image" src="https://github.com/user-attachments/assets/ef1b3d6f-2d64-4cb3-a26c-1e3edd524fd8" />

Worked like a charm.

Saved changes:
```PWSL
Set-AzLoadBalancer -LoadBalancer $lb
```

Now it was time to test the load balancer:
```PWSL
(Get-AzPublicIpAddress -Name "az104-lbpip" -ResourceGroupName "az104-rg6").IpAddress
```

Copied the public IP and pasted it to another tab:

<img width="630" height="123" alt="image" src="https://github.com/user-attachments/assets/9719f40e-2d76-4569-b28c-c86e9a233346" />

But when refreshing the tab i couldnt get the same message from vm1. This meant that only 1 VM was reachable on port 80but the load balancer was working. Basically VM1 wasnt responding to the health probe.

Learned that if a VM fails the probe then Azure removes it from rotation so the load balancer would send traffic only to VM0.



## Troubleshooting

First i got the load balancer again:
```PWSL
$lb = Get-AzLoadBalancer -Name "az104-lb" -ResourceGroupName "az104-rg6"
```

Then i checked into backend address pools to confirm both NICs were attached:
```PWSL
$lb.BackendAddressPools[0].BackendIpConfigurations
```

Output:
```
PrivateIpAddressVersion Name Primary ...
----------------------- ---- -------
                         False
                         False
```

So the backend pool existed but none of the NICs were attached to it. So the NICs were never actually linked to it because modern powershell module requires attaching the backend pool to the NIC and not the other way around.

Reloaded LB again:
```PWSL
$lb = Get-AzLoadBalancer -Name "az104-lb" -ResourceGroupName "az104-rg6"
```

Then reloaded NICs:
```PWSL
$nic0 = Get-AzNetworkInterface -Name "az104-06-nic0" -ResourceGroupName "az104-rg6"
$nic1 = Get-AzNetworkInterface -Name "az104-06-nic1" -ResourceGroupName "az104-rg6"
```

Then attached backend pool to the NICs:
```PWSL
$nic0.IpConfigurations[0].LoadBalancerBackendAddressPools = $lb.BackendAddressPools[0]
$nic1.IpConfigurations[0].LoadBalancerBackendAddressPools = $lb.BackendAddressPools[0]
```

Saved the NICs:
```PWSL
Set-AzNetworkInterface -NetworkInterface $nic0
Set-AzNetworkInterface -NetworkInterface $nic1
```

<img width="950" height="267" alt="image" src="https://github.com/user-attachments/assets/c70a5c49-b304-4b39-bd04-ef9606d298ce" />

Verified again:
```PWSL
$lb = Get-AzLoadBalancer -Name "az104-lb" -ResourceGroupName "az104-rg6"
```

```PWSL
$lb.BackendAddressPools[0].BackendIpConfigurations
```

Output:
```
PrivateIpAddressVersion Name Primary GatewayLoadBalancer PrivateIpAddress PrivateIpAllocationMethod Sub
                                                                                                    net
                                                                                                     Na
                                                                                                    me
----------------------- ---- ------- ------------------- ---------------- ------------------------- ---
                             False                                                                     
                             False                                                                     
```

Turns out that `BackendIpConfigurations is always empty now. In the new Azure model NIC > Contains backend pool reference. Backend pool > does not list NICs anymore. BackendIpConfigurations is always empty or placeholder objects.

So i needed to find other way to check if NICs were actually attached or not.

Checked NIC0:
```PWSL
$nic0.IpConfigurations[0].LoadBalancerBackendAddressPools
```

Output:

<img width="697" height="131" alt="image" src="https://github.com/user-attachments/assets/74c1bdf5-a51d-4cae-b5c4-5ad4cb43423b" />

Checked NIC1:
```PWSL
$nic1.IpConfigurations[0].LoadBalancerBackendAddressPools
```

Output:

<img width="695" height="129" alt="image" src="https://github.com/user-attachments/assets/ada1f209-7c85-427e-a68d-c87e35c0518a" />

So the NICs were attached correctly which meant that load balancer configuration wasnt the problem anymore. The issue now was VM1 failing the health probe.

Checked health probe status:
```PWSL
$lb.Probes[0]
```

Output:

<img width="921" height="128" alt="image" src="https://github.com/user-attachments/assets/ecc42e90-e198-4ec5-b1c6-dd972fee4af3" />

Then:
```PWSL
$lb | Get-AzLoadBalancerBackendAddressPoolConfig
```

Output:

<img width="661" height="129" alt="image" src="https://github.com/user-attachments/assets/969485a5-693d-4610-bac3-3e67d7454446" />

In the portal i went to `az104-06-vm1` and from the left menu > `run command` > `RunPowershellScript`:
```PWSL
Invoke-WebRequest -UseBasicParsing http://localhost
```

Output:

<img width="787" height="400" alt="image" src="https://github.com/user-attachments/assets/1a70edde-5b63-4ca6-a62f-48f6f288992e" />

Now i knew that VM1 was healthy too and health probe passed.

## Part 3: Configuring Azure application gateway

1. Created application gateway subtnet:
```PWSL
$vnet = Get-AzVirtualNetwork -Name "az104-06-vnet1" -ResourceGroupName
```

Then added subnet configuration:
```PWSL
Add-AzVirtualNetworkSubnetConfig `
-Name "subenet-appgw" `
-AddressPrefix "10.60.3.224/27" `
-VirtualNetwork $vnet
```

Output:

<img width="727" height="110" alt="image" src="https://github.com/user-attachments/assets/2cf4e9ce-e22e-41ee-9d63-16aca1444f86" />

Saved changes:
```PWSL
Set-AzVirtualNetwork -VirtualNetwork $vnet
```

2. Next i created public IP for application gateway:
```PWSL
$gwPip = New-AzPublicIpAddress `
-Name "az104-gwpip" `
-ResourceGroupName "az104-rg6" `
-Location eastus `
-AllocationMethod Static `
-Sku Standard
```

3. Created backend pools:

Main pool:
```PWSL
$beMain = New-AzApplicationGatewayBackendAddressPool `
-Name "az104-appgwbe" `
-BackendIPAddresses @("10.60.1.4","10.60.2.4")
```

Images pool VM1 only:
```PWSL
$beImages = New-AzApplicationGatewayBackendAddressPool `
-Name "az104-imagebe" `
-BackendIPAddresses @("10.60.1.4")
```

Videos pool VM2 only:
```PWSL
%beVideos = New-AzApplicationGatewayBackendAddressPool `
-Name "az104-videobe" `
-BackendIPAddresses @("10.60.2.4")
```

4. Created HTTP backend settings:
```PWSL
$http = New-AzApplicationGatewayBackendHttpSettings `
-Name "az104-http" `
-Port 80 `
-Protocol Http `
-CookieBasedAffinity Disabled
```

5. Configured the frontend:
```PWSL
$frontendIp = New-AzApplicationGatewayFrontendIPConfig `
-Name "az104-feip" `
-PublicIPAddress $gwPip
```

Set the port:
```PWSL
$frontendPort = New-AzApplicationGatewayFrontendPort `
-Name "port80" `
-Port 80
```

6. Created the listener:
```PWSL
$listener = New-AzApplicationGatewayHttpListener `
-Name "az104-listener" `
-FrontendIPConfiguration $frontendIp `
-FrontendPort $frontendPort `
-Protocol Http
```

7. Next i created basic routing rule:
```PWSL
$rule = New-AzApplicationGatewayRequestRoutingRule `
-Name "az104-gwrule" `
-RuleType Basic `
-HttpListener $listener `
-BackendAddressPool $beMain `
-BackendHttpSettings $http
```

8. Deployed the application gateway:
```PWSL
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "subnet-appgw" -VirtualNetwork $vnet
```

Deployed app gateway:
```PWSL
$appGw = New-AzApplicationGateway `
-Name "az104-appgw" `
-ResourceGroupName "az104-rg6" `
-Location eastus `
-BackendAddressPools $beMain,$beImages,$beVideos `
-BackendHttpSettingsCollection $http `
-FrontendIpConfigurations $frontendIp `
-FrontendPorts $frontendPort `
-GatewayIPConfigurations (New-AzApplicationGatewayIPConfiguration -Name "gwconfig" -Subnet $subnet) `
-HttpListeners $listener `
-RequestRoutingRules $rule `
-Sku Standard_v2 `
-Capacity 2
```

Ran into error:
```
New-AzApplicationGateway: Cannot bind parameter 'Sku'. Cannot convert the "Standard_v2" value of type "System.String" to type "Microsoft.Azure.Commands.Network.Models.PSApplicationGatewaySku".
```

The sku parameter didnt accept the standard_v2 string. So i created the object first:
```PWSL
$sku = New-AzApplicationGatewaySku `
-Name "Standard_v2" `
-Tier "Standard_v2" `
-Capacity 2
```

Then i needed to pass that object into `New-AzApplicationGateway` so i replaced on the `-Sku` line the standard_v2 to `$sku` so the full cmdlet would look now:
```PWSL
$appGw = New-AzApplicationGateway `
-Name "az104-appgw" `
-ResourceGroupName "az104-rg6" `
-Location eastus `
-BackendAddressPools $beMain,$beImages,$beVideos `
-BackendHttpSettingsCollection $http `
-FrontendIpConfigurations $frontendIp `
-FrontendPorts $frontendPort `
-GatewayIPConfigurations (New-AzApplicationGatewayIPConfiguration -Name "gwconfig" -Subnet $subnet) `
-HttpListeners $listener `
-RequestRoutingRules $rule `
-Sku $sku` < CHANGED HERE
-Capacity 2
```

Got met with another error:
```
New-AzApplicationGateway: Subnet reference is required for ipconfiguration Application gateway subnet is required.
```

Seemed that either the app gateway IP configuration was missing a valid subnet or subnet object passed wasnt the same that the cmdlet expected even tho i ran `(New-AzApplicationGatewayIPConfiguration -Name "gwconfig" -Subnet $subnet)` before.

Rebuilt the subnet reference:
```PWSL
$vnet = Get-AzVirtualNetwork -Name "az104-06-vnet1" -ResourceGroupName "az104-rg6" 
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "subnet-appgw" -VirtualNetwork $vnet
```

```PWSL
$gwSubnet = New-AzApplicationGatewaySubnet -Id $subnet.Id
```

Got another error:
```
New-AzApplicationGatewaySubnet: The term 'New-AzApplicationGatewaySubnet' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
```

Apparently there is no cmdlet as `New-AzApplicationGatewaySubnet` in the current powershell module. But the good news was that i already had correct subnet object and the issue was that application gateway v2 required different set of parameters to the ones i was using.

Built the sku in V2 format:
```PWSL
$sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
```
No capacity here since V2 gateways use autoscaling unless explicitly disabled.

Next i needed to bult autoscale configuration and the lab needed instance count exactly 2:
```PWSL
$autoscale = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 2 -MaxCapacity 2
```

Then i built gateway IP configuration:
```PWSL
$gwIpConfig = New-AzApplicationGatewayIPConfiguration `
-Name "gwconfig" `
-Subnet $subnet
```

No conversion was needed.

Now i needed to build application gateway again with correct v2 syntax:
```PWSL
$appGw = New-AzApplicationGateway `
-Name "az104-appgw" `
-ResourceGroupName "az104-rg6" `
-Location eastus `
-BackendAddressPools $beMain,$beImages,$beVideos `
-BackendHttpSettingsCollection $http `
-FrontendIpConfigurations $frontendIp `
-FrontendPorts $frontendPort `
-GatewayIPConfigurations $gwIpConfig `
-HttpListener $listener `
-RequestRoutingRules $rule `
-Sku $sku `
-AutoscaleConfiguration $autoscale
```

Ran into another error:
```
New-AzApplicationGateway: Priority for the request routing rule /subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/az104-rg6/providers/Microsoft.Network/applicationGateways/az104-appgw/requestRoutingRules/az104-gwrule cannot be empty. All request routing rules should have a priority defined starting from api-version 2021-08-01
```

az104-gwrule didnt have priority so azure refused to create the gatewa.

Added `-Priority` to the routing rule:
```PWSL
$rule = New-AzApplicationGatewayRequestRoutingRule `
-Name "az104-gwrule" `
-RuleType Basic `
-Priority 10 `
-HttpListener $listener `
-BackendAddressPool $beMain `
-BackendHttpSettings $http
```

Re ran gateway creation:
```PWSL
$appGw = New-AzApplicationGateway `
-Name "az104-appgw" `
-ResourceGroupName "az104-rg6" `
-Location "EastUS" `
-BackendAddressPools $beMain,$beImages,$beVideos `
-BackendHttpSettingsCollection $http `
-FrontendIpConfigurations $frontendIp `
-FrontendPorts $frontendPort `
-GatewayIPConfigurations $gwIpConfig `
-HttpListeners $listener `
-RequestRoutingRules $rule `
-Sku $sku `
-AutoscaleConfiguration $autoscale
```

Got another error:
```
New-AzApplicationGateway: Application Gateway /subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/az104-rg6/providers/Microsoft.Network/applicationGateways/az104-appgw has a MinCapacity value '2' for AutoscaleConfiguration that is greater or equal than the current value for MaxCapacity '2'. Please set MaxCapacity to a value bigger than MinCapacity. If no MaxCapacity is specified a default value of '20' is assumed.
```

Luckily an easy fix, azure basically said to set the maxcapacity to a bigger value than min capacity:
```PWSL
$autoscale = New-AzApplicationGatewayAutoscaleConfiguration `
-MinCapacity 2 `
-MaxCapacity 3
```

Re ran gateway creation and finally got it created:

<img width="589" height="250" alt="image" src="https://github.com/user-attachments/assets/56c4b922-58c1-4351-8c4e-1ad3c33646f5" />

Verified that both servers in the backedn pool were healthy by going to the `application gateway` > `monitoring` > `backend health`:

<img width="813" height="315" alt="image" src="https://github.com/user-attachments/assets/9a5c47f7-e4ba-41ab-8325-34c9082d349e" />

On the overview page copied the frontend public IP: `172.190.146.149` and tested the following urls:

http://172.190.146.149/image/:

<img width="591" height="126" alt="image" src="https://github.com/user-attachments/assets/fbc914b9-f6a2-4451-aca7-3d35c65446cb" />

http://172.190.146.149/video/:

<img width="626" height="149" alt="image" src="https://github.com/user-attachments/assets/5d0ee6b4-7767-492c-81ea-f24e027e9dee" />

With this i confirmed that im both directed to video server on vm2 and image server on vm1.

Peformed cleanup:
```PWSL
Remove-AzResourceGroup -Name az104-rg6
```

### Summary

This lab was basically about getting hands on with traffic management in azure and i pivoted from clicking in the portal to try to use commands instead, so lots of forum reading and trial and errors. First i deployed the whole setup with an arm template, which gave me a vnet, subnets and two vms. nothing crazy there.

Then i built the load balancer. i made the public ip, frontend config, backend pool and attached the nics. at first i thought the backend pool was empty because the output looked blank, but thats just how the new azure module works. The nic holds the reference now not the pool. So i checked the nics directly and saw they were actually linked.

When i tested the lb, only vm0 responded. vm1 didnt show up at all, so i figured it was failing the health probe. I checked vm1 with run command and saw it was responding locally, so eventually the probe passed and the lb started rotating properly. that taught me how important probes are and how misleading the backend pool output can be.

The application gateway part was way more annoying. Half the commands in the lab dont match the current az module. I created the subnet, public ip, backend pools, listener, etc., but the gateway creation kept failing. first the sku string didn’t work, so i had to build a sku object. then it complained about the subnet reference even though i passed it. then i learned that v2 gateways use autoscaling and different parameters. then it wanted a priority on the routing rule. Then it complained that mincapacity couldn’t equal maxcapacity. basically error after error until i adjusted everything to the new syntax.

After fixing all that, the gateway finally deployed. backend health showed both vms as healthy, and testing `/image/` and `/video/` worked exactly like it should. then i cleaned up the resource group.

### Personal learnings

* Azure powershell changes a lot and old labs dont match the new module which just shows how fast the environment can change over time.
* backendipconfigurations being empty doesn’t mean the nics arent attached.
* Health probes decide everything for the load balancer.
* Application gateway v2 has different rules (sku object, autoscale config, priorities etc).
* Troubleshooting azure networking is mostly reading errors carefully and adapting.
* Sometimes the browser caches connections, so private mode helps.

### Key Takeaways

* Azure Load Balancer is an excellent choice for distributing network traffic across multiple virtual machines at the transport layer (OSI layer 4 - TCP and UDP).
* Public Load Balancers are used to load balance internet traffic to your VMs. An internal (or private) load balancer is used where private IPs are needed at the frontend only.
* The Basic load balancer is for small scale applications that dont need high availability or redundancy. The Standard load balancer is for high performance and ultra low latency.
* Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications.
* The Application Gateway Standard tier offers all the L7 functionality, including load balancing, The WAF tier adds a firewall to check for malicious traffic.
* An Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example URI path or host headers.
