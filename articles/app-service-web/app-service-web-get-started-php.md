---
title: "aaaCreate PHP webové aplikace v Azure | Microsoft Docs"
description: "Během několika minut můžete nasadit svou první aplikaci PHP Hello World pomocí služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/21/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8e1022889ca162f8f15ce7435cc9393cc6efef06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-web-app-in-azure"></a><span data-ttu-id="54799-103">Vytvoření webové aplikace v PHP v Azure</span><span class="sxs-lookup"><span data-stu-id="54799-103">Create a PHP web app in Azure</span></span>

<span data-ttu-id="54799-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="54799-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="54799-105">Tento rychlý úvodní kurz ukazuje, jak toodeploy tooAzure aplikace PHP webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="54799-105">This quickstart tutorial shows how toodeploy a PHP app tooAzure Web Apps.</span></span> <span data-ttu-id="54799-106">Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) v prostředí cloudu a můžete pomocí Git toodeploy ukázkovou PHP kód toohello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54799-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell, and you use Git toodeploy sample PHP code toohello web app.</span></span>

<span data-ttu-id="54799-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span><span class="sxs-lookup"><span data-stu-id="54799-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span></span>

<span data-ttu-id="54799-108">Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="54799-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="54799-109">Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.</span><span class="sxs-lookup"><span data-stu-id="54799-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54799-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="54799-110">Prerequisites</span></span>

<span data-ttu-id="54799-111">toocomplete tento rychlý start:</span><span class="sxs-lookup"><span data-stu-id="54799-111">toocomplete this quickstart:</span></span>

* <span data-ttu-id="54799-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="54799-112">[Install Git](https://git-scm.com/)</span></span>
* <span data-ttu-id="54799-113">[Nainstalovat PHP](https://php.net).</span><span class="sxs-lookup"><span data-stu-id="54799-113">[Install PHP](https://php.net)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a><span data-ttu-id="54799-114">Stažení ukázky hello místně</span><span class="sxs-lookup"><span data-stu-id="54799-114">Download hello sample locally</span></span>

<span data-ttu-id="54799-115">Okno terminálu spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="54799-115">In a terminal window, run hello following commands.</span></span> <span data-ttu-id="54799-116">Tato akce klonovat hello ukázkové aplikace tooyour místní počítač a přejděte toohello adresář obsahující hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="54799-116">This will clone hello sample application tooyour local machine, and navigate toohello directory containing hello sample code.</span></span>

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="54799-117">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="54799-117">Run hello app locally</span></span>

<span data-ttu-id="54799-118">Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `php` příkaz toolaunch hello integrovaného PHP webového serveru.</span><span class="sxs-lookup"><span data-stu-id="54799-118">Run hello application locally by opening a terminal window and using hello `php` command toolaunch hello built-in PHP web server.</span></span>

```bash
php -S localhost:8080
```

<span data-ttu-id="54799-119">Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na adrese http://localhost: 8080.</span><span class="sxs-lookup"><span data-stu-id="54799-119">Open a web browser, and navigate toohello sample app at http://localhost:8080.</span></span>

<span data-ttu-id="54799-120">Zobrazí hello **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="54799-120">You see hello **Hello World!**</span></span> <span data-ttu-id="54799-121">zpráva z hello ukázková aplikace zobrazí stránku hello.</span><span class="sxs-lookup"><span data-stu-id="54799-121">message from hello sample app displayed in hello page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

<span data-ttu-id="54799-123">V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="54799-123">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="54799-125">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54799-125">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up too4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-toohello-app-locally"></a><span data-ttu-id="54799-126">Procházet toohello aplikace místně</span><span class="sxs-lookup"><span data-stu-id="54799-126">Browse toohello app locally</span></span>

<span data-ttu-id="54799-127">Procházet toohello nasadit aplikaci pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="54799-127">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="54799-128">Hello PHP ukázkový kód běží ve webové aplikaci Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="54799-128">hello PHP sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

<span data-ttu-id="54799-130">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="54799-130">**Congratulations!**</span></span> <span data-ttu-id="54799-131">Jste nasadili vaší první aplikace tooApp PHP služby.</span><span class="sxs-lookup"><span data-stu-id="54799-131">You've deployed your first PHP app tooApp Service.</span></span>

## <a name="update-locally-and-redeploy-hello-code"></a><span data-ttu-id="54799-132">Aktualizovat místně a znovu nasaďte hello kódu</span><span class="sxs-lookup"><span data-stu-id="54799-132">Update locally and redeploy hello code</span></span>

<span data-ttu-id="54799-133">Pomocí místní textovém editoru otevřete hello `index.php` souboru v rámci aplikace PHP hello a proveďte textový toohello malých změn v rámci hello řetězec vedle příliš`echo`:</span><span class="sxs-lookup"><span data-stu-id="54799-133">Using a local text editor, open hello `index.php` file within hello PHP app, and make a small change toohello text within hello string next too`echo`:</span></span>

```php
echo "Hello Azure!";
```

<span data-ttu-id="54799-134">Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="54799-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="54799-135">Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello **procházet toohello aplikace** kroku a aktualizovat stránku hello.</span><span class="sxs-lookup"><span data-stu-id="54799-135">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and refresh hello page.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="54799-137">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="54799-137">Manage your new Azure web app</span></span>

<span data-ttu-id="54799-138">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="54799-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="54799-139">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="54799-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

<span data-ttu-id="54799-141">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="54799-141">You see your web app's Overview page.</span></span> <span data-ttu-id="54799-142">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="54799-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span>

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

<span data-ttu-id="54799-144">levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="54799-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="54799-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54799-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="54799-146">PHP s databází MySQL</span><span class="sxs-lookup"><span data-stu-id="54799-146">PHP with MySQL</span></span>](app-service-web-tutorial-php-mysql.md)
