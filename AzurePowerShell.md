# Azure Through Powershell


#### Links :
https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/5-create-resource-interactively?pivots=windows

## Open Powershell as Admin
```
Install-Module -Name Az -AllowClobber -SkipPublisherCheck
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned
Get-ExecutionPolicy
Update-Module -Name Az
```
## Creating Resource Group:

1.Import the Azure cmdlets.
Import-Module Az

2.Connect to your Azure subscription.
Connect-AzAccount

3.Create the resource group.
Get-AzResourceGroup
New-AzResourceGroup -Name <name> -Location <location>

4.Verify that creation was successful (see the following).
Get-AzResourceGroup

5.Delete Resource Group
Remove-AzResourceGroup -Name <name>


## Creating VM Resource:
```
New-AzVM -Name MyVm -ResourceGroupName ExerciseResources -Location us east 2 -Image DemoImage
```
* Then key in the credentials upon demand. [UserName(not mailId) and PassWord]

OR
```
New-AzVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

### Try to work with scripts too .
```
Import-Module Az
Connect-AzAccount
New-AzResourceGroup -Name resoucegroup -Location centralindia
New-AzVM -Name ubuntu  -ResourceGroupName resoucegroup  -Location centralindia  -Image UbuntuLTS 

```
* Save FileName as : script.ps1
* And execute the script
```
./script.ps1
```

# Setting Up Azure CLI
## 1.Locally
#### Link : 
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli


* Select Install on windows
* Download the current release of Azure CLI
* Goto the downloads folder
* install the azure-cli-2.21.0 installer
* Run
```
az
az --version
```
* Done 

## 2.On Azure Portal

* Click on the "Cloud Shell" button near leftside of search bar in azure portal
* Click on bash
* Create Storage
* Setup Done


## For Credentials of Azure Portal
There are many ways to authenticate to the Azure provider. In this tutorial, you will use an Active Directory service principal account. 
#### RUN
```
az ad sp create-for-rbac --skip-assignment
```
#### OUTPUT
```
{
  "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "displayName": "azure-cli-2019-04-11-00-46-05",
  "name": "http://azure-cli-2019-04-11-00-46-05",
  "password": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "tenant": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa"
}
```


* MISC
#### RUN
```
az login
```
* Login window will pop up.
* Login with ur azure account.
#### OUTPUT
```
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "************************",
    "id": "************************",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Free Trial",
    "state": "Enabled",
    "tenantId": "************************",
    "user": {
      "name": "atib****@gmail.com",
      "type": "user"
    }
  }
]
```