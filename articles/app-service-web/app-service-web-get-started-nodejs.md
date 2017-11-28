---
title: "aaaCreate webové aplikace Node.js v Azure | Microsoft Docs"
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
ms.openlocfilehash: 163edf83b2353755fc9fa2d75aed489038cf7c81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="db534-103">Vytvoření webové aplikace Node.js ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="db534-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="db534-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="db534-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="db534-105">Tento rychlý start ukazuje, jak toodeploy tooAzure aplikace Node.js webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="db534-105">This quickstart shows how toodeploy a Node.js app tooAzure Web Apps.</span></span> <span data-ttu-id="db534-106">Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou Node.js kód toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db534-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Node.js code toohello web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="db534-108">Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="db534-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="db534-109">Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.</span><span class="sxs-lookup"><span data-stu-id="db534-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="db534-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db534-110">Prerequisites</span></span>

<span data-ttu-id="db534-111">toocomplete tento rychlý start:</span><span class="sxs-lookup"><span data-stu-id="db534-111">toocomplete this quickstart:</span></span>

* <span data-ttu-id="db534-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="db534-112">[Install Git](https://git-scm.com/)</span></span>
* <span data-ttu-id="db534-113">[Nainstalovat Node.js a NPM](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="db534-113">[Install Node.js and NPM](https://nodejs.org/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="db534-114">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="db534-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="db534-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="db534-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="db534-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="db534-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="db534-117">Stažení ukázky hello</span><span class="sxs-lookup"><span data-stu-id="db534-117">Download hello sample</span></span>

<span data-ttu-id="db534-118">Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="db534-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="db534-119">Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="db534-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="db534-120">Změnit toohello adresář, který obsahuje hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="db534-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="db534-121">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="db534-121">Run hello app locally</span></span>

<span data-ttu-id="db534-122">Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `npm start` skriptu toolaunch hello součástí server Node.js HTTP.</span><span class="sxs-lookup"><span data-stu-id="db534-122">Run hello application locally by opening a terminal window and using hello `npm start` script toolaunch hello built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="db534-123">Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na adresu http://localhost: 1337.</span><span class="sxs-lookup"><span data-stu-id="db534-123">Open a web browser, and navigate toohello sample app at http://localhost:1337.</span></span>

<span data-ttu-id="db534-124">Zobrazí hello **Hello, World** zprávu od hello ukázková aplikace zobrazí stránku hello.</span><span class="sxs-lookup"><span data-stu-id="db534-124">You see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="db534-126">V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="db534-126">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="db534-128">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db534-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up too4 threads.
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
remote: Node.js versions available on hello platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file toochoose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="db534-129">Procházet toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="db534-129">Browse toohello app</span></span>

<span data-ttu-id="db534-130">Procházet toohello nasadit aplikaci pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="db534-130">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="db534-131">Ukázkový kód Node.js Hello běží ve webové aplikaci Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="db534-131">hello Node.js sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="db534-133">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="db534-133">**Congratulations!**</span></span> <span data-ttu-id="db534-134">Vaše první tooApp aplikace Node.js služby jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="db534-134">You've deployed your first Node.js app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="db534-135">Aktualizace a znovu nasaďte hello kódu</span><span class="sxs-lookup"><span data-stu-id="db534-135">Update and redeploy hello code</span></span>

<span data-ttu-id="db534-136">Pomocí textového editoru otevřete hello `index.js` souboru v aplikaci Node.js hello a proveďte textový toohello malé změny ve volání hello příliš`response.end`:</span><span class="sxs-lookup"><span data-stu-id="db534-136">Using a text editor, open hello `index.js` file in hello Node.js app, and make a small change toohello text in hello call too`response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="db534-137">Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="db534-137">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="db534-138">Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello **procházet toohello aplikace** kroku a stiskněte tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="db534-138">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and hit refresh.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="db534-140">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="db534-140">Manage your new Azure web app</span></span>

<span data-ttu-id="db534-141">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="db534-141">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="db534-142">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="db534-142">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="db534-144">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="db534-144">You see your web app's Overview page.</span></span> <span data-ttu-id="db534-145">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="db534-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="db534-147">levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="db534-147">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="db534-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db534-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="db534-149">Node.js s databází MongoDB</span><span class="sxs-lookup"><span data-stu-id="db534-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
