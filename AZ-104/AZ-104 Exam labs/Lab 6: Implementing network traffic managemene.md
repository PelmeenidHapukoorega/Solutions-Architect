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

