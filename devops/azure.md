# Azure

### Show regions

```text
az account list-locations --query "sort_by([].{DisplayName:displayName, Name:name}, &DisplayName)" --output table
```

Create service principal with creadentials



```text
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

