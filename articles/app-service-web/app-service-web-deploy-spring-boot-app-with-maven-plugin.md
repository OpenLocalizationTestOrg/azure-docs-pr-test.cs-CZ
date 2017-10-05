---
title: "Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení spouštěcí pružiny aplikace do Azure"
description: "Další informace o použití modulu plug-in Maven pro webové aplikace Azure k nasazení pružiny spouštění aplikace na Azure."
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
ms.openlocfilehash: dceb7edf788bd87b1de04aa435a12cd5853755b9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a><span data-ttu-id="13364-103">Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení spouštěcí pružiny aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="13364-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure</span></span>

<span data-ttu-id="13364-104">[Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces pro vývojáře, jak nasadit webové aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="13364-104">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="13364-105">Tento článek ukazuje, pomocí modulu plug-in Maven pro webové aplikace Azure k nasazení ukázkové aplikace pružiny spouštěcí do Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="13364-105">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="13364-106">Modul plug-in Maven pro Azure Web Apps je aktuálně k dispozici jako náhled.</span><span class="sxs-lookup"><span data-stu-id="13364-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="13364-107">Teď je podporována pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucnost.</span><span class="sxs-lookup"><span data-stu-id="13364-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="13364-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="13364-108">Prerequisites</span></span>

<span data-ttu-id="13364-109">Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="13364-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="13364-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="13364-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="13364-111">[Rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="13364-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="13364-112">Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="13364-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="13364-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="13364-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="13364-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="13364-114">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="13364-115">Naklonujte ukázkovou webovou aplikaci pružiny spouštěcí</span><span class="sxs-lookup"><span data-stu-id="13364-115">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="13364-116">V této části klonovat hotová aplikace pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="13364-116">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="13364-117">Otevřete příkazový řádek nebo okno terminálu a vytvořte místní adresář pro uložení aplikace pružiny spouštěcí a změňte do tohoto adresáře; například:</span><span class="sxs-lookup"><span data-stu-id="13364-117">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="13364-118">--nebo--</span><span class="sxs-lookup"><span data-stu-id="13364-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="13364-119">Klon [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="13364-119">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="13364-120">Změňte adresář na dokončené projektu. například:</span><span class="sxs-lookup"><span data-stu-id="13364-120">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="13364-121">Sestavení na soubor JAR pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="13364-121">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="13364-122">Po vytvoření webové aplikace, spusťte webovou aplikaci pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="13364-122">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="13364-123">Test webové aplikace tak, že přejde k němu místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="13364-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="13364-124">Můžete například použít následující příkaz curl k dispozici máte-li:</span><span class="sxs-lookup"><span data-stu-id="13364-124">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="13364-125">Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="13364-125">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="13364-126">Vytvořit objekt služby Azure</span><span class="sxs-lookup"><span data-stu-id="13364-126">Create an Azure service principal</span></span>

<span data-ttu-id="13364-127">V této části vytvoříte Azure instanční objekt, který modul plug-in Maven používá při nasazení webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="13364-127">In this section, you create an Azure service principal that the Maven plugin uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="13364-128">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="13364-128">Open a command prompt.</span></span>

1. <span data-ttu-id="13364-129">Přihlaste se ke svému účtu Azure pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="13364-129">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="13364-130">Postupujte podle pokynů dokončete proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="13364-130">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="13364-131">Vytvořte objekt služby Azure:</span><span class="sxs-lookup"><span data-stu-id="13364-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="13364-132">Kde `uuuuuuuu` je uživatelské jméno a `pppppppp` je heslo pro objekt služby.</span><span class="sxs-lookup"><span data-stu-id="13364-132">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="13364-133">Azure odpoví JSON, která se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="13364-133">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="13364-134">Pokud nakonfigurujete modul plug-in Maven k nasazení vaší webové aplikace do Azure budete používat hodnoty z této odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="13364-134">You will use the values from this JSON response when you configure the Maven plugin to deploy your web app to Azure.</span></span> <span data-ttu-id="13364-135">`aaaaaaaa`, `uuuuuuuu`, `pppppppp`, A `tttttttt` jsou zástupné hodnoty, které se používají v tomto příkladu, aby bylo snazší mapovat tyto hodnoty na jejich odpovídající prvky při konfiguraci vašeho Maven `settings.xml` souboru v další části.</span><span class="sxs-lookup"><span data-stu-id="13364-135">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="13364-136">Konfigurace Maven používat Azure instančního objektu</span><span class="sxs-lookup"><span data-stu-id="13364-136">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="13364-137">V této části je použít hodnoty ze služby Azure hlavní postup konfigurace ověřování, který Maven používá při nasazení webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="13364-137">In this section, you use the values from your Azure service principal to configure the authentication that Maven uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="13364-138">Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě, jako jsou následující příklady:</span><span class="sxs-lookup"><span data-stu-id="13364-138">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="13364-139">Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu k `<servers>` kolekce v *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="13364-139">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="13364-140">Kde:</span><span class="sxs-lookup"><span data-stu-id="13364-140">Where:</span></span>
   <span data-ttu-id="13364-141">Element</span><span class="sxs-lookup"><span data-stu-id="13364-141">Element</span></span> | <span data-ttu-id="13364-142">Popis</span><span class="sxs-lookup"><span data-stu-id="13364-142">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="13364-143">Určuje jedinečný název, který používá Maven k vyhledání nastavení zabezpečení při nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="13364-143">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="13364-144">Obsahuje `appId` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="13364-144">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="13364-145">Obsahuje `tenant` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="13364-145">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="13364-146">Obsahuje `password` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="13364-146">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="13364-147">Definuje cílovém prostředí cloudu Azure, což je `AZURE` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="13364-147">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="13364-148">(Úplný seznam prostředí je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentace)</span><span class="sxs-lookup"><span data-stu-id="13364-148">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="13364-149">Uložte a zavřete *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="13364-149">Save and close the *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a><span data-ttu-id="13364-150">Volitelné: Přizpůsobení vaší pom.xml před nasazením webové aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="13364-150">OPTIONAL: Customize your pom.xml before deploying your web app to Azure</span></span>

<span data-ttu-id="13364-151">Otevřete `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte `<plugin>` element pro `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="13364-151">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="13364-152">Tento element by měl podobat následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="13364-152">This element should resemble the following example:</span></span>

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

<span data-ttu-id="13364-153">Existuje několik hodnot, které lze upravit pro modul plug-in Maven a podrobný popis pro každou z těchto elementů je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="13364-153">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="13364-154">Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="13364-154">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="13364-155">Element</span><span class="sxs-lookup"><span data-stu-id="13364-155">Element</span></span> | <span data-ttu-id="13364-156">Popis</span><span class="sxs-lookup"><span data-stu-id="13364-156">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="13364-157">Určuje verzi [Maven modul plug-in pro Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="13364-157">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="13364-158">Verze uvedené v byste měli zkontrolovat [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) zajistit, že používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="13364-158">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="13364-159">Určuje informace o ověřování pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu k vyhledání objektu služby Azure hodnoty v vaše Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="13364-159">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="13364-160">Určuje, cílová skupina prostředků, který je `maven-plugin` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="13364-160">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="13364-161">Skupina prostředků se vytvoří během nasazení, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="13364-161">The resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="13364-162">Určuje název cílového pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="13364-162">Specifies the target name for your web app.</span></span> <span data-ttu-id="13364-163">V tomto příkladu je název cílové `maven-web-app-${maven.build.timestamp}`, kde `${maven.build.timestamp}` v tomto příkladu, aby se zabránilo konfliktu se připojí přípona.</span><span class="sxs-lookup"><span data-stu-id="13364-163">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="13364-164">(Časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace.)</span><span class="sxs-lookup"><span data-stu-id="13364-164">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="13364-165">Určuje cílová oblast, která v tomto příkladu je `westus`.</span><span class="sxs-lookup"><span data-stu-id="13364-165">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="13364-166">(Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="13364-166">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="13364-167">Určuje verzi jazyka Java runtime pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13364-167">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="13364-168">(Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="13364-168">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="13364-169">Určuje typ nasazení pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13364-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="13364-170">Prozatím se pouze `ftp` je podporováno, i když podporu pro jiné typy nasazení je ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="13364-170">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="13364-171">Určuje prostředky a cílové umístění, které Maven používá při nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="13364-171">Specifies resources and target destinations which Maven uses when deploying your web app to Azure.</span></span> <span data-ttu-id="13364-172">V tomto příkladu dvě `<resource>` elementy určit, že budou Maven nasazovat na soubor JAR pro vaši webovou aplikaci a *web.config* souboru z jara spouštěcí projektu.</span><span class="sxs-lookup"><span data-stu-id="13364-172">In this example, two `<resource>` elements specify that Maven will deploy the JAR file for your web app and the *web.config* file from the Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="13364-173">Sestavení a nasazení webové aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="13364-173">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="13364-174">Jakmile nakonfigurujete všechna nastavení v předchozí části tohoto článku, budete chtít nasadit webovou aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="13364-174">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="13364-175">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="13364-175">To do so, use the following steps:</span></span>

1. <span data-ttu-id="13364-176">Z příkazového řádku nebo okno terminálu, který jste dříve používali, znovu vytvořit na soubor JAR pomocí nástroje Maven Pokud jste provedli všechny změny *pom.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="13364-176">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="13364-177">Nasazení webové aplikace do Azure pomocí Maven; například:</span><span class="sxs-lookup"><span data-stu-id="13364-177">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="13364-178">Maven bude nasazení webové aplikace do Azure; Pokud webová aplikace už neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="13364-178">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="13364-179">Pokud váš web nasazen, bude možné spravovat pomocí [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="13364-179">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="13364-180">Webové aplikace se objeví v **App Services**:</span><span class="sxs-lookup"><span data-stu-id="13364-180">Your web app will be listed in **App Services**:</span></span>

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* <span data-ttu-id="13364-182">A zobrazí se adresa URL pro webovou aplikaci v **přehled** pro webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="13364-182">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Určení adresy URL pro webovou aplikaci][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="13364-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13364-184">Next steps</span></span>

<span data-ttu-id="13364-185">Další informace o různých technologií, které jsou popsané v tomto článku najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="13364-185">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="13364-186">[Maven modul plug-in pro Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="13364-186">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="13364-187">Přihlaste se k Azure z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="13364-187">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="13364-188">Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="13364-188">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="13364-189">Vytvořit objekt služby Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13364-189">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="13364-190">Referenční příručka k nastavení maven</span><span class="sxs-lookup"><span data-stu-id="13364-190">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

<span data-ttu-id="13364-191">[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="13364-191">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="13364-192">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="13364-192">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="13364-193">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="13364-193">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="13364-194">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="13364-194">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="13364-195">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="13364-195">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="13364-196">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="13364-196">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
[Spring Boot]: http://projects.spring.io/spring-boot/
<span data-ttu-id="13364-197">[pružiny spouštěcí Začínáme]: https://github.com/microsoft/gs-spring-boot</span><span class="sxs-lookup"><span data-stu-id="13364-197">[Spring Boot Getting Started]: https://github.com/microsoft/gs-spring-boot</span></span>
[Spring Framework]: https://spring.io/
<span data-ttu-id="13364-198">[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="13364-198">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
