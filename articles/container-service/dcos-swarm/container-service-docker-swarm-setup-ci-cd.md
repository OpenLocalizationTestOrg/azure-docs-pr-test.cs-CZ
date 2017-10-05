---
title: CI/CD s Azure Container Service a Swarm | Microsoft Docs
description: "Dostat nepřetržitě aplikace .NET Core více kontejnerů pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services pomocí Azure Container Service"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: Docker, kontejnery, Micro-services, Swarm, Azure, Visual Studio Team Services, DevOps
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 99c27c37218a35d2a3416d6edd5e0a871cd5c011
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="3a184-104">Úplné kanálu CI nebo CD pro nasazení aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="3a184-104">Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="3a184-105">Jeden z největších problémů při vývoji moderní aplikace pro cloud je schopnost poskytovat nepřetržitě tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a184-105">One of the biggest challenges when developing modern applications for the cloud is being able to deliver these applications continuously.</span></span> <span data-ttu-id="3a184-106">V tomto článku zjistěte, jak implementovat úplné průběžnou integraci a nasazení (CI/CD) kanálu Azure Container Service pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services sestavení a správa verzí.</span><span class="sxs-lookup"><span data-stu-id="3a184-106">In this article, you learn how to implement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="3a184-107">Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/acs-docs), vyvinuté pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3a184-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="3a184-108">Aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:</span><span class="sxs-lookup"><span data-stu-id="3a184-108">The application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="3a184-110">Cílem je zajistit tuto aplikaci nepřetržitě v clusteru Docker Swarm, pomocí sady Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="3a184-110">The objective is to deliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="3a184-111">Následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:</span><span class="sxs-lookup"><span data-stu-id="3a184-111">The following figure details this continuous delivery pipeline:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="3a184-113">Následuje stručné vysvětlení kroky:</span><span class="sxs-lookup"><span data-stu-id="3a184-113">Here is a brief explanation of the steps:</span></span>

1. <span data-ttu-id="3a184-114">Změny kódu se zaměřuje na úložiště zdrojového kódu (tady Githubu)</span><span class="sxs-lookup"><span data-stu-id="3a184-114">Code changes are committed to the source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="3a184-115">GitHub aktivuje build ve Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="3a184-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="3a184-116">Visual Studio Team Services získá nejnovější verzi zdroje a vytvoří všechny bitové kopie, které tvoří aplikace</span><span class="sxs-lookup"><span data-stu-id="3a184-116">Visual Studio Team Services gets the latest version of the sources and builds all the images that compose the application</span></span> 
4. <span data-ttu-id="3a184-117">Visual Studio Team Services doručí každé bitové kopie do registru Docker vytvořené pomocí služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="3a184-117">Visual Studio Team Services pushes each image to a Docker registry created using the Azure Container Registry service</span></span> 
5. <span data-ttu-id="3a184-118">Visual Studio Team Services aktivuje novou verzí</span><span class="sxs-lookup"><span data-stu-id="3a184-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="3a184-119">Verze spouští některé příkazy na hlavního uzlu clusteru Azure container service pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="3a184-119">The release runs some commands using SSH on the Azure container service cluster master node</span></span> 
7. <span data-ttu-id="3a184-120">Vrátí nejnovější verzi bitové kopie docker Swarm v clusteru</span><span class="sxs-lookup"><span data-stu-id="3a184-120">Docker Swarm on the cluster pulls the latest version of the images</span></span> 
8. <span data-ttu-id="3a184-121">Nová verze aplikace se nasazuje pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="3a184-121">The new version of the application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3a184-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a184-122">Prerequisites</span></span>

<span data-ttu-id="3a184-123">Před zahájením tohoto kurzu, budete muset provést následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="3a184-123">Before starting this tutorial, you need to complete the following tasks:</span></span>

- [<span data-ttu-id="3a184-124">Vytvoření clusteru Swarm v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="3a184-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="3a184-125">Propojení s clusterem Swarm ve službě Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="3a184-125">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="3a184-126">Vytvoření služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="3a184-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="3a184-127">Máte účet a tým projekt Visual Studio Team Services vytvořen</span><span class="sxs-lookup"><span data-stu-id="3a184-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="3a184-128">Větev úložiště GitHub ke svému účtu GitHub</span><span class="sxs-lookup"><span data-stu-id="3a184-128">Fork the GitHub repository to your GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="3a184-129">Budete také potřebovat počítače Ubuntu (14.04 a 16.04) s Docker nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3a184-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="3a184-130">Tento počítač používá Visual Studio Team Services během procesy sestavení a verze.</span><span class="sxs-lookup"><span data-stu-id="3a184-130">This machine is used by Visual Studio Team Services during the build and release processes.</span></span> <span data-ttu-id="3a184-131">Jeden způsob, jak vytvořit tento počítač je používat k dispozici v bitovou kopii [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="3a184-131">One way to create this machine is to use the image available in the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="3a184-132">Krok 1: Konfigurace účtu Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="3a184-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="3a184-133">V této části můžete nakonfigurovat účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="3a184-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="3a184-134">Konfigurace agenta pro Visual Studio Team Services Linux sestavení</span><span class="sxs-lookup"><span data-stu-id="3a184-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="3a184-135">K vytvoření imagí Dockeru a vkládání těchto bitových kopií do registru kontejner Azure ze sestavení sady Visual Studio Team Services, potřebujete zaregistrovat agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="3a184-135">To create Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need to register a Linux agent.</span></span> <span data-ttu-id="3a184-136">Máte tyto možnosti instalace:</span><span class="sxs-lookup"><span data-stu-id="3a184-136">You have these installation options:</span></span>

* [<span data-ttu-id="3a184-137">Nasazení agenta v systému Linux</span><span class="sxs-lookup"><span data-stu-id="3a184-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="3a184-138">Spusťte služby VSTS agenta pomocí Docker</span><span class="sxs-lookup"><span data-stu-id="3a184-138">Use Docker to run the VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a><span data-ttu-id="3a184-139">Nainstalujte rozšíření služby VSTS integrace Docker</span><span class="sxs-lookup"><span data-stu-id="3a184-139">Install the Docker Integration VSTS extension</span></span>

<span data-ttu-id="3a184-140">Společnost Microsoft poskytuje rozšíření služby VSTS práci s Docker v sestavení a verze procesy.</span><span class="sxs-lookup"><span data-stu-id="3a184-140">Microsoft provides a VSTS extension to work with Docker in build and release processes.</span></span> <span data-ttu-id="3a184-141">Toto rozšíření je k dispozici v [služby VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="3a184-141">This extension is available in the [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="3a184-142">Klikněte na tlačítko **nainstalovat** k účtu služby VSTS přidat tuto příponu:</span><span class="sxs-lookup"><span data-stu-id="3a184-142">Click **Install** to add this extension to your VSTS account:</span></span>

![Nainstalujte integrace Dockeru](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="3a184-144">Zobrazí se výzva k připojení k účtu služby VSTS pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="3a184-144">You are asked to connect to your VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="3a184-145">Připojit Visual Studio Team Services a GitHub</span><span class="sxs-lookup"><span data-stu-id="3a184-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="3a184-146">Nastavte připojení mezi projektu služby VSTS a účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="3a184-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="3a184-147">V projektu Visual Studio Team Services, klikněte **nastavení** v panelu nástrojů a vyberte ikonu **služby**.</span><span class="sxs-lookup"><span data-stu-id="3a184-147">In your Visual Studio Team Services project, click the **Settings** icon in the toolbar, and select **Services**.</span></span>

    ![Visual Studio Team Services - externí připojení](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="3a184-149">Na levé straně klikněte na tlačítko **nový koncový bod služby** > **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="3a184-149">On the left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="3a184-151">Chcete-li autorizovat služby VSTS pro práci s účtu Githubu, klikněte na tlačítko **Authorize** a postupujte podle pokynů v okně, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="3a184-151">To authorize VSTS to work with your GitHub account, click **Authorize** and follow the procedure in the window that opens.</span></span>

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="3a184-153">Služby VSTS připojení k registru kontejner Azure a cluster Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="3a184-153">Connect VSTS to your Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="3a184-154">Poslední kroky před získáním do kanálu CI/CD jsou nakonfigurovat externí připojení k registru systému kontejneru a clusteru Docker Swarm v Azure.</span><span class="sxs-lookup"><span data-stu-id="3a184-154">The last steps before getting into the CI/CD pipeline are to configure external connections to your container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="3a184-155">V **služby** nastavení projektu Visual Studio Team Services přidat koncový bod služby typu **Docker registru**.</span><span class="sxs-lookup"><span data-stu-id="3a184-155">In the **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="3a184-156">V místní nabídce, která otevře zadejte adresu URL a pověření registru systému Windows Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3a184-156">In the popup that opens, enter the URL and the credentials of your Azure container registry.</span></span>

    ![Visual Studio Team Services - Docker registru](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="3a184-158">Pro cluster Docker Swarm, přidání koncového bodu typu **SSH**.</span><span class="sxs-lookup"><span data-stu-id="3a184-158">For the Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="3a184-159">Zadejte informace o clusteru Swarm připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="3a184-159">Then enter the SSH connection information of your Swarm cluster.</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="3a184-161">Všechny konfigurace se provádí teď.</span><span class="sxs-lookup"><span data-stu-id="3a184-161">All the configuration is done now.</span></span> <span data-ttu-id="3a184-162">V dalších krocích můžete vytvořit kanál CI/CD, který vytvoří a nasadí aplikaci do clusteru Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="3a184-162">In the next steps, you create the CI/CD pipeline that builds and deploys the application to the Docker Swarm cluster.</span></span> 

## <a name="step-2-create-the-build-definition"></a><span data-ttu-id="3a184-163">Krok 2: Vytvoření definici sestavení</span><span class="sxs-lookup"><span data-stu-id="3a184-163">Step 2: Create the build definition</span></span>

<span data-ttu-id="3a184-164">V tomto kroku můžete nastavit definitionfor sestavení projektu služby VSTS a definovat sestavení pracovního postupu pro kontejner obrázků</span><span class="sxs-lookup"><span data-stu-id="3a184-164">In this step, you set up a build definitionfor your VSTS project and define the build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="3a184-165">Definice počáteční instalace</span><span class="sxs-lookup"><span data-stu-id="3a184-165">Initial definition setup</span></span>

1. <span data-ttu-id="3a184-166">K vytvoření definice sestavení, připojení k vašemu projektu Visual Studio Team Services a klikněte na tlačítko **sestavení a verze**.</span><span class="sxs-lookup"><span data-stu-id="3a184-166">To create a build definition, connect to your Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="3a184-167">V **sestavení definice** klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="3a184-167">In the **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="3a184-168">Vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="3a184-168">Select the **Empty** template.</span></span>

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="3a184-170">Konfigurace nového sestavení se zdrojem úložiště Githubu, zkontrolujte **průběžnou integraci**a vyberte frontu agenta, kde jste zaregistrovali Linux agent.</span><span class="sxs-lookup"><span data-stu-id="3a184-170">Configure the new build with a GitHub repository source, check **Continuous integration**, and select the agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="3a184-171">Klikněte na tlačítko **vytvořit** k vytvoření definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="3a184-171">Click **Create** to create the build definition.</span></span>

    ![Visual Studio Team Services – vytvoření definice sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="3a184-173">Na **definice sestavení** stránky, poprvé otevřete **úložiště** a nakonfigurujte sestavení používat pokračovatelem MyShop projekt, který jste vytvořili v požadavky.</span><span class="sxs-lookup"><span data-stu-id="3a184-173">On the **Build Definitions** page, first open the **Repository** tab and configure the build to use the fork of the MyShop project that you created in the prerequisites.</span></span> <span data-ttu-id="3a184-174">Ujistěte se, že jste vybrali *acs dokumentace* jako **výchozí větev**.</span><span class="sxs-lookup"><span data-stu-id="3a184-174">Make sure that you select *acs-docs* as the **Default branch**.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="3a184-176">Na **aktivační události** nakonfigurujte sestavení, aby se spouštěly po každém potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3a184-176">On the **Triggers** tab, configure the build to be triggered after each commit.</span></span> <span data-ttu-id="3a184-177">Vyberte **průběžnou integraci** a **dávky změny**.</span><span class="sxs-lookup"><span data-stu-id="3a184-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - konfigurace aktivační události sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a><span data-ttu-id="3a184-179">Definice pracovního postupu sestavení</span><span class="sxs-lookup"><span data-stu-id="3a184-179">Define the build workflow</span></span>
<span data-ttu-id="3a184-180">Další kroky definovat sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="3a184-180">The next steps define the build workflow.</span></span> <span data-ttu-id="3a184-181">Pět kontejneru Image na sestavení pro *MyShop* aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a184-181">There are five container images to build for the *MyShop* application.</span></span> <span data-ttu-id="3a184-182">Každé bitové kopie je sestaven pomocí soubor Docker umístěný ve složce projektu:</span><span class="sxs-lookup"><span data-stu-id="3a184-182">Each image is built using the Dockerfile located in the project folders:</span></span>

* <span data-ttu-id="3a184-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="3a184-183">ProductsApi</span></span>
* <span data-ttu-id="3a184-184">Proxy server</span><span class="sxs-lookup"><span data-stu-id="3a184-184">Proxy</span></span>
* <span data-ttu-id="3a184-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="3a184-185">RatingsApi</span></span>
* <span data-ttu-id="3a184-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="3a184-186">RecommandationsApi</span></span>
* <span data-ttu-id="3a184-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="3a184-187">ShopFront</span></span>

<span data-ttu-id="3a184-188">Je nutné přidat dva kroky Docker pro každé bitové kopie, jeden vytvořit bitovou kopii a jeden pro uložení image v registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="3a184-188">You need to add two Docker steps for each image, one to build the image, and one to push the image in the Azure container registry.</span></span> 

1. <span data-ttu-id="3a184-189">Chcete-li přidat krok v pracovním postupu sestavení, klikněte na tlačítko **+ přidat krok sestavení** a vyberte **Docker**.</span><span class="sxs-lookup"><span data-stu-id="3a184-189">To add a step in the build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="3a184-191">Pro každé bitové kopie, nakonfigurovat jeden krok, který používá `docker build` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3a184-191">For each image, configure one step that uses the `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="3a184-193">Pro operaci sestavení, vyberte kontejner Azure registr, **vytvořit bitovou kopii** akce a soubor Docker, která definuje každé bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="3a184-193">For the build operation, select your Azure container registry, the **Build an image** action, and the Dockerfile that defines each image.</span></span> <span data-ttu-id="3a184-194">Nastavte **sestavení kontextu** jako soubor Docker kořenový adresář a definujte **název bitové kopie**.</span><span class="sxs-lookup"><span data-stu-id="3a184-194">Set the **Build context** as the Dockerfile root directory, and define the **Image Name**.</span></span> 
    
    <span data-ttu-id="3a184-195">Jak je znázorněno na předchozí obrazovce, spusťte název bitové kopie s identifikátorem URI registru systému Windows Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3a184-195">As shown on the preceding screen, start the image name with the URI of your Azure container registry.</span></span> <span data-ttu-id="3a184-196">(Můžete také použijete sestavení proměnnou o parametrizaci značka obrázku, jako je například identifikátor sestavení v tomto příkladu.)</span><span class="sxs-lookup"><span data-stu-id="3a184-196">(You can also use a build variable to parameterize the tag of the image, such as the build identifier in this example.)</span></span>

3. <span data-ttu-id="3a184-197">Pro každé bitové kopie, nakonfigurujte druhý krok, který používá `docker push` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3a184-197">For each image, configure a second step that uses the `docker push` command.</span></span>

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="3a184-199">Pomocí operace push, vyberte možnost kontejner Azure registr, **Push bitovou kopii** akce a zadejte **název bitové kopie** který je vytvořen v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3a184-199">For the push operation, select your Azure container registry, the **Push an image** action, and enter the **Image Name** that is built in the previous step.</span></span>

4. <span data-ttu-id="3a184-200">Po dokončení konfigurace sestavení a nabízených kroky pro každý z pěti bitové kopie, přidejte dva další kroky v pracovním postupu sestavení.</span><span class="sxs-lookup"><span data-stu-id="3a184-200">After you configure the build and push steps for each of the five images, add two more steps in the build workflow.</span></span>

    <span data-ttu-id="3a184-201">a.</span><span class="sxs-lookup"><span data-stu-id="3a184-201">a.</span></span> <span data-ttu-id="3a184-202">Úlohu příkazového řádku, která používá skript bash nahradit *BuildNumber* sestavení výskyt v soubor docker-compose.yml s aktuálním ID. V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3a184-202">A command-line task that uses a bash script to replace the *BuildNumber* occurrence in the docker-compose.yml file with the current build Id. See the following screen for details.</span></span>

    ![Visual Studio Team Services - vytvářené aktualizace souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="3a184-204">b.</span><span class="sxs-lookup"><span data-stu-id="3a184-204">b.</span></span> <span data-ttu-id="3a184-205">Úloha, která zahodí aktualizovaný soubor vytvářené jako sestavení artefaktů, takže ho můžete použít ve verzi.</span><span class="sxs-lookup"><span data-stu-id="3a184-205">A task that drops the updated Compose file as a build artifact so it can be used in the release.</span></span> <span data-ttu-id="3a184-206">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3a184-206">See the following screen for details.</span></span>

    ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="3a184-208">Klikněte na tlačítko **Uložit** a název vaší definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="3a184-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-the-release-definition"></a><span data-ttu-id="3a184-209">Krok 3: Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="3a184-209">Step 3: Create the release definition</span></span>

<span data-ttu-id="3a184-210">Visual Studio Team Services umožňuje [Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="3a184-210">Visual Studio Team Services allows you to [manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="3a184-211">Průběžné nasazování, abyste měli jistotu, že vaše aplikace je nasazená na vaše různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="3a184-211">You can enable continuous deployment to make sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="3a184-212">Můžete vytvořit nové prostředí, které představuje váš cluster Azure Container Service Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="3a184-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![Visual Studio Team Services - verze služby ACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="3a184-214">Instalační program původní verze</span><span class="sxs-lookup"><span data-stu-id="3a184-214">Initial release setup</span></span>

1. <span data-ttu-id="3a184-215">K vytvoření definice vydání, klikněte na tlačítko **verze** > **+ verze**</span><span class="sxs-lookup"><span data-stu-id="3a184-215">To create a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="3a184-216">Chcete-li nastavit zdroj artefaktů, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**.</span><span class="sxs-lookup"><span data-stu-id="3a184-216">To configure the artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="3a184-217">Tato nová verze definice zde odkaz na sestavení, který jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3a184-217">Here, link this new release definition to the build that you defined in the previous step.</span></span> <span data-ttu-id="3a184-218">Díky tomu je k dispozici v procesu verze soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="3a184-218">By doing this, the docker-compose.yml file is available in the release process.</span></span>

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="3a184-220">Konfigurace verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="3a184-220">To configure the release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="3a184-221">Nastavte aktivační události na stejném zdroji artefaktů.</span><span class="sxs-lookup"><span data-stu-id="3a184-221">Set the trigger on the same artifact source.</span></span> <span data-ttu-id="3a184-222">Toto nastavení zajistí, že začne novou verzi také sestavení se dokončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="3a184-222">This setting ensures that a new release starts as soon as the build completes successfully.</span></span>

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a><span data-ttu-id="3a184-224">Zadejte verzi pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="3a184-224">Define the release workflow</span></span>

<span data-ttu-id="3a184-225">Verze pracovní postup se skládá z dvě úlohy, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="3a184-225">The release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="3a184-226">Nakonfigurujte úlohu bezpečně zkopírovat soubor vytvářené *nasazení* složky na Docker Swarm hlavního uzlu pomocí připojení SSH jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="3a184-226">Configure a task to securely copy the compose file to a *deploy* folder on the Docker Swarm master node, using the SSH connection you configured previously.</span></span> <span data-ttu-id="3a184-227">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3a184-227">See the following screen for details.</span></span>

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="3a184-229">Nakonfigurujte druhý úlohy ke spuštění příkazu bash ke spuštění `docker` a `docker-compose` příkazů na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="3a184-229">Configure a second task to execute a bash command to run `docker` and `docker-compose` commands on the master node.</span></span> <span data-ttu-id="3a184-230">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3a184-230">See the following screen for details.</span></span>

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="3a184-232">Příkaz provést v hlavní použijte rozhraní příkazového řádku Dockeru a rozhraní příkazového řádku Docker Compose k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="3a184-232">The command executed on the master use the Docker CLI and the Docker-Compose CLI to do the following tasks:</span></span>

    - <span data-ttu-id="3a184-233">Přihlášení k Azure kontejneru registru (používá tři variab'les sestavení, která jsou definována v **proměnné** karty)</span><span class="sxs-lookup"><span data-stu-id="3a184-233">Login to the Azure container registry (it uses three build variab\`les that are defined in the **Variables** tab)</span></span>
    - <span data-ttu-id="3a184-234">Definování **DOCKER_HOST** proměnnou pro práci s koncovému bodu Swarm (: 2375)</span><span class="sxs-lookup"><span data-stu-id="3a184-234">Define the **DOCKER_HOST** variable to work with the Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="3a184-235">Přejděte na *nasazení* složky, který byl vytvořen předchozí zabezpečené kopírování úlohy a který obsahuje soubor docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="3a184-235">Navigate to the *deploy* folder that was created by the preceding secure copy task and that contains the docker-compose.yml file</span></span> 
    - <span data-ttu-id="3a184-236">Spuštění `docker-compose` příkazy, které nových bitových kopií pro vyžádání obsahu zastavení služeb, odeberte služby a vytvoření kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="3a184-236">Execute `docker-compose` commands that pull the new images, stop the services, remove the services, and create the containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="3a184-237">Jak je znázorněno na předchozí obrazovce, ponechte **nezdaří do datového proudu STDERR** nezaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="3a184-237">As shown on the preceding screen, leave the **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="3a184-238">To je důležité nastavit, protože `docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo odstraňuje na standardní chyba výstup.</span><span class="sxs-lookup"><span data-stu-id="3a184-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on the standard error output.</span></span> <span data-ttu-id="3a184-239">Pokud zaškrtnete políčko, Visual Studio Team Services hlásí, že nastaly chyby během verzí, i v případě, že všechno proběhne správně.</span><span class="sxs-lookup"><span data-stu-id="3a184-239">If you check the checkbox, Visual Studio Team Services reports that errors occurred during the release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="3a184-240">Uložte tuto novou verzi definici.</span><span class="sxs-lookup"><span data-stu-id="3a184-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="3a184-241">Toto nasazení obsahuje výpadky, protože jsme zastavení staré služeb a systémem novým.</span><span class="sxs-lookup"><span data-stu-id="3a184-241">This deployment includes some downtime because we are stopping the old services and running the new one.</span></span> <span data-ttu-id="3a184-242">Je možné, abyste tomu předešli díky blue zelená nasazení.</span><span class="sxs-lookup"><span data-stu-id="3a184-242">It is possible to avoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-the-cicd-pipeline"></a><span data-ttu-id="3a184-243">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="3a184-243">Step 4.</span></span> <span data-ttu-id="3a184-244">Testování CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="3a184-244">Test the CI/CD pipeline</span></span>

<span data-ttu-id="3a184-245">Teď, když jste hotovi s konfigurací, je otestovat, tento nový kanál CI/CD.</span><span class="sxs-lookup"><span data-stu-id="3a184-245">Now that you are done with the configuration, it's time to test this new CI/CD pipeline.</span></span> <span data-ttu-id="3a184-246">Aktualizujte zdrojový kód a provést změny do úložiště GitHub je nejjednodušší způsob, jak otestovat.</span><span class="sxs-lookup"><span data-stu-id="3a184-246">The easiest way to test it is to update the source code and commit the changes into your GitHub repository.</span></span> <span data-ttu-id="3a184-247">Několik sekund poté, co push kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="3a184-247">A few seconds after you push the code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="3a184-248">Po úspěšném dokončení novou verzi se spustí a provede nasazení nové verze aplikace v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="3a184-248">Once completed successfully, a new release will be triggered and will deploy the new version of the application on the Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a184-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a184-249">Next Steps</span></span>

* <span data-ttu-id="3a184-250">Další informace o CI/CD s Visual Studio Team Services najdete v tématu [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="3a184-250">For more information about CI/CD with Visual Studio Team Services, see the [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
