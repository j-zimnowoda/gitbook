# Azure

### Show regions

```text
az account list-locations --query "sort_by([].{DisplayName:displayName, Name:name}, &DisplayName)" --output table
```

