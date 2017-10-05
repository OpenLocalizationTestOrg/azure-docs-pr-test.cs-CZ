---
title: "Vytvoření aplikace pro poznámky Ruby pomocí webové aplikace v systému Linux | Microsoft Docs"
description: "Naučte se vytvářet aplikace pro poznámky Ruby s wpp službě Azure web v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů, ruby"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: wesmc;rachelap
ms.openlocfilehash: 17f3f1a2122c508501134a0c43ab6abce412fb44
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a><span data-ttu-id="65546-104">Vytvoření aplikace pro poznámky Ruby pomocí webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="65546-104">Create a Ruby App with Web Apps on Linux</span></span> 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="65546-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="65546-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="65546-106">Tento rychlý start se dozvíte, jak vytvořit základní Ruby na které aplikace můžete poté ji nasadit do Azure jako webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="65546-106">This quickstart shows you how to create a basic Ruby on Rails application you then deploy it to Azure as a Web App on Linux.</span></span>

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a><span data-ttu-id="65546-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65546-108">Prerequisites</span></span>

* <span data-ttu-id="65546-109">[Ruby 2.4.1 nebo vyšší](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span><span class="sxs-lookup"><span data-stu-id="65546-109">[Ruby 2.4.1 or higher](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span></span>
* <span data-ttu-id="65546-110">[Git](https://git-scm.com/downloads)</span><span class="sxs-lookup"><span data-stu-id="65546-110">[Git](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="65546-111">[Aktivní předplatné Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65546-111">An [active Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="65546-112">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="65546-112">Download the sample</span></span>

<span data-ttu-id="65546-113">V okně terminálu spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače:</span><span class="sxs-lookup"><span data-stu-id="65546-113">In a terminal window, run the following command to clone the sample app repository to your local machine:</span></span>

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-the-application-locally"></a><span data-ttu-id="65546-114">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="65546-114">Run the application locally</span></span>

<span data-ttu-id="65546-115">Spusťte server které v pořadí pro aplikace pro práci.</span><span class="sxs-lookup"><span data-stu-id="65546-115">Run the rails server in order for the application to work.</span></span> <span data-ttu-id="65546-116">Změnit na *hello, world* adresář a `rails server` příkaz spustí serveru.</span><span class="sxs-lookup"><span data-stu-id="65546-116">Change to the *hello-world* directory, and the `rails server` command starts the server.</span></span>

```bash
cd hello-world\bin
rails server
```
    
<span data-ttu-id="65546-117">Pomocí webového prohlížeče, přejděte do `http://localhost:3000` k testování aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="65546-117">Using your web browser, navigate to `http://localhost:3000` to test the app locally.</span></span>    

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a><span data-ttu-id="65546-119">Upravit aplikaci k zobrazení uvítací zprávy</span><span class="sxs-lookup"><span data-stu-id="65546-119">Modify app to display welcome message</span></span>

<span data-ttu-id="65546-120">Upravte aplikaci proto zobrazí uvítací zprávy.</span><span class="sxs-lookup"><span data-stu-id="65546-120">Modify the application so it displays a welcome message.</span></span> <span data-ttu-id="65546-121">Změňte řadič aplikace tak, aby vracel zprávy ve formátu HTML v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="65546-121">Change the application's controller so it returns the message as HTML to the browser.</span></span> 

<span data-ttu-id="65546-122">Otevřete *~/workspace/hello-world/app/controllers/application_controller.rb* pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="65546-122">Open *~/workspace/hello-world/app/controllers/application_controller.rb* for editing.</span></span> <span data-ttu-id="65546-123">Upravit `ApplicationController` třídy vypadat jako následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="65546-123">Modify the `ApplicationController` class to look like the following code sample:</span></span>

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

<span data-ttu-id="65546-124">Aplikace je nyní nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="65546-124">Your app is now configured.</span></span> <span data-ttu-id="65546-125">Pomocí webového prohlížeče, přejděte do `http://localhost:3000` potvrďte kořenové cílová stránka.</span><span class="sxs-lookup"><span data-stu-id="65546-125">Using your web browser, navigate to `http://localhost:3000` to confirm the root landing page.</span></span>

![Hello World nakonfigurované](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a><span data-ttu-id="65546-127">Vytvoření Ruby webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="65546-127">Create a Ruby web app on Azure</span></span>

<span data-ttu-id="65546-128">Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) příkazu vytvoříte plán služby app service pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65546-128">Use the [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) command to create an app service plan for your web app.</span></span> 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

<span data-ttu-id="65546-129">V dalším kroku vydávat [az webapp vytvořit](https://docs.microsoft.com/cli/azure/webapp) příkazu vytvořte webovou aplikaci, která používá nově vytvořený tarifu.</span><span class="sxs-lookup"><span data-stu-id="65546-129">Next, issue the [az webapp create](https://docs.microsoft.com/cli/azure/webapp) command to create the web app that uses the newly created service plan.</span></span> <span data-ttu-id="65546-130">Všimněte si, že modul runtime je nastaven na `ruby|2.3`.</span><span class="sxs-lookup"><span data-stu-id="65546-130">Notice that the runtime is set to `ruby|2.3`.</span></span> <span data-ttu-id="65546-131">Nezapomeňte nahradit `<app name>` s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="65546-131">Don't forget to replace `<app name>` with a unique app name.</span></span>

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

<span data-ttu-id="65546-132">Po vytvoření webové aplikace **přehled** stránka je k dispozici k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="65546-132">Once the web app is created, an **Overview** page is available to view.</span></span> <span data-ttu-id="65546-133">Přejděte do ní.</span><span class="sxs-lookup"><span data-stu-id="65546-133">Navigate to it.</span></span> <span data-ttu-id="65546-134">Zobrazí se následující úvodní stránka:</span><span class="sxs-lookup"><span data-stu-id="65546-134">The following splash page is displayed:</span></span>

![Úvodní stránka](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a><span data-ttu-id="65546-136">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="65546-136">Deploy your application</span></span>

<span data-ttu-id="65546-137">Pomocí Git nasaďte Ruby aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="65546-137">Use Git to deploy the Ruby application to Azure.</span></span> <span data-ttu-id="65546-138">Webové aplikace je již nakonfigurován nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="65546-138">The web app already has a Git deployment configured.</span></span> <span data-ttu-id="65546-139">Adresa URL nasazení můžete načíst vydáním [az webapp nasazení](https://docs.microsoft.com/cli/azure/webapp/deployment) příkaz.</span><span class="sxs-lookup"><span data-stu-id="65546-139">You can retrieve the deployment URL by issuing an [az webapp deployment](https://docs.microsoft.com/cli/azure/webapp/deployment) command.</span></span>  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="65546-140">Všimněte si, že adresy URL pro Git má následující formulář založený na název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="65546-140">Notice that the Git URL has the following form based on your web app name:</span></span>

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

<span data-ttu-id="65546-141">Spusťte následující příkazy k nasazení místní aplikace do webové stránky Azure:</span><span class="sxs-lookup"><span data-stu-id="65546-141">Run the following commands to deploy the local application to your Azure website:</span></span>

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="65546-142">Potvrďte, že operace vzdálené nasazení sestav úspěch.</span><span class="sxs-lookup"><span data-stu-id="65546-142">Confirm that the remote deployment operations report success.</span></span> <span data-ttu-id="65546-143">Příkazy vytvořit výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="65546-143">The commands produce output similar to the following text:</span></span>

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

<span data-ttu-id="65546-144">Po dokončení nasazení restartování webové aplikace pro nasazení se projeví pomocí [az webapp restartování](https://docs.microsoft.com/cli/azure/webapp#restart) příkaz, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="65546-144">Once the deployment has completed, restart your web app for the deployment to take effect by using the [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#restart) command, as shown here:</span></span>

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="65546-145">Přejít na váš web a ověřte výsledky.</span><span class="sxs-lookup"><span data-stu-id="65546-145">Navigate to your site and verify the results.</span></span>

```bash
http://<your web app name>.azurewebsites.net
```
![aktualizované webové aplikace](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> <span data-ttu-id="65546-147">Během restartování aplikace pokusu o vyhledání lokality výsledky v stavový kód HTTP `Error 503 Server unavailable`.</span><span class="sxs-lookup"><span data-stu-id="65546-147">While the app is restarting, attempting to browse the site results in an HTTP status code `Error 503 Server unavailable`.</span></span> <span data-ttu-id="65546-148">Ho může trvat několik minut plně restartovat.</span><span class="sxs-lookup"><span data-stu-id="65546-148">It may take a few minutes to fully restart.</span></span>
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a><span data-ttu-id="65546-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65546-149">Next steps</span></span>

[<span data-ttu-id="65546-150">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="65546-150">Azure App Service Web App on Linux FAQ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)