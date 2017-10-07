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
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a>Vytvoření aplikace pro poznámky Ruby pomocí webové aplikace v systému Linux 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento rychlý start se dozvíte, jak toocreate základní Ruby, na které aplikace je pak nasadit tooAzure jako webovou aplikaci v systému Linux.

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a>Požadavky

* [Ruby 2.4.1 nebo vyšší](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads)
* [Aktivní předplatné Azure](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Stažení ukázky hello

Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello

Spusťte server hello které aby toowork aplikace hello. Změnit toohello *hello world* adresář a hello `rails server` příkaz spustí hello serveru.

```bash
cd hello-world\bin
rails server
```
    
Pomocí webového prohlížeče, přejděte příliš`http://localhost:3000` tootest hello aplikace místně.  

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a>Upravit aplikaci toodisplay uvítací zprávy

Úprava aplikace hello proto zobrazí uvítací zprávy. Změna řadiče hello aplikace tak, aby vracel uvítací zprávu jako prohlížeč toohello HTML. 

Otevřete *~/workspace/hello-world/app/controllers/application_controller.rb* pro úpravy. Upravit hello `ApplicationController` toolook třída jako hello následující ukázka kódu:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Aplikace je nyní nakonfigurována. Pomocí webového prohlížeče, přejděte příliš`http://localhost:3000` tooconfirm hello kořenové cílová stránka.

![Hello World nakonfigurované](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Vytvoření Ruby webové aplikace v Azure

Použití hello [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) příkaz toocreate plán služby app service pro webové aplikace. 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

V dalším kroku vydání hello [az webapp vytvořit](https://docs.microsoft.com/cli/azure/webapp) příkaz toocreate hello webovou aplikaci, která používá hello nově vytvořený plán služby. Všimněte si, že runtime hello je nastaven příliš`ruby|2.3`. Nezapomeňte tooreplace `<app name>` s jedinečným názvem aplikace.

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

Po vytvoření webové aplikace hello **přehled** stránka je k dispozici tooview. Přejděte tooit. Zobrazí se následující úvodní stránku Hello:

![Úvodní stránka](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Nasazení aplikace

Použijte tooAzure Ruby aplikace hello toodeploy Git. Hello webové aplikace je již nakonfigurován nasazení Git. Můžete načíst URL nasazení hello vydáním [az webapp nasazení](https://docs.microsoft.com/cli/azure/webapp/deployment) příkaz.  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

Všimněte si, že hello adresy URL pro Git má hello následující formulář založený na název vaší webové aplikace:

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

Spusťte následující příkazy toodeploy hello místních aplikací tooyour webu Azure hello:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Potvrďte, že operace vzdálené nasazení hello sestavy úspěch. Hello příkazy produktu výstup podobný toohello následující text:

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

Po dokončení nasazení hello restartování webové aplikace pro hello nasazení tootake vliv pomocí hello [az webapp restartování](https://docs.microsoft.com/cli/azure/webapp#restart) příkaz, jak je vidět tady:

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

Přejděte tooyour lokality a ověřte hello výsledky.

```bash
http://<your web app name>.azurewebsites.net
```
![aktualizované webové aplikace](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> Během restartování aplikace hello pokus toobrowse hello lokality výsledky v stavový kód HTTP `Error 503 Server unavailable`. To může trvat několik minut toofully restartování.
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Další kroky

[Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)