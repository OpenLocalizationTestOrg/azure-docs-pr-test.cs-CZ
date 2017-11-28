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
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a><span data-ttu-id="5dbd7-103">Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy kontejnerizované tooAzure aplikace pružiny spouštěcí</span><span class="sxs-lookup"><span data-stu-id="5dbd7-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>

<span data-ttu-id="5dbd7-104">Hello [Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces hello pro vývojáře toodeploy webové aplikace tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service .</span></span>

<span data-ttu-id="5dbd7-105">Tento článek ukazuje použití hello Maven modulu plug-in pro Azure Web Apps toodeploy ukázkovou aplikaci spouštěcí Spring v tooAzure kontejner Docker aplikační služby.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application in a Docker container tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5dbd7-106">Hello Maven modul plug-in pro Azure Web Apps je aktuálně k dispozici jako náhled.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="5dbd7-107">Teď je podporován pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="5dbd7-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5dbd7-108">Prerequisites</span></span>

<span data-ttu-id="5dbd7-109">Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="5dbd7-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="5dbd7-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5dbd7-111">Hello [rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="5dbd7-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="5dbd7-112">Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="5dbd7-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="5dbd7-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="5dbd7-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-114">A [Git] client.</span></span>
* <span data-ttu-id="5dbd7-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5dbd7-116">Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="5dbd7-117">Klon hello ukázku pružiny spouštěcí Docker webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5dbd7-117">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="5dbd7-118">V této části klonovat kontejnerizované pružiny spouštěcí aplikace a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-118">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="5dbd7-119">Otevřete příkazový řádek nebo okno terminálu a vytvářet místní adresář toohold pružiny spouštěcí aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-119">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="5dbd7-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="5dbd7-120">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="5dbd7-121">Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="5dbd7-122">Změnit adresář toohello dokončit projekt; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-122">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="5dbd7-123">Vytvořit soubor JAR hello pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-123">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="5dbd7-124">Po vytvoření webové aplikace hello spusťte hello webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-124">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="5dbd7-125">Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="5dbd7-126">Můžete například použít následující příkaz, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-126">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="5dbd7-127">Měli byste vidět hello následující zpráva: **Hello, World Docker**</span><span class="sxs-lookup"><span data-stu-id="5dbd7-127">You should see hello following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="5dbd7-128">Vytvořit objekt služby Azure</span><span class="sxs-lookup"><span data-stu-id="5dbd7-128">Create an Azure service principal</span></span>

<span data-ttu-id="5dbd7-129">V této části vytvoříte Azure instanční objekt, který hello Maven modulu plug-in používá při nasazení vaší tooAzure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-129">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="5dbd7-130">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-130">Open a command prompt.</span></span>

1. <span data-ttu-id="5dbd7-131">Přihlaste se k účtu Azure pomocí hello rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-131">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="5dbd7-132">Postup hello pokyny toocomplete hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-132">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="5dbd7-133">Vytvořte objekt služby Azure:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-133">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="5dbd7-134">Kde `uuuuuuuu` je hello uživatelské jméno a `pppppppp` je hello heslo pro hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-134">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="5dbd7-135">Azure odpoví JSON, která se podobá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-135">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="5dbd7-136">Při konfiguraci vašeho kontejneru tooAzure hello Maven modulu plug-in toodeploy použijete hello hodnoty z této odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-136">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="5dbd7-137">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, a `tttttttt` jsou zástupné hodnoty, které jsou používány toomake tento příklad je snazší toomap tyto hodnoty tootheir příslušných prvky při konfiguraci vašeho Maven `settings.xml` další soubor v hello oddíl.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-137">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="5dbd7-138">Konfigurace Maven toouse Azure instančního objektu</span><span class="sxs-lookup"><span data-stu-id="5dbd7-138">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="5dbd7-139">V této části použijte hello hodnoty z vaší služby Azure hlavní tooconfigure hello ověřování, které Maven bude používat při nasazování vaší tooAzure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-139">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven will use when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="5dbd7-140">Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě jako hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-140">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="5dbd7-141">Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-141">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="5dbd7-142">Kde:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-142">Where:</span></span>
   <span data-ttu-id="5dbd7-143">Element</span><span class="sxs-lookup"><span data-stu-id="5dbd7-143">Element</span></span> | <span data-ttu-id="5dbd7-144">Popis</span><span class="sxs-lookup"><span data-stu-id="5dbd7-144">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="5dbd7-145">Určuje jedinečný název, který Maven používá toolook vaše nastavení zabezpečení při nasazení tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-145">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="5dbd7-146">Obsahuje hello `appId` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-146">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="5dbd7-147">Obsahuje hello `tenant` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-147">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="5dbd7-148">Obsahuje hello `password` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-148">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="5dbd7-149">Definuje hello cílové cloudu Azure prostředí, což je `AZURE` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-149">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="5dbd7-150">(Úplný seznam prostředí je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentace)</span><span class="sxs-lookup"><span data-stu-id="5dbd7-150">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="5dbd7-151">Uložte a zavřete hello *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-151">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a><span data-ttu-id="5dbd7-152">Volitelné: Nasazení vaší místní tooDocker soubor Docker Hub</span><span class="sxs-lookup"><span data-stu-id="5dbd7-152">OPTIONAL: Deploy your local Docker file tooDocker Hub</span></span>

<span data-ttu-id="5dbd7-153">Pokud máte účet Docker, můžete sestavit vaše Docker kontejneru image místně a poslat ho tooDocker rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-153">If you have a Docker account, you can build your Docker container image locally and push it tooDocker Hub.</span></span> <span data-ttu-id="5dbd7-154">toodo Ano, použít hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-154">toodo so, use hello following steps.</span></span>

1. <span data-ttu-id="5dbd7-155">Otevřete hello `pom.xml` souboru pružiny spuštění aplikace v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-155">Open hello `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="5dbd7-156">Vyhledejte hello `<imageName>` podřízený element hello `<containerSettings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-156">Locate hello `<imageName>` child element of hello `<containerSettings>` element.</span></span>

1. <span data-ttu-id="5dbd7-157">Aktualizace hello `${docker.image.prefix}` hodnotu s názvem svého účtu Docker:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-157">Update hello `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="5dbd7-158">Vyberte jednu z následujících metod nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-158">Choose one of hello following deployment methods:</span></span>

   * <span data-ttu-id="5dbd7-159">Vytvoření bitové kopie kontejneru místně s Maven a pak použijte Docker toopush vašeho kontejneru tooDocker rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-159">Build your container image locally with Maven, and then use Docker toopush your container tooDocker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="5dbd7-160">Pokud máte hello [Docker modul plug-in pro Maven] nainstalován, můžete automaticky vytvořit a vaše kontejneru image tooDocker centra pomocí hello `-DpushImage` parametr:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-160">If you have hello [Docker plugin for Maven] installed, you can automatically build and your container image tooDocker Hub by using hello `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a><span data-ttu-id="5dbd7-161">Volitelné: Přizpůsobení vaší pom.xml před nasazením vaší tooAzure kontejneru</span><span class="sxs-lookup"><span data-stu-id="5dbd7-161">OPTIONAL: Customize your pom.xml before deploying your container tooAzure</span></span>

<span data-ttu-id="5dbd7-162">Otevřete hello `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte hello `<plugin>` element pro `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-162">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="5dbd7-163">Tento element by měl vypadat hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-163">This element should resemble hello following example:</span></span>

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

<span data-ttu-id="5dbd7-164">Existuje několik hodnot, které lze upravit pro modul plug-in hello Maven a podrobný popis pro každou z těchto elementů je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-164">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="5dbd7-165">Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-165">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="5dbd7-166">Element</span><span class="sxs-lookup"><span data-stu-id="5dbd7-166">Element</span></span> | <span data-ttu-id="5dbd7-167">Popis</span><span class="sxs-lookup"><span data-stu-id="5dbd7-167">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="5dbd7-168">Určuje verzi hello hello [Maven modul plug-in pro Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="5dbd7-168">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="5dbd7-169">Měli byste zkontrolovat hello verze uvedené v hello [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) hello tooensure, který používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-169">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="5dbd7-170">Určuje hello ověřovací informace pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu toolook hlavní hodnot hello služba Azure ve vašem Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-170">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="5dbd7-171">Určuje hello cílová skupina prostředků, který je `maven-plugin` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-171">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="5dbd7-172">Skupina prostředků Hello se vytvoří během nasazení, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-172">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="5dbd7-173">Určuje název cílového hello pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-173">Specifies hello target name for your web app.</span></span> <span data-ttu-id="5dbd7-174">V tomto příkladu je název cílové hello `maven-linux-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-174">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="5dbd7-175">(hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)</span><span class="sxs-lookup"><span data-stu-id="5dbd7-175">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="5dbd7-176">Určuje hello cílová oblast, která v tomto příkladu je `westus`.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-176">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="5dbd7-177">(Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="5dbd7-177">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="5dbd7-178">Určuje nastavení jedinečné pro Maven toouse při nasazování tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-178">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="5dbd7-179">V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které určují hello port pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-179">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5dbd7-180">číslo portu Hello nastavení toochange hello v tomto příkladu jsou nezbytné, pouze pokud měníte hello port z výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-180">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

## <a name="build-and-deploy-your-container-tooazure"></a><span data-ttu-id="5dbd7-181">Sestavení a nasazení vaší tooAzure kontejneru</span><span class="sxs-lookup"><span data-stu-id="5dbd7-181">Build and deploy your container tooAzure</span></span>

<span data-ttu-id="5dbd7-182">Po nakonfigurování všech nastavení hello v předchozích částech tohoto článku hello jste připravené toodeploy vaše tooAzure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-182">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your container tooAzure.</span></span> <span data-ttu-id="5dbd7-183">toodo tedy použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-183">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="5dbd7-184">Z hello příkazový řádek nebo okno terminálu, který jste dříve používali, znovu sestavit pomocí nástroje Maven Pokud jste provedli jakékoli změny toohello soubor JAR hello *pom.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-184">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="5dbd7-185">Nasadit tooAzure vaší webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-185">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="5dbd7-186">Maven nasadí tooAzure vaší webové aplikace; Pokud webová aplikace hello již neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-186">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5dbd7-187">Pokud hello oblast, kterou zadáte v hello `<region>` element vaše *pom.xml* soubor nemá dostatek serverů, které jsou k dispozici po spuštění nasazení, může dojít k chybě toohello podobně jako následující příklad:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-187">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
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
> <span data-ttu-id="5dbd7-188">V takovém případě můžete zadat že jinou oblast a znovu spusťte hello Maven příkaz toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dbd7-188">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="5dbd7-189">Pokud váš web nasazen, bude možné toomanage ho pomocí hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="5dbd7-189">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="5dbd7-190">Webové aplikace se objeví v **App Services**:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-190">Your web app will be listed in **App Services**:</span></span>

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* <span data-ttu-id="5dbd7-192">A hello adresu URL pro webové aplikace se objeví v hello **přehled** pro webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-192">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5dbd7-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dbd7-194">Next steps</span></span>

<span data-ttu-id="5dbd7-195">Další informace o hello různých technologií, které jsou popsané v tomto článku najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="5dbd7-195">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="5dbd7-196">[Maven modul plug-in pro Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="5dbd7-196">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="5dbd7-197">Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5dbd7-197">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="5dbd7-198">Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure pružiny spouštěcí aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="5dbd7-198">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure App Service </span></span>](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="5dbd7-199">Vytvořit objekt služby Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5dbd7-199">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="5dbd7-200">Referenční příručka k nastavení maven</span><span class="sxs-lookup"><span data-stu-id="5dbd7-200">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="5dbd7-201">[Docker modul plug-in pro Maven]</span><span class="sxs-lookup"><span data-stu-id="5dbd7-201">[Docker plugin for Maven]</span></span>

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
