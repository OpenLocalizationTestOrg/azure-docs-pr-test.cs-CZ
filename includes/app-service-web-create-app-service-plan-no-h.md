Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz.

[!INCLUDE [app-service-plan](app-service-plan.md)]

Hello následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` v hello **volné** cenové úrovně:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Po vytvoření hello plán služby App Service, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:

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