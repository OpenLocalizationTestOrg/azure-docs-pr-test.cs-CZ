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
# <a name="create-a-static-html-web-app-in-azure"></a>Vytvoření webové aplikace ve statickém HTML ve službě Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý start ukazuje, jak toodeploy základní HTML + CSS lokality tooAzure webové aplikace. Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou HTML obsahu toohello webovou aplikaci.

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux. Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý start:

- [Nainstalovat Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Stažení ukázky hello

Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.

## <a name="view-hello-html"></a>Hello zobrazení HTML

Přejděte toohello adresář, který obsahuje ukázkové hello HTML. Otevřete hello *index.html* souboru v prohlížeči.

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-html/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

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

## <a name="browse-toohello-app"></a>Procházet toohello aplikace

Přejděte v prohlížeči, adresa URL toohello Azure webové aplikace:

```
http://<app_name>.azurewebsites.net
```

stránku Hello běží jako webová aplikace Azure App Service.

![Domovská stránka ukázkové aplikace](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Blahopřejeme!** Jste nasadili vaší první aplikace tooApp HTML služby.

## <a name="update-and-redeploy-hello-app"></a>Aktualizace a znovu nasaďte aplikace hello

Otevřete hello *index.html* soubor v textovém editoru a proveďte změnu toohello značky. Můžete například změnit záhlaví hello H1 z "Azure App Service – ukázka statické HTML Server" toojust "Azure App Service".

Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.

```bash
git commit -am "updated HTML"
git push azure master
```

Po dokončení nasazení aktualizujte změny hello toosee prohlížeče.

![Domovská stránka aktualizované ukázkové aplikace](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-html/portal1.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-html/portal2.png)

levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Mapování vlastní domény](app-service-web-tutorial-custom-domain.md)
