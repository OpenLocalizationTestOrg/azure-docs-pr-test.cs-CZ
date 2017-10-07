---
title: "aaaCreate webové aplikace Python v Azure | Microsoft Docs"
description: "Během několika minut můžete nasadit svou první aplikaci Hello World v Pythonu pomocí služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 42178d490d8aa8eaf93710667aad598794c62c8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-python-web-app-in-azure"></a>Vytvoření webové aplikace v Pythonu v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý start provede jak toodevelop a nasazení aplikace tooAzure Python webové aplikace. Vytvořit webovou aplikaci hello pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git toodeploy ukázkovou Python kód toohello webovou aplikaci.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux. Po instalaci nezbytných součástí hello, trvá asi 5 minut toocomplete hello kroky.
## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Python](https://www.python.org/downloads/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-hello-sample"></a>Stažení ukázky hello

Okno terminálu spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Se používá toto okno terminálu toorun všechny příkazy hello tento rychlý start.

Změnit toohello adresář, který obsahuje hello ukázkový kód.

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a>Místní spuštění aplikace hello

Instalovat balíčky hello vyžaduje použití `pip`.

```bash
pip install -r requirements.txt
```

Místní spuštění aplikace hello otevřete okno terminálu a použitím hello `Python` příkaz toolaunch hello integrovaného Python webového serveru.

```bash
python main.py
```

Otevřete webový prohlížeč a přejděte toohello ukázkovou aplikaci na http://localhost: 5000.

Můžete zobrazit hello **Hello, World** zprávu od hello ukázková aplikace zobrazí stránku hello.

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

V okně terminálu, stiskněte klávesu **Ctrl + C** tooexit hello webový server.

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-python/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

## <a name="configure-toouse-python"></a>Konfigurace toouse Python

Použití hello [az webapp konfigurace sady](/cli/azure/webapp/config#set) příkaz tooconfigure hello verze Python webové aplikace toouse `3.4`.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Nastavení verze Python hello tímto způsobem používá výchozí kontejner poskytované hello platformy. toouse vlastní kontejner, najdete v části hello referenční dokumentace rozhraní příkazového řádku pro hello [sadu kontejneru konfigurace webapp az](/cli/azure/webapp/config/container#set) příkaz.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up too4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a>Procházet toohello aplikace

Procházet toohello nasadit aplikaci pomocí webového prohlížeče.

```bash
http://<app_name>.azurewebsites.net
```

Hello Python ukázkový kód běží ve webové aplikaci Azure App Service.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Blahopřejeme!** Jste nasadili vaší první aplikace tooApp Python služby.

## <a name="update-and-redeploy-hello-code"></a>Aktualizace a znovu nasaďte hello kódu

Pomocí místní textovém editoru otevřete hello `main.py` souboru v aplikaci Python hello a proveďte další toohello malých změn toohello text `return` příkaz:

```python
return 'Hello, Azure!'
```

Potvrdit změny v úložišti Git a pak push tooAzure změny kódu hello.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení přepnout zpět toohello okno prohlížeče, který otevřít v hello [procházet toohello aplikace](#browse-to-the-app) kroku a aktualizovat stránku hello.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

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
> [Python sh PostgreSQL](app-service-web-tutorial-docker-python-postgresql-app.md)
