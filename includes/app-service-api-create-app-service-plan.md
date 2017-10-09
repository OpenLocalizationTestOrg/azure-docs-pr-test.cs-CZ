<span data-ttu-id="b9f6f-101">Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b9f6f-101">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span>

[!INCLUDE [app-service-plan](app-service-plan.md)]

<span data-ttu-id="b9f6f-102">Hello následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` v hello **volné** cenové úrovně:</span><span class="sxs-lookup"><span data-stu-id="b9f6f-102">hello following example creates an App Service plan named `myAppServicePlan` in hello **Free** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="b9f6f-103">Po vytvoření hello plán služby App Service, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="b9f6f-103">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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