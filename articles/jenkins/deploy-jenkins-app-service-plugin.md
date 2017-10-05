---
title: "Nasazení do Azure App Service pomocí modulu plug-in volaných | Microsoft Docs"
description: "Další informace o použití modulu plug-in Azure App Service volaných nasazení webové aplikace v jazyce Java do Azure ve volaných"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 646daad1785f3de067544b6dd38abfcb6bc67d4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-app-service-with-jenkins-plugin"></a>Nasazení do Azure App Service pomocí modulu plug-in volaných 
Nasadit webovou aplikaci Java do Azure, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](/azure/jenkins/execute-cli-jenkins-pipeline) můžete taky použít [modul plug-in Azure App Service volaných](https://plugins.jenkins.io/azure-app-service). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace volaných k nasazení do Azure App Service pomocí služby FTP 
> * Konfigurace volaných k nasazení do Azure App Service v Linuxu prostřednictvím Docker 

## <a name="create-and-configure-jenkins-instance"></a>Vytvořit a nakonfigurovat volaných instance
Pokud již nemáte volaných master, začínat [šablona řešení](install-jenkins-solution-template.md), což zahrnuje JDK8 a následující požadované moduly plug-in:

* [Modul plug-in klienta volaných Git](https://plugins.jenkins.io/git-client) v.2.4.6 
* [Modul plug-in docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0
* [Přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) v.1.2
* [Aplikační služba Azure](https://plugins.jenkins.io/azure-app-server) v.0.1

Modul plug-in služby App Service můžete nasadit webovou aplikaci ve všech jazycích (například C#, PHP, Java a node.js atd.), které jsou podporované službou Azure App Service. V tomto kurzu používáme ukázkovou aplikaci Java, [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample). Chcete-li rozvětvit úložiště k účtu GitHub, klikněte **rozvětvení** tlačítko v horním pravém rohu.  

Java JDK a Maven jsou vyžadovány pro sestavení projektu Java. Ujistěte se, že pokud použijete jeden pro nepřetržitou integraci se v hlavním volaných nebo agenta virtuálního počítače nainstalovat součásti. 

Pokud chcete nainstalovat, přihlaste se k instanci volaných pomocí SSH a spusťte následující příkazy:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Pro nasazení do služby App Service v systému Linux, musíte taky nainstalovat Docker na hlavním serveru volaných nebo agenta virtuálního počítače použít pro sestavení. Najdete v tomto článku k instalaci Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Přidat objekt zabezpečení služby Azure k volaných pověření

Objektu zabezpečení služby Azure, je potřeba k nasazení do Azure. 

<ol>
<li>Použití [rozhraní příkazového řádku Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) nebo [portál Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal) k vytvoření objektu služby Azure</li>
<li>V rámci volaných řídicí panel, klikněte na **pověření -> Systém ->**. Klikněte na tlačítko **globální credentials(unrestricted)**.</li>
<li>Klikněte na tlačítko **přidat přihlašovací údaje** přidat hlavní název služby Microsoft Azure tak, že vyplníte ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0. Zadejte ID, **mySp**, pro použití v postupných krocích.</li>
</ol>

## <a name="azure-app-service-plugin"></a>Modul plug-in Azure App Service

V1.0 modul plug-in Azure App Service podporuje průběžné nasazování do webové aplikace Azure prostřednictvím:

* Git a FTP
* Docker pro webovou aplikaci v systému Linux

## <a name="configure-jenkins-to-deploy-web-app-through-ftp-using-the-jenkins-dashboard"></a>Konfigurace volaných nasadit webovou aplikaci pomocí služby FTP volaných řídicího panelu

Postup nasazení projektu do webové aplikace Azure, můžete nahrát artefaktů sestavení (například .war souboru v jazyce Java) pomocí Git a FTP.

Před nastavením pro úlohu ve volaných, potřebujete plán aplikační služby Azure a webovou aplikaci pro spuštění aplikace v jazyce Java.


1. Vytvořit plán aplikační služby Azure s **volné** cenová úroveň pomocí [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz. Plán aplikační služby definuje fyzické prostředky, které jsou použity k hostování vaší aplikace. Všechny aplikace, které jsou přiřazené plán služby App Service sdílení těchto prostředků, což umožňuje uložit nákladů při hostování více aplikací.
2. Vytvořte webovou aplikaci. Můžete použít [portál Azure](/azure/app-service-web/web-sites-configure) nebo použijte následující příkaz Az rozhraní příkazového řádku:
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. Ujistěte se, že nastavíte konfiguraci Java runtime, která vaše aplikace musí. Následující příkaz rozhraní příkazového řádku Azure nakonfiguruje webové aplikace ke spuštění na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-the-jenkins-job"></a>Nastavení úloh volaných


1. Vytvořte novou **volný styl** projekt v volaných řídicí panel
2. Konfigurace **správu zdrojového kódu** používat vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje **adresu URL úložiště**. Příklad: http://github.com/&lt;yourID > / javawebappsample.
3. Přidejte krok sestavení projekt pomocí nástroje Maven sestavíte. Učinit přidáním **spustit prostředí**. V tomto příkladu potřebujeme další krok přejmenujte soubor *.war v cílové složce na ROOT.war.   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.
5. Zadejte, "mySp" Azure instanční objekt uložená v předchozím kroku.
6. V **konfigurace aplikací** zvolte prostředků skupiny a webové aplikace v rámci vašeho předplatného. Tento modul plug-in automaticky zjistí, zda webová aplikace je Windows nebo linuxu. Pro webovou aplikaci systému Windows se zobrazí možnost "publikovat soubory".
7. Zadejte soubory, které chcete nasadit (například pokud používáte Java war balíčku.) Zdrojový a cílový adresář jsou volitelné. Parametry umožňují určit zdrojové a cílové složky při nahrávání souborů. Webové aplikace v jazyce Java v Azure běží na serveru Tomcat. Proto můžete nahrávat na server války se balíček umístěna složka webových aplikací. V tomto příkladu nastavit **zdrojový adresář** "cílit na" a "webapps" zadat **cílový adresář**.
8. Pokud chcete nasadit do přihrádky než produkční, můžete také nastavit **slotu** název.
9. Uložte projekt a sestavte jej. Webové aplikace se nasadí do Azure, po dokončení sestavení.

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a>Nasazení webové aplikace pomocí služby FTP volaných zřetězením příkazů

Tento modul plug-in je připravené pro kanál. Najdete ukázku v úložišti GitHub.

1. V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_ftp_plugin** souboru. Klikněte na ikonu tužky upravit tento soubor se aktualizují skupinu prostředků a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Změňte řádek 14 až ID přihlašovacích údajů v instanci volaných aktualizace.    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a>Vytvoření kanálu volaných

1. Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.
2. Zadejte název pro úlohy, vyberte **kanálu**. Klikněte na **OK**.
3. Klikněte **kanálu** kartě Další.
4. Pro **definice**, vyberte **kanálu skript z SCM**.
5. Pro **SCM**, vyberte **Git**. Zadejte adresu URL webu GitHub pro vaše forked úložišti: https:&lt;vaše forked úložišti > .git
6. Aktualizace **skript cesta** na "Jenkinsfile_ftp_plugin"
7. Klikněte na tlačítko **Uložit** a spusťte úlohu.

## <a name="configure-jenkins-to-deploy-web-app-on-linux-through-docker"></a>Konfigurace volaných nasazení webové aplikace v systému Linux pomocí Docker

Kromě Git/FTP webové aplikace v systému Linux podporuje nasazení pomocí Docker. Pokud chcete nasadit, pomocí Docker, potřebujete poskytovat soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie docker. Potom tento modul plug-in vytvoří bitovou kopii, doručí do registru docker a nasadí bitovou kopii do vaší webové aplikace.

Webové aplikace v systému Linux také podporuje tradiční způsobů jako Git a FTP, ale jenom pro předdefinované jazyků (.NET Core, Node.js, PHP a Ruby). Pro jiné jazyky potřebujete společně balíček runtime kód a služby vaší aplikace do bitové kopie docker a nasadit pomocí docker.

Před nastavením pro úlohu ve volaných, potřebujete Azure aplikační službu v systému Linux. Kontejner registru je také potřeba uchovávat a spravovat privátní kontejner imagí Dockeru. Můžete použít DockerHub; v tomto příkladu používáme registru kontejner Azure.

* Můžete provést kroky [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) k vytvoření webové aplikace v systému Linux 
* Azure registru kontejneru je spravovaný [Docker registru] (https://docs.docker.com/registry/) služby podle 2.0 registru Docker open source. Postupujte podle kroků [sem] (/ azure/container-registry/container-registry-get-started-azure-cli) Další informace o tom, jak to provést. Můžete také použít DockerHub.

### <a name="to-deploy-using-docker"></a>Nasazení pomocí docker:

1. Vytvoření nového projektu volný styl volaných řídicím panelu.
2. Konfigurace **správu zdrojového kódu** používat vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje **adresu URL úložiště**. Příklad: http://github.com/&lt;yourid > / javawebappsample.
Přidejte krok sestavení projekt pomocí nástroje Maven sestavíte. Učinit přidáním **spustit prostředí** a přidejte následující řádek v **příkaz**:    
```bash
mvn clean package
```

3. Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.
4. Zadejte, **mySp**, Azure instanční objekt uložená v předchozím kroku jako přihlašovací údaje Azure.
5. V **konfigurace aplikací** zvolte skupinu prostředků a Linux webové aplikace v rámci vašeho předplatného.
6. Zvolte publikování přes Docker.
7. Vyplňte **soubor Docker** cesta. Můžete ponechat výchozí "nebo soubor Docker" pro **Docker registru URL**, dodávky ve formátu https://&lt;myRegistry >. azurecr.io, pokud používáte Azure kontejneru registru. Necháte prázdné, pokud používáte DockerHub.
8. Pro **registru pověření**, přidat přihlašovací údaje pro registru kontejner Azure. ID uživatele a heslo můžete získat spuštěním následujících příkazů v rozhraní příkazového řádku Azure. První příkaz umožňuje účet správce.    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. Název bitové kopie docker a značky v **Upřesnit** kartě jsou volitelné. Ve výchozím nastavení je název bitové kopie získali z název bitové kopie, které jste nakonfigurovali v portálu Azure (v nastavení kontejner Docker.) Značky se generují z $BUILD_NUMBER. Ujistěte se, zadejte název bitové kopie na buď portálu Azure nebo zadejte hodnotu pro **Docker Image** v **Upřesnit** kartě. V tomto příkladu zadejte "&lt;yourRegistry >.azurecr.io/calculator" pro **Docker image** a nechte **značka obrázku Docker** prázdné.
10. Poznámka: nasazení selže, pokud použijete předdefinované nastavení Docker bitové kopie. Ujistěte se, že změna konfigurace docker používat vlastní image v kontejner Docker nastavení na portálu Azure. Předdefinované bitové kopie použijte k nasazení přístupu nahrávání souboru.
11. Podobně jako u Postup nahrání souboru, můžete vybrat jiný slot než produkční.
12. Uložte a sestavte projekt. Zobrazí bitové kopie kontejneru vložena do registru a webové aplikace je nasazená.

### <a name="deploy-to-web-app-on-linux-through-docker-using-jenkins-pipeline"></a>Nasazení do webové aplikace v systému Linux pomocí Docker volaných zřetězením příkazů

1. V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_container_plugin** souboru. Klikněte na ikonu tužky upravit tento soubor se aktualizují skupinu prostředků a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Změňte řádek 13 k registru serveru kontejneru    
```java
def registryServer = '<registryURL>'
```    

3. Změňte řádek 16 aktualizovat ID pověření v instanci volaných    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a>Vytvoření kanálu volaných    

1. Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.
2. Zadejte název pro úlohy, vyberte **kanálu**. Klikněte na **OK**.
3. Klikněte **kanálu** kartě Další.
4. Pro **definice**, vyberte **kanálu skript z SCM**.
5. Pro **SCM**, vyberte **Git**.
6. Zadejte adresu URL webu GitHub pro vaše forked úložišti: https:&lt;vaše forked úložišti > .git</li>
Aktualizace 7, **skript cesta** na "Jenkinsfile_container_plugin"
8. Klikněte na tlačítko **Uložit** a spusťte úlohu.

## <a name="verify-your-web-app"></a>Ověření webové aplikace

1. Chcete-li ověřit WAR souboru úspěšně nasazena do vaší webové aplikace. Otevřete webový prohlížeč.
2. Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping zobrazí:    
     Vítá vás webovou aplikaci Java! Tato hodnota aktualizuje!
   Sun června 17 16:39:10 UTC 2017
3. Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y        
    ![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a>Pro službu App service v systému Linux

* Pokud chcete ověřit, v Azure CLI, spusťte příkaz:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Získáte následující výsledek:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping. Zobrazí se zpráva: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y
    
## <a name="next-steps"></a>Další kroky

V tomto kurzu použijete modul plug-in Azure App Service k nasazení do Azure.

Jste se dozvěděli, jak na:

> [!div class="checklist"]
> * Konfigurace volaných k nasazení služby Azure App Service pomocí služby FTP 
> * Konfigurace volaných k nasazení do Azure App Service v Linuxu prostřednictvím Docker 