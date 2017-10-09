---
title: "aaaExecute hello rozhraní příkazového řádku Azure s volaných | Microsoft Docs"
description: "Zjistěte, jak toouse rozhraní příkazového řádku Azure toodeploy Java webové aplikace tooAzure v volaných kanálu"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a>Nasazení tooAzure App Service pomocí volaných a hello rozhraní příkazového řádku Azure
toodeploy tooAzure webové aplikace Java, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](https://jenkins.io/doc/book/pipeline/). V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Konfigurace volaných
> * Vytvoření webové aplikace v Azure
> * Příprava úložiště GitHub
> * Vytvoření kanálu volaných
> * Spusťte hello kanálu a ověřte hello webové aplikace

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. verze hello toofind, spusťte `az --version`. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Vytvořit a nakonfigurovat volaných instance
Pokud již nemáte volaných master, začínat hello [šablona řešení](install-jenkins-solution-template.md), což zahrnuje hello požadované [přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) modulu plug-in ve výchozím nastavení. 

modul plug-in Hello přihlašovacích údajů Azure umožňuje hlavní pověření služby Microsoft Azure toostore ve volaných. Ve verzi 1.2 jsme doplnili podporu hello tak, že volaných kanálu můžete získat hello přihlašovacích údajů služby Azure. 

Ujistěte se, že máte verze 1.2 nebo vyšší:
* V rámci hello volaných řídicí panel, klikněte na **volaných spravovat -> modul plug-in Manager ->** a vyhledejte **přihlašovacích údajů Azure**. 
* Aktualizace modulu plug-in hello, pokud je starší než 1.2 hello verze.

V hlavním volaných hello také vyžadují Java JDK a Maven. tooinstall, přihlaste se pomocí protokolu SSH hlavní tooJenkins a spusťte následující příkazy hello:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a>Přidat přihlašovací údaje služby Azure hlavní tooJenkins

Azure přihlašovacích údajů je potřebné tooexecute rozhraní příkazového řádku Azure.

* V rámci hello volaných řídicí panel, klikněte na **pověření -> Systém ->**. Klikněte na tlačítko **globální credentials(unrestricted)**.
* Klikněte na tlačítko **přidat přihlašovací údaje** tooadd [objektu služby Microsoft Azure](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) tak, že vyplníte hello ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0. Zadejte ID pro použití v následném kroku.

![Přidejte pověření](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a>Vytvoření Azure App Service pro nasazení webové aplikace v jazyce Java hello

Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz. plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace. Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Při plánování hello je připraven, výstup hello rozhraní příkazového řádku Azure zobrazuje podobné toohello následující ukázka:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure

 Použití hello [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz toocreate definici webové aplikace v hello `myAppServicePlan` plán služby App Service. definice Hello webové aplikace poskytuje tooaccess adresa URL vaší aplikace pomocí a konfiguruje několik možností toodeploy tooAzure vašeho kódu. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

SUBSTITUTE hello `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace. Tento jedinečný název je součástí hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure. Můžete namapovat vlastní doménu název položky toohello webové aplikace ještě před zveřejněním tooyour uživatele.

Při definici hello webové aplikace je připraven, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurace Java 

Nastavení konfigurace hello Java runtime, která vaše aplikace, musí se hello [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.

Hello následující příkaz nakonfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Příprava úložiště GitHub
Otevřete hello [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) úložišti. toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.

* V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile** souboru. Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 20 a 21 v uvedeném pořadí.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Změňte ID pověření 23 tooupdate řádek v instanci volaných

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Vytvoření kanálu volaných
Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**. 

* Zadejte název úlohy hello a vyberte **kanálu**. Klikněte na **OK**.
* Klikněte na tlačítko hello **kanálu** kartě Další. 
* Pro **definice**, vyberte **kanálu skript z SCM**.
* Pro **SCM**, vyberte **Git**.
* Zadejte hello Githubu adresu URL pro váš forked úložišti: https:\<vaše forked úložišti\>.git
* Klikněte na tlačítko **uložit**

## <a name="test-your-pipeline"></a>Testování vaší kanálu
* Přejděte toohello kanálu, které jste vytvořili, klikněte na tlačítko **nyní sestavení**
* Sestavení uspěli za několik sekund, a můžete se vrátit toohello sestavení a klikněte na tlačítko **výstup konzoly** toosee hello podrobnosti

## <a name="verify-your-web-app"></a>Ověření webové aplikace
soubor WAR hello tooverify úspěšně nasadil tooyour webové aplikace. Otevřete webový prohlížeč:

* Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping  
Zobrazí:

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y

![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a>Nasazení tooAzure webové aplikace v systému Linux
Teď, když víte, jak toouse rozhraní příkazového řádku Azure ve vašem volaných kanálu, můžete upravit hello skriptu toodeploy tooan webové aplikace Azure v systému Linux.

Webové aplikace v systému Linux podporuje nasazení hello toodo jiný způsob, který je toouse Docker. toodeploy, musíte tooprovide soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie Docker. modul plug-in Hello pak sestavení hello bitové kopie, poslat ho tooa Docker registru a nasazení hello image tooyour webové aplikace.

* Postupujte podle kroků hello [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate na Azure webová aplikace spuštěná v systému Linux.
* Nainstalujte Docker ve vaší instanci volaných podle hello pokyny v tomto [článku](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Vytvoření kontejneru registru v hello portálu Azure pomocí kroků hello [zde](/azure/container-registry/container-registry-get-started-azure-cli).
* V hello stejné [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) úložišti vám forked, upravit hello **Jenkinsfile2** souboru:
    * Řádek 18 21, aktualizovat názvy toohello skupinu prostředků, webové aplikace a ACR v uvedeném pořadí. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Řádek 24, aktualizace \<azsrvprincipal\> ID tooyour pověření
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Vytvořit nový kanál volaných stejně jako při nasazení tooAzure webové aplikace v systému Windows, pouze tentokrát použijte **Jenkinsfile2** místo.
* Spustíte novou úlohu.
* tooverify v Azure CLI, spusťte:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Můžete získat hello následující výsledek:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping. Zobrazí zpráva hello: 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y
    
## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali volaných kanál, který rezervuje hello zdrojový kód v úložišti GitHub. Spustí soubor war Maven toobuild a pak používá tooAzure toodeploy rozhraní příkazového řádku Azure App Service. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače volaných
> * Konfigurace volaných
> * Vytvoření webové aplikace v Azure
> * Příprava úložiště GitHub
> * Vytvoření kanálu volaných
> * Spusťte hello kanálu a ověřte hello webové aplikace
