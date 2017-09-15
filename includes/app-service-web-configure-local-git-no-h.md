<span data-ttu-id="3d6e6-101">Nakonfigurujte nasazení do webové aplikace prostřednictvím místního Gitu pomocí příkazu [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git).</span><span class="sxs-lookup"><span data-stu-id="3d6e6-101">Configure local Git deployment to the web app with the [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command.</span></span>

<span data-ttu-id="3d6e6-102">App Service podporuje řadu způsobů nasazení obsahu do webové aplikace, jako například FTP, místní Git, GitHub, Visual Studio Team Services nebo Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-102">App Service supports several ways to deploy content to a web app, such as FTP, local Git, GitHub, Visual Studio Team Services, and Bitbucket.</span></span> <span data-ttu-id="3d6e6-103">Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-103">For this quickstart, you deploy by using local Git.</span></span> <span data-ttu-id="3d6e6-104">To znamená, že nasazení provedete použitím příkazu Gitu, který přenese obsah z místního úložiště do úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-104">That means you deploy by using a Git command to push from a local repository to a repository in Azure.</span></span> 

<span data-ttu-id="3d6e6-105">V následujícím příkazu nahraďte *\<app_name>* názvem webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-105">In the following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="3d6e6-106">Výstup má následující formát:</span><span class="sxs-lookup"><span data-stu-id="3d6e6-106">The output has the following format:</span></span>

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

<span data-ttu-id="3d6e6-107">`<username>` je [uživatel nasazení](#configure-a-deployment-user), kterého jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-107">The `<username>` is the [deployment user](#configure-a-deployment-user) that you created in a previous step.</span></span>

<span data-ttu-id="3d6e6-108">Zkopírujte zobrazený identifikátor URI. Použijete ho v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="3d6e6-108">Copy the URI shown; you'll use it in the next step.</span></span>
