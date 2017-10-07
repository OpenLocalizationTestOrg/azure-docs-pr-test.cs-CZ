---
title: "aaaHow toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy kontejnerizované tooAzure aplikace pružiny spouštěcí"
description: "Zjistěte, jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spuštění aplikace."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: e7e760d4ef5bd4c92a4126a50a2b12e5c8f2b4a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a>Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy kontejnerizované tooAzure aplikace pružiny spouštěcí

Hello [Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces hello pro vývojáře toodeploy webové aplikace tooAzure služby App Service.

Tento článek ukazuje použití hello Maven modulu plug-in pro Azure Web Apps toodeploy ukázkovou aplikaci spouštěcí Spring v tooAzure kontejner Docker aplikační služby.

> [!NOTE]
>
> Hello Maven modul plug-in pro Azure Web Apps je aktuálně k dispozici jako náhled. Teď je podporován pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucí hello.
>

## <a name="prerequisites"></a>Požadavky

Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* Hello [rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a>Klon hello ukázku pružiny spouštěcí Docker webové aplikace

V této části klonovat kontejnerizované pružiny spouštěcí aplikace a otestovat ji místně.

1. Otevřete příkazový řádek nebo okno terminálu a vytvářet místní adresář toohold pružiny spouštěcí aplikace a změnit adresář toothat; například:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. Změnit adresář toohello dokončit projekt; například:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Vytvořit soubor JAR hello pomocí nástroje Maven; například:
   ```shell
   mvn clean package
   ```

1. Po vytvoření webové aplikace hello spusťte hello webové aplikace pomocí nástroje Maven; například:
   ```shell
   mvn spring-boot:run
   ```

1. Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče. Můžete například použít následující příkaz, pokud máte k dispozici curl hello:
   ```shell
   curl http://localhost:8080
   ```

1. Měli byste vidět hello následující zpráva: **Hello, World Docker**

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který hello Maven modulu plug-in používá při nasazení vaší tooAzure kontejneru.

1. Otevřete příkazový řádek.

1. Přihlaste se k účtu Azure pomocí hello rozhraní příkazového řádku Azure:
   ```shell
   az login
   ```
   Postup hello pokyny toocomplete hello přihlášení.

1. Vytvořte objekt služby Azure:
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Kde `uuuuuuuu` je hello uživatelské jméno a `pppppppp` je hello heslo pro hello instanční objekt.

1. Azure odpoví JSON, která se podobá hello následující ukázka:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > Při konfiguraci vašeho kontejneru tooAzure hello Maven modulu plug-in toodeploy použijete hello hodnoty z této odpovědi JSON. Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, a `tttttttt` jsou zástupné hodnoty, které jsou používány toomake tento příklad je snazší toomap tyto hodnoty tootheir příslušných prvky při konfiguraci vašeho Maven `settings.xml` další soubor v hello oddíl.
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a>Konfigurace Maven toouse Azure instančního objektu

V této části použijte hello hodnoty z vaší služby Azure hlavní tooconfigure hello ověřování, které Maven bude používat při nasazování vaší tooAzure kontejneru.

1. Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě jako hello následující příklady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<id>` | Určuje jedinečný název, který Maven používá toolook vaše nastavení zabezpečení při nasazení tooAzure vaší webové aplikace.
   `<client>` | Obsahuje hello `appId` hodnotu z instanční objekt.
   `<tenant>` | Obsahuje hello `tenant` hodnotu z instanční objekt.
   `<key>` | Obsahuje hello `password` hodnotu z instanční objekt.
   `<environment>` | Definuje hello cílové cloudu Azure prostředí, což je `AZURE` v tomto příkladu. (Úplný seznam prostředí je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentace)

1. Uložte a zavřete hello *souborech settings.xml* souboru.

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a>Volitelné: Nasazení vaší místní tooDocker soubor Docker Hub

Pokud máte účet Docker, můžete sestavit vaše Docker kontejneru image místně a poslat ho tooDocker rozbočovače. toodo Ano, použít hello následující kroky.

1. Otevřete hello `pom.xml` souboru pružiny spuštění aplikace v textovém editoru.

1. Vyhledejte hello `<imageName>` podřízený element hello `<containerSettings>` elementu.

1. Aktualizace hello `${docker.image.prefix}` hodnotu s názvem svého účtu Docker:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Vyberte jednu z následujících metod nasazení hello:

   * Vytvoření bitové kopie kontejneru místně s Maven a pak použijte Docker toopush vašeho kontejneru tooDocker rozbočovače:
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * Pokud máte hello [Docker modul plug-in pro Maven] nainstalován, můžete automaticky vytvořit a vaše kontejneru image tooDocker centra pomocí hello `-DpushImage` parametr:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a>Volitelné: Přizpůsobení vaší pom.xml před nasazením vaší tooAzure kontejneru

Otevřete hello `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte hello `<plugin>` element pro `azure-webapp-maven-plugin`. Tento element by měl vypadat hello následující ukázka:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Existuje několik hodnot, které lze upravit pro modul plug-in hello Maven a podrobný popis pro každou z těchto elementů je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci. Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:

Element | Popis
---|---|---
`<version>` | Určuje verzi hello hello [Maven modul plug-in pro Azure Web Apps]. Měli byste zkontrolovat hello verze uvedené v hello [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) hello tooensure, který používáte nejnovější verzi.
`<authentication>` | Určuje hello ověřovací informace pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu toolook hlavní hodnot hello služba Azure ve vašem Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.
`<resourceGroup>` | Určuje hello cílová skupina prostředků, který je `maven-plugin` v tomto příkladu. Skupina prostředků Hello se vytvoří během nasazení, pokud ještě neexistuje.
`<appName>` | Určuje název cílového hello pro vaši webovou aplikaci. V tomto příkladu je název cílové hello `maven-linux-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona. (hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)
`<region>` | Určuje hello cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<appSettings>` | Určuje nastavení jedinečné pro Maven toouse při nasazování tooAzure vaší webové aplikace. V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které určují hello port pro vaši aplikaci.

> [!NOTE]
>
> číslo portu Hello nastavení toochange hello v tomto příkladu jsou nezbytné, pouze pokud měníte hello port z výchozí hello.
>

## <a name="build-and-deploy-your-container-tooazure"></a>Sestavení a nasazení vaší tooAzure kontejneru

Po nakonfigurování všech nastavení hello v předchozích částech tohoto článku hello jste připravené toodeploy vaše tooAzure kontejneru. toodo tedy použijte hello následující kroky:

1. Z hello příkazový řádek nebo okno terminálu, který jste dříve používali, znovu sestavit pomocí nástroje Maven Pokud jste provedli jakékoli změny toohello soubor JAR hello *pom.xml* souboru; například:
   ```shell
   mvn clean package
   ```

1. Nasadit tooAzure vaší webové aplikace pomocí nástroje Maven; například:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven nasadí tooAzure vaší webové aplikace; Pokud webová aplikace hello již neexistuje, bude vytvořen.

> [!NOTE]
>
> Pokud hello oblast, kterou zadáte v hello `<region>` element vaše *pom.xml* soubor nemá dostatek serverů, které jsou k dispozici po spuštění nasazení, může dojít k chybě toohello podobně jako následující příklad:
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> V takovém případě můžete zadat že jinou oblast a znovu spusťte hello Maven příkaz toodeploy vaší aplikace.
>
>

Pokud váš web nasazen, bude možné toomanage ho pomocí hello [portál Azure].

* Webové aplikace se objeví v **App Services**:

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* A hello adresu URL pro webové aplikace se objeví v hello **přehled** pro webové aplikace:

   ![Určení hello adresu URL pro webovou aplikaci][AP02]

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

Další informace o hello různých technologií, které jsou popsané v tomto článku najdete hello následující články:

* [Maven modul plug-in pro Azure Web Apps]

* [Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure](/azure/xplat-cli-connect)

* [Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spouštěcí aplikace služby App Service](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

* [Docker modul plug-in pro Maven]

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker modul plug-in pro Maven]: https://github.com/spotify/docker-maven-plugin
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
