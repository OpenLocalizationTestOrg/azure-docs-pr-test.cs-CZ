
Vytvoření [aplikace API](../articles/app-service-api/app-service-api-apps-why-best-platform.md) v hello `myAppServicePlan` plán služby App Service se hello [az webapp vytvořit](/cli/azure/appservice/web#create) příkaz. 

Hello webovou aplikaci obsahuje hostování místa pro vaše rozhraní API a adresu URL tooview hello nasazené aplikace.

Následující příkaz a nahraďte v hello  *\<app_name >* s jedinečným názvem. Pokud `<app_name>` není jedinečný. získáte hello chybová zpráva "Web se zadaným názvem < název_aplikace > již existuje." Výchozí adresa URL webové aplikace hello je Hello `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:

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