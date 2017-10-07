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
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a><span data-ttu-id="ea16e-103">Nasazení aplikace pružiny spouštění v systému Linux v hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="ea16e-103">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>

<span data-ttu-id="ea16e-104">Hello  **[pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="ea16e-104">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="ea16e-105">Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="ea16e-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="ea16e-106">**[Docker]**  je open-source řešení, které pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="ea16e-106">**[Docker]** is open-source solutions that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="ea16e-107">Tento kurz vás provede pomocí Docker toodevelop a nasadit spouštěcí pružiny aplikace tooa Linux hostitele v hello [Azure Container Service (ACS)].</span><span class="sxs-lookup"><span data-stu-id="ea16e-107">This tutorial walks you through using Docker toodevelop and deploy a Spring Boot application tooa Linux host in hello [Azure Container Service (ACS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea16e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ea16e-108">Prerequisites</span></span>

<span data-ttu-id="ea16e-109">Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="ea16e-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="ea16e-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="ea16e-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="ea16e-111">Hello [rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="ea16e-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="ea16e-112">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="ea16e-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="ea16e-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="ea16e-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="ea16e-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="ea16e-114">A [Git] client.</span></span>
* <span data-ttu-id="ea16e-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="ea16e-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ea16e-116">Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="ea16e-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="ea16e-117">Vytvoření hello pružiny spouštěcí na Docker Začínáme webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ea16e-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="ea16e-118">Hello následující kroky vás provedou hello kroky, které jsou požadované toocreate jednoduchou webovou aplikaci pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="ea16e-118">hello following steps walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="ea16e-119">Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="ea16e-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="ea16e-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="ea16e-121">Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="ea16e-122">Změnit adresář toohello dokončit projekt; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-122">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="ea16e-123">Vytvořit soubor JAR hello pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-123">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="ea16e-124">Po vytvoření webové aplikace hello změnit adresář toohello `target` adresáře, kde se nachází soubor JAR hello a spusťte hello webové aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-124">Once hello web app has been created, change directory toohello `target` directory where hello JAR file is located and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="ea16e-125">Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ea16e-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="ea16e-126">Například pokud máte curl k dispozici a je nakonfigurován hello Tomcat server toorun na portu 80:</span><span class="sxs-lookup"><span data-stu-id="ea16e-126">For example, if you have curl available and you configured hello Tomcat server toorun on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="ea16e-127">Měli byste vidět hello následující zpráva: **Docker Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="ea16e-127">You should see hello following message displayed: **Hello Docker World!**</span></span>

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a><span data-ttu-id="ea16e-129">Vytvoření služby Azure kontejneru registru toouse jako privátní registru Docker</span><span class="sxs-lookup"><span data-stu-id="ea16e-129">Create an Azure Container Registry toouse as a Private Docker Registry</span></span>

<span data-ttu-id="ea16e-130">Hello následující kroky vás provedou pomocí hello Azure portálu toocreate registru kontejneru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ea16e-130">hello following steps walk you through using hello Azure portal toocreate an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ea16e-131">Pokud chcete toouse hello rozhraní příkazového řádku Azure místo hello portálu Azure, postupujte podle kroků hello v [vytvořit privátní registru kontejner Docker pomocí hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ea16e-131">If you want toouse hello Azure CLI instead of hello Azure portal, follow hello steps in [Create a private Docker container registry using hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span></span>
>

1. <span data-ttu-id="ea16e-132">Procházet toohello [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ea16e-132">Browse toohello [Azure portal] and sign in.</span></span>

   <span data-ttu-id="ea16e-133">Po přihlášení tooyour účet na hello portálu Azure můžete provést kroky hello v hello [vytvořit privátní registru kontejner Docker pomocí portálu Azure hello] článku, které jsou parafrázována v hello následující kroky pro hello zájmu vhodnosti.</span><span class="sxs-lookup"><span data-stu-id="ea16e-133">Once you have signed in tooyour account on hello Azure portal, you can follow hello steps in hello [Create a private Docker container registry using hello Azure portal] article, which are paraphrased in hello following steps for hello sake of expediency.</span></span>

1. <span data-ttu-id="ea16e-134">Klikněte na ikonu nabídky hello **+ nový**, pak klikněte na tlačítko **kontejnery**a potom klikněte na **registru kontejner Azure**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-134">Click hello menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Vytvořit nový kontejner registru Azure][AR01]

1. <span data-ttu-id="ea16e-136">Při zobrazení stránky hello informace pro šablonu Azure kontejneru registru hello, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-136">When hello information page for hello Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Vytvořit nový kontejner registru Azure][AR02]

1. <span data-ttu-id="ea16e-138">Když hello **vytvořit kontejner registru** zobrazí se stránka, zadejte vaše **název registru** a **skupiny prostředků**, zvolte **povolit** pro Hello **uživatel s oprávněními správce**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-138">When hello **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for hello **Admin user**, and then click **Create**.</span></span>

   ![Konfigurace nastavení registru kontejner Azure][AR03]

1. <span data-ttu-id="ea16e-140">Po vytvoření kontejneru registr, přejděte tooyour kontejneru registru hello portál Azure a pak klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-140">Once your container registry has been created, navigate tooyour container registry in hello Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="ea16e-141">Poznamenejte si hello uživatelské jméno a heslo pro další kroky hello.</span><span class="sxs-lookup"><span data-stu-id="ea16e-141">Take note of hello username and password for hello next steps.</span></span>

   ![Azure přístupové klíče registru kontejneru][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a><span data-ttu-id="ea16e-143">Konfigurace Maven toouse klíče pro přístup k registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="ea16e-143">Configure Maven toouse your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="ea16e-144">Přejděte toohello konfiguračního adresáře pro instalaci Maven a otevřete hello *souborech settings.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="ea16e-144">Navigate toohello configuration directory for your Maven installation and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="ea16e-145">Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto kurzu toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-145">Add your Azure Container Registry access settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="ea16e-146">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (například: "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker / dokončení*") a otevřete hello *pom.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="ea16e-146">Navigate toohello completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="ea16e-147">Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaši Azure kontejneru registru z předchozí části hello tohoto kurzu; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-147">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="ea16e-148">Aktualizace hello `<plugins>` kolekce v hello *pom.xml* souboru, který hello `<plugin>` obsahuje hello přihlašovací adresu a registru název serveru pro váš registru kontejner Azure z hello předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ea16e-148">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry from hello previous section of this tutorial.</span></span> <span data-ttu-id="ea16e-149">Například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-149">For example:</span></span>

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

1. <span data-ttu-id="ea16e-150">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toorebuild hello aplikace hello a push hello kontejneru tooyour registru kontejner Azure:</span><span class="sxs-lookup"><span data-stu-id="ea16e-150">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="ea16e-151">Pokud nabízíte vaší tooAzure kontejner Docker, může se zobrazit chybová zpráva, která je podobné tooone hello následujících to i v případě, že vaše kontejner Docker byla úspěšně vytvořena.:</span><span class="sxs-lookup"><span data-stu-id="ea16e-151">When you are pushing your Docker container tooAzure, you may receive an error message that is similar tooone of hello following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="ea16e-152">Pokud k tomu dojde, může být nutné toosign v tooyour účet Azure z příkazového řádku Dockeru hello; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-152">If this happens, you may need toosign in tooyour Azure account from hello Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="ea16e-153">Potom můžete posouvat vašeho kontejneru z příkazového řádku hello; například:</span><span class="sxs-lookup"><span data-stu-id="ea16e-153">You can then push your container from hello command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="ea16e-154">Vytvoření webové aplikace v systému Linux v Azure App Service pomocí bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="ea16e-154">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="ea16e-155">Procházet toohello [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ea16e-155">Browse toohello [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="ea16e-156">Klikněte na ikonu nabídky hello **+ nový**, pak klikněte na tlačítko **Web + mobilní**a potom klikněte na **webové aplikace v systému Linux**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-156">Click hello menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Vytvořit novou webovou aplikaci v hello portálu Azure][LX01]

1. <span data-ttu-id="ea16e-158">Když hello **webové aplikace v systému Linux** se zobrazí stránka, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ea16e-158">When hello **Web App on Linux** page is displayed, enter hello following information:</span></span>

   <span data-ttu-id="ea16e-159">a.</span><span class="sxs-lookup"><span data-stu-id="ea16e-159">a.</span></span> <span data-ttu-id="ea16e-160">Zadejte jedinečný název pro hello **název aplikace**; například: "*wingtiptoyslinux*."</span><span class="sxs-lookup"><span data-stu-id="ea16e-160">Enter a unique name for hello **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="ea16e-161">b.</span><span class="sxs-lookup"><span data-stu-id="ea16e-161">b.</span></span> <span data-ttu-id="ea16e-162">Zvolte vaše **předplatné** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ea16e-162">Choose your **Subscription** from hello drop-down list.</span></span>

   <span data-ttu-id="ea16e-163">c.</span><span class="sxs-lookup"><span data-stu-id="ea16e-163">c.</span></span> <span data-ttu-id="ea16e-164">Vybrat existující **skupiny prostředků**, nebo zadejte název toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ea16e-164">Choose an existing **Resource Group**, or specify a name toocreate a new resource group.</span></span>

   <span data-ttu-id="ea16e-165">d.</span><span class="sxs-lookup"><span data-stu-id="ea16e-165">d.</span></span> <span data-ttu-id="ea16e-166">Klikněte na tlačítko **kontejneru konfigurace** a zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ea16e-166">Click **Configure container** and enter hello following information:</span></span>

      * <span data-ttu-id="ea16e-167">Zvolte **privátní registru**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-167">Choose **Private registry**.</span></span>

      * <span data-ttu-id="ea16e-168">**Bitové kopie a volitelné značky**: Zadejte název kontejneru z dříve; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span><span class="sxs-lookup"><span data-stu-id="ea16e-168">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="ea16e-169">**Adresa URL serveru**: Zadejte svoji adresu URL registru z dříve; například: "*https://wingtiptoysregistry.azurecr.io*"</span><span class="sxs-lookup"><span data-stu-id="ea16e-169">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="ea16e-170">**Uživatelské jméno přihlášení** a **heslo**: Zadejte své přihlašovací údaje z vaší **přístupové klíče** který jste použili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="ea16e-170">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="ea16e-171">e.</span><span class="sxs-lookup"><span data-stu-id="ea16e-171">e.</span></span> <span data-ttu-id="ea16e-172">Po zadání všech hello výše informace, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-172">Once you have entered all of hello above information, click **OK**.</span></span>

   ![Konfigurovat nastavení webové aplikace][LX02]

1. <span data-ttu-id="ea16e-174">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="ea16e-175">Azure bude automaticky mapovat Internet požadavky tooembedded Tomcat serveru, který běží na standardní porty hello 80 nebo 8080.</span><span class="sxs-lookup"><span data-stu-id="ea16e-175">Azure will automatically map Internet requests tooembedded Tomcat server that is running on hello standard ports of 80 or 8080.</span></span> <span data-ttu-id="ea16e-176">Pokud jste nakonfigurovali vaší embedded toorun server Tomcat na vlastní port, ale musíte tooadd prostředí proměnné tooyour webovou aplikaci, která definuje hello port pro embedded serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea16e-176">However, if you configured your embedded Tomcat server toorun on a custom port, you need tooadd an environment variable tooyour web app that defines hello port for your embedded Tomcat server.</span></span> <span data-ttu-id="ea16e-177">toodo tedy použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ea16e-177">toodo so, use hello following steps:</span></span>
>
> 1. <span data-ttu-id="ea16e-178">Procházet toohello [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ea16e-178">Browse toohello [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="ea16e-179">Klikněte na ikonu hello **App Services**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-179">Click hello icon for **App Services**.</span></span> <span data-ttu-id="ea16e-180">(Viz položku #1 hello obrázek níže).</span><span class="sxs-lookup"><span data-stu-id="ea16e-180">(See item #1 in hello image below.)</span></span>
>
> 3. <span data-ttu-id="ea16e-181">Vyberte ze seznamu hello vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea16e-181">Select your web app from hello list.</span></span> <span data-ttu-id="ea16e-182">(Položka #2 hello obrázek níže.)</span><span class="sxs-lookup"><span data-stu-id="ea16e-182">(Item #2 in hello image below.)</span></span>
>
> 4. <span data-ttu-id="ea16e-183">Klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-183">Click **Application Settings**.</span></span> <span data-ttu-id="ea16e-184">(Položka #3 hello obrázek níže.)</span><span class="sxs-lookup"><span data-stu-id="ea16e-184">(Item #3 in hello image below.)</span></span>
>
> 5. <span data-ttu-id="ea16e-185">V hello **nastavení aplikace** přidejte novou proměnnou prostředí s názvem **PORT** a zadejte vlastní port číslo pro hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="ea16e-185">In hello **App settings** section, add a new environment variable named **PORT** and enter your custom port number for hello value.</span></span> <span data-ttu-id="ea16e-186">(Položka #4 v následující obrázek hello.)</span><span class="sxs-lookup"><span data-stu-id="ea16e-186">(Item #4 in hello image below.)</span></span>
>
> 6. <span data-ttu-id="ea16e-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ea16e-187">Click **Save**.</span></span> <span data-ttu-id="ea16e-188">(Položka #5 hello obrázek níže.)</span><span class="sxs-lookup"><span data-stu-id="ea16e-188">(Item #5 in hello image below.)</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="ea16e-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea16e-190">Next steps</span></span>

<span data-ttu-id="ea16e-191">Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ea16e-191">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="ea16e-192">Nasazení aplikace spouštěcí pružiny toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ea16e-192">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="ea16e-193">Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="ea16e-193">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="ea16e-194">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="ea16e-194">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="ea16e-195">Další podrobnosti o hello pružiny spouštěcí na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].</span><span class="sxs-lookup"><span data-stu-id="ea16e-195">For further details about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="ea16e-196">Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu hello **pružiny Initializr** v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="ea16e-196">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="ea16e-197">Další informace o začátcích se vytvoření jednoduché aplikace pružiny spouštěcí najdete v části hello Spring Initializr v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="ea16e-197">For more information about getting started with creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="ea16e-198">Další příklady jak toouse vlastní Docker obrázků s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].</span><span class="sxs-lookup"><span data-stu-id="ea16e-198">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

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
