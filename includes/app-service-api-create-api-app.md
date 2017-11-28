
<span data-ttu-id="f1f8f-101">Pomocí příkazu [az webapp create](/cli/azure/appservice/web#create) vytvořte [aplikaci API](../articles/app-service-api/app-service-api-apps-why-best-platform.md) v plánu služby App Service `myAppServicePlan`.</span><span class="sxs-lookup"><span data-stu-id="f1f8f-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="f1f8f-102">Tato webová aplikace poskytuje prostor pro hostování vašeho rozhraní API a adresu URL, na které si můžete nasazenou aplikaci zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f1f8f-102">The web app provides a hosting space for your API and provides a URL to view the deployed app.</span></span>

<span data-ttu-id="f1f8f-103">V následujícím příkazu nahraďte *\<app_name >* jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="f1f8f-103">In the following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="f1f8f-104">Pokud název `<app_name>` není jedinečný, zobrazí se chybová zpráva „Web se zadaným názvem <název_aplikace> již existuje“.</span><span class="sxs-lookup"><span data-stu-id="f1f8f-104">If `<app_name>` is not unique, you get the error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="f1f8f-105">Výchozí adresa URL webové aplikace je `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="f1f8f-105">The default URL of the web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="f1f8f-106">Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f1f8f-106">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```