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
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a>Vytvoření aplikace pro poznámky Ruby pomocí webové aplikace v systému Linux 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento rychlý start se dozvíte, jak vytvořit základní Ruby na které aplikace můžete poté ji nasadit do Azure jako webové aplikace v systému Linux.

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a>Požadavky

* [Ruby 2.4.1 nebo vyšší](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads)
* [Aktivní předplatné Azure](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-the-application-locally"></a>Místní spuštění aplikace

Spusťte server které v pořadí pro aplikace pro práci. Změnit na *hello, world* adresář a `rails server` příkaz spustí serveru.

```bash
cd hello-world\bin
rails server
```
    
Pomocí webového prohlížeče, přejděte do `http://localhost:3000` k testování aplikace místně.    

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Upravit aplikaci k zobrazení uvítací zprávy

Upravte aplikaci proto zobrazí uvítací zprávy. Změňte řadič aplikace tak, aby vracel zprávy ve formátu HTML v prohlížeči. 

Otevřete *~/workspace/hello-world/app/controllers/application_controller.rb* pro úpravy. Upravit `ApplicationController` třídy vypadat jako následující ukázka kódu:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Aplikace je nyní nakonfigurována. Pomocí webového prohlížeče, přejděte do `http://localhost:3000` potvrďte kořenové cílová stránka.

![Hello World nakonfigurované](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Vytvoření Ruby webové aplikace v Azure

Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) příkazu vytvoříte plán služby app service pro webové aplikace. 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

V dalším kroku vydávat [az webapp vytvořit](https://docs.microsoft.com/cli/azure/webapp) příkazu vytvořte webovou aplikaci, která používá nově vytvořený tarifu. Všimněte si, že modul runtime je nastaven na `ruby|2.3`. Nezapomeňte nahradit `<app name>` s jedinečným názvem aplikace.

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

Po vytvoření webové aplikace **přehled** stránka je k dispozici k zobrazení. Přejděte do ní. Zobrazí se následující úvodní stránka:

![Úvodní stránka](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Nasazení aplikace

Pomocí Git nasaďte Ruby aplikaci do Azure. Webové aplikace je již nakonfigurován nasazení Git. Adresa URL nasazení můžete načíst vydáním [az webapp nasazení](https://docs.microsoft.com/cli/azure/webapp/deployment) příkaz.  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

Všimněte si, že adresy URL pro Git má následující formulář založený na název vaší webové aplikace:

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

Spusťte následující příkazy k nasazení místní aplikace do webové stránky Azure:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Potvrďte, že operace vzdálené nasazení sestav úspěch. Příkazy vytvořit výstup podobný následujícímu:

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

Po dokončení nasazení restartování webové aplikace pro nasazení se projeví pomocí [az webapp restartování](https://docs.microsoft.com/cli/azure/webapp#restart) příkaz, jak je vidět tady:

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

Přejít na váš web a ověřte výsledky.

```bash
http://<your web app name>.azurewebsites.net
```
![aktualizované webové aplikace](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> Během restartování aplikace pokusu o vyhledání lokality výsledky v stavový kód HTTP `Error 503 Server unavailable`. Ho může trvat několik minut plně restartovat.
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Další kroky

[Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)