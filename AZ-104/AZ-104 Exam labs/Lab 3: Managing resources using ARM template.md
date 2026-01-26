# Lab 3: Managing resources using ARM templates

**Job Skills**

* Creating ARM templates.
* Editing ARM template and redoplying it.
* Configuring Cloud shell and deploying template with Azure powershell.
* Deploying template with CLI.
* Deplyoing resource using Bicep.

## Part 1: Creating ARM template

1. Went to portal and searched for `Disks` to create a simple managed disk for ARM deployment later on.
2. On the landing page selected `Create` and configured it as follows: 

<img width="710" height="896" alt="image" src="https://github.com/user-attachments/assets/9cd8fd1e-9d9f-4a66-80cc-d24b41fa862b" />

I deselected StandardSSD for performance since its a premium service and would incur unnecessary high costs, additionally for AZ i selected `no infrastructure redundancy required`
