#### Security Group
High Level Logical Container to group all Azure resources, for management, security, billing etc 
#### vnet
Represents your private network into cloud
#### Subnet
Splits your private network into subnets
#### Gateway Subnet
Only used for VPN (to be discussed in Azure VPN section)
#### Network Security Group
Group of firewall settings

![image](https://user-images.githubusercontent.com/13016162/71248577-2d6a2980-2341-11ea-93c1-4e0fa73865eb.png)


* Create a virtual network with two subnets 

```powershell
New-AzResourceGroup -Name TestResourceGroup -Location centralus
$frontendSubnet = New-AzVirtualNetworkSubnetConfig -Name frontendSubnet -AddressPrefix "10.0.1.0/24"
$backendSubnet  = New-AzVirtualNetworkSubnetConfig -Name backendSubnet  -AddressPrefix "10.0.2.0/24"
New-AzVirtualNetwork -Name MyVirtualNetwork -ResourceGroupName TestResourceGroup -Location centralus -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet
```
Above example creates a virtual network with two subnets. 
First, a new resource group is created in the centralus region. 
Then, the example creates in-memory representations of two subnets. 
The New-AzVirtualNetworkSubnetConfig cmdlet will not create any subnet on the server side. 
There is one subnet called frontendSubnet and one subnet called backendSubnet. 
The New-AzVirtualNetwork cmdlet then creates a virtual network using the CIDR 10.0.0.0/16 as the address prefix and two subnets.

* Create a virtual network with DNS settings
```powershell
New-AzResourceGroup -Name TestResourceGroup -Location centralus
$frontendSubnet = New-AzVirtualNetworkSubnetConfig -Name frontendSubnet -AddressPrefix "10.0.1.0/24"
$backendSubnet  = New-AzVirtualNetworkSubnetConfig -Name backendSubnet  -AddressPrefix "10.0.2.0/24"
New-AzVirtualNetwork -Name MyVirtualNetwork -ResourceGroupName TestResourceGroup -Location centralus -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6
```
Above example create a virtual network with two subnets and two DNS servers. 
The effect of specifying the DNS servers on the virtual network is that the NICs/VMs that are deployed into this virtual network inherit these DNS servers as defaults. 
These defaults can be overwritten per NIC through a NIC-level setting. If no DNS servers are specified on a VNET and no DNS servers on the NICs, then the default Azure DNS servers are used for DNS resolution.

* Create a virtual network with a subnet referencing a network security group
```powershell
New-AzResourceGroup -Name TestResourceGroup -Location centralus
$rdpRule = New-AzNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
$networkSecurityGroup = New-AzNetworkSecurityGroup -ResourceGroupName TestResourceGroup -Location centralus -Name "NSG-FrontEnd" -SecurityRules $rdpRule
$frontendSubnet = New-AzVirtualNetworkSubnetConfig -Name frontendSubnet -AddressPrefix "10.0.1.0/24" -NetworkSecurityGroup $networkSecurityGroup
$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet  -AddressPrefix "10.0.2.0/24" -NetworkSecurityGroup $networkSecurityGroup
New-AzVirtualNetwork -Name MyVirtualNetwork -ResourceGroupName TestResourceGroup -Location centralus -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet
```
Above example creates a virtual network with subnets that reference a network security group. 
First, the example creates a resource group as a container for the resources that will be created. Then, a network security group is created that allows inbound RDP access, but otherwise enforces the default network security group rules. The New-AzVirtualNetworkSubnetConfig cmdlet then creates in-memory representations of two subnets that both reference the network security group that was created. The New-AzVirtualNetwork command then creates the virtual network.
