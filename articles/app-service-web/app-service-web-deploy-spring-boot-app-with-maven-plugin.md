---
title: "aaaHow toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spouštěcí aplikace"
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
ms.openlocfilehash: 376fe90fe20621e15d7c9856214937c78b66026a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-tooazure"></a>Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spouštěcí aplikace

Hello [Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces hello pro vývojáře toodeploy webové aplikace tooAzure služby App Service.

Tento článek ukazuje použití hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure aplikace pružiny spouštěcí ukázkové aplikace služby.

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

## <a name="clone-hello-sample-spring-boot-web-app"></a>Klonování hello ukázka pružiny spouštěcí webové aplikace

V této části klonovat hotová aplikace pružiny spouštěcí a otestovat ji místně.

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

1. Klon hello [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. Změnit adresář toohello dokončit projekt; například:
   ```shell
   cd gs-spring-boot/complete
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

1. Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který hello používá modul plug-in Maven při nasazování tooAzure vaší webové aplikace.

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
   > Hello hodnoty z této odpovědi JSON budete používat při konfiguraci hello Maven modulu plug-in toodeploy tooAzure vaší webové aplikace. Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, a `tttttttt` jsou zástupné hodnoty, které jsou používány toomake tento příklad je snazší toomap tyto hodnoty tootheir příslušných prvky při konfiguraci vašeho Maven `settings.xml` další soubor v hello oddíl.
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a>Konfigurace Maven toouse Azure instančního objektu

V této části použijte hello hodnoty z vaší služby Azure hlavní tooconfigure hello ověřování, který Maven používá při nasazování tooAzure vaší webové aplikace.

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

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-tooazure"></a>Volitelné: Přizpůsobení vaší pom.xml před nasazením tooAzure vaší webové aplikace

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

Existuje několik hodnot, které lze upravit pro modul plug-in hello Maven a podrobný popis pro každou z těchto elementů je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci. Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:

Element | Popis
---|---|---
`<version>` | Určuje verzi hello hello [Maven modul plug-in pro Azure Web Apps]. Měli byste zkontrolovat hello verze uvedené v hello [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) hello tooensure, který používáte nejnovější verzi.
`<authentication>` | Určuje hello ověřovací informace pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu toolook hlavní hodnot hello služba Azure ve vašem Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.
`<resourceGroup>` | Určuje hello cílová skupina prostředků, který je `maven-plugin` v tomto příkladu. Pokud ještě neexistuje, vytvoří se skupina prostředků Hello během nasazení.
`<appName>` | Určuje název cílového hello pro vaši webovou aplikaci. V tomto příkladu je název cílové hello `maven-web-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona. (hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)
`<region>` | Určuje hello cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<javaVersion>` | Určuje verzi modulu runtime hello Java pro vaši webovou aplikaci. (Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<deploymentType>` | Určuje typ nasazení pro vaši webovou aplikaci. Prozatím se pouze `ftp` je podporováno, i když podporu pro jiné typy nasazení je ve vývoji.
`<resources>` | Určuje prostředky a cílové umístění, které Maven používá při nasazování tooAzure vaší webové aplikace. V tomto příkladu dvě `<resource>` prvky zadat, že budou Maven nasazovat soubor JAR hello pro webovou aplikaci a hello *web.config* souboru z projektu pružiny spouštěcí hello.

## <a name="build-and-deploy-your-web-app-tooazure"></a>Vytváření a nasazování tooAzure vaší webové aplikace

Po nakonfigurování všech nastavení hello v předchozích částech tohoto článku hello jste připravené toodeploy tooAzure vaší webové aplikace. toodo tedy použijte hello následující kroky:

1. Z hello příkazový řádek nebo okno terminálu, který jste dříve používali, znovu sestavit pomocí nástroje Maven Pokud jste provedli jakékoli změny toohello soubor JAR hello *pom.xml* souboru; například:
   ```shell
   mvn clean package
   ```

1. Nasadit tooAzure vaší webové aplikace pomocí nástroje Maven; například:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven nasadí tooAzure vaší webové aplikace; Pokud webová aplikace hello již neexistuje, bude vytvořen.

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

* [Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy kontejnerizované tooAzure aplikace pružiny spouštěcí](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí Začínáme]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
