<span data-ttu-id="54162-101">Vytvoření [webové aplikace](../articles/app-service-web/app-service-web-overview.md) v hello `myAppServicePlan` plán služby App Service se hello [az webapp vytvořit](/cli/azure/webapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="54162-101">Create a [web app](../articles/app-service-web/app-service-web-overview.md) in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="54162-102">webové aplikace Hello hostování místo kódu a poskytuje adresu URL tooview hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="54162-102">hello web app provides a hosting space for your code and provides a URL tooview hello deployed app.</span></span>

<span data-ttu-id="54162-103">Následující příkaz a nahraďte v hello  *\<app_name >* s jedinečným názvem (platnými znaky jsou `a-z`, `0-9`, a `-`).</span><span class="sxs-lookup"><span data-stu-id="54162-103">In hello following command, replace *\<app_name>* with a unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="54162-104">Pokud `<app_name>` není jedinečný. získáte hello chybová zpráva "Web se zadaným názvem < název_aplikace > již existuje."</span><span class="sxs-lookup"><span data-stu-id="54162-104">If `<app_name>` is not unique, you get hello error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="54162-105">Výchozí adresa URL webové aplikace hello je Hello `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="54162-105">hello default URL of hello web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="54162-106">Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="54162-106">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="54162-107">Procházejte toohello lokality toosee nově vytvořenou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54162-107">Browse toohello site toosee your newly created web app.</span></span>

```bash
http://<app_name>.azurewebsites.net
```
