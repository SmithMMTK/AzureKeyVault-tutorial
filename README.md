# AzureKeyVault-tutorial

## Preparation

### Install dependency
```bash
pip install azure-keyvault-secrets
pip install azure.identity
```

## Initial variable
```bash
export rg="az-keyvault"
export kv="az-smi15-keyvault"

export rg="az-keyvault-demo"
#export keyvaultname="smi15-keyvault-demo"
export AZURE_CLIENT_ID="8dd8b0dc-4f0c-42e6-92d4-8fa46878cba1"
export AZURE_CLIENT_SECRET="6hk-eCf0L6e.OCj1RVfRQ99s8mOIftCo5t"
export AZURE_TENANT_ID="72f988bf-86f1-41af-91ab-2d7cd011db47"
export KEY_VAULT_NAME=$kv

```

## Create Key vault
```bash
az group create --name $rg -l "southeastasia"
az keyvault create --name $kv -g $rg
```

## Create a service principle
```bash
az ad sp create-for-rbac --skip-assignment -n "http://az-smi15-keyvault" --sdk-auth
```

```json
{
  "clientId": "8dd8b0dc-4f0c-42e6-92d4-8fa46878cba1",
  "clientSecret": "6hk-eCf0L6e.OCj1RVfRQ99s8mOIftCo5t",
  "subscriptionId": "a5690002-a0db-48a6-bd96-634ce65c45ee",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

### Give the service principle access key vault
```bash
az keyvault set-policy -n $kv --spn $AZURE_CLIENT_ID --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey

```

### Run python application
```bash
python keyvaultapp.py
```
