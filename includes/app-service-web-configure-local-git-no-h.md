<span data-ttu-id="3c945-101">Konfigurace místního Git nasazení toohello webové aplikace s hello [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c945-101">Configure local Git deployment toohello web app with hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command.</span></span>

<span data-ttu-id="3c945-102">App Service podporuje několik způsobů toodeploy obsahu tooa webové aplikace, jako je například FTP, Git místní, GitHub, Visual Studio Team Services a Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="3c945-102">App Service supports several ways toodeploy content tooa web app, such as FTP, local Git, GitHub, Visual Studio Team Services, and Bitbucket.</span></span> <span data-ttu-id="3c945-103">Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu.</span><span class="sxs-lookup"><span data-stu-id="3c945-103">For this quickstart, you deploy by using local Git.</span></span> <span data-ttu-id="3c945-104">To znamená, že můžete nasadit pomocí příkazu toopush Git z úložiště tooa místní úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c945-104">That means you deploy by using a Git command toopush from a local repository tooa repository in Azure.</span></span> 

<span data-ttu-id="3c945-105">Následující příkaz a nahraďte v hello  *\<app_name >* s názvem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c945-105">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="3c945-106">výstup Hello má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="3c945-106">hello output has hello following format:</span></span>

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

<span data-ttu-id="3c945-107">Hello `<username>` je hello [nasazení uživatele](#configure-a-deployment-user) který jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3c945-107">hello `<username>` is hello [deployment user](#configure-a-deployment-user) that you created in a previous step.</span></span>

<span data-ttu-id="3c945-108">Zkopírujte hello URI vidět; budete ho používat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3c945-108">Copy hello URI shown; you'll use it in hello next step.</span></span>
