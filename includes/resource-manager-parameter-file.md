<span data-ttu-id="850e5-101">Pokud použijete soubor s parametry předat hodnoty parametrů během nasazování, budete muset vytvořte soubor JSON ve formátu podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="850e5-101">If you use a parameter file to pass parameter values during deployment, you need to create a JSON file with a format similar to the following example:</span></span>

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

<span data-ttu-id="850e5-102">Velikost souboru parametr nemůže být delší než 64 KB.</span><span class="sxs-lookup"><span data-stu-id="850e5-102">The size of the parameter file cannot be more than 64 KB.</span></span>

<span data-ttu-id="850e5-103">Pokud potřebujete zajistit citlivou hodnotu pro parametr (třeba heslo), přidejte tuto hodnotu do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="850e5-103">If you need to provide a sensitive value for a parameter (such as a password), add that value to a key vault.</span></span> <span data-ttu-id="850e5-104">Během nasazování načtěte trezoru klíčů, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="850e5-104">Retrieve the key vault during deployment as shown in the previous example.</span></span> <span data-ttu-id="850e5-105">Další informace najdete v tématu [předat zabezpečené hodnoty během nasazení](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="850e5-105">For more information, see [Pass secure values during deployment](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span> 

