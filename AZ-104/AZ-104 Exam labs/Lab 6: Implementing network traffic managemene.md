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

## Part 2: Configuring load balancer

The goal was to implement load balance in front of the 2 VMs in the VNet. Load balance provides layer 4 connectivity across VMs and would include configuration of front end IP address to accept the connections, backend pool and rules defining how connections would traverse the load balancer.

1. Created load balancer:
```PWSL
$lb = New-AzLoadBalancer -ResourceGroupName "deploymenttest" `
-name "deploymmenttest-lb" `
-Location eastus `
-Sku "Standard" `
-Tier "Regional"
```
2. Created public IP. Set the location the same as the VMs:
```PWSL
$location = "eastus"
```

Then created new public IP:
```PWSL
$pip = New-AzPublicIpAddress -ResourceGroupName "deploymenttest" `
-Name "deployment-lbpip" `
-Location eastus `
-Sku "Standard" `
-Tier "Regional" `
-AllocationMethod "Static"
```

3. Next i needed to add the IP to the load balancer.

Got the latest version of load balancer:
```PWSL
$lb = Get-AzLoadBalancer -ResourceGroupName "deploymenttest" -Name "deployment-lb"
```

Added frontend setting:
```PWSL
$lb = Add-AzLoadBalancerFrontendIpConfig -LoadBalancer $lb `
-Name "deployment-fe" `
-PublicIpAddress $pip
```

Saved changes to Azure:
```PWSL
$lb | Set-AzLoadBalancer
```

Verification:
```PWSL
Get-AzLoadBalancer -ResourceGroupName "deploymenttest" -Name "deployment-lb"
```

Output:
