---
title: "aaaCreate Ruby aplikace s webovými aplikacemi v systému Linux | Microsoft Docs"
description: "Další informace toocreate Ruby aplikace s wpp službě Azure web v systému Linux."
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
ms.openlocfilehash: 99ce3b5ee16703a147787387bb02973defce8190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a><span data-ttu-id="51eb4-104">Vytvoření aplikace pro poznámky Ruby pomocí webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="51eb4-104">Create a Ruby App with Web Apps on Linux</span></span> 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="51eb4-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="51eb4-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="51eb4-106">Tento rychlý start se dozvíte, jak toocreate základní Ruby, na které aplikace je pak nasadit tooAzure jako webovou aplikaci v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="51eb4-106">This quickstart shows you how toocreate a basic Ruby on Rails application you then deploy it tooAzure as a Web App on Linux.</span></span>

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a><span data-ttu-id="51eb4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51eb4-108">Prerequisites</span></span>

* <span data-ttu-id="51eb4-109">[Ruby 2.4.1 nebo vyšší](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span><span class="sxs-lookup"><span data-stu-id="51eb4-109">[Ruby 2.4.1 or higher](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span></span>
* <span data-ttu-id="51eb4-110">[Git](https://git-scm.com/downloads)</span><span class="sxs-lookup"><span data-stu-id="51eb4-110">[Git](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="51eb4-111">[Aktivní předplatné Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51eb4-111">An [active Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="51eb4-112">Stažení ukázky hello</span><span class="sxs-lookup"><span data-stu-id="51eb4-112">Download hello sample</span></span>

<span data-ttu-id="51eb4-113">Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="51eb4-113">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a><span data-ttu-id="51eb4-114">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="51eb4-114">Run hello application locally</span></span>

<span data-ttu-id="51eb4-115">Spusťte server hello které aby toowork aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="51eb4-115">Run hello rails server in order for hello application toowork.</span></span> <span data-ttu-id="51eb4-116">Změnit toohello *hello world* adresář a hello `rails server` příkaz spustí hello serveru.</span><span class="sxs-lookup"><span data-stu-id="51eb4-116">Change toohello *hello-world* directory, and hello `rails server` command starts hello server.</span></span>

```bash
cd hello-world\bin
rails server
```
    
<span data-ttu-id="51eb4-117">Pomocí webového prohlížeče, přejděte příliš`http://localhost:3000` tootest hello aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="51eb4-117">Using your web browser, navigate too`http://localhost:3000` tootest hello app locally.</span></span>  

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a><span data-ttu-id="51eb4-119">Upravit aplikaci toodisplay uvítací zprávy</span><span class="sxs-lookup"><span data-stu-id="51eb4-119">Modify app toodisplay welcome message</span></span>

<span data-ttu-id="51eb4-120">Úprava aplikace hello proto zobrazí uvítací zprávy.</span><span class="sxs-lookup"><span data-stu-id="51eb4-120">Modify hello application so it displays a welcome message.</span></span> <span data-ttu-id="51eb4-121">Změna řadiče hello aplikace tak, aby vracel uvítací zprávu jako prohlížeč toohello HTML.</span><span class="sxs-lookup"><span data-stu-id="51eb4-121">Change hello application's controller so it returns hello message as HTML toohello browser.</span></span> 

<span data-ttu-id="51eb4-122">Otevřete *~/workspace/hello-world/app/controllers/application_controller.rb* pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="51eb4-122">Open *~/workspace/hello-world/app/controllers/application_controller.rb* for editing.</span></span> <span data-ttu-id="51eb4-123">Upravit hello `ApplicationController` toolook třída jako hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="51eb4-123">Modify hello `ApplicationController` class toolook like hello following code sample:</span></span>

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

<span data-ttu-id="51eb4-124">Aplikace je nyní nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="51eb4-124">Your app is now configured.</span></span> <span data-ttu-id="51eb4-125">Pomocí webového prohlížeče, přejděte příliš`http://localhost:3000` tooconfirm hello kořenové cílová stránka.</span><span class="sxs-lookup"><span data-stu-id="51eb4-125">Using your web browser, navigate too`http://localhost:3000` tooconfirm hello root landing page.</span></span>

![Hello World nakonfigurované](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a><span data-ttu-id="51eb4-127">Vytvoření Ruby webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="51eb4-127">Create a Ruby web app on Azure</span></span>

<span data-ttu-id="51eb4-128">Použití hello [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) příkaz toocreate plán služby app service pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="51eb4-128">Use hello [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) command toocreate an app service plan for your web app.</span></span> 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

<span data-ttu-id="51eb4-129">V dalším kroku vydání hello [az webapp vytvořit](https://docs.microsoft.com/cli/azure/webapp) příkaz toocreate hello webovou aplikaci, která používá hello nově vytvořený plán služby.</span><span class="sxs-lookup"><span data-stu-id="51eb4-129">Next, issue hello [az webapp create](https://docs.microsoft.com/cli/azure/webapp) command toocreate hello web app that uses hello newly created service plan.</span></span> <span data-ttu-id="51eb4-130">Všimněte si, že runtime hello je nastaven příliš`ruby|2.3`.</span><span class="sxs-lookup"><span data-stu-id="51eb4-130">Notice that hello runtime is set too`ruby|2.3`.</span></span> <span data-ttu-id="51eb4-131">Nezapomeňte tooreplace `<app name>` s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="51eb4-131">Don't forget tooreplace `<app name>` with a unique app name.</span></span>

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

<span data-ttu-id="51eb4-132">Po vytvoření webové aplikace hello **přehled** stránka je k dispozici tooview.</span><span class="sxs-lookup"><span data-stu-id="51eb4-132">Once hello web app is created, an **Overview** page is available tooview.</span></span> <span data-ttu-id="51eb4-133">Přejděte tooit.</span><span class="sxs-lookup"><span data-stu-id="51eb4-133">Navigate tooit.</span></span> <span data-ttu-id="51eb4-134">Zobrazí se následující úvodní stránku Hello:</span><span class="sxs-lookup"><span data-stu-id="51eb4-134">hello following splash page is displayed:</span></span>

![Úvodní stránka](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a><span data-ttu-id="51eb4-136">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="51eb4-136">Deploy your application</span></span>

<span data-ttu-id="51eb4-137">Použijte tooAzure Ruby aplikace hello toodeploy Git.</span><span class="sxs-lookup"><span data-stu-id="51eb4-137">Use Git toodeploy hello Ruby application tooAzure.</span></span> <span data-ttu-id="51eb4-138">Hello webové aplikace je již nakonfigurován nasazení Git.</span><span class="sxs-lookup"><span data-stu-id="51eb4-138">hello web app already has a Git deployment configured.</span></span> <span data-ttu-id="51eb4-139">Můžete načíst URL nasazení hello vydáním [az webapp nasazení](https://docs.microsoft.com/cli/azure/webapp/deployment) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51eb4-139">You can retrieve hello deployment URL by issuing an [az webapp deployment](https://docs.microsoft.com/cli/azure/webapp/deployment) command.</span></span>  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="51eb4-140">Všimněte si, že hello adresy URL pro Git má hello následující formulář založený na název vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="51eb4-140">Notice that hello Git URL has hello following form based on your web app name:</span></span>

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

<span data-ttu-id="51eb4-141">Spusťte následující příkazy toodeploy hello místních aplikací tooyour webu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="51eb4-141">Run hello following commands toodeploy hello local application tooyour Azure website:</span></span>

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="51eb4-142">Potvrďte, že operace vzdálené nasazení hello sestavy úspěch.</span><span class="sxs-lookup"><span data-stu-id="51eb4-142">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="51eb4-143">Hello příkazy produktu výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="51eb4-143">hello commands produce output similar toohello following text:</span></span>

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

<span data-ttu-id="51eb4-144">Po dokončení nasazení hello restartování webové aplikace pro hello nasazení tootake vliv pomocí hello [az webapp restartování](https://docs.microsoft.com/cli/azure/webapp#restart) příkaz, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="51eb4-144">Once hello deployment has completed, restart your web app for hello deployment tootake effect by using hello [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#restart) command, as shown here:</span></span>

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="51eb4-145">Přejděte tooyour lokality a ověřte hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="51eb4-145">Navigate tooyour site and verify hello results.</span></span>

```bash
http://<your web app name>.azurewebsites.net
```
![aktualizované webové aplikace](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> <span data-ttu-id="51eb4-147">Během restartování aplikace hello pokus toobrowse hello lokality výsledky v stavový kód HTTP `Error 503 Server unavailable`.</span><span class="sxs-lookup"><span data-stu-id="51eb4-147">While hello app is restarting, attempting toobrowse hello site results in an HTTP status code `Error 503 Server unavailable`.</span></span> <span data-ttu-id="51eb4-148">To může trvat několik minut toofully restartování.</span><span class="sxs-lookup"><span data-stu-id="51eb4-148">It may take a few minutes toofully restart.</span></span>
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a><span data-ttu-id="51eb4-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51eb4-149">Next steps</span></span>

[<span data-ttu-id="51eb4-150">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="51eb4-150">Azure App Service Web App on Linux FAQ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)