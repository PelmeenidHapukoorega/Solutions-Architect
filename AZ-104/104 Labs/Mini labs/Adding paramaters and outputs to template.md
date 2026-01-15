# Lab 1: Adding paramaters and outputs to template

1. Opened `azuredeploy.json` and updated `parameters: {},` with the following
```JSON
"parameters": {
  "storageName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24,
    "metadata": {
      "description": "The name of the Azure storage resource"
    }
  }
},
```

2. Updated resource `name` and `displayName`
```Bicep
"name": "[parameters('storageName')]",
```

```Bicep
"displayName": "[parameters('storageName')]"
```

2. Created new RG group in Portal named it `garbage` and then opened terminal in VSC and deployed ARM template again
```PWSL
$templateFile="azuredeploy.json"
$today=Get-Date -Format "DD-mm-yyyy"
$deploymentName="addnameparameter-"+"$today"
New-AzResourceGroupDeployment `
-Name $deploymentName `
-TemplateFile $templateFile `
-storageName "homeofgarbage"
```

Checked portal:

<img width="941" height="525" alt="image" src="https://github.com/user-attachments/assets/72dabff9-72ad-4a9f-bce5-64e31de3cebb" />

3. Added parameter for `storageSKU` to `azuredeploy.json`.
```JSON
"storageSKU": {
   "type": "string",
   "defaultValue": "Standard_LRS",
   "allowedValues": [
     "Standard_LRS",
     "Standard_GRS",
     "Standard_RAGRS",
     "Standard_ZRS",
     "Premium_LRS",
     "Premium_ZRS",
     "Standard_GZRS",
     "Standard_RAGZRS"
   ]
 }
```

4. Update resources to actually use the storageSKU parameter.
```JSON
"sku": {
     "name": "[parameters('storageSKU')]"
   }
```

5. Deployed ARM again using storageSKU parameter that is in the list.
```Bicep
$today=Get-Date -Format "DD-mm-yyyy"
```

```Bicep
$deploymentName="addSkuParameter-"+"$today"
```

```Bicep
New-AzResourceGroupDeployment `
-Name $deploymentName `
-TemplateFile $templateFile `
-storageName "homelessgarbage" `
-storageSKU Basic
```

Deployment failed because "basic" is not in the SKU list.

6. Updated outputs for storageEndpoint
```JSON
"outputs": {
  "storageEndpoint": {
    "type": "object",
    "value": "[reference(parameters('storageName')).primaryEndpoints]"
  }
}
```

7. Deployed again
```PWSL
$today=Get-Date -Format "DD-mm-yyyy"
$deploymentName="addnameparameter-"+"$today"
New-AzResourceGroupDeployment `
-Name $deploymentName `
-TemplateFile $templateFile `
-storageName "rocketshipofgarbage" `
-storageSKU Standard_LRS
```

Checked deployment in the portal

<img width="905" height="250" alt="image" src="https://github.com/user-attachments/assets/b622626c-1a4a-4c23-a2f0-96b6890dbb22" />

Success.


###Summary
