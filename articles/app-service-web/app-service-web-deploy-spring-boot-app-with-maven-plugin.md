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
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-tooazure"></a><span data-ttu-id="9ff24-103">Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spouštěcí aplikace</span><span class="sxs-lookup"><span data-stu-id="9ff24-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure</span></span>

<span data-ttu-id="9ff24-104">Hello [Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces hello pro vývojáře toodeploy webové aplikace tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9ff24-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service.</span></span>

<span data-ttu-id="9ff24-105">Tento článek ukazuje použití hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure aplikace pružiny spouštěcí ukázkové aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="9ff24-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9ff24-106">Hello Maven modul plug-in pro Azure Web Apps je aktuálně k dispozici jako náhled.</span><span class="sxs-lookup"><span data-stu-id="9ff24-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="9ff24-107">Teď je podporován pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="9ff24-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="9ff24-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ff24-108">Prerequisites</span></span>

<span data-ttu-id="9ff24-109">Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="9ff24-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="9ff24-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="9ff24-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="9ff24-111">Hello [rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="9ff24-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="9ff24-112">Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9ff24-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="9ff24-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="9ff24-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="9ff24-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="9ff24-114">A [Git] client.</span></span>

## <a name="clone-hello-sample-spring-boot-web-app"></a><span data-ttu-id="9ff24-115">Klonování hello ukázka pružiny spouštěcí webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9ff24-115">Clone hello sample Spring Boot web app</span></span>

<span data-ttu-id="9ff24-116">V této části klonovat hotová aplikace pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="9ff24-116">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="9ff24-117">Otevřete příkazový řádek nebo okno terminálu a vytvářet místní adresář toohold pružiny spouštěcí aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-117">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="9ff24-118">--nebo--</span><span class="sxs-lookup"><span data-stu-id="9ff24-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="9ff24-119">Klon hello [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-119">Clone hello [Spring Boot Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="9ff24-120">Změnit adresář toohello dokončit projekt; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-120">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="9ff24-121">Vytvořit soubor JAR hello pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-121">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="9ff24-122">Po vytvoření webové aplikace hello spusťte hello webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-122">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="9ff24-123">Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9ff24-123">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="9ff24-124">Můžete například použít následující příkaz, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="9ff24-124">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="9ff24-125">Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="9ff24-125">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="9ff24-126">Vytvořit objekt služby Azure</span><span class="sxs-lookup"><span data-stu-id="9ff24-126">Create an Azure service principal</span></span>

<span data-ttu-id="9ff24-127">V této části vytvoříte Azure instanční objekt, který hello používá modul plug-in Maven při nasazování tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-127">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="9ff24-128">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="9ff24-128">Open a command prompt.</span></span>

1. <span data-ttu-id="9ff24-129">Přihlaste se k účtu Azure pomocí hello rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="9ff24-129">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="9ff24-130">Postup hello pokyny toocomplete hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9ff24-130">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="9ff24-131">Vytvořte objekt služby Azure:</span><span class="sxs-lookup"><span data-stu-id="9ff24-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="9ff24-132">Kde `uuuuuuuu` je hello uživatelské jméno a `pppppppp` je hello heslo pro hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="9ff24-132">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="9ff24-133">Azure odpoví JSON, která se podobá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9ff24-133">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="9ff24-134">Hello hodnoty z této odpovědi JSON budete používat při konfiguraci hello Maven modulu plug-in toodeploy tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-134">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your web app tooAzure.</span></span> <span data-ttu-id="9ff24-135">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, a `tttttttt` jsou zástupné hodnoty, které jsou používány toomake tento příklad je snazší toomap tyto hodnoty tootheir příslušných prvky při konfiguraci vašeho Maven `settings.xml` další soubor v hello oddíl.</span><span class="sxs-lookup"><span data-stu-id="9ff24-135">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="9ff24-136">Konfigurace Maven toouse Azure instančního objektu</span><span class="sxs-lookup"><span data-stu-id="9ff24-136">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="9ff24-137">V této části použijte hello hodnoty z vaší služby Azure hlavní tooconfigure hello ověřování, který Maven používá při nasazování tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-137">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="9ff24-138">Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě jako hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="9ff24-138">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="9ff24-139">Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-139">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="9ff24-140">Kde:</span><span class="sxs-lookup"><span data-stu-id="9ff24-140">Where:</span></span>
   <span data-ttu-id="9ff24-141">Element</span><span class="sxs-lookup"><span data-stu-id="9ff24-141">Element</span></span> | <span data-ttu-id="9ff24-142">Popis</span><span class="sxs-lookup"><span data-stu-id="9ff24-142">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="9ff24-143">Určuje jedinečný název, který Maven používá toolook vaše nastavení zabezpečení při nasazení tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-143">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="9ff24-144">Obsahuje hello `appId` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="9ff24-144">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="9ff24-145">Obsahuje hello `tenant` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="9ff24-145">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="9ff24-146">Obsahuje hello `password` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="9ff24-146">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="9ff24-147">Definuje hello cílové cloudu Azure prostředí, což je `AZURE` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="9ff24-147">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="9ff24-148">(Úplný seznam prostředí je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentace)</span><span class="sxs-lookup"><span data-stu-id="9ff24-148">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="9ff24-149">Uložte a zavřete hello *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="9ff24-149">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-tooazure"></a><span data-ttu-id="9ff24-150">Volitelné: Přizpůsobení vaší pom.xml před nasazením tooAzure vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9ff24-150">OPTIONAL: Customize your pom.xml before deploying your web app tooAzure</span></span>

<span data-ttu-id="9ff24-151">Otevřete hello `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte hello `<plugin>` element pro `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="9ff24-151">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="9ff24-152">Tento element by měl vypadat hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9ff24-152">This element should resemble hello following example:</span></span>

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

<span data-ttu-id="9ff24-153">Existuje několik hodnot, které lze upravit pro modul plug-in hello Maven a podrobný popis pro každou z těchto elementů je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9ff24-153">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="9ff24-154">Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="9ff24-154">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="9ff24-155">Element</span><span class="sxs-lookup"><span data-stu-id="9ff24-155">Element</span></span> | <span data-ttu-id="9ff24-156">Popis</span><span class="sxs-lookup"><span data-stu-id="9ff24-156">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="9ff24-157">Určuje verzi hello hello [Maven modul plug-in pro Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="9ff24-157">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="9ff24-158">Měli byste zkontrolovat hello verze uvedené v hello [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) hello tooensure, který používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="9ff24-158">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="9ff24-159">Určuje hello ověřovací informace pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu toolook hlavní hodnot hello služba Azure ve vašem Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9ff24-159">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="9ff24-160">Určuje hello cílová skupina prostředků, který je `maven-plugin` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="9ff24-160">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="9ff24-161">Pokud ještě neexistuje, vytvoří se skupina prostředků Hello během nasazení.</span><span class="sxs-lookup"><span data-stu-id="9ff24-161">hello resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="9ff24-162">Určuje název cílového hello pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ff24-162">Specifies hello target name for your web app.</span></span> <span data-ttu-id="9ff24-163">V tomto příkladu je název cílové hello `maven-web-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona.</span><span class="sxs-lookup"><span data-stu-id="9ff24-163">In this example, hello target name is `maven-web-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="9ff24-164">(hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)</span><span class="sxs-lookup"><span data-stu-id="9ff24-164">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="9ff24-165">Určuje hello cílová oblast, která v tomto příkladu je `westus`.</span><span class="sxs-lookup"><span data-stu-id="9ff24-165">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="9ff24-166">(Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="9ff24-166">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="9ff24-167">Určuje verzi modulu runtime hello Java pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ff24-167">Specifies hello Java runtime version for your web app.</span></span> <span data-ttu-id="9ff24-168">(Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="9ff24-168">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="9ff24-169">Určuje typ nasazení pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ff24-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="9ff24-170">Prozatím se pouze `ftp` je podporováno, i když podporu pro jiné typy nasazení je ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="9ff24-170">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="9ff24-171">Určuje prostředky a cílové umístění, které Maven používá při nasazování tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-171">Specifies resources and target destinations which Maven uses when deploying your web app tooAzure.</span></span> <span data-ttu-id="9ff24-172">V tomto příkladu dvě `<resource>` prvky zadat, že budou Maven nasazovat soubor JAR hello pro webovou aplikaci a hello *web.config* souboru z projektu pružiny spouštěcí hello.</span><span class="sxs-lookup"><span data-stu-id="9ff24-172">In this example, two `<resource>` elements specify that Maven will deploy hello JAR file for your web app and hello *web.config* file from hello Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-tooazure"></a><span data-ttu-id="9ff24-173">Vytváření a nasazování tooAzure vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9ff24-173">Build and deploy your web app tooAzure</span></span>

<span data-ttu-id="9ff24-174">Po nakonfigurování všech nastavení hello v předchozích částech tohoto článku hello jste připravené toodeploy tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff24-174">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your web app tooAzure.</span></span> <span data-ttu-id="9ff24-175">toodo tedy použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ff24-175">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="9ff24-176">Z hello příkazový řádek nebo okno terminálu, který jste dříve používali, znovu sestavit pomocí nástroje Maven Pokud jste provedli jakékoli změny toohello soubor JAR hello *pom.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-176">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="9ff24-177">Nasadit tooAzure vaší webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="9ff24-177">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="9ff24-178">Maven nasadí tooAzure vaší webové aplikace; Pokud webová aplikace hello již neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="9ff24-178">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

<span data-ttu-id="9ff24-179">Pokud váš web nasazen, bude možné toomanage ho pomocí hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="9ff24-179">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="9ff24-180">Webové aplikace se objeví v **App Services**:</span><span class="sxs-lookup"><span data-stu-id="9ff24-180">Your web app will be listed in **App Services**:</span></span>

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* <span data-ttu-id="9ff24-182">A hello adresu URL pro webové aplikace se objeví v hello **přehled** pro webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="9ff24-182">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="9ff24-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ff24-184">Next steps</span></span>

<span data-ttu-id="9ff24-185">Další informace o hello různých technologií, které jsou popsané v tomto článku najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="9ff24-185">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="9ff24-186">[Maven modul plug-in pro Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="9ff24-186">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="9ff24-187">Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9ff24-187">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="9ff24-188">Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy kontejnerizované tooAzure aplikace pružiny spouštěcí</span><span class="sxs-lookup"><span data-stu-id="9ff24-188">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="9ff24-189">Vytvořit objekt služby Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9ff24-189">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="9ff24-190">Referenční příručka k nastavení maven</span><span class="sxs-lookup"><span data-stu-id="9ff24-190">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

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
