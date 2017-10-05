---
title: "Vytvoření webové aplikace Node.js ve službě Azure | Dokumentace Microsoftu"
description: "Během několika minut můžete nasadit svou první aplikaci Node.js Hello World pomocí služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ce845da09a7c088b8a2ba29b818a46a3b41aa4e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="7ad8d-103">Vytvoření webové aplikace Node.js ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="7ad8d-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="7ad8d-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="7ad8d-105">V tomto kurzu Rychlý start se dozvíte, jak nasadit aplikaci Node.js pomocí služby Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-105">This quickstart shows how to deploy a Node.js app to Azure Web Apps.</span></span> <span data-ttu-id="7ad8d-106">Vytvoříte webovou aplikaci pomocí rozhraní příkazového řádku [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a pomocí Gitu nasadíte ukázkový kód Node.js do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Node.js code to the web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="7ad8d-108">Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="7ad8d-109">Pokud máte nainstalované všechny požadované prostředky, zabere vám tento postup zhruba pět minut.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="7ad8d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7ad8d-110">Prerequisites</span></span>

<span data-ttu-id="7ad8d-111">K provedení kroků v tomto kurzu Rychlý start je potřeba:</span><span class="sxs-lookup"><span data-stu-id="7ad8d-111">To complete this quickstart:</span></span>

* <span data-ttu-id="7ad8d-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="7ad8d-112">[Install Git](https://git-scm.com/)</span></span>
* <span data-ttu-id="7ad8d-113">[Nainstalovat Node.js a NPM](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="7ad8d-113">[Install Node.js and NPM](https://nodejs.org/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7ad8d-114">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7ad8d-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="7ad8d-116">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7ad8d-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="7ad8d-117">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="7ad8d-117">Download the sample</span></span>

<span data-ttu-id="7ad8d-118">V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="7ad8d-119">Toto okno terminálu budete používat ke spuštění všech příkazů v tomto kurzu Rychlý start.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="7ad8d-120">Přejděte do adresáře, který obsahuje vzorový kód.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-120">Change to the directory that contains the sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="7ad8d-121">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="7ad8d-121">Run the app locally</span></span>

<span data-ttu-id="7ad8d-122">Aplikaci spustíte místně tak, že otevřete okno terminálu a pomocí skriptu `npm start` spustíte integrovaný server HTTP aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-122">Run the application locally by opening a terminal window and using the `npm start` script to launch the built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="7ad8d-123">Otevřete webový prohlížeč a přejděte na ukázkovou aplikaci na adrese http://localhost:1337.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-123">Open a web browser, and navigate to the sample app at http://localhost:1337.</span></span>

<span data-ttu-id="7ad8d-124">Na stránce se zobrazí zpráva **Hello World** z ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-124">You see the **Hello World** message from the sample app displayed in the page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="7ad8d-126">V okně terminálu ukončete webový server stisknutím **Ctrl + C**.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-126">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="7ad8d-128">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="7ad8d-129">Přechod do aplikace</span><span class="sxs-lookup"><span data-stu-id="7ad8d-129">Browse to the app</span></span>

<span data-ttu-id="7ad8d-130">V prohlížeči zadejte adresu nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-130">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="7ad8d-131">Vzorový kód Node.js je spuštěný ve webové aplikaci služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-131">The Node.js sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="7ad8d-133">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="7ad8d-133">**Congratulations!**</span></span> <span data-ttu-id="7ad8d-134">Nasadili jste svoji první aplikaci Node.js do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-134">You've deployed your first Node.js app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="7ad8d-135">Aktualizace a opětovné nasazení kódu</span><span class="sxs-lookup"><span data-stu-id="7ad8d-135">Update and redeploy the code</span></span>

<span data-ttu-id="7ad8d-136">V textovém editoru otevřete soubor `index.js`, který je součástí aplikace Node.js, a proveďte malou změnu textu u volání `response.end`:</span><span class="sxs-lookup"><span data-stu-id="7ad8d-136">Using a text editor, open the `index.js` file in the Node.js app, and make a small change to the text in the call to `response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="7ad8d-137">Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-137">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="7ad8d-138">Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a stiskněte tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-138">Once deployment has completed, switch back to the browser window that opened in the **Browse to the app** step, and hit refresh.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="7ad8d-140">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="7ad8d-140">Manage your new Azure web app</span></span>

<span data-ttu-id="7ad8d-141">Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-141">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="7ad8d-142">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-142">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="7ad8d-144">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-144">You see your web app's Overview page.</span></span> <span data-ttu-id="7ad8d-145">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="7ad8d-147">Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ad8d-147">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="7ad8d-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ad8d-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7ad8d-149">Node.js s databází MongoDB</span><span class="sxs-lookup"><span data-stu-id="7ad8d-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
