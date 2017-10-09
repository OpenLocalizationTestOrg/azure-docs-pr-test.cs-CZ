
<span data-ttu-id="8bfa2-101">Vytvoření [aplikace API](../articles/app-service-api/app-service-api-apps-why-best-platform.md) v hello `myAppServicePlan` plán služby App Service se hello [az webapp vytvořit](/cli/azure/appservice/web#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8bfa2-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="8bfa2-102">Hello webovou aplikaci obsahuje hostování místa pro vaše rozhraní API a adresu URL tooview hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8bfa2-102">hello web app provides a hosting space for your API and provides a URL tooview hello deployed app.</span></span>

<span data-ttu-id="8bfa2-103">Následující příkaz a nahraďte v hello  *\<app_name >* s jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="8bfa2-103">In hello following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="8bfa2-104">Pokud `<app_name>` není jedinečný. získáte hello chybová zpráva "Web se zadaným názvem < název_aplikace > již existuje."</span><span class="sxs-lookup"><span data-stu-id="8bfa2-104">If `<app_name>` is not unique, you get hello error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="8bfa2-105">Výchozí adresa URL webové aplikace hello je Hello `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="8bfa2-105">hello default URL of hello web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="8bfa2-106">Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8bfa2-106">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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