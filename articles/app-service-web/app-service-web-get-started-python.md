---
title: "Vytvoření webové aplikace v Pythonu v Azure | Dokumentace Microsoftu"
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
ms.openlocfilehash: 119f9770097c010cc360e0e204d06a307a268814
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="290cf-103">Vytvoření webové aplikace v Pythonu v Azure</span><span class="sxs-lookup"><span data-stu-id="290cf-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="290cf-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="290cf-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="290cf-105">Tento kurz Rychlý start vás provede vývojem a nasazením aplikace v Pythonu do Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="290cf-105">This quickstart walks through how to develop and deploy a Python app to Azure Web Apps.</span></span> <span data-ttu-id="290cf-106">Vytvoříte webovou aplikaci pomocí rozhraní příkazového řádku [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a pomocí Gitu nasadíte do této webové aplikace ukázkový kód v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="290cf-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Python code to the web app.</span></span>

![Ukázková aplikace spuštěná v Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="290cf-108">Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="290cf-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="290cf-109">Pokud máte nainstalované všechny požadované prostředky, zabere vám tento postup zhruba pět minut.</span><span class="sxs-lookup"><span data-stu-id="290cf-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="290cf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="290cf-110">Prerequisites</span></span>

<span data-ttu-id="290cf-111">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="290cf-111">To complete this tutorial:</span></span>

1. <span data-ttu-id="290cf-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="290cf-112">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="290cf-113">[Nainstalovat Python](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="290cf-113">[Install Python](https://www.python.org/downloads/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="290cf-114">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="290cf-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="290cf-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="290cf-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="290cf-116">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="290cf-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="290cf-117">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="290cf-117">Download the sample</span></span>

<span data-ttu-id="290cf-118">V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="290cf-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="290cf-119">Toto okno terminálu budete používat ke spuštění všech příkazů v tomto kurzu Rychlý start.</span><span class="sxs-lookup"><span data-stu-id="290cf-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="290cf-120">Přejděte do adresáře, který obsahuje vzorový kód.</span><span class="sxs-lookup"><span data-stu-id="290cf-120">Change to the directory that contains the sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="290cf-121">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="290cf-121">Run the app locally</span></span>

<span data-ttu-id="290cf-122">Nainstalujte požadovaný balíček pomocí `pip`.</span><span class="sxs-lookup"><span data-stu-id="290cf-122">Install the required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="290cf-123">Aplikaci spustíte místně tak, že otevřete okno terminálu a pomocí příkazu `Python` spustíte integrovaný webový server Python.</span><span class="sxs-lookup"><span data-stu-id="290cf-123">Run the application locally by opening a terminal window and using the `Python` command to launch the built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="290cf-124">Otevřete webový prohlížeč a přejděte na ukázkovou aplikaci na adrese http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="290cf-124">Open a web browser, and navigate to the sample app at http://localhost:5000.</span></span>

<span data-ttu-id="290cf-125">Na stránce se zobrazí zpráva **Hello World** od ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="290cf-125">You can see the **Hello World** message from the sample app displayed in the page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="290cf-127">V okně terminálu ukončete webový server stisknutím **Ctrl + C**.</span><span class="sxs-lookup"><span data-stu-id="290cf-127">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="290cf-129">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="290cf-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-to-use-python"></a><span data-ttu-id="290cf-130">Konfigurace pro použití Pythonu</span><span class="sxs-lookup"><span data-stu-id="290cf-130">Configure to use Python</span></span>

<span data-ttu-id="290cf-131">Pomocí příkazu [az webapp config set](/cli/azure/webapp/config#set) nakonfigurujte webovou aplikaci tak, aby používala Python verze `3.4`.</span><span class="sxs-lookup"><span data-stu-id="290cf-131">Use the [az webapp config set](/cli/azure/webapp/config#set) command to configure the web app to use Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="290cf-132">Pokud nastavíte verzi Pythonu tímto způsobem, použije se výchozí kontejner poskytnutý platformou.</span><span class="sxs-lookup"><span data-stu-id="290cf-132">Setting the Python version this way uses a default container provided by the platform.</span></span> <span data-ttu-id="290cf-133">Pokud chcete použít vlastní kontejner, v referenci k rozhraní CLI vyhledejte příkaz [az webapp config container set](/cli/azure/webapp/config/container#set).</span><span class="sxs-lookup"><span data-stu-id="290cf-133">To use your own container, see the CLI reference for the [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="290cf-134">Přechod do aplikace</span><span class="sxs-lookup"><span data-stu-id="290cf-134">Browse to the app</span></span>

<span data-ttu-id="290cf-135">V prohlížeči zadejte adresu nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="290cf-135">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="290cf-136">Ukázkový kód Pythonu je spuštěný ve webové aplikaci služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="290cf-136">The Python sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná v Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="290cf-138">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="290cf-138">**Congratulations!**</span></span> <span data-ttu-id="290cf-139">Nasadili jste svoji první aplikaci v Pythonu do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="290cf-139">You've deployed your first Python app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="290cf-140">Aktualizace a opětovné nasazení kódu</span><span class="sxs-lookup"><span data-stu-id="290cf-140">Update and redeploy the code</span></span>

<span data-ttu-id="290cf-141">Pomocí místního textového editoru otevřete soubor `main.py` v rámci aplikace v Pythonu a proveďte malou změnu textu vedle příkazu `return`:</span><span class="sxs-lookup"><span data-stu-id="290cf-141">Using a local text editor, open the `main.py` file in the Python app, and make a small change to the text next to the `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="290cf-142">Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="290cf-142">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="290cf-143">Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku [Přechod do aplikace](#browse-to-the-app), a aktualizujte zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="290cf-143">Once deployment has completed, switch back to the browser window that opened in the [Browse to the app](#browse-to-the-app) step, and refresh the page.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="290cf-145">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="290cf-145">Manage your new Azure web app</span></span>

<span data-ttu-id="290cf-146">Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="290cf-146">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="290cf-147">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="290cf-147">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="290cf-149">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="290cf-149">You see your web app's Overview page.</span></span> <span data-ttu-id="290cf-150">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="290cf-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="290cf-152">Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="290cf-152">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="290cf-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="290cf-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="290cf-154">Python sh PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="290cf-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
