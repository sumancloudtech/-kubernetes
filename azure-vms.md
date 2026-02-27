[<<back](index.md)
## Azure VMs

* Lets Straight away start creating a VM 
* We will create network resources as we go along
* If you create VM without providing underlying network layer, then azure will create them for you as defaults.

```powershell
#---- Vars
$ResourceGroup = "viki-resource-group"
$Location = "westindia"
$VnetName = "viki-enterprise"
$VnetAddress = "11.66.0.0/22" #11.66.0.0 - 11.66.3.256
$FrontendSubnetConfig = @{
                Name = "frontendsubnet";
                AddressPrefix = "11.66.1.0/24"; #11.66.1.0 - 11.66.1.255 
}

$BackendSubnetConfig = @{
                Name = "backendsubnet";
                AddressPrefix = "11.66.0.0/24"; #11.66.0.0 - 11.66.0.255
}

#---- Main
New-AzResourceGroup -Name $ResourceGroup -Location $Location
$frontendSubnet = New-AzVirtualNetworkSubnetConfig @FrontendSubnetConfig
$backendSubnet  = New-AzVirtualNetworkSubnetConfig @BackendSubnetConfig

New-AzVirtualNetwork -Name $VnetName -ResourceGroupName $ResourceGroup `
-Location centralus -AddressPrefix $VnetAddress -Subnet $frontendSubnet,$backendSubnet
```
