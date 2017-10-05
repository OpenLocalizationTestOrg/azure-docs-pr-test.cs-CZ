---
title: "Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webové aplikace Azure"
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
ms.openlocfilehash: 883040590291cee94daa227fbc6715ad4be0b393
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-containerized-spring-boot-app-to-azure"></a><span data-ttu-id="5996c-103">Jak nasadit kontejnerizované pružiny spouštěcí aplikaci do Azure pomocí modulu plug-in Maven pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="5996c-103">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>

<span data-ttu-id="5996c-104">[Maven modul plug-in pro Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) pro [Apache Maven](http://maven.apache.org/) zajišťuje bezproblémovou integraci služby Azure App Service do projekty Maven a zjednodušuje proces pro vývojáře, jak nasadit webové aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5996c-104">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service .</span></span>

<span data-ttu-id="5996c-105">Tento článek ukazuje, pomocí modulu plug-in Maven pro webové aplikace Azure k nasazení ukázkové aplikace spouštěcí Spring v kontejner Docker do Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="5996c-105">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application in a Docker container to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5996c-106">Modul plug-in Maven pro Azure Web Apps je aktuálně k dispozici jako náhled.</span><span class="sxs-lookup"><span data-stu-id="5996c-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="5996c-107">Teď je podporována pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucnost.</span><span class="sxs-lookup"><span data-stu-id="5996c-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="5996c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5996c-108">Prerequisites</span></span>

<span data-ttu-id="5996c-109">Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="5996c-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="5996c-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="5996c-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5996c-111">[Rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="5996c-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="5996c-112">Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5996c-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="5996c-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="5996c-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="5996c-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="5996c-114">A [Git] client.</span></span>
* <span data-ttu-id="5996c-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="5996c-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5996c-116">Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="5996c-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="5996c-117">Clone – ukázka pružiny spouštěcí ve webové aplikaci Docker</span><span class="sxs-lookup"><span data-stu-id="5996c-117">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="5996c-118">V této části klonovat kontejnerizované pružiny spouštěcí aplikace a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="5996c-118">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="5996c-119">Otevřete příkazový řádek nebo okno terminálu a vytvořte místní adresář pro uložení aplikace pružiny spouštěcí a změňte do tohoto adresáře; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-119">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="5996c-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="5996c-120">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="5996c-121">Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="5996c-122">Změňte adresář na dokončené projektu. například:</span><span class="sxs-lookup"><span data-stu-id="5996c-122">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="5996c-123">Sestavení na soubor JAR pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-123">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="5996c-124">Po vytvoření webové aplikace, spusťte webovou aplikaci pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-124">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="5996c-125">Test webové aplikace tak, že přejde k němu místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5996c-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="5996c-126">Můžete například použít následující příkaz curl k dispozici máte-li:</span><span class="sxs-lookup"><span data-stu-id="5996c-126">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="5996c-127">Měli byste vidět zobrazenou následující zprávu: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="5996c-127">You should see the following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="5996c-128">Vytvořit objekt služby Azure</span><span class="sxs-lookup"><span data-stu-id="5996c-128">Create an Azure service principal</span></span>

<span data-ttu-id="5996c-129">V této části vytvoříte Azure instanční objekt, který modul plug-in Maven používá při nasazení vaší kontejneru v Azure.</span><span class="sxs-lookup"><span data-stu-id="5996c-129">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="5996c-130">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="5996c-130">Open a command prompt.</span></span>

1. <span data-ttu-id="5996c-131">Přihlaste se ke svému účtu Azure pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="5996c-131">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="5996c-132">Postupujte podle pokynů dokončete proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5996c-132">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="5996c-133">Vytvořte objekt služby Azure:</span><span class="sxs-lookup"><span data-stu-id="5996c-133">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="5996c-134">Kde `uuuuuuuu` je uživatelské jméno a `pppppppp` je heslo pro objekt služby.</span><span class="sxs-lookup"><span data-stu-id="5996c-134">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="5996c-135">Azure odpoví JSON, která se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="5996c-135">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="5996c-136">Pokud nakonfigurujete modul plug-in Maven k nasazení vašeho kontejneru na Azure budete používat hodnoty z této odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="5996c-136">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="5996c-137">`aaaaaaaa`, `uuuuuuuu`, `pppppppp`, A `tttttttt` jsou zástupné hodnoty, které se používají v tomto příkladu, aby bylo snazší mapovat tyto hodnoty na jejich odpovídající prvky při konfiguraci vašeho Maven `settings.xml` souboru v další části.</span><span class="sxs-lookup"><span data-stu-id="5996c-137">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="5996c-138">Konfigurace Maven používat Azure instančního objektu</span><span class="sxs-lookup"><span data-stu-id="5996c-138">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="5996c-139">V této části je použít hodnoty ze služby Azure hlavní postup konfigurace ověřování, který Maven bude používat při nasazování vašeho kontejneru do Azure.</span><span class="sxs-lookup"><span data-stu-id="5996c-139">In this section, you use the values from your Azure service principal to configure the authentication that Maven will use when deploying your container to Azure.</span></span>

1. <span data-ttu-id="5996c-140">Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě, jako jsou následující příklady:</span><span class="sxs-lookup"><span data-stu-id="5996c-140">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="5996c-141">Přidat nastavení hlavní služby Azure z předchozí části tohoto kurzu k `<servers>` kolekce v *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-141">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="5996c-142">Kde:</span><span class="sxs-lookup"><span data-stu-id="5996c-142">Where:</span></span>
   <span data-ttu-id="5996c-143">Element</span><span class="sxs-lookup"><span data-stu-id="5996c-143">Element</span></span> | <span data-ttu-id="5996c-144">Popis</span><span class="sxs-lookup"><span data-stu-id="5996c-144">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="5996c-145">Určuje jedinečný název, který používá Maven k vyhledání nastavení zabezpečení při nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="5996c-145">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="5996c-146">Obsahuje `appId` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5996c-146">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="5996c-147">Obsahuje `tenant` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5996c-147">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="5996c-148">Obsahuje `password` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="5996c-148">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="5996c-149">Definuje cílovém prostředí cloudu Azure, což je `AZURE` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5996c-149">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="5996c-150">(Úplný seznam prostředí je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentace)</span><span class="sxs-lookup"><span data-stu-id="5996c-150">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="5996c-151">Uložte a zavřete *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="5996c-151">Save and close the *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a><span data-ttu-id="5996c-152">Volitelné: Nasazení vaší místní soubor Docker do úložiště Docker Hub</span><span class="sxs-lookup"><span data-stu-id="5996c-152">OPTIONAL: Deploy your local Docker file to Docker Hub</span></span>

<span data-ttu-id="5996c-153">Pokud máte účet Docker, můžete sestavit vaše Docker kontejneru image místně a poslat ho do úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="5996c-153">If you have a Docker account, you can build your Docker container image locally and push it to Docker Hub.</span></span> <span data-ttu-id="5996c-154">Uděláte to tak, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="5996c-154">To do so, use the following steps.</span></span>

1. <span data-ttu-id="5996c-155">Otevřete `pom.xml` souboru pružiny spuštění aplikace v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="5996c-155">Open the `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="5996c-156">Vyhledejte `<imageName>` podřízený element `<containerSettings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5996c-156">Locate the `<imageName>` child element of the `<containerSettings>` element.</span></span>

1. <span data-ttu-id="5996c-157">Aktualizace `${docker.image.prefix}` hodnotu s názvem svého účtu Docker:</span><span class="sxs-lookup"><span data-stu-id="5996c-157">Update the `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="5996c-158">Vyberte jednu z následujících metod nasazení:</span><span class="sxs-lookup"><span data-stu-id="5996c-158">Choose one of the following deployment methods:</span></span>

   * <span data-ttu-id="5996c-159">Vytvoření bitové kopie kontejneru místně s Maven a pak pomocí Docker push vaší kontejner Docker hub:</span><span class="sxs-lookup"><span data-stu-id="5996c-159">Build your container image locally with Maven, and then use Docker to push your container to Docker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="5996c-160">Pokud máte [Docker modul plug-in pro Maven] nainstalován, můžete automaticky vytvořit a vaše kontejneru bitové kopie do úložiště Docker Hub pomocí `-DpushImage` parametr:</span><span class="sxs-lookup"><span data-stu-id="5996c-160">If you have the [Docker plugin for Maven] installed, you can automatically build and your container image to Docker Hub by using the `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a><span data-ttu-id="5996c-161">Volitelné: Přizpůsobení vaší pom.xml před nasazením vaší kontejneru do Azure</span><span class="sxs-lookup"><span data-stu-id="5996c-161">OPTIONAL: Customize your pom.xml before deploying your container to Azure</span></span>

<span data-ttu-id="5996c-162">Otevřete `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte `<plugin>` element pro `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="5996c-162">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="5996c-163">Tento element by měl podobat následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="5996c-163">This element should resemble the following example:</span></span>

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

<span data-ttu-id="5996c-164">Existuje několik hodnot, které lze upravit pro modul plug-in Maven a podrobný popis pro každou z těchto elementů je k dispozici v [Maven modul plug-in pro Azure Web Apps] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="5996c-164">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="5996c-165">Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="5996c-165">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="5996c-166">Element</span><span class="sxs-lookup"><span data-stu-id="5996c-166">Element</span></span> | <span data-ttu-id="5996c-167">Popis</span><span class="sxs-lookup"><span data-stu-id="5996c-167">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="5996c-168">Určuje verzi [Maven modul plug-in pro Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="5996c-168">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="5996c-169">Verze uvedené v byste měli zkontrolovat [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) zajistit, že používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5996c-169">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="5996c-170">Určuje informace o ověřování pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu k vyhledání objektu služby Azure hodnoty v vaše Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="5996c-170">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="5996c-171">Určuje, cílová skupina prostředků, který je `maven-plugin` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5996c-171">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="5996c-172">Skupina prostředků se vytvoří během nasazení, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5996c-172">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="5996c-173">Určuje název cílového pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5996c-173">Specifies the target name for your web app.</span></span> <span data-ttu-id="5996c-174">V tomto příkladu je název cílové `maven-linux-app-${maven.build.timestamp}`, kde `${maven.build.timestamp}` v tomto příkladu, aby se zabránilo konfliktu se připojí přípona.</span><span class="sxs-lookup"><span data-stu-id="5996c-174">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="5996c-175">(Časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace.)</span><span class="sxs-lookup"><span data-stu-id="5996c-175">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="5996c-176">Určuje cílová oblast, která v tomto příkladu je `westus`.</span><span class="sxs-lookup"><span data-stu-id="5996c-176">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="5996c-177">(Úplný seznam je v [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="5996c-177">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="5996c-178">Určuje nastavení jedinečné pro Maven pro použití při nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="5996c-178">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="5996c-179">V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které zadejte port pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5996c-179">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5996c-180">Nastavení můžete změnit číslo portu v tomto příkladu jsou nezbytné, pouze pokud měníte z výchozího portu.</span><span class="sxs-lookup"><span data-stu-id="5996c-180">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

## <a name="build-and-deploy-your-container-to-azure"></a><span data-ttu-id="5996c-181">Sestavení a nasazení vaší kontejneru do Azure</span><span class="sxs-lookup"><span data-stu-id="5996c-181">Build and deploy your container to Azure</span></span>

<span data-ttu-id="5996c-182">Jakmile nakonfigurujete všechna nastavení v předchozí části tohoto článku, jste připraveni k nasazení vašeho kontejneru na Azure.</span><span class="sxs-lookup"><span data-stu-id="5996c-182">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your container to Azure.</span></span> <span data-ttu-id="5996c-183">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5996c-183">To do so, use the following steps:</span></span>

1. <span data-ttu-id="5996c-184">Z příkazového řádku nebo okno terminálu, který jste dříve používali, znovu vytvořit na soubor JAR pomocí nástroje Maven Pokud jste provedli všechny změny *pom.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-184">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="5996c-185">Nasazení webové aplikace do Azure pomocí Maven; například:</span><span class="sxs-lookup"><span data-stu-id="5996c-185">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="5996c-186">Maven bude nasazení webové aplikace do Azure; Pokud webová aplikace už neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5996c-186">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5996c-187">Pokud oblast, kterou zadáte v `<region>` element vaše *pom.xml* soubor nemá dostatek serverů, které jsou k dispozici po spuštění nasazení, může dojít k chybě, podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5996c-187">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="5996c-188">V takovém případě můžete zadat jiné oblasti a znovu spusťte příkaz Maven pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="5996c-188">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="5996c-189">Pokud váš web nasazen, bude možné spravovat pomocí [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="5996c-189">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="5996c-190">Webové aplikace se objeví v **App Services**:</span><span class="sxs-lookup"><span data-stu-id="5996c-190">Your web app will be listed in **App Services**:</span></span>

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* <span data-ttu-id="5996c-192">A zobrazí se adresa URL pro webovou aplikaci v **přehled** pro webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="5996c-192">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5996c-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5996c-194">Next steps</span></span>

<span data-ttu-id="5996c-195">Další informace o různých technologií, které jsou popsané v tomto článku najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="5996c-195">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="5996c-196">[Maven modul plug-in pro Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="5996c-196">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="5996c-197">Přihlaste se k Azure z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5996c-197">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="5996c-198">Jak používat modul plug-in Maven pro webové aplikace Azure k nasazení aplikace pružiny spouštěcí do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5996c-198">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure App Service </span></span>](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="5996c-199">Vytvořit objekt služby Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5996c-199">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="5996c-200">Referenční příručka k nastavení maven</span><span class="sxs-lookup"><span data-stu-id="5996c-200">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="5996c-201">[Docker modul plug-in pro Maven]</span><span class="sxs-lookup"><span data-stu-id="5996c-201">[Docker plugin for Maven]</span></span>

<!-- URL List -->

<span data-ttu-id="5996c-202">[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="5996c-202">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="5996c-203">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="5996c-203">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="5996c-204">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="5996c-204">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="5996c-205">[Docker modul plug-in pro Maven]: https://github.com/spotify/docker-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="5996c-205">[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin</span></span>
<span data-ttu-id="5996c-206">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="5996c-206">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="5996c-207">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="5996c-207">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="5996c-208">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="5996c-208">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="5996c-209">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="5996c-209">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
[Spring Boot]: http://projects.spring.io/spring-boot/
<span data-ttu-id="5996c-210">[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="5996c-210">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
[Spring Framework]: https://spring.io/
<span data-ttu-id="5996c-211">[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="5996c-211">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
