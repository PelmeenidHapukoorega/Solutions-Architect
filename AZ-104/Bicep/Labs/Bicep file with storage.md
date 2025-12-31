# Lab: Bicep file that contains storage account

## Writing Bicep in VSC

1. Opened up VSC
2. Created a new file by clicking top left on 3 lines and selecting `file` > `new file`. Named it `main.bicep`.
   * Saved the file so that VSC could load Bicep tooling.
3. Added the following Bicep code which i wrote out myself:
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

## Deploying Bicep to Azure

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

Fixed it by running:
```PWSL
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

