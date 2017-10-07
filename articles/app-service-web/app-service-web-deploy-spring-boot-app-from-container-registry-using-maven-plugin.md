---
title: "aaaHow toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy pružiny spuštění aplikace v tooAzure registru kontejner Azure App Service"
description: "Tento kurz vás provede, když hello kroky toodeploy pružiny spuštění aplikace v tooAzure tooAzure registru kontejner Azure App Service pomocí modulu plug-in Maven."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a>Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy pružiny spuštění aplikace v tooAzure registru kontejner Azure App Service

Hello  **[pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java. Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny tooget rychle začít.

Tento článek ukazuje, jak toodeploy tooAzure aplikace pružiny spouštěcí ukázkový kontejner registru a pak použijte hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure vaše aplikace služby App Service.

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
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
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

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-service-principal"></a>Vytvořit objekt služby Azure

V této části vytvoříte Azure instanční objekt, který hello Maven modulu plug-in používá při nasazení vaší tooAzure kontejneru.

1. Otevřete příkazový řádek.

1. Přihlaste se k účtu Azure pomocí hello rozhraní příkazového řádku Azure:
   ```azurecli
   az login
   ```
   Postup hello pokyny toocomplete hello přihlášení.

1. Vytvořte objekt služby Azure:
   ```azurecli
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

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Vytvoření registru kontejneru služby Azure pomocí hello rozhraní příkazového řádku Azure

1. Otevřete příkazový řádek.

1. Přihlaste se tooyour účet Azure:
   ```azurecli
   az login
   ```

1. Vytvořte skupinu prostředků pro hello prostředky Azure, které budete používat v tomto článku:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Nahraďte `wingtiptoysresources` v tomto příkladu s jedinečným názvem skupiny prostředků.

1. Vytvořte kontejner privátní Azure registru ve skupině prostředků hello pružiny spouštěcí aplikace: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Nahraďte `wingtiptoysregistry` v tomto příkladu se jedinečný název vašeho kontejneru registru.

1. Načtení hello hesla pro vaše registru kontejneru:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure bude odpovídat pomocí hesla; například:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a>Přidat kontejner Azure registru a nastavení Maven hlavní tooyour služba Azure

1. Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě jako hello následující příklady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto článku toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<id>` | Obsahuje název vaší privátní kontejner Azure registru hello.
   `<username>` | Obsahuje název vaší privátní kontejner Azure registru hello.
   `<password>` | Obsahuje hello heslo, které jste získali v předchozí části tohoto článku hello.

1. Přidat nastavení hlavní služby Azure z předcházející části tohoto článku toohello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:

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

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a>Sestavení vaší Docker kontejneru bitové kopie a poslat ho tooyour kontejner Azure registru

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (např.) "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") a otevřete hello *pom.xml* soubor s textový editor.

1. Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaši Azure kontejneru registru z předchozí části hello tohoto kurzu; například:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<azure.containerRegistry>` | Určuje název hello vaší privátní kontejner Azure registru.
   `<docker.image.prefix>` | Určuje adresu URL registru privátní kontejner Azure, které je odvozeno připojením hello ". azurecr.io" toohello název vaší privátní kontejneru registru.

1. Ověřte, že `<plugin>` pro modul plug-in Docker hello ve vaší *pom.xml* soubor obsahuje hello správné vlastnosti pro hello přihlašovací adresu a registru název serveru hello v předchozím kroku v tomto kurzu. Například:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Kde:
   Element | Popis
   ---|---|---
   `<serverId>` | Určuje hello vlastnost, která obsahuje název vaší privátní kontejner Azure registru.
   `<registryUrl>` | Určuje hello vlastnost, která obsahuje adresu URL hello vaší privátní kontejner Azure registru.

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toorebuild hello aplikace hello a push hello kontejneru tooyour kontejner Azure registru:

   ```
   mvn package docker:build -DpushImage 
   ```

1. Volitelné: Procházet toohello [portál Azure] a ověřte, zda je image kontejner Docker s názvem **gs pružiny spouštěcí docker** v registru kontejneru.

   ![Ověřte kontejneru na portálu Azure][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a>Přizpůsobení pom.xml, pak sestavení a nasazení vaší tooAzure kontejneru

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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
`<resourceGroup>` | Určuje hello cílová skupina prostředků, který je `wingtiptoysresources` v tomto příkladu. Skupina prostředků Hello se vytvoří během nasazení, pokud ještě neexistuje.
`<appName>` | Určuje název cílového hello pro vaši webovou aplikaci. V tomto příkladu je název cílové hello `maven-linux-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona. (hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)
`<region>` | Určuje hello cílová oblast, která v tomto příkladu je `westus`. (Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)
`<containerSettings>` | Určuje hello vlastnosti, které obsahují hello název a adresu URL vašeho kontejneru.
`<appSettings>` | Určuje nastavení jedinečné pro Maven toouse při nasazování tooAzure vaší webové aplikace. V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které určují hello port pro vaši aplikaci.

> [!NOTE]
>
> číslo portu Hello nastavení toochange hello v tomto příkladu jsou nezbytné, pouze pokud měníte hello port z výchozí hello.
>

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

## <a name="next-steps"></a>Další kroky

Další informace o hello různých technologií, které jsou popsané v tomto článku najdete hello následující články:

* [Maven modul plug-in pro Azure Web Apps]

* [Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure](/azure/xplat-cli-connect)

* [Vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referenční příručka k nastavení maven](https://maven.apache.org/settings.html)

* [Modul plug-in docker pro Maven]

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Modul plug-in docker pro Maven]: https://github.com/spotify/docker-maven-plugin
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
