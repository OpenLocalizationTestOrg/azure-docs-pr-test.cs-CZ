---
title: "Vytvoření webové aplikace PHP ve službě Azure | Dokumentace Microsoftu"
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
ms.openlocfilehash: 3a78e0b485046ad6228bf4819d3908042c298d1a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-php-web-app-in-azure"></a><span data-ttu-id="b3385-103">Vytvoření webové aplikace v PHP v Azure</span><span class="sxs-lookup"><span data-stu-id="b3385-103">Create a PHP web app in Azure</span></span>

<span data-ttu-id="b3385-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="b3385-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="b3385-105">V tomto kurzu Rychlý start se dozvíte, jak nasadit aplikaci PHP pomocí služby Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b3385-105">This quickstart tutorial shows how to deploy a PHP app to Azure Web Apps.</span></span> <span data-ttu-id="b3385-106">Pomocí [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) ve službě Cloud Shell vytvoříte webovou aplikaci a pomocí Gitu do této webové aplikace nasadíte vzorový kód PHP.</span><span class="sxs-lookup"><span data-stu-id="b3385-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell, and you use Git to deploy sample PHP code to the web app.</span></span>

<span data-ttu-id="b3385-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span><span class="sxs-lookup"><span data-stu-id="b3385-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span></span>

<span data-ttu-id="b3385-108">Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="b3385-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="b3385-109">Pokud máte nainstalované všechny požadované prostředky, zabere vám tento postup zhruba pět minut.</span><span class="sxs-lookup"><span data-stu-id="b3385-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3385-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b3385-110">Prerequisites</span></span>

<span data-ttu-id="b3385-111">K provedení kroků v tomto kurzu Rychlý start je potřeba:</span><span class="sxs-lookup"><span data-stu-id="b3385-111">To complete this quickstart:</span></span>

* <span data-ttu-id="b3385-112">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="b3385-112">[Install Git](https://git-scm.com/)</span></span>
* <span data-ttu-id="b3385-113">[Nainstalovat PHP](https://php.net).</span><span class="sxs-lookup"><span data-stu-id="b3385-113">[Install PHP](https://php.net)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample-locally"></a><span data-ttu-id="b3385-114">Místní stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="b3385-114">Download the sample locally</span></span>

<span data-ttu-id="b3385-115">V okně terminálu spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b3385-115">In a terminal window, run the following commands.</span></span> <span data-ttu-id="b3385-116">Tím se na váš místní počítač naklonuje ukázková aplikace a přejdete do adresáře se vzorovým kódem.</span><span class="sxs-lookup"><span data-stu-id="b3385-116">This will clone the sample application to your local machine, and navigate to the directory containing the sample code.</span></span>

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="b3385-117">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b3385-117">Run the app locally</span></span>

<span data-ttu-id="b3385-118">Aplikaci spustíte místně tak, že otevřete okno terminálu a pomocí příkazu `php` spustíte integrovaný webový server PHP.</span><span class="sxs-lookup"><span data-stu-id="b3385-118">Run the application locally by opening a terminal window and using the `php` command to launch the built-in PHP web server.</span></span>

```bash
php -S localhost:8080
```

<span data-ttu-id="b3385-119">Otevřete webový prohlížeč a přejděte na ukázkovou aplikaci na adrese http://localhost:8080.</span><span class="sxs-lookup"><span data-stu-id="b3385-119">Open a web browser, and navigate to the sample app at http://localhost:8080.</span></span>

<span data-ttu-id="b3385-120">Na stránce se zobrazí zpráva **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="b3385-120">You see the **Hello World!**</span></span> <span data-ttu-id="b3385-121">z ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3385-121">message from the sample app displayed in the page.</span></span>

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

<span data-ttu-id="b3385-123">V okně terminálu ukončete webový server stisknutím **Ctrl + C**.</span><span class="sxs-lookup"><span data-stu-id="b3385-123">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="b3385-125">Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b3385-125">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app-locally"></a><span data-ttu-id="b3385-126">Místní přechod do aplikace</span><span class="sxs-lookup"><span data-stu-id="b3385-126">Browse to the app locally</span></span>

<span data-ttu-id="b3385-127">V prohlížeči zadejte adresu nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3385-127">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="b3385-128">Vzorový kód PHP je spuštěný ve webové aplikaci služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b3385-128">The PHP sample code is running in an Azure App Service web app.</span></span>

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

<span data-ttu-id="b3385-130">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="b3385-130">**Congratulations!**</span></span> <span data-ttu-id="b3385-131">Nasadili jste svoji první aplikaci v PHP do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b3385-131">You've deployed your first PHP app to App Service.</span></span>

## <a name="update-locally-and-redeploy-the-code"></a><span data-ttu-id="b3385-132">Místní aktualizace a opětovné nasazení kódu</span><span class="sxs-lookup"><span data-stu-id="b3385-132">Update locally and redeploy the code</span></span>

<span data-ttu-id="b3385-133">Pomocí místního textového editoru otevřete soubor `index.php`, který je součástí PHP aplikace, a proveďte malou změnu textu v řetězci vedle `echo`:</span><span class="sxs-lookup"><span data-stu-id="b3385-133">Using a local text editor, open the `index.php` file within the PHP app, and make a small change to the text within the string next to `echo`:</span></span>

```php
echo "Hello Azure!";
```

<span data-ttu-id="b3385-134">Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="b3385-134">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="b3385-135">Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a aktualizujte zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="b3385-135">Once deployment has completed, switch back to the browser window that opened in the **Browse to the app** step, and refresh the page.</span></span>

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="b3385-137">Správa vaší nové webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="b3385-137">Manage your new Azure web app</span></span>

<span data-ttu-id="b3385-138">Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="b3385-138">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="b3385-139">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="b3385-139">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

<span data-ttu-id="b3385-141">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3385-141">You see your web app's Overview page.</span></span> <span data-ttu-id="b3385-142">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="b3385-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span>

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

<span data-ttu-id="b3385-144">Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3385-144">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="b3385-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3385-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3385-146">PHP s databází MySQL</span><span class="sxs-lookup"><span data-stu-id="b3385-146">PHP with MySQL</span></span>](app-service-web-tutorial-php-mysql.md)
