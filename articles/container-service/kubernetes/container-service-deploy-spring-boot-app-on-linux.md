---
title: "aaaDeploy pružiny spuštění webové aplikace v systému Linux v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede, když hello kroky toodeploy pružiny spouštěcí aplikace jako webové aplikace Linux v Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2c44be1c7f66a38f48239001f0be9e90c7e6edef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a>Nasazení aplikace pružiny spouštění v systému Linux v hello Azure Container Service

Hello  **[pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

**[Docker]**  je open-source řešení, které pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede pomocí Docker toodevelop a nasadit spouštěcí pružiny aplikace tooa Linux hostitele v hello [Azure Container Service (ACS)].

## <a name="prerequisites"></a>Požadavky

Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* Hello [rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Developer Kit (JDK)].
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a>Vytvoření hello pružiny spouštěcí na Docker Začínáme webové aplikace

Hello následující kroky vás provedou hello kroky, které jsou požadované toocreate jednoduchou webovou aplikaci pružiny spouštěcí a otestovat ji místně.

1. Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Změnit adresář toohello dokončit projekt; například:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Vytvořit soubor JAR hello pomocí nástroje Maven; například:
   ```
   mvn package
   ```

1. Po vytvoření webové aplikace hello změnit adresář toohello `target` adresáře, kde se nachází soubor JAR hello a spusťte hello webové aplikace, například:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče. Například pokud máte curl k dispozici a je nakonfigurován hello Tomcat server toorun na portu 80:
   ```
   curl http://localhost
   ```

1. Měli byste vidět hello následující zpráva: **Docker Hello, World!**

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a>Vytvoření služby Azure kontejneru registru toouse jako privátní registru Docker

Hello následující kroky vás provedou pomocí hello Azure portálu toocreate registru kontejneru služby Azure.

> [!NOTE]
>
> Pokud chcete toouse hello rozhraní příkazového řádku Azure místo hello portálu Azure, postupujte podle kroků hello v [vytvořit privátní registru kontejner Docker pomocí hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Procházet toohello [portál Azure] a přihlaste se.

   Po přihlášení tooyour účet na hello portálu Azure můžete provést kroky hello v hello [vytvořit privátní registru kontejner Docker pomocí portálu Azure hello] článku, které jsou parafrázována v hello následující kroky pro hello zájmu vhodnosti.

1. Klikněte na ikonu nabídky hello **+ nový**, pak klikněte na tlačítko **kontejnery**a potom klikněte na **registru kontejner Azure**.
   
   ![Vytvořit nový kontejner registru Azure][AR01]

1. Při zobrazení stránky hello informace pro šablonu Azure kontejneru registru hello, klikněte na tlačítko **vytvořit**. 

   ![Vytvořit nový kontejner registru Azure][AR02]

1. Když hello **vytvořit kontejner registru** zobrazí se stránka, zadejte vaše **název registru** a **skupiny prostředků**, zvolte **povolit** pro Hello **uživatel s oprávněními správce**a potom klikněte na **vytvořit**.

   ![Konfigurace nastavení registru kontejner Azure][AR03]

1. Po vytvoření kontejneru registr, přejděte tooyour kontejneru registru hello portál Azure a pak klikněte na tlačítko **přístupové klíče**. Poznamenejte si hello uživatelské jméno a heslo pro další kroky hello.

   ![Azure přístupové klíče registru kontejneru][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a>Konfigurace Maven toouse klíče pro přístup k registru kontejner Azure

1. Přejděte toohello konfiguračního adresáře pro instalaci Maven a otevřete hello *souborech settings.xml* soubor v textovém editoru.

1. Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto kurzu toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker / dokončení*") a otevřete hello *pom.xml* soubor v textovém editoru.

1. Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaši Azure kontejneru registru z předchozí části hello tohoto kurzu; například:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizace hello `<plugins>` kolekce v hello *pom.xml* souboru, který hello `<plugin>` obsahuje hello přihlašovací adresu a registru název serveru pro váš registru kontejner Azure z hello předchozí části tohoto kurzu. Například:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toorebuild hello aplikace hello a push hello kontejneru tooyour registru kontejner Azure:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Pokud nabízíte vaší tooAzure kontejner Docker, může se zobrazit chybová zpráva, která je podobné tooone hello následujících to i v případě, že vaše kontejner Docker byla úspěšně vytvořena.:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Pokud k tomu dojde, může být nutné toosign v tooyour účet Azure z příkazového řádku Dockeru hello; například:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Potom můžete posouvat vašeho kontejneru z příkazového řádku hello; například:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Vytvoření webové aplikace v systému Linux v Azure App Service pomocí bitové kopie kontejneru

1. Procházet toohello [portál Azure] a přihlaste se.

1. Klikněte na ikonu nabídky hello **+ nový**, pak klikněte na tlačítko **Web + mobilní**a potom klikněte na **webové aplikace v systému Linux**.
   
   ![Vytvořit novou webovou aplikaci v hello portálu Azure][LX01]

1. Když hello **webové aplikace v systému Linux** se zobrazí stránka, zadejte hello následující informace:

   a. Zadejte jedinečný název pro hello **název aplikace**; například: "*wingtiptoyslinux*."

   b. Zvolte vaše **předplatné** z rozevíracího seznamu hello.

   c. Vybrat existující **skupiny prostředků**, nebo zadejte název toocreate novou skupinu prostředků.

   d. Klikněte na tlačítko **kontejneru konfigurace** a zadejte hello následující informace:

      * Zvolte **privátní registru**.

      * **Bitové kopie a volitelné značky**: Zadejte název kontejneru z dříve; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"

      * **Adresa URL serveru**: Zadejte svoji adresu URL registru z dříve; například: "*https://wingtiptoysregistry.azurecr.io*"

      * **Uživatelské jméno přihlášení** a **heslo**: Zadejte své přihlašovací údaje z vaší **přístupové klíče** který jste použili v předchozích krocích.
   
   e. Po zadání všech hello výše informace, klikněte na tlačítko **OK**.

   ![Konfigurovat nastavení webové aplikace][LX02]

1. Klikněte na možnost **Vytvořit**.

> [!NOTE]
>
> Azure bude automaticky mapovat Internet požadavky tooembedded Tomcat serveru, který běží na standardní porty hello 80 nebo 8080. Pokud jste nakonfigurovali vaší embedded toorun server Tomcat na vlastní port, ale musíte tooadd prostředí proměnné tooyour webovou aplikaci, která definuje hello port pro embedded serveru Tomcat. toodo tedy použijte hello následující kroky:
>
> 1. Procházet toohello [portál Azure] a přihlaste se.
> 
> 2. Klikněte na ikonu hello **App Services**. (Viz položku #1 hello obrázek níže).
>
> 3. Vyberte ze seznamu hello vaší webové aplikace. (Položka #2 hello obrázek níže.)
>
> 4. Klikněte na tlačítko **nastavení aplikace**. (Položka #3 hello obrázek níže.)
>
> 5. V hello **nastavení aplikace** přidejte novou proměnnou prostředí s názvem **PORT** a zadejte vlastní port číslo pro hodnotu hello. (Položka #4 v následující obrázek hello.)
>
> 6. Klikněte na **Uložit**. (Položka #5 hello obrázek níže.)
>
> ![Ukládání vlastní číslo portu v hello portálu Azure][LX03]
>

<!--
##  OPTIONAL: Configure hello embedded Tomcat server toorun on a different port

hello embedded Tomcat server in hello sample Spring Boot application is configured toorun on port 8080 by default. However, if you want toorun hello embedded Tomcat server toorun on a different port, such as port 80 for local testing, you can configure hello port by using hello following steps.

1. Go toohello *resources* directory (or create hello directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open hello *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify hello **server** setting so that hello server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close hello *application.yml* file.
-->

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:

* [Nasazení aplikace spouštěcí pružiny toohello Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service](container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

Další podrobnosti o hello pružiny spouštěcí na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].

Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu hello **pružiny Initializr** v https://start.spring.io/.

Další informace o začátcích se vytvoření jednoduché aplikace pružiny spouštěcí najdete v části hello Spring Initializr v https://start.spring.io/.

Další příklady jak toouse vlastní Docker obrázků s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[vytvořit privátní registru kontejner Docker pomocí portálu Azure hello]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
