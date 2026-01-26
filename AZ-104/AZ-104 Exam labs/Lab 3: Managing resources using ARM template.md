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

I deselected StandardSSD for performance since its a premium service and would incur unnecessary high costs, additionally for AZ i selected `no infrastructure redundancy required` for the same reason (it tells azure to basically shove the VM where ever is free in the selected region) and another reason for it was that im creating a simple managed disk to practice with templates so there was no reason for defined AZ zone.

3. After deployment was finished i selected `Go to resource` and chose `Automation` from the left panel. Then selected > `Export template`. Then selected `Arm template` from the top and then `download` under it to download (for parameters file i switched tab from `template` to `parameters` and selected download. Then i stored both files to my local drive:

<img width="659" height="83" alt="image" src="https://github.com/user-attachments/assets/76743e6e-18e3-4886-82ac-c26a234ca2ac" />

I downloaded `bicep` file too for later use.

## Part 2: Editing ARM template and then redeploying it.

1. Searched for `Deploy custom template` in the portal. I noticed in the landing page there was an option for `quickstart template`, these help to deploy entire stacks instantly without manually clicking through the portal to set up different resources. For now tho i chose `build your own template in editor` to edit template i downloaded earlier.
2. On the `Edit template` landing page it gives you bit of code to get started with but not what i came here for. Selected `Load file` option to upload my template.json file for editing.
3. Changed the following:

* `"disks_VM_Disk1_Name"` > `disk_name`
* `"name": "[parameters('disks_VM_Disk1_name')]",` > `"name": "[parameters('disk_name')]",`
* ` "defaultValue": "VM-Disk1",` > ` "defaultValue": "VM-Disk2",`

Then opened `Parameters` file and made changes there:
```JSON
 "parameters": {
    "disks_VM_Disk1_name": { < CHANGED TO: disk_name
```

Selected `Review + create` and then `create`.

4. Went to my resource group to verify if i had now 2 disks in it:

<img width="927" height="556" alt="image" src="https://github.com/user-attachments/assets/0b7c5116-75ff-4600-af0b-4214d7b39147" />

I checked under deployment details and realised that first deployment was me basically setting the blueprint for the ARM. I deployed the first disk, then i copied its template, changed parameters, deployed again and got identical result. The only difference was in inputs after 2nd deployment was just the disk name and the deployment name was different, specifically `Microsoft.Template` this told me that whenever a resource is deployed it will tell me as the user how it was deployed. In the bigger picture this is one of the ways to scale quickly if needed.

## Part 3: Configuring Cloud shell and deploying template with PWSL

1. Opened up cloud shell classic and uploaded both `template.json` and `parameters.json`
2. Opened editor by clicking on brackets icon `{}`.
3. In the `template.json` changed: `"name": "[parameters('disks_VM_Disk1_name')]",` > `"name": "[parameters('disk_name')]",` and ` "defaultValue": "VM-Disk1",` > ` "defaultValue": "VM-Disk3",`.
4. In the `parameters.json` changed the following:
```JSON
```JSON
 "parameters": {
    "disks_VM_Disk1_name": { < CHANGED TO: disk_name
```

5. My experience with Bicep learn path came in handy here. Anyway went ahead with deployment:
```PWSL
New-AzResourceGroupDeployment - ResourceGroupName VirtualMachines1 `
-TemplateFile template.json `
-TemplateParameterFile parameters.json
```

Provisioning successfull:

<img width="817" height="368" alt="image" src="https://github.com/user-attachments/assets/c3caf635-8f11-4cdb-af7f-9d46bc6579a7" />

Confirmed that the disk was create with `Get-AzDisk | ft`

<img width="879" height="182" alt="image" src="https://github.com/user-attachments/assets/25797cc0-5821-4bf0-a2c8-1300397d8cac" />

Instead of 2 there are now 3 just as planned, but verified in portal too for naming reasons:

<img width="614" height="124" alt="image" src="https://github.com/user-attachments/assets/09b88b51-0af5-41f6-92ea-cbc679add55f" />

## Part 4: Deploying template with CLI

1. Verified that files are available in cloud storage with `ls` command:
<img width="358" height="66" alt="image" src="https://github.com/user-attachments/assets/590eca4a-465f-493e-8af0-9aeecf370108" />

2. Made changes in template file, changed the disk name to `VM_Disk4` and saved changes in editor.\
3. Deployed again:
```PWSL
 az deployment group create `
--resource-group VirtualMachines1 `
--template-file template.json `
--parameters parameters.json
```

4. Confirmed that disk was created:
```PWSL
az disk list `
--resource-group VirtualMachines1 `
--output table
```

Result:

<img width="802" height="195" alt="image" src="https://github.com/user-attachments/assets/9e8c2354-2130-4dd2-8e25-1c6bd0051bf1" />

## Part 5: Deploying resource using Bicep

1. Uploaded `main.bicep` template file i had downloaded earlier. Went to editor and made changes to `sku name` by adding `StandardSSD_LRS` and left the rest for default. I didnt need to specify name for another disk with bicep because when you deploy bicep template it will ask you in the terminal to set the value if asked.

2. So i went ahead with deployment instead:
```PWSL
 az deployment group create `
--resource-group VirtualMachines1 `
--template-file main.bicep
```

It asked for string value as expected so i filled it:

<img width="697" height="35" alt="image" src="https://github.com/user-attachments/assets/8a40490c-fa0b-49f2-a1be-b8df50019264" />

Confirmed that the disk was created:
```PWSL
az disk list `
--resource-grouP VirtualMachines1 `
--output table
```

<img width="815" height="138" alt="image" src="https://github.com/user-attachments/assets/d270655b-78fc-49c0-b808-daf98f9a06ce" />

Verified disk 5 in portal:

<img width="878" height="474" alt="image" src="https://github.com/user-attachments/assets/08ac9e5d-ba90-46e5-ac29-2f5f97194c13" />

Verified deployments too:

<img width="637" height="202" alt="image" src="https://github.com/user-attachments/assets/abc0538f-8b6b-447f-ab2e-fe3fe44aa1e7" />

Then did cleanup by deleting the resource group.

### Summary

Learned how to create a simple managed disk and then export its JSON blueprint to use it as a "recipe" for more deployments. 

Additionally noticed that when i wanted to scale up, i could just edit the template and parameters files to change the disk names. I practiced this in a few different ways, using the portal editor first and then moving to Cloud Shell with PowerShell and CLI. It was cool to see that every time i deployed, the `Microsoft.Template` log confirmed that Azure was just following my instructions to create identical resources with different names, however with bicep it shows up as main, or whichever name you used for bicep.

Bicep is pretty smart because it just asks me for the disk name directly in the terminal during deployment instead of making me edit a file beforehand. Lab showed me how templates are the way to go for automation and making sure everything is consistent across the environment but luckily i was ahead on this labs by doing Bicep learn path couple of weeks ago.
