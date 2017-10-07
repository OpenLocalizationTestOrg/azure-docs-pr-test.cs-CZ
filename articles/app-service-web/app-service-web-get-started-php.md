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
# <a name="create-a-php-web-app-in-azure"></a>Vytvoření webové aplikace v PHP v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý úvodní kurz ukazuje, jak toodeploy tooAzure aplikace PHP webové aplikace. Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) v prostředí cloudu a můžete pomocí Git toodeploy ukázkovou PHP kód toohello webovou aplikaci.

![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)

Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux. Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý start:

* [Nainstalovat Git](https://git-scm.com/).
* [Nainstalovat PHP](https://php.net).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a>Stažení ukázky hello místně

Okno terminálu spusťte následující příkazy hello. Tato akce klonovat hello ukázkové aplikace tooyour místní počítač a přejděte toohello adresář obsahující hello ukázkový kód.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Místní spuštění aplikace hello

Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `php` příkaz toolaunch hello integrovaného PHP webového serveru.

```bash
php -S localhost:8080
```

Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na adrese http://localhost: 8080.

Zobrazí hello **Hello, World!** zpráva z hello ukázková aplikace zobrazí stránku hello.

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

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

## <a name="browse-toohello-app-locally"></a>Procházet toohello aplikace místně

Procházet toohello nasadit aplikaci pomocí webového prohlížeče.

```bash
http://<app_name>.azurewebsites.net
```

Hello PHP ukázkový kód běží ve webové aplikaci Azure App Service.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

**Blahopřejeme!** Jste nasadili vaší první aplikace tooApp PHP služby.

## <a name="update-locally-and-redeploy-hello-code"></a>Aktualizovat místně a znovu nasaďte hello kódu

Pomocí místní textovém editoru otevřete hello `index.php` souboru v rámci aplikace PHP hello a proveďte textový toohello malých změn v rámci hello řetězec vedle příliš`echo`:

```php
echo "Hello Azure!";
```

Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello **procházet toohello aplikace** kroku a aktualizovat stránku hello.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [PHP s databází MySQL](app-service-web-tutorial-php-mysql.md)
