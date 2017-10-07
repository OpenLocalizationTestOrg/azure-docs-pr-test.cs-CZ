---
title: "aaaCreate webové aplikace Python v Azure | Microsoft Docs"
description: "Během několika minut můžete nasadit svou první aplikaci Hello World v Pythonu pomocí služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 42178d490d8aa8eaf93710667aad598794c62c8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="b1bbc-103">Vytvoření webové aplikace v Pythonu v Azure</span><span class="sxs-lookup"><span data-stu-id="b1bbc-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="b1bbc-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="b1bbc-105">Tento rychlý start provede jak toodevelop a nasazení aplikace tooAzure Python webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-105">This quickstart walks through how toodevelop and deploy a Python app tooAzure Web Apps.</span></span> <span data-ttu-id="b1bbc-106">Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou Python kód toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Python code toohello web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="b1bbc-108">Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="b1bbc-109">Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="b1bbc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b1bbc-110">Prerequisites</span></span>

<span data-ttu-id="b1bbc-111">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="b1bbc-111">toocomplete this tutorial:</span></span>

1. <span data-ttu-id="b1bbc-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="b1bbc-112">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="b1bbc-113">[Nainstalovat Python](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b1bbc-113">[Install Python](https://www.python.org/downloads/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b1bbc-114">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b1bbc-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b1bbc-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b1bbc-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="b1bbc-117">Stažení ukázky hello</span><span class="sxs-lookup"><span data-stu-id="b1bbc-117">Download hello sample</span></span>

<span data-ttu-id="b1bbc-118">Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="b1bbc-119">Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="b1bbc-120">Změnit toohello adresář, který obsahuje hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="b1bbc-121">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b1bbc-121">Run hello app locally</span></span>

<span data-ttu-id="b1bbc-122">Instalovat balíčky hello vyžaduje použití `pip`.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-122">Install hello required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="b1bbc-123">Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `Python` příkaz toolaunch hello integrovaného Python webového serveru.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-123">Run hello application locally by opening a terminal window and using hello `Python` command toolaunch hello built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="b1bbc-124">Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-124">Open a web browser, and navigate toohello sample app at http://localhost:5000.</span></span>

<span data-ttu-id="b1bbc-125">Můžete zobrazit hello **Hello, World** zprávu od hello ukázková aplikace zobrazí stránku hello.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-125">You can see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="b1bbc-127">V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-127">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="b1bbc-129">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-toouse-python"></a><span data-ttu-id="b1bbc-130">Konfigurace toouse Python</span><span class="sxs-lookup"><span data-stu-id="b1bbc-130">Configure toouse Python</span></span>

<span data-ttu-id="b1bbc-131">Použití hello [az webapp konfigurace sady](/cli/azure/webapp/config#set) příkaz tooconfigure hello verze Python webové aplikace toouse `3.4`.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-131">Use hello [az webapp config set](/cli/azure/webapp/config#set) command tooconfigure hello web app toouse Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="b1bbc-132">Nastavení verze Python hello tímto způsobem používá výchozí kontejner poskytované hello platformy.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-132">Setting hello Python version this way uses a default container provided by hello platform.</span></span> <span data-ttu-id="b1bbc-133">toouse vlastní kontejner, najdete v části hello referenční dokumentace rozhraní příkazového řádku pro hello [sadu kontejneru konfigurace webapp az](/cli/azure/webapp/config/container#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-133">toouse your own container, see hello CLI reference for hello [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up too4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="b1bbc-134">Procházet toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="b1bbc-134">Browse toohello app</span></span>

<span data-ttu-id="b1bbc-135">Procházet toohello nasadit aplikaci pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-135">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="b1bbc-136">Hello Python ukázkový kód běží ve webové aplikaci Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-136">hello Python sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="b1bbc-138">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="b1bbc-138">**Congratulations!**</span></span> <span data-ttu-id="b1bbc-139">Jste nasadili vaší první aplikace tooApp Python služby.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-139">You've deployed your first Python app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="b1bbc-140">Aktualizace a znovu nasaďte hello kódu</span><span class="sxs-lookup"><span data-stu-id="b1bbc-140">Update and redeploy hello code</span></span>

<span data-ttu-id="b1bbc-141">Pomocí místní textovém editoru otevřete hello `main.py` souboru v aplikaci Python hello a proveďte další toohello malých změn toohello text `return` příkaz:</span><span class="sxs-lookup"><span data-stu-id="b1bbc-141">Using a local text editor, open hello `main.py` file in hello Python app, and make a small change toohello text next toohello `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="b1bbc-142">Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-142">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="b1bbc-143">Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello [procházet toohello aplikace](#browse-to-the-app) kroku a aktualizovat stránku hello.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-143">Once deployment has completed, switch back toohello browser window that opened in hello [Browse toohello app](#browse-to-the-app) step, and refresh hello page.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="b1bbc-145">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="b1bbc-145">Manage your new Azure web app</span></span>

<span data-ttu-id="b1bbc-146">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-146">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="b1bbc-147">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-147">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="b1bbc-149">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-149">You see your web app's Overview page.</span></span> <span data-ttu-id="b1bbc-150">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="b1bbc-152">levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bbc-152">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="b1bbc-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1bbc-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b1bbc-154">Python sh PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b1bbc-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
