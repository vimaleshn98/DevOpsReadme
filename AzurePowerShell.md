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
