---
title: "Nasazení aplikace spouštěcí pružiny na Kubernetes v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede když kroky k nasazení aplikace spouštěcí Spring v Kubernetes clusteru v Microsoft Azure."
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
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="dbb1b-103">Nasazení aplikace Spring Boot Application v clusteru Kubernetes ve službě Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="dbb1b-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="dbb1b-104"> **[Pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-104">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="dbb1b-105">Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny rychle začít.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="dbb1b-106">**[Kubernetes]**  a  **[Docker]**  jsou open-source řešení, která pomáhají vývojáři automatizovat nasazení, škálování a správu jejich aplikace běžící v kontejnery.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="dbb1b-107">Tento kurz vás provede, když kombinaci těchto dvou oblíbených, open-source technologií pro vývoj a nasazení spouštěcí pružiny aplikace do služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-107">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="dbb1b-108">Přesněji řečeno, použijete  *[pružiny spouštěcí]*  pro vývoj aplikací  *[Kubernetes]*  pro kontejner nasazení a [ Azure Container Service (ACS)] kvůli hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (ACS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="dbb1b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dbb1b-109">Prerequisites</span></span>

* <span data-ttu-id="dbb1b-110">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="dbb1b-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="dbb1b-111">[Rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="dbb1b-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="dbb1b-112">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="dbb1b-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="dbb1b-113">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="dbb1b-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="dbb1b-114">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-114">A [Git] client.</span></span>
* <span data-ttu-id="dbb1b-115">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="dbb1b-116">Z důvodu požadavků na virtualizace tohoto kurzu nelze na virtuálním počítači; podle kroků v tomto článku fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="dbb1b-117">Vytvoření spouštěcích pružiny ve webové aplikaci Docker Začínáme</span><span class="sxs-lookup"><span data-stu-id="dbb1b-117">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="dbb1b-118">Následující postup vás provede procesem vytváření webové aplikace pružiny spouštěcí a místní testování.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-118">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="dbb1b-119">Otevřete příkazový řádek a vytvořte místní adresář pro uložení aplikace, změnit do tohoto adresáře; například:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-119">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="dbb1b-120">--nebo--</span><span class="sxs-lookup"><span data-stu-id="dbb1b-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="dbb1b-121">Klon [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="dbb1b-122">Změňte adresář na dokončený projekt.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-122">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="dbb1b-123">Používání Maven k sestavení a spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-123">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="dbb1b-124">Test webové aplikace tak, že přejde na adrese http://localhost: 8080 nebo s následující `curl` příkaz:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="dbb1b-125">Měli byste vidět zobrazenou následující zprávu: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="dbb1b-125">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="dbb1b-127">Vytvoření registru kontejneru služby Azure pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="dbb1b-127">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="dbb1b-128">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-128">Open a command prompt.</span></span>

1. <span data-ttu-id="dbb1b-129">Přihlaste se k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-129">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="dbb1b-130">Vytvořte skupinu prostředků pro použité v tomto kurzu prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="dbb1b-131">Vytvořte kontejner privátní Azure registru ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="dbb1b-132">Kurz doručí ukázková aplikace jako obrázek na Docker tento registru v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="dbb1b-133">Nahraďte `wingtiptoysregistry` s jedinečným názvem pro vaše registru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="dbb1b-134">Push vaší aplikace do registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="dbb1b-134">Push your app to the container registry</span></span>

1. <span data-ttu-id="dbb1b-135">Přejděte do adresáře konfigurace pro instalaci Maven (výchozí ~/.m2/ nebo C:\Users\username\.m2) a otevřete *souborech settings.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-135">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="dbb1b-136">Načtení hesla pro vaše kontejneru registru z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-136">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="dbb1b-137">Přidat kontejner registru Azure id a heslo na nový `<server>` kolekce *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-137">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="dbb1b-138">`id` a `username` jsou název registru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-138">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="dbb1b-139">Použití `password` hodnotu z předchozí příkaz (bez uvozovek).</span><span class="sxs-lookup"><span data-stu-id="dbb1b-139">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="dbb1b-140">Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí (například "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") a otevřete *pom.xml* soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-140">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="dbb1b-141">Aktualizace `<properties>` kolekce *pom.xml* soubor s hodnotou server přihlášení pro vaše registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-141">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="dbb1b-142">Aktualizace `<plugins>` kolekce *pom.xml* souboru tak, aby `<plugin>` obsahuje přihlašovací adresu a registru název serveru pro váš registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-142">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="dbb1b-143">Přejděte do adresáře dokončený projekt pro vaše aplikace pružiny spouštěcí a spusťte následující příkaz a vytvořit kontejner Docker push bitovou kopii do registru:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-143">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="dbb1b-144">Zobrazí chybovou zprávu, která je jednu z následujících při Maven sami bitovou kopii do Azure:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-144">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="dbb1b-145">Pokud se tato chyba přihlášení k Azure z příkazového řádku Dockeru.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-145">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="dbb1b-146">Potom push vaší kontejneru:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a><span data-ttu-id="dbb1b-147">Vytvoření clusteru s podporou Kubernetes na ACS pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="dbb1b-147">Create a Kubernetes Cluster on ACS using the Azure CLI</span></span>

1. <span data-ttu-id="dbb1b-148">Vytvoření clusteru Kubernetes v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="dbb1b-149">Následující příkaz vytvoří *kubernetes* v clusteru *Northwind kubernetes* prostředků skupiny s *Northwind containerservice* jako název clusteru, a *Northwind kubernetes* jako DNS předpony:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-149">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="dbb1b-150">Tento příkaz může trvat nějakou dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-150">This command may take a while to complete.</span></span>

1. <span data-ttu-id="dbb1b-151">Nainstalujte `kubectl` pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-151">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="dbb1b-152">Linux uživatelé mohou mít k předpony tento příkaz s `sudo` vzhledem k tomu, že ji nasadí Kubernetes rozhraní příkazového řádku pro `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-152">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="dbb1b-153">Stáhněte si informace o konfiguraci clusteru, můžete spravovat cluster z webové rozhraní Kubernetes a `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-153">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="dbb1b-154">Nasazení bitové kopie do clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="dbb1b-154">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="dbb1b-155">V tomto kurzu nasadí aplikace pomocí `kubectl`, pak umožňují prozkoumat nasazení pomocí rozhraní web Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-155">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="dbb1b-156">Nasazení s Kubernetes webové rozhraní</span><span class="sxs-lookup"><span data-stu-id="dbb1b-156">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="dbb1b-157">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-157">Open a command prompt.</span></span>

1. <span data-ttu-id="dbb1b-158">Otevřete stránku konfigurace pro váš cluster Kubernetes ve výchozím prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-158">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="dbb1b-159">Když web konfigurace Kubernetes otevře v prohlížeči, klikněte na odkaz **nasadit kontejnerizované aplikaci**:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-159">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes konfigurace webu][KB01]

1. <span data-ttu-id="dbb1b-161">Když **nasadit kontejnerizované aplikaci** se zobrazí stránka, určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-161">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="dbb1b-162">a.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-162">a.</span></span> <span data-ttu-id="dbb1b-163">Vyberte **zadejte níže uvedené podrobnosti o aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="dbb1b-164">b.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-164">b.</span></span> <span data-ttu-id="dbb1b-165">Zadejte název aplikace pružiny spouštěcí pro **název aplikace**; například: "*gs pružiny spouštěcí docker*".</span><span class="sxs-lookup"><span data-stu-id="dbb1b-165">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="dbb1b-166">c.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-166">c.</span></span> <span data-ttu-id="dbb1b-167">Zadejte přihlašovací serveru a kontejneru bitové kopie z dříve pro **kontejneru image**; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span><span class="sxs-lookup"><span data-stu-id="dbb1b-167">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="dbb1b-168">d.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-168">d.</span></span> <span data-ttu-id="dbb1b-169">Zvolte **externí** pro **služby**.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-169">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="dbb1b-170">e.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-170">e.</span></span> <span data-ttu-id="dbb1b-171">Zadejte vaše externí i interní porty **Port** a **cíle port** textová pole.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-171">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes konfigurace webu][KB02]


1. <span data-ttu-id="dbb1b-173">Klikněte na tlačítko **nasadit** nasaďte kontejner.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-173">Click **Deploy** to deploy the container.</span></span>

   ![Nasazení kontejneru][KB05]

1. <span data-ttu-id="dbb1b-175">Po nasazení vaší aplikace, zobrazí se aplikace pružiny spouštěcí uvedené v části **služby**.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes služby][KB06]

1. <span data-ttu-id="dbb1b-177">Pokud kliknete na odkaz **externí koncové body**, uvidíte pružiny spouštěcí aplikace spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-177">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes služby][KB07]

   ![Procházet ukázkovou aplikaci v Azure][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="dbb1b-180">Nasazení s kubectl</span><span class="sxs-lookup"><span data-stu-id="dbb1b-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="dbb1b-181">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-181">Open a command prompt.</span></span>

1. <span data-ttu-id="dbb1b-182">Spuštění vaší kontejneru v clusteru Kubernetes pomocí `kubectl run` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-182">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="dbb1b-183">Zadejte název služby pro aplikaci v Kubernetes a názvu úplnou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-183">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="dbb1b-184">Například:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="dbb1b-185">V tomto příkazu:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-185">In this command:</span></span>

   * <span data-ttu-id="dbb1b-186">Název kontejneru `gs-spring-boot-docker` je zadán ihned po `run` příkaz</span><span class="sxs-lookup"><span data-stu-id="dbb1b-186">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="dbb1b-187">`--image` Parametr určuje název serveru a bitové kopie kombinované přihlášení jako`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="dbb1b-187">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="dbb1b-188">Vystavení clusteru Kubernetes externě pomocí `kubectl expose` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-188">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="dbb1b-189">Zadejte název vaší služby, veřejné port TCP používá pro přístup k aplikaci a interní cílový port, který aplikace naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-189">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="dbb1b-190">Například:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="dbb1b-191">V tomto příkazu:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-191">In this command:</span></span>

   * <span data-ttu-id="dbb1b-192">Název kontejneru `gs-spring-boot-docker` je zadán ihned po `expose deployment` příkaz</span><span class="sxs-lookup"><span data-stu-id="dbb1b-192">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="dbb1b-193">`--type` Parametr určuje, zda cluster používá nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="dbb1b-193">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="dbb1b-194">`--port` Parametr určuje veřejné port TCP 80.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-194">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="dbb1b-195">Máte přístup k aplikaci na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-195">You access the app on this port.</span></span>

   * <span data-ttu-id="dbb1b-196">`--target-port` Parametr určuje vnitřní TCP port 8080.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-196">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="dbb1b-197">Nástroje pro vyrovnávání zatížení předá požadavky na aplikace na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-197">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="dbb1b-198">Po nasazení aplikace do clusteru, dotazování na externí IP adresu a otevřete ve webovém prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-198">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Procházet ukázkovou aplikaci v Azure][SB02]


## <a name="next-steps"></a><span data-ttu-id="dbb1b-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dbb1b-200">Next steps</span></span>

<span data-ttu-id="dbb1b-201">Další informace o používání spouštěcí Spring v Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-201">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="dbb1b-202">Nasazení aplikace spouštěcí pružiny do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dbb1b-202">Deploy a Spring Boot Application to the Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="dbb1b-203">Nasazení aplikace pružiny spouštění v systému Linux v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="dbb1b-203">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="dbb1b-204">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="dbb1b-204">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="dbb1b-205">Další informace o spouštění pružiny na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].</span><span class="sxs-lookup"><span data-stu-id="dbb1b-205">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="dbb1b-206">Následující odkazy obsahují další informace o vytváření aplikací pro spouštěcí pružiny:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-206">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="dbb1b-207">Další informace o vytvoření jednoduché aplikace pružiny spouštěcí najdete v části Initializr Spring v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-207">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="dbb1b-208">Následující odkazy obsahují další informace o používání Kubernetes s Azure:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-208">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="dbb1b-209">Začínáme s Kubernetes cluster Container Service</span><span class="sxs-lookup"><span data-stu-id="dbb1b-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="dbb1b-210">Pomocí Azure Container Service Kubernetes webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="dbb1b-210">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="dbb1b-211">Další informace o používání rozhraní příkazového řádku Kubernetes je k dispozici v **kubectl** v uživatelské příručce <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="dbb1b-211">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="dbb1b-212">Kubernetes web má několik články, které popisují pomocí bitové kopie v privátní registrech:</span><span class="sxs-lookup"><span data-stu-id="dbb1b-212">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="dbb1b-213">[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]</span><span class="sxs-lookup"><span data-stu-id="dbb1b-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="dbb1b-214">[Obory názvů]</span><span class="sxs-lookup"><span data-stu-id="dbb1b-214">[Namespaces]</span></span>
* <span data-ttu-id="dbb1b-215">[Stahování bitovou kopii z privátní registru]</span><span class="sxs-lookup"><span data-stu-id="dbb1b-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="dbb1b-216">Další příklady použití vlastních imagí Dockeru s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].</span><span class="sxs-lookup"><span data-stu-id="dbb1b-216">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

<span data-ttu-id="dbb1b-217">[Rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="dbb1b-217">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
<span data-ttu-id="dbb1b-218">[ Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-218">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span></span>
<span data-ttu-id="dbb1b-219">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-219">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
<span data-ttu-id="dbb1b-220">[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span><span class="sxs-lookup"><span data-stu-id="dbb1b-220">[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span></span>
<span data-ttu-id="dbb1b-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-221">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="dbb1b-222">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-222">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="dbb1b-223">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-223">[Git]: https://github.com/</span></span>
<span data-ttu-id="dbb1b-224">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-224">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="dbb1b-225">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-225">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="dbb1b-226">[Kubernetes]: https://kubernetes.io/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-226">[Kubernetes]: https://kubernetes.io/</span></span>
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
<span data-ttu-id="dbb1b-227">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-227">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="dbb1b-228">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-228">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="dbb1b-229">[pružiny spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-229">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="dbb1b-230">[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="dbb1b-230">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="dbb1b-231">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-231">[Spring Framework]: https://spring.io/</span></span>
<span data-ttu-id="dbb1b-232">[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-232">[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span></span>
<span data-ttu-id="dbb1b-233">[Obory názvů]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-233">[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span></span>
<span data-ttu-id="dbb1b-234">[Stahování bitovou kopii z privátní registru]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span><span class="sxs-lookup"><span data-stu-id="dbb1b-234">[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span></span>

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
