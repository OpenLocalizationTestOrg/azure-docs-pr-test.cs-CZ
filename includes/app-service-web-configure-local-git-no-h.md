Konfigurace místního Git nasazení toohello webové aplikace s hello [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz.

App Service podporuje několik způsobů toodeploy obsahu tooa webové aplikace, jako je například FTP, Git místní, GitHub, Visual Studio Team Services a Bitbucket. Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu. To znamená, že můžete nasadit pomocí příkazu toopush Git z úložiště tooa místní úložiště v Azure. 

Následující příkaz a nahraďte v hello  *\<app_name >* s názvem vaší webové aplikace.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

výstup Hello má hello následující formát:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Hello `<username>` je hello [nasazení uživatele](#configure-a-deployment-user) který jste vytvořili v předchozím kroku.

Zkopírujte hello URI vidět; budete ho používat v dalším kroku hello.
