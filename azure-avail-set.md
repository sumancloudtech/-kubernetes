[<<back](index.md)
## Azure Availability Set

```bash
az vm availability-set list --output table
```

![image](https://user-images.githubusercontent.com/13016162/71400089-af14cc80-264b-11ea-8818-bac252050f4b.png)

* List VMs in an availability set (a lil tricky and indirect way)

```bash
az vm show -d \
--ids $(az vm availability-set show --resource-group viki-resource-group --name viki-webserver-avail-set --query "virtualMachines[].id" --output tsv) \
--query "[].{VMS:name, PublicIps:publicIps}" \
-o table
```

![image](https://user-images.githubusercontent.com/13016162/71403653-631b5500-2656-11ea-8581-d2506e8d7bc1.png)

* Simply get VM ids in Avail set

```bash
az vm availability-set show --resource-group viki-resource-group --name viki-webserver-avail-set --query "virtualMachines[].id" --output tsv
```
```powershell
(Get-AzAvailabilitySet -ResourceGroupName viki-resource-group -Name viki-webserver-avail-set).VirtualMachinesReferences
```

