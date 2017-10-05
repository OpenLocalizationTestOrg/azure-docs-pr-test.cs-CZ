---
title: "Nasazení webové aplikace pružiny spouštění v systému Linux v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede, když kroky nasazení spouštěcí pružiny aplikace jako webové aplikace v Microsoft Azure Linux."
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
ms.openlocfilehash: 5f0b470bd46cfeaf00b3092dbe9db507ed50f622
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a><span data-ttu-id="eb0cc-103">Nasazení aplikace pružiny spouštění v systému Linux v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="eb0cc-103">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>

<span data-ttu-id="eb0cc-104"> **[Pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-104">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="eb0cc-105">Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="eb0cc-106">**[Docker]**  je open-source řešení, které pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-106">**[Docker]** is open-source solutions that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="eb0cc-107">Tento kurz vás provede pomocí Docker pro vývoj a nasazení aplikace na hostitele platformy Linux v pružiny spouštěcí [Azure Container Service (ACS)].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-107">This tutorial walks you through using Docker to develop and deploy a Spring Boot application to a Linux host in the [Azure Container Service (ACS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb0cc-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eb0cc-108">Prerequisites</span></span>

<span data-ttu-id="eb0cc-109">Aby bylo možné provést kroky v tomto kurzu, musíte mít následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="eb0cc-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="eb0cc-111">[Rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="eb0cc-112">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="eb0cc-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="eb0cc-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-114">A [Git] client.</span></span>
* <span data-ttu-id="eb0cc-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="eb0cc-116">Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="eb0cc-117">Vytvoření spouštěcích pružiny ve webové aplikaci Docker Začínáme</span><span class="sxs-lookup"><span data-stu-id="eb0cc-117">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="eb0cc-118">Následující kroky vás provedou kroky, které jsou potřebné k vytvoření jednoduché webové aplikace pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-118">The following steps walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="eb0cc-119">Otevřete příkazový řádek a vytvořte místní adresář pro uložení aplikace, změnit do tohoto adresáře; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-119">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="eb0cc-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="eb0cc-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="eb0cc-121">Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře, který jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="eb0cc-122">Změňte adresář na dokončené projektu. například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-122">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="eb0cc-123">Sestavení na soubor JAR pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-123">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="eb0cc-124">Po vytvoření webové aplikace, změňte adresáře `target` adresáře, kde se nachází na soubor JAR a spuštění webové aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-124">Once the web app has been created, change directory to the `target` directory where the JAR file is located and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="eb0cc-125">Test webové aplikace tak, že přejde k němu místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="eb0cc-126">Například pokud máte curl k dispozici a je nakonfigurován server Tomcat ke spuštění na portu 80:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-126">For example, if you have curl available and you configured the Tomcat server to run on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="eb0cc-127">Měli byste vidět zobrazenou následující zprávu: **Docker Vítáme vás!**</span><span class="sxs-lookup"><span data-stu-id="eb0cc-127">You should see the following message displayed: **Hello Docker World!**</span></span>

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a><span data-ttu-id="eb0cc-129">Vytvoření registru kontejner Azure používat jako privátní registru Docker</span><span class="sxs-lookup"><span data-stu-id="eb0cc-129">Create an Azure Container Registry to use as a Private Docker Registry</span></span>

<span data-ttu-id="eb0cc-130">Následující postup vás provede procesem vytvoření registru kontejneru služby Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-130">The following steps walk you through using the Azure portal to create an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="eb0cc-131">Pokud chcete používat rozhraní příkazového řádku Azure místo portálu Azure, postupujte podle kroků v [vytvořit privátní registru kontejner Docker pomocí Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-131">If you want to use the Azure CLI instead of the Azure portal, follow the steps in [Create a private Docker container registry using the Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span></span>
>

1. <span data-ttu-id="eb0cc-132">Vyhledejte [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-132">Browse to the [Azure portal] and sign in.</span></span>

   <span data-ttu-id="eb0cc-133">Po přihlášení ke svému účtu na portálu Azure, můžete podle kroků v [privátní registru kontejner Docker pomocí portálu Azure vytvořit] článku, které jsou v následujících krocích pro sake z parafrázována vhodnosti.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-133">Once you have signed in to your account on the Azure portal, you can follow the steps in the [Create a private Docker container registry using the Azure portal] article, which are paraphrased in the following steps for the sake of expediency.</span></span>

1. <span data-ttu-id="eb0cc-134">Klikněte na ikonu nabídky **+ nový**, pak klikněte na tlačítko **kontejnery**a potom klikněte na **registru kontejner Azure**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-134">Click the menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Vytvořit nový kontejner registru Azure][AR01]

1. <span data-ttu-id="eb0cc-136">Pokud stránka informace o šabloně registru kontejner Azure se zobrazí, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-136">When the information page for the Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Vytvořit nový kontejner registru Azure][AR02]

1. <span data-ttu-id="eb0cc-138">Při **vytvořit kontejner registru** zobrazí se stránka, zadejte vaše **název registru** a **skupiny prostředků**, zvolte **povolit** pro **Uživatel s oprávněními správce**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-138">When the **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for the **Admin user**, and then click **Create**.</span></span>

   ![Konfigurace nastavení registru kontejner Azure][AR03]

1. <span data-ttu-id="eb0cc-140">Po vytvoření kontejneru registr, přejděte do kontejneru registr na portálu Azure a pak klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-140">Once your container registry has been created, navigate to your container registry in the Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="eb0cc-141">Poznamenejte si uživatelské jméno a heslo pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-141">Take note of the username and password for the next steps.</span></span>

   ![Azure přístupové klíče registru kontejneru][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a><span data-ttu-id="eb0cc-143">Konfigurace Maven použití klíče pro přístup k registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="eb0cc-143">Configure Maven to use your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="eb0cc-144">Přejděte do adresáře konfigurace pro instalaci Maven a otevřete *souborech settings.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-144">Navigate to the configuration directory for your Maven installation and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="eb0cc-145">Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto kurzu k `<servers>` kolekce *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-145">Add your Azure Container Registry access settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="eb0cc-146">Přejděte do adresáře dokončený projekt pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") a otevřete *pom.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-146">Navigate to the completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="eb0cc-147">Aktualizace `<properties>` kolekce *pom.xml* soubor s hodnotou server přihlášení pro vaši Azure kontejneru registru z předchozí části tohoto kurzu; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-147">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="eb0cc-148">Aktualizace `<plugins>` kolekce *pom.xml* souboru tak, aby `<plugin>` obsahuje přihlašovací adresu a registru název serveru pro váš Azure kontejneru registru z předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-148">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry from the previous section of this tutorial.</span></span> <span data-ttu-id="eb0cc-149">Například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-149">For example:</span></span>

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

1. <span data-ttu-id="eb0cc-150">Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte příkaz znovu sestavte aplikaci a nabízená kontejneru registru kontejneru systému Azure:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-150">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="eb0cc-151">Pokud vaše kontejner Docker nabízíte do Azure, můžete obdržet chybovou zprávu, která je podobná jednu z následujících i když vaše kontejner Docker byla úspěšně vytvořena:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-151">When you are pushing your Docker container to Azure, you may receive an error message that is similar to one of the following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="eb0cc-152">Pokud k tomu dojde, budete se muset přihlásit k účtu Azure z příkazového řádku Dockeru; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-152">If this happens, you may need to sign in to your Azure account from the Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="eb0cc-153">Potom můžete posouvat vašeho kontejneru z příkazového řádku; například:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-153">You can then push your container from the command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="eb0cc-154">Vytvoření webové aplikace v systému Linux v Azure App Service pomocí bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="eb0cc-154">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="eb0cc-155">Vyhledejte [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-155">Browse to the [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="eb0cc-156">Klikněte na ikonu nabídky **+ nový**, pak klikněte na tlačítko **Web + mobilní**a potom klikněte na **webové aplikace v systému Linux**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-156">Click the menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Vytvořit novou webovou aplikaci na portálu Azure][LX01]

1. <span data-ttu-id="eb0cc-158">Když **webové aplikace v systému Linux** se zobrazí stránka, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-158">When the **Web App on Linux** page is displayed, enter the following information:</span></span>

   <span data-ttu-id="eb0cc-159">a.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-159">a.</span></span> <span data-ttu-id="eb0cc-160">Zadejte jedinečný název **název aplikace**; například: "*wingtiptoyslinux*."</span><span class="sxs-lookup"><span data-stu-id="eb0cc-160">Enter a unique name for the **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="eb0cc-161">b.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-161">b.</span></span> <span data-ttu-id="eb0cc-162">Zvolte vaše **předplatné** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-162">Choose your **Subscription** from the drop-down list.</span></span>

   <span data-ttu-id="eb0cc-163">c.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-163">c.</span></span> <span data-ttu-id="eb0cc-164">Vybrat existující **skupiny prostředků**, nebo zadejte název a vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-164">Choose an existing **Resource Group**, or specify a name to create a new resource group.</span></span>

   <span data-ttu-id="eb0cc-165">d.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-165">d.</span></span> <span data-ttu-id="eb0cc-166">Klikněte na tlačítko **kontejneru konfigurace** a zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-166">Click **Configure container** and enter the following information:</span></span>

      * <span data-ttu-id="eb0cc-167">Zvolte **privátní registru**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-167">Choose **Private registry**.</span></span>

      * <span data-ttu-id="eb0cc-168">**Bitové kopie a volitelné značky**: Zadejte název kontejneru z dříve; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span><span class="sxs-lookup"><span data-stu-id="eb0cc-168">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="eb0cc-169">**Adresa URL serveru**: Zadejte svoji adresu URL registru z dříve; například: "*https://wingtiptoysregistry.azurecr.io*"</span><span class="sxs-lookup"><span data-stu-id="eb0cc-169">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="eb0cc-170">**Uživatelské jméno přihlášení** a **heslo**: Zadejte své přihlašovací údaje z vaší **přístupové klíče** který jste použili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-170">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="eb0cc-171">e.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-171">e.</span></span> <span data-ttu-id="eb0cc-172">Až zadáte všechny výše uvedené informace, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-172">Once you have entered all of the above information, click **OK**.</span></span>

   ![Konfigurovat nastavení webové aplikace][LX02]

1. <span data-ttu-id="eb0cc-174">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="eb0cc-175">Azure bude automaticky mapovat žádosti internetových embedded server Tomcat, která běží na standardní porty 80 nebo 8080.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-175">Azure will automatically map Internet requests to embedded Tomcat server that is running on the standard ports of 80 or 8080.</span></span> <span data-ttu-id="eb0cc-176">Ale pokud jste nakonfigurovali embedded server Tomcat, abyste mohli spustit na vlastní port, budete muset přidat proměnnou prostředí do webové aplikace, která definuje port pro embedded serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-176">However, if you configured your embedded Tomcat server to run on a custom port, you need to add an environment variable to your web app that defines the port for your embedded Tomcat server.</span></span> <span data-ttu-id="eb0cc-177">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-177">To do so, use the following steps:</span></span>
>
> 1. <span data-ttu-id="eb0cc-178">Vyhledejte [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-178">Browse to the [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="eb0cc-179">Klikněte na ikonu **App Services**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-179">Click the icon for **App Services**.</span></span> <span data-ttu-id="eb0cc-180">(Viz položku #1 na obrázku níže.)</span><span class="sxs-lookup"><span data-stu-id="eb0cc-180">(See item #1 in the image below.)</span></span>
>
> 3. <span data-ttu-id="eb0cc-181">Vyberte ze seznamu vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-181">Select your web app from the list.</span></span> <span data-ttu-id="eb0cc-182">(Položka #2 na obrázku níže.)</span><span class="sxs-lookup"><span data-stu-id="eb0cc-182">(Item #2 in the image below.)</span></span>
>
> 4. <span data-ttu-id="eb0cc-183">Klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-183">Click **Application Settings**.</span></span> <span data-ttu-id="eb0cc-184">(Položka #3 na obrázku níže.)</span><span class="sxs-lookup"><span data-stu-id="eb0cc-184">(Item #3 in the image below.)</span></span>
>
> 5. <span data-ttu-id="eb0cc-185">V **nastavení aplikace** přidejte novou proměnnou prostředí s názvem **PORT** a zadejte vlastní port číslo pro hodnotu.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-185">In the **App settings** section, add a new environment variable named **PORT** and enter your custom port number for the value.</span></span> <span data-ttu-id="eb0cc-186">(Položka #4 na obrázku níže.)</span><span class="sxs-lookup"><span data-stu-id="eb0cc-186">(Item #4 in the image below.)</span></span>
>
> 6. <span data-ttu-id="eb0cc-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-187">Click **Save**.</span></span> <span data-ttu-id="eb0cc-188">(Položka #5 na obrázku níže.)</span><span class="sxs-lookup"><span data-stu-id="eb0cc-188">(Item #5 in the image below.)</span></span>
>
> ![Ukládání vlastní číslo portu na portálu Azure][LX03]
>

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

## <a name="next-steps"></a><span data-ttu-id="eb0cc-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eb0cc-190">Next steps</span></span>

<span data-ttu-id="eb0cc-191">Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="eb0cc-191">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="eb0cc-192">Nasazení aplikace spouštěcí pružiny do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eb0cc-192">Deploy a Spring Boot Application to the Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="eb0cc-193">Nasazení aplikace spouštěcí pružiny na Cluster Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="eb0cc-193">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="eb0cc-194">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-194">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="eb0cc-195">Další informace o spouštění pružiny na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-195">For further details about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="eb0cc-196">Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu **pružiny Initializr** v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-196">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="eb0cc-197">Další informace o začátcích se vytvoření jednoduché aplikace pružiny spouštěcí najdete v části Initializr Spring v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-197">For more information about getting started with creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="eb0cc-198">Další příklady použití vlastních imagí Dockeru s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].</span><span class="sxs-lookup"><span data-stu-id="eb0cc-198">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

<span data-ttu-id="eb0cc-199">[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="eb0cc-199">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
<span data-ttu-id="eb0cc-200">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-200">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span></span>
<span data-ttu-id="eb0cc-201">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-201">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="eb0cc-202">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-202">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="eb0cc-203">[privátní registru kontejner Docker pomocí portálu Azure vytvořit]: /azure/container-registry/container-registry-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="eb0cc-203">[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal</span></span>
<span data-ttu-id="eb0cc-204">[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span><span class="sxs-lookup"><span data-stu-id="eb0cc-204">[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span></span>
<span data-ttu-id="eb0cc-205">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-205">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="eb0cc-206">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-206">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="eb0cc-207">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-207">[Git]: https://github.com/</span></span>
<span data-ttu-id="eb0cc-208">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-208">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="eb0cc-209">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-209">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="eb0cc-210">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-210">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="eb0cc-211">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-211">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="eb0cc-212">[pružiny spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-212">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="eb0cc-213">[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="eb0cc-213">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="eb0cc-214">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="eb0cc-214">[Spring Framework]: https://spring.io/</span></span>

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
