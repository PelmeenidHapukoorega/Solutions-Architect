# Lab 1: Creating ARM template

1. Opened up VSC and created JSON fie `azuredeploy.json`
2. Wrote the following instead of copy pasting to familiarise myself with JSON
```Json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "functions": [],
    "variables": {},
    "resources": [],
    "outputs": {}
}
```
3. Saved changes.
4. Since i was already signed in to Azure i wnet ahead and created resource group and added location where to deploy it.
```PWSL
New-AzResourceGroup -name armbase -Location norwayeast
```

<img width="685" height="121" alt="image" src="https://github.com/user-attachments/assets/40536acc-0b0f-44f0-8145-cc83017739af" />

5. Set the resource group as default
```PWSL
Set-AzDefault -ResourceGroupName armbase
```

<img width="680" height="182" alt="image" src="https://github.com/user-attachments/assets/44d896b6-54d5-4c20-89ad-6fb3e40a4a74" />

6. Depoyed the template with following commands in terminal in order

`$templateFile="azuredeploy.json"`

`$today=Get-Date -Format "DD-mm-yyyy"`

`$deploymentName="blanktemplate-"+"$today"`

```PWSL
New-AzResourceGroupDeployment `
-Name $deploymentName `
-TemplateFile $templateFile
```

<img width="612" height="260" alt="image" src="https://github.com/user-attachments/assets/42cc51c1-c26f-4886-9fcb-d7b9a6e70b4d" />

7. Checked portal to see if resource group was created and check whether deployment succeeded

<img width="945" height="275" alt="image" src="https://github.com/user-attachments/assets/bdd767b2-e2fc-421e-aaed-3dfe059f26ae" />

Checked the template itself:

<img width="441" height="226" alt="image" src="https://github.com/user-attachments/assets/051202e9-9f1d-4ba4-b772-2abf1748a198" />

Deployment details was empty because i hadnt created a resource yet.

8. Went back to VSC `azuredeploy.json` and added storage account resource.
```JSON
 "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2025-01-01",
            "name": "armbasestorageregular",
            "tags": {
                "displayName": "armbasestorage72"
            },
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
                "name": "Standard_LRS"
            }
        }
    ],
```

Saved changes.

9. Deployed again in order

``$templateFile="azuredeploy.json"``

`$today=Get-Date -Format "DD-mm-yyyy"`

`$deploymentName="addstorage-"+"$today"`

```PWSL
New-AzResourceGroupDeployment `
  -Name $deploymentName `
  -TemplateFile $templateFile
```

Since i set the default resource group earlier it would deploy to established resource group again.

<img width="595" height="274" alt="image" src="https://github.com/user-attachments/assets/bb06d419-be1f-43ff-bdb6-f23efa673281" />

Verified in portal:

<img width="938" height="289" alt="image" src="https://github.com/user-attachments/assets/71813ee0-58f0-473e-959e-fb6f2192e580" />

Checked deployment details for storage account:

<img width="644" height="335" alt="image" src="https://github.com/user-attachments/assets/b9ce0ea8-aa0b-4754-9be1-a0fa63c8c6a5" />

### Summary

In this lab i created a simple arm template, defined default resource group where resources would be deployed unless changed. Since i had finished Bicep fundamentals lab i had some more extensive knowledge in how to do this, it felt basic but still necessary to understand. Things i learned from this that would differ from Bicep fundamentals would mainly be setting a default resource group for deployment, adding blank template and then editing code by adding storage account resource, Bicep is deployed the same way, but what i mean here is that blank templates werent covered there. 

Deployment would always use the same commands, however it under `$deploymentName` its a good idea to add a message as in what exactly is being added so it can be seen in the deployment itself. Overall a fairly simple lab compared to Bicep ones so far. In hindsight deploying blank templates is actually fairly useful, you can test whether resource group actually exists, is the deployment engine listening and if the credentials work. 
