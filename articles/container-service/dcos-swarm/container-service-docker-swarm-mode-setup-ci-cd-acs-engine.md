---
title: "CI/CD s Azure Container Service modul a režim Swarm | Microsoft Docs"
description: "Použití stroj kontejneru služby Azure se Docker Swarm režimu, registru kontejner Azure a Visual Studio Team Services k poskytování nepřetržitě aplikace .NET Core více kontejneru"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Docker, kontejnery, Micro-services, Swarm, Azure, Visual Studio Team služby, DevOps, modul služby ACS"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 2c0e5fe4f60738fcc1aa67a78674e6f3c62e5628
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a><span data-ttu-id="7eb07-104">Úplné kanálu CI nebo CD pro nasazení aplikace s více kontejnerů v Azure Container Service pomocí modulu služby ACS a Docker Swarm režimu pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="7eb07-104">Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with ACS Engine and Docker Swarm Mode using Visual Studio Team Services</span></span>

<span data-ttu-id="7eb07-105">*Tento článek vychází z [kanálu úplné CI nebo CD pro nasazení aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) dokumentace*</span><span class="sxs-lookup"><span data-stu-id="7eb07-105">*This article is based on [Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentation*</span></span>

<span data-ttu-id="7eb07-106">V současné době jeden z největších problémů, když vývoj moderních aplikací pro cloud je schopnost poskytovat nepřetržitě tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="7eb07-106">Nowadays, One of the biggest challenges when developing modern applications for the cloud is being able to deliver these applications continuously.</span></span> <span data-ttu-id="7eb07-107">V tomto článku se naučíte, jak implementovat úplné průběžnou integraci a nasazení (CI/CD) s použitím kanálu:</span><span class="sxs-lookup"><span data-stu-id="7eb07-107">In this article, you learn how to implement a full continuous integration and deployment (CI/CD) pipeline using:</span></span> 
* <span data-ttu-id="7eb07-108">Stroj služby Azure kontejner s režimem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="7eb07-108">Azure Container Service Engine with Docker Swarm Mode</span></span>
* <span data-ttu-id="7eb07-109">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="7eb07-109">Azure Container Registry</span></span>
* <span data-ttu-id="7eb07-110">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="7eb07-110">Visual Studio Team Services</span></span>

<span data-ttu-id="7eb07-111">Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/docker-linux), vyvinuté pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7eb07-111">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), developed with ASP.NET Core.</span></span> <span data-ttu-id="7eb07-112">Aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:</span><span class="sxs-lookup"><span data-stu-id="7eb07-112">The application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

<span data-ttu-id="7eb07-114">Cílem je zajistit tuto aplikaci nepřetržitě v clusteru Docker Swarm režimu, pomocí sady Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="7eb07-114">The objective is to deliver this application continuously in a Docker Swarm Mode cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="7eb07-115">Následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:</span><span class="sxs-lookup"><span data-stu-id="7eb07-115">The following figure details this continuous delivery pipeline:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

<span data-ttu-id="7eb07-117">Následuje stručné vysvětlení kroky:</span><span class="sxs-lookup"><span data-stu-id="7eb07-117">Here is a brief explanation of the steps:</span></span>

1. <span data-ttu-id="7eb07-118">Změny kódu se zaměřuje na úložiště zdrojového kódu (tady Githubu)</span><span class="sxs-lookup"><span data-stu-id="7eb07-118">Code changes are committed to the source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="7eb07-119">GitHub aktivuje build ve Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="7eb07-119">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="7eb07-120">Visual Studio Team Services získá nejnovější verzi zdroje a vytvoří všechny bitové kopie, které tvoří aplikace</span><span class="sxs-lookup"><span data-stu-id="7eb07-120">Visual Studio Team Services gets the latest version of the sources and builds all the images that compose the application</span></span> 
4. <span data-ttu-id="7eb07-121">Visual Studio Team Services doručí každé bitové kopie do registru Docker vytvořené pomocí služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="7eb07-121">Visual Studio Team Services pushes each image to a Docker registry created using the Azure Container Registry service</span></span> 
5. <span data-ttu-id="7eb07-122">Visual Studio Team Services aktivuje novou verzí</span><span class="sxs-lookup"><span data-stu-id="7eb07-122">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="7eb07-123">Verze spouští některé příkazy na hlavního uzlu clusteru Azure container service pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="7eb07-123">The release runs some commands using SSH on the Azure container service cluster master node</span></span> 
7. <span data-ttu-id="7eb07-124">Vrátí nejnovější verzi bitové kopie docker Swarm režimu v clusteru</span><span class="sxs-lookup"><span data-stu-id="7eb07-124">Docker Swarm Mode on the cluster pulls the latest version of the images</span></span> 
8. <span data-ttu-id="7eb07-125">Nová verze aplikace se nasazuje pomocí Docker zásobníku</span><span class="sxs-lookup"><span data-stu-id="7eb07-125">The new version of the application is deployed using Docker Stack</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7eb07-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7eb07-126">Prerequisites</span></span>

<span data-ttu-id="7eb07-127">Před zahájením tohoto kurzu, budete muset provést následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="7eb07-127">Before starting this tutorial, you need to complete the following tasks:</span></span>

- [<span data-ttu-id="7eb07-128">Vytvoření clusteru Swarm režimu v Azure Container Service s modulem služby ACS</span><span class="sxs-lookup"><span data-stu-id="7eb07-128">Create a Swarm Mode cluster in Azure Container Service with ACS Engine</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [<span data-ttu-id="7eb07-129">Propojení s clusterem Swarm ve službě Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="7eb07-129">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="7eb07-130">Vytvoření služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="7eb07-130">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="7eb07-131">Máte účet a tým projekt Visual Studio Team Services vytvořen</span><span class="sxs-lookup"><span data-stu-id="7eb07-131">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="7eb07-132">Větev úložiště GitHub ke svému účtu GitHub</span><span class="sxs-lookup"><span data-stu-id="7eb07-132">Fork the GitHub repository to your GitHub account</span></span>](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> <span data-ttu-id="7eb07-133">Orchestrátor Docker Swarm v Azure Container Service používá starší verzi samostatného Swarmu.</span><span class="sxs-lookup"><span data-stu-id="7eb07-133">The Docker Swarm orchestrator in Azure Container Service uses legacy standalone Swarm.</span></span> <span data-ttu-id="7eb07-134">Integrovaný [režim Swarm](https://docs.docker.com/engine/swarm/) (v Dockeru 1.12 a novějším) aktuálně není podporovaným orchestrátorem v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="7eb07-134">Currently, the integrated [Swarm mode](https://docs.docker.com/engine/swarm/) (in Docker 1.12 and higher) is not a supported orchestrator in Azure Container Service.</span></span> <span data-ttu-id="7eb07-135">Z tohoto důvodu se používá [modul služby ACS](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), komunity podílí [šablony rychlý Start](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), nebo v řešení Docker [Azure Marketplace](https://azuremarketplace.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7eb07-135">For this reason, we are using [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), a community-contributed [quickstart template](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), or a Docker solution in the [Azure Marketplace](https://azuremarketplace.microsoft.com).</span></span>
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="7eb07-136">Krok 1: Konfigurace účtu Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="7eb07-136">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="7eb07-137">V této části můžete nakonfigurovat účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="7eb07-137">In this section, you configure your Visual Studio Team Services account.</span></span> <span data-ttu-id="7eb07-138">Chcete-li konfigurovat služby VSTS služby koncových bodů, ve vašem projektu Visual Studio Team Services, klikněte na tlačítko **nastavení** v panelu nástrojů a vyberte ikonu **služby**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-138">To configure VSTS Services Endpoints, in your Visual Studio Team Services project, click the **Settings** icon in the toolbar, and select **Services**.</span></span>

![Koncový bod služby otevřete](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a><span data-ttu-id="7eb07-140">Připojení účtu Visual Studio Team Services a Azure</span><span class="sxs-lookup"><span data-stu-id="7eb07-140">Connect Visual Studio Team Services and Azure account</span></span>

<span data-ttu-id="7eb07-141">Nastavte připojení mezi projektu služby VSTS a účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="7eb07-141">Set up a connection between your VSTS project and your Azure account.</span></span>

1. <span data-ttu-id="7eb07-142">Na levé straně klikněte na tlačítko **nový koncový bod služby** > **Azure Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-142">On the left, click **New Service Endpoint** > **Azure Resource Manager**.</span></span>
2. <span data-ttu-id="7eb07-143">Autorizace služby VSTS pro práci s vaším účtem Azure, vyberte vaše **předplatné** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-143">To authorize VSTS to work with your Azure account, select your **Subscription** and click **OK**.</span></span>

    ![Visual Studio Team Services - autorizovat Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="7eb07-145">Připojit Visual Studio Team Services a GitHub</span><span class="sxs-lookup"><span data-stu-id="7eb07-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="7eb07-146">Nastavte připojení mezi projektu služby VSTS a účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="7eb07-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="7eb07-147">Na levé straně klikněte na tlačítko **nový koncový bod služby** > **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-147">On the left, click **New Service Endpoint** > **GitHub**.</span></span>
2. <span data-ttu-id="7eb07-148">Chcete-li autorizovat služby VSTS pro práci s účtu Githubu, klikněte na tlačítko **Authorize** a postupujte podle pokynů v okně, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="7eb07-148">To authorize VSTS to work with your GitHub account, click **Authorize** and follow the procedure in the window that opens.</span></span>

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-to-your-azure-container-service-cluster"></a><span data-ttu-id="7eb07-150">Služby VSTS připojit ke clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="7eb07-150">Connect VSTS to your Azure Container Service cluster</span></span>

<span data-ttu-id="7eb07-151">Poslední kroky před získáním do kanálu CI/CD jsou konfigurace externí připojení ke clusteru Docker Swarm v Azure.</span><span class="sxs-lookup"><span data-stu-id="7eb07-151">The last steps before getting into the CI/CD pipeline are to configure external connections to your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="7eb07-152">Pro cluster Docker Swarm, přidání koncového bodu typu **SSH**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-152">For the Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="7eb07-153">Zadejte informace o připojení SSH clusteru Swarm (hlavní uzel).</span><span class="sxs-lookup"><span data-stu-id="7eb07-153">Then enter the SSH connection information of your Swarm cluster (master node).</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

<span data-ttu-id="7eb07-155">Všechny konfigurace se provádí teď.</span><span class="sxs-lookup"><span data-stu-id="7eb07-155">All the configuration is done now.</span></span> <span data-ttu-id="7eb07-156">V dalších krocích můžete vytvořit kanál CI/CD, který vytvoří a nasadí aplikaci do clusteru Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="7eb07-156">In the next steps, you create the CI/CD pipeline that builds and deploys the application to the Docker Swarm cluster.</span></span> 

## <a name="step-2-create-the-build-definition"></a><span data-ttu-id="7eb07-157">Krok 2: Vytvoření definici sestavení</span><span class="sxs-lookup"><span data-stu-id="7eb07-157">Step 2: Create the build definition</span></span>

<span data-ttu-id="7eb07-158">V tomto kroku můžete nastavit definice sestavení pro svůj projekt služby VSTS a definovat sestavení pracovního postupu pro kontejner obrázků</span><span class="sxs-lookup"><span data-stu-id="7eb07-158">In this step, you set up a build definition for your VSTS project and define the build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="7eb07-159">Definice počáteční instalace</span><span class="sxs-lookup"><span data-stu-id="7eb07-159">Initial definition setup</span></span>

1. <span data-ttu-id="7eb07-160">K vytvoření definice sestavení, připojení k vašemu projektu Visual Studio Team Services a klikněte na tlačítko **sestavení a verze**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-160">To create a build definition, connect to your Visual Studio Team Services project and click **Build & Release**.</span></span> <span data-ttu-id="7eb07-161">V **sestavení definice** klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-161">In the **Build definitions** section, click **+ New**.</span></span> 

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. <span data-ttu-id="7eb07-163">Vyberte **prázdný proces**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-163">Select the **Empty process**.</span></span>

    ![Visual Studio Team Services – nové definice buildu prázdný](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. <span data-ttu-id="7eb07-165">Potom klikněte **proměnné** kartě a vytvořte dva nové proměnné: **RegistryURL** a **AgentURL**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-165">Then, click the **Variables** tab and create two new variables: **RegistryURL** and **AgentURL**.</span></span> <span data-ttu-id="7eb07-166">Vložení hodnoty registru a DNS agenty clusteru.</span><span class="sxs-lookup"><span data-stu-id="7eb07-166">Paste the values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - proměnné konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. <span data-ttu-id="7eb07-168">Na **definice sestavení** otevřete **aktivační události** a nakonfigurujte sestavení pro použití průběžnou integraci s pokračovatelem MyShop projekt, který jste vytvořili v požadavky.</span><span class="sxs-lookup"><span data-stu-id="7eb07-168">On the **Build Definitions** page, open the **Triggers** tab and configure the build to use continuous integration with the fork of the MyShop project that you created in the prerequisites.</span></span> <span data-ttu-id="7eb07-169">Pak vyberte **dávky změny**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-169">Then, select **Batch changes**.</span></span> <span data-ttu-id="7eb07-170">Ujistěte se, že jste vybrali *docker linux* jako **větev specifikace**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-170">Make sure that you select *docker-linux* as the **Branch specification**.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. <span data-ttu-id="7eb07-172">Nakonec klikněte na **možnosti** a nakonfigurujte agenta frontě výchozí **Preview Linux hostované**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-172">Finally, click the **Options** tab and configure the Default agent queue to **Hosted Linux Preview**.</span></span>

    ![Visual Studio Team Services - Host Agent Configuration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a><span data-ttu-id="7eb07-174">Definice pracovního postupu sestavení</span><span class="sxs-lookup"><span data-stu-id="7eb07-174">Define the build workflow</span></span>
<span data-ttu-id="7eb07-175">Další kroky definovat sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="7eb07-175">The next steps define the build workflow.</span></span> <span data-ttu-id="7eb07-176">Nejdřív musíte nakonfigurovat zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="7eb07-176">First, you need to configure the source of the code.</span></span> <span data-ttu-id="7eb07-177">Chcete-li provést, vyberte **Githubu** a **úložiště** a **větve** (docker-linux).</span><span class="sxs-lookup"><span data-stu-id="7eb07-177">To do it, select **GitHub** and your **repository** and **branch** (docker-linux).</span></span>

![Visual Studio Team Services - konfigurace zdrojového kódu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

<span data-ttu-id="7eb07-179">Pět kontejneru Image na sestavení pro *MyShop* aplikace.</span><span class="sxs-lookup"><span data-stu-id="7eb07-179">There are five container images to build for the *MyShop* application.</span></span> <span data-ttu-id="7eb07-180">Každé bitové kopie je sestaven pomocí soubor Docker umístěný ve složce projektu:</span><span class="sxs-lookup"><span data-stu-id="7eb07-180">Each image is built using the Dockerfile located in the project folders:</span></span>

* <span data-ttu-id="7eb07-181">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="7eb07-181">ProductsApi</span></span>
* <span data-ttu-id="7eb07-182">Proxy server</span><span class="sxs-lookup"><span data-stu-id="7eb07-182">Proxy</span></span>
* <span data-ttu-id="7eb07-183">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="7eb07-183">RatingsApi</span></span>
* <span data-ttu-id="7eb07-184">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="7eb07-184">RecommandationsApi</span></span>
* <span data-ttu-id="7eb07-185">ShopFront</span><span class="sxs-lookup"><span data-stu-id="7eb07-185">ShopFront</span></span>

<span data-ttu-id="7eb07-186">Budete potřebovat dva kroky Docker pro každé bitové kopie, jeden vytvořit bitovou kopii a jeden pro uložení image v registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="7eb07-186">You need two Docker steps for each image, one to build the image, and one to push the image in the Azure container registry.</span></span> 

1. <span data-ttu-id="7eb07-187">Chcete-li přidat krok v pracovním postupu sestavení, klikněte na tlačítko **+ přidat krok sestavení** a vyberte **Docker**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-187">To add a step in the build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. <span data-ttu-id="7eb07-189">Pro každé bitové kopie, nakonfigurovat jeden krok, který používá `docker build` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7eb07-189">For each image, configure one step that uses the `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    <span data-ttu-id="7eb07-191">Pro operaci sestavení, vyberte kontejner registr Azure, **vytvořit bitovou kopii** akce a soubor Docker, která definuje každé bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7eb07-191">For the build operation, select your Azure Container Registry, the **Build an image** action, and the Dockerfile that defines each image.</span></span> <span data-ttu-id="7eb07-192">Nastavte **pracovní adresář** jako soubor Docker kořenový adresář, zadejte **název bitové kopie**a vyberte **zahrnují nejnovější značky**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-192">Set the **Working Directory** as the Dockerfile root directory, define the **Image Name**, and select **Include Latest Tag**.</span></span>
    
    <span data-ttu-id="7eb07-193">Název bitové kopie musí být v tomto formátu: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span><span class="sxs-lookup"><span data-stu-id="7eb07-193">The Image Name has to be in this format: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span></span> <span data-ttu-id="7eb07-194">Nahraďte **[NAME]** s název bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="7eb07-194">Replace **[NAME]** with the image name:</span></span>
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. <span data-ttu-id="7eb07-195">Pro každé bitové kopie, nakonfigurujte druhý krok, který používá `docker push` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7eb07-195">For each image, configure a second step that uses the `docker push` command.</span></span>

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    <span data-ttu-id="7eb07-197">Pomocí operace push, vyberte možnost kontejner Azure registr, **Push bitovou kopii** akce, zadejte **název bitové kopie** který je vytvořen v předchozím kroku a vyberte **zahrnout nejnovější značky**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-197">For the push operation, select your Azure container registry, the **Push an image** action, enter the **Image Name** that is built in the previous step and select **Include Latest Tag**.</span></span>

4. <span data-ttu-id="7eb07-198">Po dokončení konfigurace sestavení a nabízených kroky pro každý z pěti bitové kopie, přidejte tři další kroky v pracovním postupu sestavení.</span><span class="sxs-lookup"><span data-stu-id="7eb07-198">After you configure the build and push steps for each of the five images, add three more steps in the build workflow.</span></span>

   ![Visual Studio Team Services - přidat úlohu příkazového řádku](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. <span data-ttu-id="7eb07-200">Úlohu příkazového řádku, která používá skript bash nahradit *RegistryURL* výskyt v soubor docker-compose.yml s RegistryURL proměnnou.</span><span class="sxs-lookup"><span data-stu-id="7eb07-200">A command-line task that uses a bash script to replace the *RegistryURL* occurrence in the docker-compose.yml file with the RegistryURL variable.</span></span> 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - vytvářené aktualizace souboru s adresou URL registru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. <span data-ttu-id="7eb07-202">Úlohu příkazového řádku, která používá skript bash nahradit *AgentURL* výskyt v soubor docker-compose.yml s AgentURL proměnnou.</span><span class="sxs-lookup"><span data-stu-id="7eb07-202">A command-line task that uses a bash script to replace the *AgentURL* occurrence in the docker-compose.yml file with the AgentURL variable.</span></span>
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. <span data-ttu-id="7eb07-203">Úloha, která zahodí aktualizovaný soubor vytvářené jako sestavení artefaktů, takže ho můžete použít ve verzi.</span><span class="sxs-lookup"><span data-stu-id="7eb07-203">A task that drops the updated Compose file as a build artifact so it can be used in the release.</span></span> <span data-ttu-id="7eb07-204">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb07-204">See the following screen for details.</span></span>

         ![Visual Studio Team Services - publikování artefaktů](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. <span data-ttu-id="7eb07-207">Klikněte na tlačítko **Uložit & fronty** k testování vaší definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="7eb07-207">Click **Save & queue** to test your build definition.</span></span>

   ![Visual Studio Team Services - uložit a fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services – nové fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. <span data-ttu-id="7eb07-210">Pokud **sestavení** je správný, budete muset tato obrazovka:</span><span class="sxs-lookup"><span data-stu-id="7eb07-210">If the **Build** is correct, you have to see this screen:</span></span>

  ![Visual Studio Team Services - sestavení bylo úspěšně dokončeno](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-definition"></a><span data-ttu-id="7eb07-212">Krok 3: Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="7eb07-212">Step 3: Create the release definition</span></span>

<span data-ttu-id="7eb07-213">Visual Studio Team Services umožňuje [Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="7eb07-213">Visual Studio Team Services allows you to [manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="7eb07-214">Průběžné nasazování, abyste měli jistotu, že vaše aplikace je nasazená na vaše různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="7eb07-214">You can enable continuous deployment to make sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="7eb07-215">Můžete vytvořit prostředí, které představuje váš cluster Azure Container Service Docker Swarm režimu.</span><span class="sxs-lookup"><span data-stu-id="7eb07-215">You can create an environment that represents your Azure Container Service Docker Swarm Mode cluster.</span></span>

![Visual Studio Team Services - verze služby ACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="7eb07-217">Instalační program původní verze</span><span class="sxs-lookup"><span data-stu-id="7eb07-217">Initial release setup</span></span>

1. <span data-ttu-id="7eb07-218">K vytvoření definice vydání, klikněte na tlačítko **verze** > **+ verze**</span><span class="sxs-lookup"><span data-stu-id="7eb07-218">To create a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="7eb07-219">Chcete-li nastavit zdroj artefaktů, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-219">To configure the artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="7eb07-220">Tato nová verze definice zde odkaz na sestavení, který jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="7eb07-220">Here, link this new release definition to the build that you defined in the previous step.</span></span> <span data-ttu-id="7eb07-221">Potom je k dispozici v procesu verze soubor docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="7eb07-221">After that, the docker-compose.yml file is available in the release process.</span></span>

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. <span data-ttu-id="7eb07-223">Konfigurace verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-223">To configure the release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="7eb07-224">Nastavte aktivační události na stejném zdroji artefaktů.</span><span class="sxs-lookup"><span data-stu-id="7eb07-224">Set the trigger on the same artifact source.</span></span> <span data-ttu-id="7eb07-225">Toto nastavení zajistí, že nová verze začne po úspěšném dokončení sestavení.</span><span class="sxs-lookup"><span data-stu-id="7eb07-225">This setting ensures that a new release starts when the build completes successfully.</span></span>

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. <span data-ttu-id="7eb07-227">Chcete-li nakonfigurovat proměnné verze, klikněte na tlačítko **proměnné** a vyberte **+ proměnné** vytvořit tři nové proměnné s informacemi registru: **docker.username**, **docker.password**, a **docker.registry**.</span><span class="sxs-lookup"><span data-stu-id="7eb07-227">To configure the release variables, click **Variables** and select **+Variable** to create three new variables with the info of the registry: **docker.username**, **docker.password**, and **docker.registry**.</span></span> <span data-ttu-id="7eb07-228">Vložení hodnoty registru a DNS agenty clusteru.</span><span class="sxs-lookup"><span data-stu-id="7eb07-228">Paste the values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > <span data-ttu-id="7eb07-230">Jak je znázorněno na předchozí obrazovce, klikněte na tlačítko **zámku** zaškrtnout políčko docker.password.</span><span class="sxs-lookup"><span data-stu-id="7eb07-230">As shown on the previous screen, click the **Lock** checkbox in docker.password.</span></span> <span data-ttu-id="7eb07-231">Toto nastavení je důležité omezit heslo.</span><span class="sxs-lookup"><span data-stu-id="7eb07-231">This setting is important to restrict the password.</span></span>
    >

### <a name="define-the-release-workflow"></a><span data-ttu-id="7eb07-232">Zadejte verzi pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="7eb07-232">Define the release workflow</span></span>

<span data-ttu-id="7eb07-233">Verze pracovní postup se skládá z dvě úlohy, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="7eb07-233">The release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="7eb07-234">Nakonfigurujte úlohu bezpečně zkopírovat soubor vytvářené *nasazení* složky na Docker Swarm hlavního uzlu pomocí připojení SSH jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="7eb07-234">Configure a task to securely copy the compose file to a *deploy* folder on the Docker Swarm master node, using the SSH connection you configured previously.</span></span> <span data-ttu-id="7eb07-235">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb07-235">See the following screen for details.</span></span>
    
    <span data-ttu-id="7eb07-236">Zdrojová složka:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span><span class="sxs-lookup"><span data-stu-id="7eb07-236">Source folder: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span></span>

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. <span data-ttu-id="7eb07-238">Nakonfigurujte druhý úlohy ke spuštění příkazu bash ke spuštění `docker` a `docker stack deploy` příkazů na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="7eb07-238">Configure a second task to execute a bash command to run `docker` and `docker stack deploy` commands on the master node.</span></span> <span data-ttu-id="7eb07-239">V následující obrazovku podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb07-239">See the following screen for details.</span></span>

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    <span data-ttu-id="7eb07-241">Příkaz proveden na hlavním serveru používá rozhraní příkazového řádku Dockeru a rozhraní příkazového řádku Docker Compose provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="7eb07-241">The command executed on the master uses the Docker CLI and the Docker-Compose CLI to do the following tasks:</span></span>

    - <span data-ttu-id="7eb07-242">Přihlaste se k Azure kontejneru registru (používá tři proměnné sestavení, které jsou definovány v **proměnné** karty)</span><span class="sxs-lookup"><span data-stu-id="7eb07-242">Log in to the Azure container registry (it uses three build variables that are defined in the **Variables** tab)</span></span>
    - <span data-ttu-id="7eb07-243">Definování **DOCKER_HOST** proměnnou pro práci s koncovému bodu Swarm (: 2375)</span><span class="sxs-lookup"><span data-stu-id="7eb07-243">Define the **DOCKER_HOST** variable to work with the Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="7eb07-244">Přejděte na *nasazení* složky, který byl vytvořen předchozí zabezpečené kopírování úlohy a který obsahuje soubor docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="7eb07-244">Navigate to the *deploy* folder that was created by the preceding secure copy task and that contains the docker-compose.yml file</span></span> 
    - <span data-ttu-id="7eb07-245">Spuštění `docker stack deploy` příkazy, které nových bitových kopií pro vyžádání obsahu a vytvoření kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="7eb07-245">Execute `docker stack deploy` commands that pull the new images and create the containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="7eb07-246">Jak je znázorněno na předchozí obrazovce, ponechte **nezdaří do datového proudu STDERR** nezaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="7eb07-246">As shown on the previous screen, leave the **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="7eb07-247">Toto nastavení umožňuje nám dokončete proces verzi z důvodu `docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo odstraňuje na standardní chyba výstup.</span><span class="sxs-lookup"><span data-stu-id="7eb07-247">This setting allows us to complete the release process due to `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on the standard error output.</span></span> <span data-ttu-id="7eb07-248">Pokud zaškrtnete políčko, Visual Studio Team Services hlásí, že nastaly chyby během verzí, i v případě, že všechno proběhne správně.</span><span class="sxs-lookup"><span data-stu-id="7eb07-248">If you check the checkbox, Visual Studio Team Services reports that errors occurred during the release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="7eb07-249">Uložte tuto novou verzi definici.</span><span class="sxs-lookup"><span data-stu-id="7eb07-249">Save this new release definition.</span></span>

## <a name="step-4-test-the-cicd-pipeline"></a><span data-ttu-id="7eb07-250">Krok 4: Test CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="7eb07-250">Step 4: Test the CI/CD pipeline</span></span>

<span data-ttu-id="7eb07-251">Teď, když jste hotovi s konfigurací, je otestovat, tento nový kanál CI/CD.</span><span class="sxs-lookup"><span data-stu-id="7eb07-251">Now that you are done with the configuration, it's time to test this new CI/CD pipeline.</span></span> <span data-ttu-id="7eb07-252">Aktualizujte zdrojový kód a provést změny do úložiště GitHub je nejjednodušší způsob, jak otestovat.</span><span class="sxs-lookup"><span data-stu-id="7eb07-252">The easiest way to test it is to update the source code and commit the changes into your GitHub repository.</span></span> <span data-ttu-id="7eb07-253">Několik sekund poté, co push kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="7eb07-253">A few seconds after you push the code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="7eb07-254">Po úspěšném dokončení novou verzi se aktivuje a nasadit novou verzi aplikace v clusteru Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="7eb07-254">Once completed successfully, a new release is triggered and deployed the new version of the application on the Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7eb07-255">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7eb07-255">Next steps</span></span>

* <span data-ttu-id="7eb07-256">Další informace o CI/CD s Visual Studio Team Services najdete v tématu [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="7eb07-256">For more information about CI/CD with Visual Studio Team Services, see the [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
* <span data-ttu-id="7eb07-257">Další informace o modulu služby ACS, najdete v článku [úložiště GitHub modul ACS](https://github.com/Azure/acs-engine).</span><span class="sxs-lookup"><span data-stu-id="7eb07-257">For more information about ACS Engine, see the [ACS Engine GitHub repo](https://github.com/Azure/acs-engine).</span></span>
* <span data-ttu-id="7eb07-258">Další informace o Docker Swarm režimu najdete v tématu [Docker Swarm přehled režimu](https://docs.docker.com/engine/swarm/).</span><span class="sxs-lookup"><span data-stu-id="7eb07-258">For more information about Docker Swarm mode, see the [Docker Swarm mode overview](https://docs.docker.com/engine/swarm/).</span></span>
