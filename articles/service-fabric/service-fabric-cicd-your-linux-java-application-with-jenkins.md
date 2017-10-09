---
title: "aaaContinuous sestavení a integraci pro aplikace v jazyce Linux Java Azure Service Fabric pomocí volaných | Microsoft Docs"
description: "Průběžné sestavování a integrace linuxových aplikací v Javě pomocí Jenkinse"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 15da2cb8c759233219369ea889550da93748129f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-jenkins-toobuild-and-deploy-your-linux-java-application"></a>Použití volaných toobuild a nasazení aplikace v jazyce Linux Java
Jenkins je oblíbený nástroj pro průběžnou integraci a nasazování aplikací. Tady je způsob toobuild a nasadit aplikace Azure Service Fabric pomocí volaných.

## <a name="general-prerequisites"></a>Obecné požadavky
- Máte lokálně nainstalovaný Git. Můžete nainstalovat hello příslušnou verzi Git z [hello Git stáhne stránky](https://git-scm.com/downloads), podle operačního systému. Pokud jste nový tooGit, získáte další informace z hello [Git dokumentaci](https://git-scm.com/docs).
- Mít hello Service Fabric volaných modulu plug-in užitečné. Můžete ho stáhnout ze stránky pro [stažení Service Fabric](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Nastavení Jenkinse uvnitř clusteru Service Fabric

Jenkinse můžete nastavit uvnitř clusteru Service Fabric nebo mimo něj. Hello následující části zobrazit jak tooset ho nahoru v clusteru s podporou při používání Azure storage účet toosave hello stavu instance hello kontejneru.

### <a name="prerequisites"></a>Požadavky
1. Máte připravený cluster Service Fabric s Linuxem. Cluster Service Fabric vytvořené z hello portál Azure již má nainstalované Docker. Pokud používáte hello cluster místně, zkontrolujte, zda je nainstalována pomocí příkazu hello Docker ``docker info``. Pokud nainstalovaná není, nainstalujte ji podle toho pomocí hello následující příkazy:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Máte hello Service Fabric kontejnerové aplikace nasazené na clusteru hello pomocí hello následující kroky:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
cd service-fabric-java-getting-started/Services/JenkinsDocker/
```

3. Je třeba hello připojit možnost Podrobnosti o hello úložiště Azure sdílenou, místo, kam chcete toopersist hello stavu instance hello volaných kontejneru. Pokud používáte portál Microsoft Azure hello pro hello stejné, prosím postupujte podle kroků hello – vytvoření účtu úložiště Azure, například ``sfjenkinsstorage1``. Vytvoření **sdílené složky** pod tímto účtem úložiště vyslovení ``sfjenkins``. Klikněte na **připojit** pro sdílené složky a Poznámka hello hello hodnoty, se zobrazí v části **připojení z Linux**, například to může vypadat třeba takto -
```sh
sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
```

4. Aktualizujte zástupný symbol hodnoty hello v hello ```setupentrypoint.sh``` skriptu s podrobnostmi o odpovídající úložiště azure.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
Nahraďte ``[REMOTE_FILE_SHARE_LOCATION]`` s hodnotou hello ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` ze hello výstup hello připojit v bodě 3 výše.
Nahraďte ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` s hodnotou hello ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` z bodu 3 výše.

5. Připojte toohello cluster a instalace aplikace hello kontejneru.
```azurecli
sfctl cluster select --endpoint http://PublicIPorFQDN:19080   # cluster connect command
bash Scripts/install.sh
```
Nainstaluje kontejner volaných hello clusteru a můžete monitorovat pomocí hello Service Fabric Exploreru.

### <a name="steps"></a>Kroky
1. Z prohlížeče, přejděte příliš``http://PublicIPorFQDN:8081``. Poskytuje hello cestu hello počáteční správce požadováno heslo toosign v. Můžete dál toouse volaných jako uživatel s oprávněními správce. Nebo můžete vytvořit a změnit hello uživatele po přihlásíte pomocí účtu správce. počáteční hello.

   > [!NOTE]
   > Zajistěte, aby byl hello 8081 port jako port koncového bodu aplikace hello při vytváření clusteru hello.
   >

2. Získat ID instance kontejneru hello pomocí ``docker ps -a``.
3. Secure Shell (SSH) přihlášení toohello kontejneru a vložte hello cesta, kterou jste se zobrazí na portálu volaných hello. Například, pokud hello portálu se zobrazí cesta hello `PATH_TO_INITIAL_ADMIN_PASSWORD`, spusťte následující hello:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Nastavit Githubu toowork s volaných, pomocí kroků hello v [generování nového klíče SSH a její přidání agenta SSH toohello](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Pomocí pokynů hello klíč SSH hello toogenerate Githubu a tooadd hello SSH klíče toohello účtu GitHub, který je hostitelem úložiště.
    * Spusťte příkazy hello uvedený v hello předcházející odkaz v hello volaných Docker prostředí (a ne na vašem hostiteli).
    * toosign v toohello volaných prostředí z hostitele, hello použijte následující příkaz:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Nastavení Jenkinse mimo cluster Service Fabric

Jenkinse můžete nastavit uvnitř clusteru Service Fabric nebo mimo něj. Dobrý den, následující části zobrazit jak tooset ho nahoru mimo cluster.

### <a name="prerequisites"></a>Požadavky
Je nutné toohave Docker nainstalována. použít tooinstall Docker z hello terminálu může být Hello následující příkazy:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Nyní když spustíte ``docker info`` v hello terminálu, měli byste vidět ve výstupu hello této hello Docker se službou.

### <a name="steps"></a>Kroky
  1. Hello Service Fabric volaných kontejneru image pro vyžádání obsahu:``docker pull raunakpandya/jenkins:v1``
  2. Spusťte kontejner hello bitové kopie:``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Získání ID hello hello kontejneru image instance. Můžete vytvořit seznam všechny kontejnery Docker hello příkazem hello``docker ps –a``
  4. Přihlaste se toohello volaných portálu pomocí hello následující kroky:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Pokud ID kontejneru je 2d24a73b5964, použijte 2d24.
    * Heslo je vyžadováno pro přihlášení toohello volaných řídicího panelu portálu, který je``http://<HOST-IP>:8080``
    * Po přihlášení pro hello poprvé, můžete vytvořit svůj vlastní uživatelský účet a použít pro budoucí účely, nebo můžete pokračovat toouse hello správce účtu. Po vytvoření uživatele, musíte toocontinue s třídou.
  5. Nastavit Githubu toowork s volaných, pomocí kroků hello v [generování nového klíče SSH a její přidání agenta SSH toohello](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
        * Pomocí pokynů hello klíč SSH hello toogenerate Githubu a tooadd hello SSH klíče toohello účtu GitHub, který je hostitelem hello úložiště.
        * Spusťte příkazy hello uvedený v hello předcházející odkaz v hello volaných Docker prostředí (a ne na vašem hostiteli).
      * toosign v toohello volaných prostředí z hostitele, hello použijte následující příkazy:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Ujistěte se, že hello clusteru nebo počítače, kde hello, který je hostován volaných kontejneru image má veřejné IP adresu. To umožňuje hello volaných instance tooreceive oznámení z Githubu.

## <a name="install-hello-service-fabric-jenkins-plug-in-from-hello-portal"></a>Instalovat hello Service Fabric volaných modulu plug-in z portálu hello

1. Přejděte příliš``http://PublicIPorFQDN:8081``
2. Hello volaných řídicí panel, vyberte **spravovat volaných** > **Správa modulů plug-in** > **Upřesnit**.
Tady můžete nahrát modul plug-in. Vyberte **zvolte soubor**a potom vyberte hello **serviceFabric.hpi** souboru, který jste stáhli v části požadavky. Když vyberete **nahrát**, volaných automaticky nainstaluje modul plug-in hello. Pokud je vyžadováno restartování, povolte ho.

## <a name="create-and-configure-a-jenkins-job"></a>Vytvoření a konfigurace úlohy Jenkinse

1. Na řídicím panelu vytvořte **novou položku**.
2. Zadejte název položky (třeba **MyJob**). Vyberte **free-style project** (volný styl projektu) a klikněte na **OK**.
3. Přejděte na stránku hello úlohy a klikněte na tlačítko **konfigurace**.

   a. V části Obecné hello pod **Githubu projektu**, zadejte svoji adresu URL projektu Githubu. Tato adresa URL hostitele hello služby Fabric aplikaci Java, které chcete toointegrate s hello volaných průběžnou integraci, toku průběžné nasazování (CI/CD) (například ``https://github.com/sayantancs/SFJenkins``).

   b. V části hello **správu zdrojového kódu** vyberte **Git**. Zadejte adresu URL hello úložiště, který je hostitelem aplikace hello Service Fabric Java, které chcete toointegrate s hello toku volaných CI/CD (například ``https://github.com/sayantancs/SFJenkins.git``). Navíc můžete zde určíte které větve toobuild (například **/hlavní**).
4. Konfigurace vaší *Githubu* (který je hostitelem úložiště hello), aby bylo možné tootalk tooJenkins. Použijte hello následující kroky:

   a. Přejděte tooyour stránce úložiště Githubu. Přejděte příliš**nastavení** > **integrace služby a služby**.

   b. Vyberte **přidat službu**, typ **volaných**a vyberte hello **modulu plug-in volaných Githubu**.

   c. Zadejte adresu URL webhooku Jenkinse (ve výchozím nastavení by měla být ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klikněte na **add/update service** (Přidat/aktualizovat službu).

   d. Událost testování se odesílají tooyour volaných instance. Měli byste vidět zelené zaškrtnutí ve hello webhooku v Githubu a bude sestavení projektu.

   e. V části hello **sestavení aktivační události** vyberte, které možnost, kterou chcete vytvořit. V tomto příkladu budete chtít tootrigger sestavení, vždy, když se stane, některé nabízené toohello úložiště. Proto vyberete **GitHub hook trigger for GITScm polling** (Trigger webhooku GitHubu pro cyklické dotazování GitHubu). (Dříve, tato možnost byla volána **sestavení při změně je nabídnutých tooGitHub**.)

   f. V části hello **sestavení části**, z rozevíracího seznamu hello **přidat krok sestavení**, vyberte možnost hello **vyvolání skriptu Gradle**. V hello pomůcky, který jste dostali, zadejte cestu hello příliš**kořenové sestavení skriptu** pro vaši aplikaci. Převezme build.gradle ze zadané cesty hello a odpovídajícím způsobem funguje. Pokud vytvoříte projekt s názvem ``MyActor`` (s použitím modulu plug-in nebo Yeoman generátor Eclipse hello), by měl obsahovat skriptu buildu kořenové hello ``${WORKSPACE}/MyActor``. V tématu hello následující snímek obrazovky příklad co vypadá takto:

    ![Akce sestavení v Jenkinsu pro Service Fabric][build-step]

   g. Z hello **akce po sestavení** rozevíracího seznamu, vyberte **nasazení projektu služby Fabric**. Zde je třeba tooprovide cluster, ve kterém se nasazuje podrobnosti, kde hello volaných kompilované aplikace Service Fabric. Můžete zadat také podrobnosti o další aplikace používá toodeploy hello aplikace. V tématu hello následující snímek obrazovky příklad co vypadá takto:

    ![Akce sestavení v Jenkinsu pro Service Fabric][post-build-step]

   > [!NOTE]
   > Zde Hello clusteru může být stejný jako jeden hostitelský hello hello volaných aplikaci kontejneru, v případě, že používáte Service Fabric toodeploy hello volaných kontejneru image.
   >

## <a name="next-steps"></a>Další kroky
GitHub a Jenkins jsou teď nakonfigurované. Zvažte některé ukázkové změnit ve vaší ``MyActor`` projektu v příkladu úložiště hello https://github.com/sayantancs/SFJenkins. Vaše změny tooa push vzdáleného ``master`` větve (nebo všechny firemní pobočky, který jste nakonfigurovali toowork s). Tím se spustí úlohu volaných hello ``MyJob``, který jste nakonfigurovali. Načte hello změny z Githubu, je sestavení a nasadí hello toohello clusteru koncový bod aplikace, které jste zadali v akce po sestavení.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png
