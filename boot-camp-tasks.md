## Azure bootcamp tasks

1. #### Create a VM with default values (Use Powershell, too)

```powershell
# Execute below command with no params (PS will ask mandatory params i.e. Name and VM Cred only)
New-AzVm
```
* Now Observe the Resource Group created (if used PS then this will be crated by default. You must name the RG if created via Portal)  
* See the members of Resource Group via Portal, Az-Cli and PowerShell

```bash
#PowerShell
Get-AzResource -ResourceGroupName vikiVm01
#Az-Cli
az group list --output table
az resource list --resource-group vikivm01 --output table
```

2. #### Create a VM with custom network config using Azure Portal and Poweshell
  
```  
  * 1 vnet  
  * 2 subnet (frontend, backend)  
  * 1 nsg with inbound ports open 22, 3389, 80 attached to frontend subnet  
  * 1 nic with 1 public Ip attached to frontend subnet 
  * 1 VM attached to frontend subnet and no nsg to nic
```

3. #### Export deployment template from ResourceGroup of previous exercise

12. #### Create a custom deployment template and deploy
