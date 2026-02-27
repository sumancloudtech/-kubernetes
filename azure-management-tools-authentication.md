[<<back](index.md)
## Azure Management Tools and Authentication

1. #### [Azure Portal](https://portal.azure.com/) 
2. #### [Azure Cloud Shell](https://shell.azure.com/)
  Online Azure Shell  
  Both Powershell and Bash supported  
  Azure Module, git, ansible, Terraform already installed  

  ![image](https://user-images.githubusercontent.com/13016162/71394577-27719280-2638-11ea-8d98-d2aeec5bf498.png)

  ![image](https://user-images.githubusercontent.com/13016162/71394857-6e13bc80-2639-11ea-8de5-258a03d0f5f0.png)

3. #### Install Powershell Az Module (Powershell 5.1 required, can be installed on Powershell Core 6.x versions)  

```powershell
# Powershell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```
4. #### Install Azure CLI (Windows MSI based Install, Ubuntu apt based --- Issues with WSL)

```powershell
# Powershell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```

```bash
# bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

![image](https://user-images.githubusercontent.com/13016162/71341172-1a449d00-257f-11ea-84aa-3d0e17c102b7.png)

## Switching Subscriptions across Tenents

#### Az-Cli
```bash
# Get the Current subscription
az account show
# Find out what roles you/others have
az role assignment list --output table
# Switch subscription (you can switch across tenents)
az account set --subscription my-subscription-name
```

#### Azure PowerShell
```powershell
# Get the Current subscription
Get-AzContext
# Switch subscription (you can switch across tenents)
Set-AzContext -subscription my-subscription-name
```

## Using Azure Managed Indentity VM

```bash
# Azure Cli
az login --identity

# API
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' -H Metadata:true

```

## Setting up Azure service principal with Azure CLI -- for third party, app access (Use it for Terraform)

>Automated tools that use Azure services should always have restricted permissions. Instead of having applications sign in as a fully privileged user, Azure offers service principals.
>
>An Azure service principal is an identity created for use with applications, hosted services, and automated tools to access Azure resources. This access is restricted by the roles assigned to the service principal, giving you control over which resources can be accessed and at which level. For security reasons, it's always recommended to use service principals with automated tools rather than allowing them to log in with a user identity.

```bash
# Create Service Principal with password string shown in output
az ad sp create-for-rbac --name vikiSp
# Create and assign your certificate
az ad sp create-for-rbac --name vikiSp --cert @/path/to/cert.pem
# Create sp and a new cert too (cab be stored in an azure vault)
az ad sp create-for-rbac --name vikisp --create-cert #----cert vikiSpCert --keyvault viki-vault
# Retrieve cert from vault
az keyvault secret show --name vikiSpCert --vault-name viki-vault
```
![image](https://user-images.githubusercontent.com/13016162/71341098-ed908580-257e-11ea-9ba7-375a65a7dbe9.png)

![image](https://user-images.githubusercontent.com/13016162/71341855-0f8b0780-2581-11ea-9f22-89e94dbd956a.png)

* Please keep your password and keys which you see in output safe as it can not be recovered in future
* However, you can reset it and get a new password/cert

```bash
az ad sp credential reset --name vikiServicePrincipal
```
>The default role for a service principal is Contributor. This role has full permissions to read and write to an Azure account. The Reader role is more restrictive, with read-only access. For more information on Role-Based Access Control (RBAC) and roles, see RBAC: Built-in roles.

```bash
az role assignment create --assignee APP_ID --role Reader
az role assignment delete --assignee APP_ID --role Contributor
```
Login using Service Principal

```bash
az login --service-principal -u <app-url> -p <password-or-cert> --tenant <tenant>
```

Generate api oAuth access token after login (for api calls)

```bash
az account get-access-token

{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InBpVmxsb1FEU01LeGgxbTJ5Z3FHU1ZkZ0ZwQSIsImtpZCI6InBpVmxsb1FEU01LeGgxbTJ5Z3FHU1ZkZ0ZwQSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuY29yZS53aW5kb3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC84YTIwYWI3Yy1jNDQ2LTQ1OWItYTNjYS1lMTI1NzliNjE4MDQvIiwiaWF0IjoxNTc4MTA5MzY4LCJuYmYiOjE1NzgxMDkzNjgsImV4cCI6MTU3ODExMzI2OCwiYWNyIjoiMSIsImFpbyI6IkFVUUF1LzhOQUFBQWdEVUN1UHd0M0ZtSFpDTGpsWHN5dzhvc29kam1BeWRnZHgzQ3JoT25TeGZaL29Ocklaa3A0WWJ0M09vRExhb1ZreitQMGpIaUlQNDR1RVI2MitDZkx3PT0iLCJhbHRzZWNpZCI6IjE6bGl2ZS5jb206MDAwMzQwMDEyMDc2MTYxRCIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiIwNGIwNzc5NS04ZGRiLTQ2MWEtYmJlZS0wMmY5ZTFiZjdiNDYiLCJhcHBpZGFjciI6IjAiLCJlbWFpbCI6InZpa2lzY3JpcHRzQGdtYWlsLmNvbSIsImZhbWlseV9uYW1lIjoiUGFuZGV5IiwiZ2l2ZW5fbmFtZSI6IlZpa2FzIFBhbmRleSIsImdyb3VwcyI6WyI5MjI0NmMzYy01OWM5LTRjZGQtOWQzZC0yMjY0ZWM0NTIwMTAiXSwiaWRwIjoibGl2ZS5jb20iLCJpcGFkZHIiOiIyMjMuMjI1Ljk2LjE1MSIsIm5hbWUiOiJWaWthcyBQYW5kZXkgUGFuZGV5Iiwib2lkIjoiNGFiMWE5NTQtMDlmMS00MmQ4LTllMjMtYzhlODU5NmFlNTIzIiwicHVpZCI6IjEwMDMyMDAwOTI3MTM4MDUiLCJzY3AiOiJTQmG7jdhNUs7E6vjFs9MD81XsIjkktT7RrK2SCVkn2UIueuSO_eDMQuTY71Xs8rUZgrcPaDeeWIHDqt38C5emXnNfw0zS4YJvUpS1fY2VKkBVlMVdYlFulQZjiRpY0SnlJURGaJcJpPEsdKM_cYTlvAJ0cI8Rdn7LhJEn7Tnbg1HJMdVcXaYeoYkVJupzMMvIkN5Bu41tqsWbnqU18mVm-yRlgO9nx8SA4czsSpUjPa3TTDaABzrUPdLaJj23HkA",
  "expiresOn": "2020-01-04 10:17:47.692445",
  "subscription": "e7691dac-480f-40ed-9270-3ab6eea695f7",
  "tenant": "8a20ab7c-c446-459b-a3ca-e12579b61804",
  "tokenType": "Bearer"
}
```
