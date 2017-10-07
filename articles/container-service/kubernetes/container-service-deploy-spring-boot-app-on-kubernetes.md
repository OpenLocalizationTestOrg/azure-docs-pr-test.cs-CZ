---
title: "aaaDeploy pružiny spouštění aplikace na Kubernetes v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede, když hello kroky toodeploy pružiny spuštění aplikace v clusteru s podporou Kubernetes v Microsoft Azure."
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
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a><span data-ttu-id="91eb6-103">Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="91eb6-103">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>

<span data-ttu-id="91eb6-104">Hello  **[pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="91eb6-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="91eb6-105">Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny tooget rychle začít.</span><span class="sxs-lookup"><span data-stu-id="91eb6-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="91eb6-106">**[Kubernetes]**  a  **[Docker]**  jsou open-source řešení, která pomáhají vývojáři automatizovat, hello nasazení, škálování a správu svých aplikací běžících v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="91eb6-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate hello deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="91eb6-107">Tento kurz vás provede, když kombinace těchto dvou oblíbených, open-source technologie toodevelop a nasaďte tooMicrosoft pružiny spouštěcí aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-107">This tutorial walks you though combining these two popular, open-source technologies toodevelop and deploy a Spring Boot application tooMicrosoft Azure.</span></span> <span data-ttu-id="91eb6-108">Přesněji řečeno, použijete  *[pružiny spouštěcí]*  pro vývoj aplikací  *[Kubernetes]*  pro nasazení kontejneru a hello [Azure Container Service (ACS)] toohost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="91eb6-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and hello [Azure Container Service (ACS)] toohost your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="91eb6-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="91eb6-109">Prerequisites</span></span>

* <span data-ttu-id="91eb6-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="91eb6-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="91eb6-111">Hello [rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="91eb6-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="91eb6-112">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="91eb6-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="91eb6-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="91eb6-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="91eb6-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="91eb6-114">A [Git] client.</span></span>
* <span data-ttu-id="91eb6-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="91eb6-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91eb6-116">Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="91eb6-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="91eb6-117">Vytvoření hello pružiny spouštěcí na Docker Začínáme webové aplikace</span><span class="sxs-lookup"><span data-stu-id="91eb6-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="91eb6-118">Hello následující kroky vás provede procesem vytváření webové aplikace pružiny spouštěcí a místní testování.</span><span class="sxs-lookup"><span data-stu-id="91eb6-118">hello following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="91eb6-119">Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="91eb6-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="91eb6-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="91eb6-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="91eb6-121">Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="91eb6-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="91eb6-122">Změňte adresář toohello dokončení projektu.</span><span class="sxs-lookup"><span data-stu-id="91eb6-122">Change directory toohello completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="91eb6-123">Používejte Maven toobuild a spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91eb6-123">Use Maven toobuild and run hello sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="91eb6-124">Testování hello webové aplikace procházením toohttp://localhost:8080 nebo s následující hello `curl` příkaz:</span><span class="sxs-lookup"><span data-stu-id="91eb6-124">Test hello web app by browsing toohttp://localhost:8080, or with hello following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="91eb6-125">Měli byste vidět hello následující zpráva: **Hello, World Docker**</span><span class="sxs-lookup"><span data-stu-id="91eb6-125">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="91eb6-127">Vytvoření registru kontejneru služby Azure pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="91eb6-127">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="91eb6-128">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="91eb6-128">Open a command prompt.</span></span>

1. <span data-ttu-id="91eb6-129">Přihlaste se tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="91eb6-129">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="91eb6-130">Vytvořte skupinu prostředků pro hello použité v tomto kurzu prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-130">Create a resource group for hello Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="91eb6-131">Vytvořte kontejner privátní Azure registru ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="91eb6-131">Create a private Azure container registry in hello resource group.</span></span> <span data-ttu-id="91eb6-132">kurz Hello doručí hello ukázkové aplikace jako soubor Docker image toothis registru v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="91eb6-132">hello tutorial pushes hello sample app as a Docker image toothis registry in later steps.</span></span> <span data-ttu-id="91eb6-133">Nahraďte `wingtiptoysregistry` s jedinečným názvem pro vaše registru.</span><span class="sxs-lookup"><span data-stu-id="91eb6-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a><span data-ttu-id="91eb6-134">Push vaší aplikace toohello kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="91eb6-134">Push your app toohello container registry</span></span>

1. <span data-ttu-id="91eb6-135">Přejděte toohello konfiguračního adresáře pro instalaci Maven (výchozí ~/.m2/ nebo C:\Users\username\.m2) a otevřené hello *souborech settings.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="91eb6-135">Navigate toohello configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="91eb6-136">Načtení hello hesla pro vaše kontejneru registru z hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-136">Retrieve hello password for your container registry from hello Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="91eb6-137">Přidání vaší registru kontejner Azure id a heslo tooa nové `<server>` kolekce v hello *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="91eb6-137">Add your Azure Container Registry id and password tooa new `<server>` collection in hello *settings.xml* file.</span></span>
<span data-ttu-id="91eb6-138">Hello `id` a `username` jsou hello název registru hello.</span><span class="sxs-lookup"><span data-stu-id="91eb6-138">hello `id` and `username` are hello name of hello registry.</span></span> <span data-ttu-id="91eb6-139">Použití hello `password` hodnotu z předchozí příkaz hello (bez uvozovek).</span><span class="sxs-lookup"><span data-stu-id="91eb6-139">Use hello `password` value from hello previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="91eb6-140">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (například "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker / dokončení*") a otevřete hello *pom.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="91eb6-140">Navigate toohello completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="91eb6-141">Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaše registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-141">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="91eb6-142">Aktualizace hello `<plugins>` kolekce v hello *pom.xml* souboru, který hello `<plugin>` obsahuje hello přihlašovací adresu a registru název serveru pro váš registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-142">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="91eb6-143">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toobuild hello Docker kontejneru a nabízených hello image toohello registru hello:</span><span class="sxs-lookup"><span data-stu-id="91eb6-143">Navigate toohello completed project directory for your Spring Boot application and run hello following command toobuild hello Docker container and push hello image toohello registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="91eb6-144">Zobrazí chybovou zprávu, která je podobné tooone hello následující při Maven nabízených oznámení tooAzure hello bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="91eb6-144">You may receive an error message that is similar tooone of hello following when Maven pushes hello image tooAzure:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="91eb6-145">Pokud se tato chyba, přihlaste se tooAzure z příkazového řádku Dockeru hello.</span><span class="sxs-lookup"><span data-stu-id="91eb6-145">If you get this error, log in tooAzure from hello Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="91eb6-146">Potom push vaší kontejneru:</span><span class="sxs-lookup"><span data-stu-id="91eb6-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a><span data-ttu-id="91eb6-147">Vytvoření clusteru s podporou Kubernetes na ACS pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="91eb6-147">Create a Kubernetes Cluster on ACS using hello Azure CLI</span></span>

1. <span data-ttu-id="91eb6-148">Vytvoření clusteru Kubernetes v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="91eb6-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="91eb6-149">Hello následující příkaz vytvoří *kubernetes* clusteru v hello *Northwind kubernetes* prostředků skupiny s *Northwind containerservice* jako hello cluster název, a *Northwind kubernetes* jako předpona DNS hello:</span><span class="sxs-lookup"><span data-stu-id="91eb6-149">hello following command creates a *kubernetes* cluster in hello *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as hello cluster name, and *wingtiptoys-kubernetes* as hello DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="91eb6-150">Tento příkaz může chvíli trvat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="91eb6-150">This command may take a while toocomplete.</span></span>

1. <span data-ttu-id="91eb6-151">Nainstalujte `kubectl` pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-151">Install `kubectl` using hello Azure CLI.</span></span> <span data-ttu-id="91eb6-152">Linux uživatelé mohou mít tooprefix tento příkaz s `sudo` vzhledem k tomu, že nasadí hello Kubernetes CLI příliš`/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="91eb6-152">Linux users may have tooprefix this command with `sudo` since it deploys hello Kubernetes CLI too`/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="91eb6-153">Stáhněte si informace o konfiguraci clusteru hello tak můžete spravovat cluster z hello Kubernetes webové rozhraní a `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="91eb6-153">Download hello cluster configuration information so you can manage your cluster from hello Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a><span data-ttu-id="91eb6-154">Nasazení clusteru Kubernetes tooyour image hello</span><span class="sxs-lookup"><span data-stu-id="91eb6-154">Deploy hello image tooyour Kubernetes cluster</span></span>

<span data-ttu-id="91eb6-155">V tomto kurzu nasadí hello aplikace pomocí `kubectl`, pak umožňují tooexplore hello nasazení prostřednictvím hello Kubernetes webového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="91eb6-155">This tutorial deploys hello app using `kubectl`, then allow you tooexplore hello deployment through hello Kubernetes web interface.</span></span>

### <a name="deploy-with-hello-kubernetes-web-interface"></a><span data-ttu-id="91eb6-156">Nasazení s hello Kubernetes webové rozhraní</span><span class="sxs-lookup"><span data-stu-id="91eb6-156">Deploy with hello Kubernetes web interface</span></span>

1. <span data-ttu-id="91eb6-157">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="91eb6-157">Open a command prompt.</span></span>

1. <span data-ttu-id="91eb6-158">Otevřít web hello konfigurace pro váš cluster Kubernetes ve výchozím prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="91eb6-158">Open hello configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="91eb6-159">Když hello Kubernetes konfigurace webu se otevře v prohlížeči, klikněte na odkaz hello příliš**nasadit kontejnerizované aplikaci**:</span><span class="sxs-lookup"><span data-stu-id="91eb6-159">When hello Kubernetes configuration website opens in your browser, click hello link too**deploy a containerized app**:</span></span>

   ![Kubernetes konfigurace webu][KB01]

1. <span data-ttu-id="91eb6-161">Když hello **nasadit kontejnerizované aplikaci** se zobrazí stránka, zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="91eb6-161">When hello **Deploy a containerized app** page is displayed, specify hello following options:</span></span>

   <span data-ttu-id="91eb6-162">a.</span><span class="sxs-lookup"><span data-stu-id="91eb6-162">a.</span></span> <span data-ttu-id="91eb6-163">Vyberte **zadejte níže uvedené podrobnosti o aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="91eb6-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="91eb6-164">b.</span><span class="sxs-lookup"><span data-stu-id="91eb6-164">b.</span></span> <span data-ttu-id="91eb6-165">Zadejte název aplikace pružiny spouštěcí pro hello **název aplikace**; například: "*gs pružiny spouštěcí docker*".</span><span class="sxs-lookup"><span data-stu-id="91eb6-165">Enter your Spring Boot application name for hello **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="91eb6-166">c.</span><span class="sxs-lookup"><span data-stu-id="91eb6-166">c.</span></span> <span data-ttu-id="91eb6-167">Zadejte přihlašovací serveru a kontejneru bitové kopie z dříve hello **kontejneru image**; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="91eb6-167">Enter your login server and container image from earlier for hello **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="91eb6-168">d.</span><span class="sxs-lookup"><span data-stu-id="91eb6-168">d.</span></span> <span data-ttu-id="91eb6-169">Zvolte **externí** pro hello **služby**.</span><span class="sxs-lookup"><span data-stu-id="91eb6-169">Choose **External** for hello **Service**.</span></span>

   <span data-ttu-id="91eb6-170">e.</span><span class="sxs-lookup"><span data-stu-id="91eb6-170">e.</span></span> <span data-ttu-id="91eb6-171">Zadejte vaše externí i interní portů v hello **Port** a **cíle port** textových polí.</span><span class="sxs-lookup"><span data-stu-id="91eb6-171">Specify your external and internal ports in hello **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes konfigurace webu][KB02]


1. <span data-ttu-id="91eb6-173">Klikněte na tlačítko **nasadit** toodeploy hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="91eb6-173">Click **Deploy** toodeploy hello container.</span></span>

   ![Nasazení kontejneru][KB05]

1. <span data-ttu-id="91eb6-175">Po nasazení vaší aplikace, zobrazí se aplikace pružiny spouštěcí uvedené v části **služby**.</span><span class="sxs-lookup"><span data-stu-id="91eb6-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes služby][KB06]

1. <span data-ttu-id="91eb6-177">Pokud kliknete na odkaz hello **externí koncové body**, uvidíte pružiny spouštěcí aplikace spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="91eb6-177">If you click hello link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes služby][KB07]

   ![Procházet ukázkovou aplikaci v Azure][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="91eb6-180">Nasazení s kubectl</span><span class="sxs-lookup"><span data-stu-id="91eb6-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="91eb6-181">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="91eb6-181">Open a command prompt.</span></span>

1. <span data-ttu-id="91eb6-182">Spusťte vašeho kontejneru v clusteru Kubernetes hello pomocí hello `kubectl run` příkaz.</span><span class="sxs-lookup"><span data-stu-id="91eb6-182">Run your container in hello Kubernetes cluster by using hello `kubectl run` command.</span></span> <span data-ttu-id="91eb6-183">Zadejte název služby pro aplikaci v Kubernetes a název hello úplnou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="91eb6-183">Give a service name for your app in Kubernetes and hello full image name.</span></span> <span data-ttu-id="91eb6-184">Například:</span><span class="sxs-lookup"><span data-stu-id="91eb6-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="91eb6-185">V tomto příkazu:</span><span class="sxs-lookup"><span data-stu-id="91eb6-185">In this command:</span></span>

   * <span data-ttu-id="91eb6-186">název kontejneru Hello `gs-spring-boot-docker` je zadán ihned po hello `run` příkaz</span><span class="sxs-lookup"><span data-stu-id="91eb6-186">hello container name `gs-spring-boot-docker` is specified immediately after hello `run` command</span></span>

   * <span data-ttu-id="91eb6-187">Hello `--image` parametr určuje hello kombinaci přihlášení na server a název bitové kopie jako`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="91eb6-187">hello `--image` parameter specifies hello combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="91eb6-188">Vystavení clusteru Kubernetes externě pomocí hello `kubectl expose` příkaz.</span><span class="sxs-lookup"><span data-stu-id="91eb6-188">Expose your Kubernetes cluster externally by using hello `kubectl expose` command.</span></span> <span data-ttu-id="91eb6-189">Zadejte název vaší služby, hello veřejné TCP port používaný tooaccess hello aplikace a hello interní cílový port, který aplikace naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="91eb6-189">Specify your service name, hello public-facing TCP port used tooaccess hello app, and hello internal target port your app listens on.</span></span> <span data-ttu-id="91eb6-190">Například:</span><span class="sxs-lookup"><span data-stu-id="91eb6-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="91eb6-191">V tomto příkazu:</span><span class="sxs-lookup"><span data-stu-id="91eb6-191">In this command:</span></span>

   * <span data-ttu-id="91eb6-192">název kontejneru Hello `gs-spring-boot-docker` je zadán ihned po hello `expose deployment` příkaz</span><span class="sxs-lookup"><span data-stu-id="91eb6-192">hello container name `gs-spring-boot-docker` is specified immediately after hello `expose deployment` command</span></span>

   * <span data-ttu-id="91eb6-193">Hello `--type` parametr určuje, že cluster hello používá nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="91eb6-193">hello `--type` parameter specifies that hello cluster uses load balancer</span></span>

   * <span data-ttu-id="91eb6-194">Hello `--port` parametr určuje hello veřejné TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="91eb6-194">hello `--port` parameter specifies hello public-facing TCP port of 80.</span></span> <span data-ttu-id="91eb6-195">Máte přístup k aplikaci hello na tento port.</span><span class="sxs-lookup"><span data-stu-id="91eb6-195">You access hello app on this port.</span></span>

   * <span data-ttu-id="91eb6-196">Hello `--target-port` parametr určuje hello interní TCP port 8080.</span><span class="sxs-lookup"><span data-stu-id="91eb6-196">hello `--target-port` parameter specifies hello internal TCP port of 8080.</span></span> <span data-ttu-id="91eb6-197">Nástroj pro vyrovnávání zatížení Hello předává požadavky tooyour aplikace na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="91eb6-197">hello load balancer forwards requests tooyour app on this port.</span></span>

1. <span data-ttu-id="91eb6-198">Po nasazení aplikace hello toohello clusteru dotaz hello externí IP adresu a otevřete ve webovém prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="91eb6-198">Once hello app is deployed toohello cluster, query hello external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Procházet ukázkovou aplikaci v Azure][SB02]


## <a name="next-steps"></a><span data-ttu-id="91eb6-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91eb6-200">Next steps</span></span>

<span data-ttu-id="91eb6-201">Další informace o používání spouštěcí Spring v Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="91eb6-201">For more information about using Spring Boot on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="91eb6-202">Nasazení aplikace spouštěcí pružiny toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="91eb6-202">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="91eb6-203">Nasazení aplikace pružiny spouštění v systému Linux v hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="91eb6-203">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="91eb6-204">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="91eb6-204">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="91eb6-205">Další informace o hello pružiny spouštěcí na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].</span><span class="sxs-lookup"><span data-stu-id="91eb6-205">For more information about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="91eb6-206">Hello následující odkazy obsahují další informace o vytváření aplikací pro spouštěcí pružiny:</span><span class="sxs-lookup"><span data-stu-id="91eb6-206">hello following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="91eb6-207">Další informace o vytvoření jednoduché aplikace pružiny spouštěcí najdete v části hello Spring Initializr v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="91eb6-207">For more information about creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="91eb6-208">Hello následující odkazy obsahují další informace o používání Kubernetes s Azure:</span><span class="sxs-lookup"><span data-stu-id="91eb6-208">hello following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="91eb6-209">Začínáme s Kubernetes cluster Container Service</span><span class="sxs-lookup"><span data-stu-id="91eb6-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="91eb6-210">Pomocí hello Kubernetes webové uživatelské rozhraní s Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="91eb6-210">Using hello Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="91eb6-211">Další informace o používání rozhraní příkazového řádku Kubernetes je k dispozici v hello **kubectl** v uživatelské příručce <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="91eb6-211">More information about using Kubernetes command-line interface is available in hello **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="91eb6-212">Hello Kubernetes web má několik články, které popisují pomocí bitové kopie v privátní registrech:</span><span class="sxs-lookup"><span data-stu-id="91eb6-212">hello Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="91eb6-213">[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]</span><span class="sxs-lookup"><span data-stu-id="91eb6-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="91eb6-214">[Obory názvů]</span><span class="sxs-lookup"><span data-stu-id="91eb6-214">[Namespaces]</span></span>
* <span data-ttu-id="91eb6-215">[Stahování bitovou kopii z privátní registru]</span><span class="sxs-lookup"><span data-stu-id="91eb6-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="91eb6-216">Další příklady jak toouse vlastní Docker obrázků s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].</span><span class="sxs-lookup"><span data-stu-id="91eb6-216">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[pružiny Framework]: https://spring.io/
[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Obory názvů]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Stahování bitovou kopii z privátní registru]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
