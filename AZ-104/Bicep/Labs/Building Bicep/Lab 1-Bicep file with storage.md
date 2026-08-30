# Lab 1: Bicep file that contains storage account

## Writing Bicep in VSC

1. Opened up VSC
2. Created a new folder for Bicep Projects
```PWSL
New-Item -Path "$home\Desktop\Bicep-Projects" -ItemType Directory
```
3. Navigated to that folder
```PWSL
cd "$home\Desktop\Bicep-Projects"
```
4. Entered `code .` to start working on the Bicep.
5. Created a new file by clicking in the main menu that pops up fresh after new directory. Named it `main.bicep`.
   * Saved the file so that VSC could load Bicep tooling.
6. Added the following Bicep code which i wrote out myself:
```Bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'toylaunchstorage'
  location: 'eastus'
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```
Made changes to the `name` to somthing more unique, since its needs to be globally unique name. Named it `toys4bob`

4. Saved changes using CTRL + S
## Deploying Bicep file to Azure

1. Opened up terminal with a shortcut Ctrl+`
2. Went to directory where i save Bicep file
```PWSL
Set-Location -Path S:\Bicep
```
3. Installed Bicep
```PWSL
winget install -e --id Microsoft.Bicep
```

Success:

<img width="660" height="207" alt="image" src="https://github.com/user-attachments/assets/b98d4a35-ed2a-4a66-aa5a-6be91454dcbd" />

## Signing into Azure

1. Signed into Azure with `Connect-AzAccount`. Pop up window came up, logged in with email.
2. Needed to get subscription ID for the lab:
```PWSL
 Get-AzSubscription
```
3. Copied my ID: `f37270cc-38a5-41a1-bd72-30ffc482d6c2`
4. Set the default subscription for all the AZ PWSL commands that i ran in the session:
```PWSL
Set-AzContext -SubscriptionID {f37270cc-38a5-41a1-bd72-30ffc482d6c2}
```
Success:

<img width="687" height="187" alt="image" src="https://github.com/user-attachments/assets/4b6cc5e4-baa7-4ace-bbc8-f80476119697" />

## Deployment of Bicep

1. Now that I managed to sign in, set default subscription ID for PWSL command i could move onto deployment.
```PWSL
New-AzResourceGroupDeployment -name toyshop -TemplateFile main.bicep
```

2. Made a mistake of not closing VSC after installing Bicep, since it wont register the installation before.

**Mistake**

I noticed i had created the Bicep file on S: drive but my path has been on C: drive. 

So i opened file explorer, moved the Bicep file to `C:\Users\Mooses\Desktop\Bicep-Projects`. 

Went back to VSC code, deleted old Bicep file. From File menu opened up the main.bicep from correct path and fixed the issue.

Ran `New-AzResourceGroupDeployment -name toyshop -TemplateFile main.bicep` again which worked naturally:

<img width="851" height="127" alt="image" src="https://github.com/user-attachments/assets/54734d7d-9f00-45de-a264-6278db993f73" />

The command will ask you to type resource group name again for confirmation.

3. Missed a crucial step which was forgetting to create a resource group in the first place, so i quickly created that with:
```PWSL
New-AzResourceGroup -Name "toyshop" -Location "eastus"
```

<img width="796" height="209" alt="image" src="https://github.com/user-attachments/assets/0067a5c6-a328-4eb6-a463-8b5b67ee78f8" />

Ran the deployment again `New-AzResourceGroupDeployment -name toyshop -TemplateFile main.bicep`

Success finally:

<img width="610" height="279" alt="image" src="https://github.com/user-attachments/assets/eda8dd42-811e-40f8-b85f-ca3240e45960" />

## Verying deployment

1. Went to Azure portal
2. Selected Resource groups > toyshop
<img width="508" height="211" alt="image" src="https://github.com/user-attachments/assets/03f760a9-0b50-47f7-8bca-4fc3151d760b" />

4. Clicked on `1 Succeeded` under **Deployments**

<img width="667" height="132" alt="image" src="https://github.com/user-attachments/assets/37ecbdfb-5026-46e5-9ce9-f0e761996452" />

5. Clicked on `toyshop` seperately to see whether storage account was deployed as intended:

<img width="639" height="400" alt="image" src="https://github.com/user-attachments/assets/2ab181c6-0e5a-47cf-ac78-192f7d888323" />

## Adding App Service plan to Bicep file

1. Added the following to `main.bicep` file in VSC
```Bicep
resource appServicePlan 'Microsoft.Web/serverfarms@2024-04-01' = {
  name: 'toy-product-launch-plan-starter'
  location: 'eastus'
  sku: {
    name: 'F1'
  }
}

resource appServiceApp 'Microsoft.Web/sites@2024-04-01' = {
  name: 'willywonkamonster'
  location: 'eastus'
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
  }
}
```
2. Deployed the updated Bicep file with `New-AzResourceGroupDeployment -name toyshop -TemplateFile main.bicep`

Result:

<img width="342" height="177" alt="image" src="https://github.com/user-attachments/assets/fc4d2212-d15e-4ca0-b86d-10e5e437bfd9" />

Since i am familiar with Azure and prior VM deployments i knew instantly that the issue was that region was full. So i went back to bicep file, and changed regions for dependencies from `eastus` to `norwayeast`.

This made it succeed:

<img width="343" height="200" alt="image" src="https://github.com/user-attachments/assets/f5c1a41d-a128-42d9-90bd-440d0015e71f" />

3. Went back to portal to verify my results, and low and behold:

<img width="646" height="459" alt="image" src="https://github.com/user-attachments/assets/668bcfe6-beca-4717-8d20-56cf2e94eade" />

Both App Service plan and App itself were now deployed.

## Summary

This was fun in its own way, i learned how to switch Bicep file paths quickly through VSC, how to write bicep in general. Writing bicep is extremely straightforward and doesnt take much to understand what you are writing. Ran into various errors, took initiative to figure out what i was doing wrong.

Its crucial to really pay attention where your files are, what path you are using in terminal and how to move around in VSC to fix those mistakes.

Learned how to deploy a resource group using bicep with 1 deployment, and additionally learned how to add more code in VSC for dependencies which makes it easier to deploy whole solutions. In essence what i created with this Bicep is a toyshop that uses storage account for content delivery (pictures of toys), app service is the shop itself aka URL what customers would visit and App Service plan is the basically the "server" on where the shop runs on.

