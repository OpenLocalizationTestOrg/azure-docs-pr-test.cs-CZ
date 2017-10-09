---
title: "aaaDeploy tooAzure App Service pomocí modulu plug-in volaných | Microsoft Docs"
description: "Zjistěte, jak toodeploy modul plug-in Azure App Service volaných toouse Java webové aplikace tooAzure ve volaných"
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
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a>Nasazení pomocí modulu plug-in volaných tooAzure služby App Service 
toodeploy tooAzure webové aplikace Java, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](/azure/jenkins/execute-cli-jenkins-pipeline) můžete taky použít hello [modul plug-in Azure App Service volaných](https://plugins.jenkins.io/azure-app-service). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace volaných toodeploy tooAzure služby App Service pomocí služby FTP 
> * Konfigurace volaných toodeploy tooAzure služby App Service v Linuxu prostřednictvím Docker 

## <a name="create-and-configure-jenkins-instance"></a>Vytvořit a nakonfigurovat volaných instance
Pokud již nemáte volaných master, začínat hello [šablona řešení](install-jenkins-solution-template.md), což zahrnuje JDK8 a hello následující požadované moduly plug-in:

* [Modul plug-in klienta volaných Git](https://plugins.jenkins.io/git-client) v.2.4.6 
* [Modul plug-in docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0
* [Přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) v.1.2
* [Aplikační služba Azure](https://plugins.jenkins.io/azure-app-server) v.0.1

Můžete použít hello toodeploy modul plug-in služby App Service webovou aplikaci ve všech jazycích (například C#, PHP, Java a node.js atd.), které jsou podporované službou Azure App Service. V tomto kurzu používáme hello ukázkovou aplikaci Java, [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample). toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.  

Jsou vyžadovány pro sestavení projektu Java hello Java JDK a Maven. Ujistěte se, že pokud použijete jeden pro nepřetržitou integraci instalujete hello součásti v hlavní volaných hello nebo hello agenta virtuálního počítače. 

tooinstall, přihlaste instance volaných toohello pomocí protokolu SSH a spusťte následující příkazy hello:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Pro nasazení tooApp služby v systému Linux, musíte taky tooinstall Docker v předloze volaných hello nebo hello agenta virtuálního počítače použít pro sestavení. Najdete v článku tooinstall toothis Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.

## <a name="add-azure-service-principal-toojenkins-credential"></a>Přidat přihlašovací údaje služby Azure hlavní tooJenkins

Objektu zabezpečení služby Azure je potřebné toodeploy tooAzure. 

<ol>
<li>Použití [rozhraní příkazového řádku Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) nebo [portál Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate objektu zabezpečení služby Azure</li>
<li>V rámci hello volaných řídicí panel, klikněte na **pověření -> Systém ->**. Klikněte na tlačítko **globální credentials(unrestricted)**.</li>
<li>Klikněte na tlačítko **přidat přihlašovací údaje** tooadd objekt služby Microsoft Azure tak, že vyplníte hello ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0. Zadejte ID, **mySp**, pro použití v postupných krocích.</li>
</ol>

## <a name="azure-app-service-plugin"></a>Modul plug-in Azure App Service

V1.0 modul plug-in Azure App Service podporuje průběžné nasazování tooAzure webové aplikace prostřednictvím:

* Git a FTP
* Docker pro webovou aplikaci v systému Linux

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a>Konfigurace volaných toodeploy webové aplikace pomocí služby FTP pomocí řídicího panelu volaných hello

toodeploy tooAzure vašeho projektu webové aplikace, můžete nahrát artefaktů sestavení (například .war souboru v jazyce Java) pomocí Git a FTP.

Před nastavením hello úlohu ve volaných, potřebujete pro spuštěné aplikaci Java hello plán aplikační služby Azure a webovou aplikaci.


1. Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz. plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace. Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací.
2. Vytvořte webovou aplikaci. Můžete buď použít hello [portál Azure](/azure/app-service-web/web-sites-configure) nebo hello použijte následující příkaz Az rozhraní příkazového řádku:
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. Ujistěte se, že nastavíte hello Java runtime konfigurace, které vaše aplikace musí. Následující příkaz rozhraní příkazového řádku Azure Hello konfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a>Nastavení úloh volaných hello


1. Vytvořte novou **volný styl** projekt v volaných řídicí panel
2. Konfigurace **správu zdrojového kódu** toouse vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje hello **adresu URL úložiště**. Příklad: http://github.com/&lt;yourID > / javawebappsample.
3. Přidáte sestavení krok toobuild hello projekt pomocí nástroje Maven. Učinit přidáním **spustit prostředí**. V tomto příkladu potřebujeme další krok toorename hello *.war souboru v cílové složce tooROOT.war.   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.
5. Zadejte, "mySp", hello Azure instanční objekt uložená v předchozím kroku.
6. V **konfigurace aplikací** zvolte hello prostředků skupiny a webové aplikace v rámci vašeho předplatného. modul plug-in Hello automaticky zjistí, zda text hello webové aplikace je Windows nebo linuxu. Pro webovou aplikaci systému Windows se zobrazí možnost hello "Publikovat soubory".
7. Zadejte v souborech hello chcete toodeploy (například war balíček Pokud používáte Java.) Zdrojový a cílový adresář jsou volitelné. Parametry Hello povolit toospecify zdrojové a cílové složky při nahrávání souborů. Webové aplikace v jazyce Java v Azure běží na serveru Tomcat. Proto můžete nahrávat na server války se balíček umístěna složka webových aplikací. V tomto příkladu nastavit **zdrojový adresář** příliš "cíle" a "webapps" zadat **cílový adresář**.
8. Pokud chcete toodeploy tooa slotu než produkční, můžete také nastavit **slotu** název.
9. Uložit hello projektu a sestavte jej. Webové aplikace je nasazená tooAzure po dokončení sestavení.

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a>Nasazení webové aplikace pomocí služby FTP volaných zřetězením příkazů

modul plug-in Hello je připravené pro kanál. Ukázka tooa v úložišti GitHub hello lze odkazovat.

1. V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_ftp_plugin** souboru. Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Změňte ID pověření 14 tooupdate řádek v instanci volaných.    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a>Vytvoření kanálu volaných

1. Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.
2. Zadejte název úlohy hello a vyberte **kanálu**. Klikněte na **OK**.
3. Klikněte na tlačítko hello **kanálu** kartě Další.
4. Pro **definice**, vyberte **kanálu skript z SCM**.
5. Pro **SCM**, vyberte **Git**. Zadejte hello Githubu adresu URL pro váš forked úložišti: https:&lt;vaše forked úložišti > .git
6. Aktualizace **cestu ke skriptu** příliš "Jenkinsfile_ftp_plugin"
7. Klikněte na tlačítko **Uložit** a úlohu spusťte hello.

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a>Konfigurace volaných toodeploy webové aplikace v systému Linux pomocí Docker

Kromě Git/FTP webové aplikace v systému Linux podporuje nasazení pomocí Docker. toodeploy pomocí Docker, je nutné tooprovide soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie docker. Potom modul plug-in hello sestavení hello image, nabízených oznámení tooa docker registru a nasadí hello image tooyour webové aplikace.

Webové aplikace v systému Linux také podporuje tradiční způsobů jako Git a FTP, ale jenom pro předdefinované jazyků (.NET Core, Node.js, PHP a Ruby). Pro jiné jazyky toopackage vaší aplikace kód a služby modulu runtime společně do bitové kopie docker a pomocí docker toodeploy.

Před nastavením hello úlohu ve volaných, potřebujete Azure aplikační službu v systému Linux. Kontejner registru je také potřebné toostore a spravovat privátní kontejner imagí Dockeru. Můžete použít DockerHub; v tomto příkladu používáme registru kontejner Azure.

* Můžete provést kroky pro hello [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate webové aplikace v systému Linux 
* Azure registru kontejneru je spravovaný [Docker registru] služby (https://docs.docker.com/registry/) na základě hello open-source Docker registru 2.0. Postupujte podle kroků hello [sem] (/ azure/container-registry/container-registry-get-started-azure-cli) pro další pokyny toodo tak. Můžete také použít DockerHub.

### <a name="toodeploy-using-docker"></a>pomocí docker toodeploy:

1. Vytvoření nového projektu volný styl volaných řídicím panelu.
2. Konfigurace **správu zdrojového kódu** toouse vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje hello **adresu URL úložiště**. Příklad: http://github.com/&lt;yourid > / javawebappsample.
Přidáte sestavení krok toobuild hello projekt pomocí nástroje Maven. Učinit přidáním **spustit prostředí** a přidejte následující řádek ve hello **příkaz**:    
```bash
mvn clean package
```

3. Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.
4. Zadejte, **mySp**, hello Azure instanční objekt uložená v předchozím kroku jako přihlašovací údaje Azure.
5. V **konfigurace aplikací** zvolte skupinu prostředků hello a Linux webové aplikace v rámci vašeho předplatného.
6. Zvolte publikování přes Docker.
7. Vyplňte **soubor Docker** cesta. Můžete ponechat výchozí hello "nebo soubor Docker" pro **Docker registru URL**, dodávky ve formátu hello https://&lt;myRegistry >. azurecr.io, pokud používáte Azure kontejneru registru. Necháte prázdné, pokud používáte DockerHub.
8. Pro **registru pověření**, přidat hello přihlašovací údaje pro hello registru kontejner Azure. Spuštěním následujících příkazů v Azure CLI hello můžete získat hello ID uživatele a heslo. První příkaz Hello umožňuje hello účet správce.    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. Hello docker název bitové kopie a značky v **Upřesnit** kartě jsou volitelné. Ve výchozím nastavení, je získat název bitové kopie z bitové kopie hello název jste nakonfigurovali v Azure portálu (v kontejner Docker nastavení.) hello značky se generují z $BUILD_NUMBER. Ujistěte se, zadejte název bitové kopie hello buď portálu Azure nebo zadejte hodnotu pro **Docker Image** v **Upřesnit** kartě. V tomto příkladu zadejte "&lt;yourRegistry >.azurecr.io/calculator" pro **Docker image** a nechte **značka obrázku Docker** prázdné.
10. Poznámka: nasazení selže, pokud použijete předdefinované nastavení Docker bitové kopie. Ujistěte se, že změníte docker konfigurace toouse vlastní image ve kontejner Docker nastavení na portálu Azure. Předdefinované bitové kopie použijte toodeploy přístup nahrávání souboru.
11. Podobný postup nahrání toofile, můžete vybrat jiný slot než produkční.
12. Uložte a sestavení projektu hello. Zobrazí bitové kopie kontejneru se posune tooyour registru a webové aplikace je nasazená.

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a>Nasazení tooWeb aplikace v systému Linux pomocí Docker volaných zřetězením příkazů

1. V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_container_plugin** souboru. Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Změna řádku 13 tooyour kontejneru registru serveru    
```java
def registryServer = '<registryURL>'
```    

3. Změňte ID pověření 16 tooupdate řádek v instanci volaných    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a>Vytvoření kanálu volaných    

1. Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.
2. Zadejte název úlohy hello a vyberte **kanálu**. Klikněte na **OK**.
3. Klikněte na tlačítko hello **kanálu** kartě Další.
4. Pro **definice**, vyberte **kanálu skript z SCM**.
5. Pro **SCM**, vyberte **Git**.
6. Zadejte hello Githubu adresu URL pro váš forked úložišti: https:&lt;vaše forked úložišti > .git</li>
Aktualizace 7, **cestu ke skriptu** příliš "Jenkinsfile_container_plugin"
8. Klikněte na tlačítko **Uložit** a úlohu spusťte hello.

## <a name="verify-your-web-app"></a>Ověření webové aplikace

1. soubor WAR hello tooverify úspěšně nasadil tooyour webové aplikace. Otevřete webový prohlížeč.
2. Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping zobrazí:    
     Vítejte tooJava webové aplikace! Tato hodnota aktualizuje!
   Sun června 17 16:39:10 UTC 2017
3. Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y        
    ![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a>Pro službu App service v systému Linux

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

V tomto kurzu použijete tooAzure toodeploy modul plug-in Azure App Service hello.

Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Konfigurace volaných toodeploy Azure App Service pomocí služby FTP 
> * Konfigurace volaných toodeploy tooAzure služby App Service v Linuxu prostřednictvím Docker 
