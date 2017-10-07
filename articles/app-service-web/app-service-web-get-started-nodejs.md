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
# <a name="create-a-nodejs-web-app-in-azure"></a>Vytvoření webové aplikace Node.js ve službě Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý start ukazuje, jak toodeploy tooAzure aplikace Node.js webové aplikace. Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou Node.js kód toohello webovou aplikaci.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux. Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý start:

* [Nainstalovat Git](https://git-scm.com/).
* [Nainstalovat Node.js a NPM](https://nodejs.org/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Stažení ukázky hello

Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.

Změnit toohello adresář, který obsahuje hello ukázkový kód.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Místní spuštění aplikace hello

Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `npm start` skriptu toolaunch hello součástí server Node.js HTTP.

```bash
npm start
```

Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na adresu http://localhost: 1337.

Zobrazí hello **Hello, World** zprávu od hello ukázková aplikace zobrazí stránku hello.

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

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

## <a name="browse-toohello-app"></a>Procházet toohello aplikace

Procházet toohello nasadit aplikaci pomocí webového prohlížeče.

```bash
http://<app_name>.azurewebsites.net
```

Ukázkový kód Node.js Hello běží ve webové aplikaci Azure App Service.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

**Blahopřejeme!** Vaše první tooApp aplikace Node.js služby jste nasadili.

## <a name="update-and-redeploy-hello-code"></a>Aktualizace a znovu nasaďte hello kódu

Pomocí textového editoru otevřete hello `index.js` souboru v aplikaci Node.js hello a proveďte textový toohello malé změny ve volání hello příliš`response.end`:

```nodejs
response.end("Hello Azure!");
```

Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello **procházet toohello aplikace** kroku a stiskněte tlačítko Aktualizovat.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Node.js s databází MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)
