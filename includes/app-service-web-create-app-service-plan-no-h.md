<span data-ttu-id="c7284-101">Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c7284-101">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span>

[!INCLUDE [app-service-plan](app-service-plan.md)]

<span data-ttu-id="c7284-102">Následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` v cenové úrovni **Free**:</span><span class="sxs-lookup"><span data-stu-id="c7284-102">The following example creates an App Service plan named `myAppServicePlan` in the **Free** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="c7284-103">Po vytvoření plánu služby App Service se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c7284-103">When the App Service plan has been created, the Azure CLI shows information similar to the following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```