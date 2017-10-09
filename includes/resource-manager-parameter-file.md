<span data-ttu-id="be702-101">Pokud používáte hodnoty parametr toopass souboru parametrů během nasazování, je třeba toocreate soubor JSON s formátu podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="be702-101">If you use a parameter file toopass parameter values during deployment, you need toocreate a JSON file with a format similar toohello following example:</span></span>

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

<span data-ttu-id="be702-102">velikost Hello hello parametr souboru nemůže být delší než 64 KB.</span><span class="sxs-lookup"><span data-stu-id="be702-102">hello size of hello parameter file cannot be more than 64 KB.</span></span>

<span data-ttu-id="be702-103">Pokud potřebujete tooprovide citlivou hodnotu pro parametr (třeba heslo), přidejte tuto hodnotu tooa klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="be702-103">If you need tooprovide a sensitive value for a parameter (such as a password), add that value tooa key vault.</span></span> <span data-ttu-id="be702-104">Načtěte trezoru klíčů hello během nasazení, jak je uvedeno v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="be702-104">Retrieve hello key vault during deployment as shown in hello previous example.</span></span> <span data-ttu-id="be702-105">Další informace najdete v tématu [předat zabezpečené hodnoty během nasazení](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="be702-105">For more information, see [Pass secure values during deployment](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span> 

