# Azure

## Azure DNS

Get all FQDN registered at certain domain

```
resourceGroup='external-dns'
dnsZone='example.com'
az network dns record-set a list -g "$resourceGroup" -z "$dnsZone" | jq '.[].fqdn' -r 
```

## Regions

### Show regions

```
az account list-locations --query "sort_by([].{DisplayName:displayName, Name:name}, &DisplayName)" --output table
```

## Service principal

### Create service principal with creadentials

[https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authorization#use-file-based-authentication](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authorization#use-file-based-authentication)

```
> az ad sp create-for-rbac --sdk-auth > azure.auth 
> cat azure.auth
{
  "clientId": "clientId",
  "clientSecret": "clientSecret",
  "subscriptionId": "subscriptionId",
  "tenantId": "tenantId",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

