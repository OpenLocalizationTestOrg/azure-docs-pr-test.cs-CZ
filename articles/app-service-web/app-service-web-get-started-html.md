---
title: "aaaCreate statické HTML webové aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak toorun webové aplikace v Azure App Service nasazením statické HTML ukázková aplikace."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/26/2017
ms.author: riande
ms.custom: mvc
ms.openlocfilehash: efd8c8189a3aa1ac35602b688eeb31bff6f5a373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a><span data-ttu-id="1dbc5-103">Vytvoření webové aplikace ve statickém HTML ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="1dbc5-103">Create a static HTML web app in Azure</span></span>

<span data-ttu-id="1dbc5-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="1dbc5-105">Tento rychlý start ukazuje, jak toodeploy základní HTML + CSS lokality tooAzure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-105">This quickstart shows how toodeploy a basic HTML+CSS site tooAzure Web Apps.</span></span> <span data-ttu-id="1dbc5-106">Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou HTML obsahu toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample HTML content toohello web app.</span></span>

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="1dbc5-108">Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="1dbc5-109">Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dbc5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1dbc5-110">Prerequisites</span></span>

<span data-ttu-id="1dbc5-111">toocomplete tento rychlý start:</span><span class="sxs-lookup"><span data-stu-id="1dbc5-111">toocomplete this quickstart:</span></span>

- <span data-ttu-id="1dbc5-112">[Nainstalovat Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)].</span><span class="sxs-lookup"><span data-stu-id="1dbc5-112">[Install Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1dbc5-113">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1dbc5-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1dbc5-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1dbc5-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="1dbc5-116">Stažení ukázky hello</span><span class="sxs-lookup"><span data-stu-id="1dbc5-116">Download hello sample</span></span>

<span data-ttu-id="1dbc5-117">Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-117">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

<span data-ttu-id="1dbc5-118">Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-118">You use this terminal window toorun all hello commands in this quickstart.</span></span>

## <a name="view-hello-html"></a><span data-ttu-id="1dbc5-119">Hello zobrazení HTML</span><span class="sxs-lookup"><span data-stu-id="1dbc5-119">View hello HTML</span></span>

<span data-ttu-id="1dbc5-120">Přejděte toohello adresář, který obsahuje ukázkové hello HTML.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-120">Navigate toohello directory that contains hello sample HTML.</span></span> <span data-ttu-id="1dbc5-121">Otevřete hello *index.html* souboru v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-121">Open hello *index.html* file in your browser.</span></span>

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-html/app-service-web-service-created.png)

<span data-ttu-id="1dbc5-124">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-124">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up too4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="1dbc5-125">Procházet toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="1dbc5-125">Browse toohello app</span></span>

<span data-ttu-id="1dbc5-126">Přejděte v prohlížeči, adresa URL toohello Azure webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="1dbc5-126">In a browser, go toohello Azure web app URL:</span></span>

```
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="1dbc5-127">stránku Hello běží jako webová aplikace Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-127">hello page is running as an Azure App Service web app.</span></span>

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="1dbc5-129">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="1dbc5-129">**Congratulations!**</span></span> <span data-ttu-id="1dbc5-130">Jste nasadili vaší první aplikace tooApp HTML služby.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-130">You've deployed your first HTML app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-app"></a><span data-ttu-id="1dbc5-131">Aktualizace a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1dbc5-131">Update and redeploy hello app</span></span>

<span data-ttu-id="1dbc5-132">Otevřete hello *index.html* soubor v textovém editoru a proveďte změnu toohello značky.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-132">Open hello *index.html* file in a text editor, and make a change toohello markup.</span></span> <span data-ttu-id="1dbc5-133">Můžete například změnit záhlaví hello H1 z "Azure App Service – ukázka statické HTML Server" toojust "Azure App Service".</span><span class="sxs-lookup"><span data-stu-id="1dbc5-133">For example, change hello H1 heading from "Azure App Service - Sample Static HTML Site" toojust "Azure App Service\`.</span></span>

<span data-ttu-id="1dbc5-134">Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated HTML"
git push azure master
```

<span data-ttu-id="1dbc5-135">Po dokončení nasazení aktualizujte změny hello toosee prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-135">Once deployment has completed, refresh your browser toosee hello changes.</span></span>

![Domovská stránka aktualizované ukázkové aplikace](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="1dbc5-137">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="1dbc5-137">Manage your new Azure web app</span></span>

<span data-ttu-id="1dbc5-138">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="1dbc5-139">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-html/portal1.png)

<span data-ttu-id="1dbc5-141">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-141">You see your web app's Overview page.</span></span> <span data-ttu-id="1dbc5-142">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-html/portal2.png)

<span data-ttu-id="1dbc5-144">levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dbc5-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="1dbc5-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1dbc5-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1dbc5-146">Mapování vlastní domény</span><span class="sxs-lookup"><span data-stu-id="1dbc5-146">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
