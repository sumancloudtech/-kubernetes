[<<back](index.md)
```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# You can also use a specific Tenant if you would like a faster log in experience
# Login-AzAccount -TenantId xxxx

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session.
# This is useful when you have multiple subscriptions.
Get-AzureRmSubscription -SubscriptionName "your sub" | Select-AzureRmSubscription

# View your current Azure PowerShell session context
# This session state is only applicable to the current session and will not affect other sessions
Get-AzureRmContext

# To select the default storage context for your current session
Set-AzureRmCurrentStorageAccount -ResourceGroupName "your resource group" -StorageAccountName "your storage account name"

# View your current Azure PowerShell session context
# Note: the CurrentStorageAccount is now set in your session context
Get-AzureRmContext

# To list all of the blobs in all of your containers in all of your accounts
Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob

# Install the Az modules from the PowerShell Gallery
Install-Module Az -Scope CurrentUser

# To make sure the Azure PowerShell module is available after you install
Get-Module -ListAvailable Az*

# Enable access from Remote
Set-ExecutionPolicy RemoteSigned
```
