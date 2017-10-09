Pokud používáte hodnoty parametr toopass souboru parametrů během nasazování, je třeba toocreate soubor JSON s formátu podobné toohello následující ukázka:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

velikost Hello hello parametr souboru nemůže být delší než 64 KB.

Pokud potřebujete tooprovide citlivou hodnotu pro parametr (třeba heslo), přidejte tuto hodnotu tooa klíče trezoru. Načtěte trezoru klíčů hello během nasazení, jak je uvedeno v předchozím příkladu hello. Další informace najdete v tématu [předat zabezpečené hodnoty během nasazení](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

