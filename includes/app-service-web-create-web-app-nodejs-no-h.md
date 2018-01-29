V prostředí cloudu, vytvoření webové aplikace v `myAppServicePlan` plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) příkaz. 

V následujícím příkladu nahraďte `<app_name>` s globálně jedinečným názvem aplikace (platnými znaky jsou `a-z`, `0-9`, a `-`). Modul runtime je nastaven na `NODE|6.9`. Pokud chcete zobrazit všechny podporované moduly runtime, spusťte [az webapp seznamu runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
```

Po vytvoření webové aplikace Azure CLI ukazuje výstup podobně jako v následujícím příkladu:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Nasazení git povoleno jste vytvořili prázdný webové aplikace.

> [!NOTE]
> Adresa URL Git vzdáleného jsou uvedené v `deploymentLocalGitUrl` vlastnost ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tato adresa URL uložte, protože ho budete potřebovat později.
>
