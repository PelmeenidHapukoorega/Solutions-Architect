# Lab: Bicep file that contains storage account

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

## Signing into Azure

1. Opened up Terminal on VSC (Azure powershell has already been installed priorly) and opened up New terminal.
2. I created a new path for Bicep seperately and now I needed to set the location:

```Bicep
Set-Location -Path S:\Bicep
```

3. In the VSC terminal, signed into my Azure account with:
```PWSH
Connect-AzAccount
```

Ran into the following error:

```error
Connect-AzAccount : The term 'Connect-AzAccount' is not recognized as the name of a cmdlet, function, script
 file, or operable program. Check the spelling of the name, or if a path was included, verify that the path 
is correct and try again.
At line:1 char:1
+ Connect-AzAccount
+ ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Connect-AzAccount:String) [], CommandNotFoundException       
    + FullyQualifiedErrorId : CommandNotFoundException
```

I realised i created the bicep file to a different repository where Az PowerShell module wasnt installed on for the current session.

So i ran:
```PWSL
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Tried to run `Connect-AzAccount` again and got met with another error:
```error
Import-Module : File C:\Users\Mooses\Documents\WindowsPowerShell\Modules\Az\15.1.0\Az.psm1 cannot be loaded  
because running scripts is disabled on this system. For more information, see about_Execution_Policies at ht 
tps:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ Import-Module Az
+ ~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

New error confirmed that the module is installed but my systems Execution Policy blocked it from running. 
By default Windows disables script execution to prevent malicious scripts from running.

So i needed to change execution policy for the current session so PowerShell would be allowed to load the `Az` module i installed.

Ran the following:
```PWSL
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

And then tried `Import-Module Az` again. Success.

Used `Connect-AzAccount` again but forgot I had MFA enabled.

This meant I needed to log in and specify my tenant ID:
`Connect-AzAccount -TenantId df983b9b-4e77-48d8-8a4a-f8f606a43268`

Entered my code and got in!

<img width="671" height="214" alt="image" src="https://github.com/user-attachments/assets/dcbdd7ff-33e0-4712-98cb-7d568dce9a20" />

3. Needed to get subscription ID for the lab:
   
```PWSL
 Get-AzSubscription
```

4. Set the default subscription for all the AZ PWSL commands that i ran in the session:
```PWSL
Set-AzContext -SubscriptionID {f37270cc-38a5-41a1-bd72-30ffc482d6c2}
```

## Deployment of Bicep

Now that I managed to sign in, set default subscription ID for PWSL command i could move onto deployment.
```PWSL
New-AzResourceGroupDeployment -name toyshop -TemplateFile main.bicep
```

