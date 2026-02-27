[<<back](index.md)
## Azure Virtual Machine Scale Set (VMSS)

```bash
az vmss list --output table
```
![image](https://user-images.githubusercontent.com/13016162/71400553-3151c080-264d-11ea-9b6d-28f8467d12c4.png)

```bash
az vmss list-instances --resource-group viki-resource-group --name VikiCluster --output table
```
![image](https://user-images.githubusercontent.com/13016162/71400653-6d852100-264d-11ea-8278-8dd644bcaf01.png)

```powershell
Get-AzVmssVM
```
![image](https://user-images.githubusercontent.com/13016162/71400914-382d0300-264e-11ea-9c4b-dfc4b59938c4.png)

```powershell
Get-AzVmssVM -ResourceGroupName viki-resource-group -VMScaleSetName VikiCluster
```
![image](https://user-images.githubusercontent.com/13016162/71401003-7f1af880-264e-11ea-909d-671a4f17140f.png)

