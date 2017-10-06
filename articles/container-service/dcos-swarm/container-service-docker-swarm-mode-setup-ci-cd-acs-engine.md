---
title: "aaaCI/CD s Azure Container Service modul a režim Swarm | Microsoft Docs"
description: "Pomocí Docker Swarm režimu, registru kontejner Azure a Visual Studio Team Services toodeliver nepřetržitě aplikace .NET Core více kontejnerů Azure Container Service modul"
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
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a><span data-ttu-id="8c57d-104">Úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí modulu služby ACS a Docker Swarm režimu pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8c57d-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with ACS Engine and Docker Swarm Mode using Visual Studio Team Services</span></span>

<span data-ttu-id="8c57d-105">*Tento článek vychází z [úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) dokumentace*</span><span class="sxs-lookup"><span data-stu-id="8c57d-105">*This article is based on [Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentation*</span></span>

<span data-ttu-id="8c57d-106">V současné době jedna z hello nejnáročnějších úkolů při vývoj moderních aplikací pro hello cloud se může toodeliver tyto aplikace nepřetržitě.</span><span class="sxs-lookup"><span data-stu-id="8c57d-106">Nowadays, One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="8c57d-107">V tomto článku se dozvíte, jak se tooimplement úplné průběžnou integraci a nasazení (CI/CD) kanálu, pomocí:</span><span class="sxs-lookup"><span data-stu-id="8c57d-107">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using:</span></span> 
* <span data-ttu-id="8c57d-108">Stroj služby Azure kontejner s režimem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="8c57d-108">Azure Container Service Engine with Docker Swarm Mode</span></span>
* <span data-ttu-id="8c57d-109">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="8c57d-109">Azure Container Registry</span></span>
* <span data-ttu-id="8c57d-110">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8c57d-110">Visual Studio Team Services</span></span>

<span data-ttu-id="8c57d-111">Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/docker-linux), vyvinuté pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c57d-111">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), developed with ASP.NET Core.</span></span> <span data-ttu-id="8c57d-112">Hello aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:</span><span class="sxs-lookup"><span data-stu-id="8c57d-112">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

<span data-ttu-id="8c57d-114">cíl Hello je toodeliver tuto aplikaci nepřetržitě v clusteru Docker Swarm režimu, pomocí sady Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8c57d-114">hello objective is toodeliver this application continuously in a Docker Swarm Mode cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="8c57d-115">Hello následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:</span><span class="sxs-lookup"><span data-stu-id="8c57d-115">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

<span data-ttu-id="8c57d-117">Následuje stručné vysvětlení hello kroky:</span><span class="sxs-lookup"><span data-stu-id="8c57d-117">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="8c57d-118">Změny kódu jsou potvrzené toohello úložiště zdrojového kódu (tady Githubu)</span><span class="sxs-lookup"><span data-stu-id="8c57d-118">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="8c57d-119">GitHub aktivuje build ve Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8c57d-119">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="8c57d-120">Visual Studio Team Services získá hello nejnovější verzi hello zdroje a vytvoří všechny hello bitové kopie, které tvoří aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-120">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="8c57d-121">Visual Studio Team Services doručí registru Docker tooa každé bitové kopie vytvořené pomocí služby Azure kontejneru registru hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-121">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="8c57d-122">Visual Studio Team Services aktivuje novou verzí</span><span class="sxs-lookup"><span data-stu-id="8c57d-122">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="8c57d-123">verze Hello spouští některé příkazy hlavního uzlu clusteru serveru hello Azure container service pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="8c57d-123">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="8c57d-124">Docker Swarm režimu v clusteru hello vrátí hello nejnovější verzi bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-124">Docker Swarm Mode on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="8c57d-125">Hello nové verze aplikace hello se nasazuje pomocí Docker zásobníku</span><span class="sxs-lookup"><span data-stu-id="8c57d-125">hello new version of hello application is deployed using Docker Stack</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8c57d-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8c57d-126">Prerequisites</span></span>

<span data-ttu-id="8c57d-127">Před zahájením tohoto kurzu potřebujete toocomplete hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="8c57d-127">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="8c57d-128">Vytvoření clusteru Swarm režimu v Azure Container Service s modulem služby ACS</span><span class="sxs-lookup"><span data-stu-id="8c57d-128">Create a Swarm Mode cluster in Azure Container Service with ACS Engine</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [<span data-ttu-id="8c57d-129">Propojení s clusterem Swarm hello v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="8c57d-129">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="8c57d-130">Vytvoření služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="8c57d-130">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="8c57d-131">Máte účet a tým projekt Visual Studio Team Services vytvořen</span><span class="sxs-lookup"><span data-stu-id="8c57d-131">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="8c57d-132">Rozvětvení hello Githubu úložiště tooyour účet GitHub</span><span class="sxs-lookup"><span data-stu-id="8c57d-132">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> <span data-ttu-id="8c57d-133">Hello orchestrator Docker Swarm v Azure Container Service používá starší verzi samostatné Swarm.</span><span class="sxs-lookup"><span data-stu-id="8c57d-133">hello Docker Swarm orchestrator in Azure Container Service uses legacy standalone Swarm.</span></span> <span data-ttu-id="8c57d-134">V současné době hello integrované [Swarm režimu](https://docs.docker.com/engine/swarm/) (v Docker 1.12 a vyšší) není podporovaný orchestrator v Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8c57d-134">Currently, hello integrated [Swarm mode](https://docs.docker.com/engine/swarm/) (in Docker 1.12 and higher) is not a supported orchestrator in Azure Container Service.</span></span> <span data-ttu-id="8c57d-135">Z tohoto důvodu používáme [ACS modul](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), komunity podílí [šablony rychlý Start](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), nebo řešení Docker v hello [Azure Marketplace](https://azuremarketplace.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8c57d-135">For this reason, we are using [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), a community-contributed [quickstart template](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), or a Docker solution in hello [Azure Marketplace](https://azuremarketplace.microsoft.com).</span></span>
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="8c57d-136">Krok 1: Konfigurace účtu Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="8c57d-136">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="8c57d-137">V této části můžete nakonfigurovat účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8c57d-137">In this section, you configure your Visual Studio Team Services account.</span></span> <span data-ttu-id="8c57d-138">Koncové tooconfigure služby VSTS služby body, ve vašem projektu Visual Studio Team Services, klikněte na tlačítko hello **nastavení** v hello nástrojů a vyberte ikonu **služby**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-138">tooconfigure VSTS Services Endpoints, in your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

![Koncový bod služby otevřete](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a><span data-ttu-id="8c57d-140">Připojení účtu Visual Studio Team Services a Azure</span><span class="sxs-lookup"><span data-stu-id="8c57d-140">Connect Visual Studio Team Services and Azure account</span></span>

<span data-ttu-id="8c57d-141">Nastavte připojení mezi projektu služby VSTS a účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="8c57d-141">Set up a connection between your VSTS project and your Azure account.</span></span>

1. <span data-ttu-id="8c57d-142">Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Azure Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-142">On hello left, click **New Service Endpoint** > **Azure Resource Manager**.</span></span>
2. <span data-ttu-id="8c57d-143">služby VSTS toowork tooauthorize s vaším účtem Azure, vyberte vaše **předplatné** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-143">tooauthorize VSTS toowork with your Azure account, select your **Subscription** and click **OK**.</span></span>

    ![Visual Studio Team Services - autorizovat Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="8c57d-145">Připojit Visual Studio Team Services a GitHub</span><span class="sxs-lookup"><span data-stu-id="8c57d-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="8c57d-146">Nastavte připojení mezi projektu služby VSTS a účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="8c57d-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="8c57d-147">Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-147">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>
2. <span data-ttu-id="8c57d-148">služby VSTS toowork tooauthorize s vaším účtem Githubu, klikněte na tlačítko **Authorize** a použijte postup hello v otevřeném okně hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-148">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a><span data-ttu-id="8c57d-150">Připojení clusteru Azure Container Service tooyour služby VSTS</span><span class="sxs-lookup"><span data-stu-id="8c57d-150">Connect VSTS tooyour Azure Container Service cluster</span></span>

<span data-ttu-id="8c57d-151">Hello poslední kroky před získáním do kanálu hello CI/CD jsou tooconfigure externí připojení tooyour Docker Swarm clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="8c57d-151">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="8c57d-152">Pro cluster hello Docker Swarm, přidání koncového bodu typu **SSH**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-152">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="8c57d-153">Zadejte informace o připojení SSH hello clusteru Swarm (hlavní uzel).</span><span class="sxs-lookup"><span data-stu-id="8c57d-153">Then enter hello SSH connection information of your Swarm cluster (master node).</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

<span data-ttu-id="8c57d-155">Všechny konfigurace hello je nyní provádí.</span><span class="sxs-lookup"><span data-stu-id="8c57d-155">All hello configuration is done now.</span></span> <span data-ttu-id="8c57d-156">V dalších krocích hello můžete vytvořit hello CI/CD kanálu, který vytvoří a nasadí hello aplikace toohello Docker Swarm clusteru.</span><span class="sxs-lookup"><span data-stu-id="8c57d-156">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="8c57d-157">Krok 2: Vytvoření definici sestavení hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-157">Step 2: Create hello build definition</span></span>

<span data-ttu-id="8c57d-158">V tomto kroku můžete nastavit definice sestavení pro svůj projekt služby VSTS a definovat hello sestavení pracovního postupu pro kontejner obrázků</span><span class="sxs-lookup"><span data-stu-id="8c57d-158">In this step, you set up a build definition for your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="8c57d-159">Definice počáteční instalace</span><span class="sxs-lookup"><span data-stu-id="8c57d-159">Initial definition setup</span></span>

1. <span data-ttu-id="8c57d-160">toocreate definice sestavení, připojit tooyour Visual Studio Team Services projektu a klikněte na tlačítko **sestavení a verze**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-160">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> <span data-ttu-id="8c57d-161">V hello **sestavení definice** klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-161">In hello **Build definitions** section, click **+ New**.</span></span> 

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. <span data-ttu-id="8c57d-163">Vyberte hello **prázdný proces**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-163">Select hello **Empty process**.</span></span>

    ![Visual Studio Team Services – nové definice buildu prázdný](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. <span data-ttu-id="8c57d-165">Potom klikněte na hello **proměnné** kartě a vytvořte dva nové proměnné: **RegistryURL** a **AgentURL**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-165">Then, click hello **Variables** tab and create two new variables: **RegistryURL** and **AgentURL**.</span></span> <span data-ttu-id="8c57d-166">Vložte hello hodnoty registru a DNS agenty clusteru.</span><span class="sxs-lookup"><span data-stu-id="8c57d-166">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - proměnné konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. <span data-ttu-id="8c57d-168">Na hello **definice sestavení** stránky, otevřete hello **aktivační události** a nakonfigurujte hello sestavení toouse průběžnou integraci s hello rozvětvení hello MyShop projektu, který jste vytvořili v hello požadavky.</span><span class="sxs-lookup"><span data-stu-id="8c57d-168">On hello **Build Definitions** page, open hello **Triggers** tab and configure hello build toouse continuous integration with hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="8c57d-169">Pak vyberte **dávky změny**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-169">Then, select **Batch changes**.</span></span> <span data-ttu-id="8c57d-170">Ujistěte se, že jste vybrali *docker linux* jako hello **větev specifikace**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-170">Make sure that you select *docker-linux* as hello **Branch specification**.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. <span data-ttu-id="8c57d-172">Nakonec klikněte na hello **možnosti** a nakonfigurujte hello výchozí agenta fronty příliš**Preview Linux hostované**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-172">Finally, click hello **Options** tab and configure hello Default agent queue too**Hosted Linux Preview**.</span></span>

    ![Visual Studio Team Services - Host Agent Configuration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="8c57d-174">Definování hello sestavení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="8c57d-174">Define hello build workflow</span></span>
<span data-ttu-id="8c57d-175">Další kroky Hello definovat hello sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="8c57d-175">hello next steps define hello build workflow.</span></span> <span data-ttu-id="8c57d-176">Nejprve je třeba zdroj hello tooconfigure hello kódu.</span><span class="sxs-lookup"><span data-stu-id="8c57d-176">First, you need tooconfigure hello source of hello code.</span></span> <span data-ttu-id="8c57d-177">toodo ho vyberte **Githubu** a **úložiště** a **větve** (docker-linux).</span><span class="sxs-lookup"><span data-stu-id="8c57d-177">toodo it, select **GitHub** and your **repository** and **branch** (docker-linux).</span></span>

![Visual Studio Team Services - konfigurace zdrojového kódu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

<span data-ttu-id="8c57d-179">Existují pět toobuild bitové kopie kontejner pro hello *MyShop* aplikace.</span><span class="sxs-lookup"><span data-stu-id="8c57d-179">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="8c57d-180">Každé bitové kopie je sestaven pomocí hello soubor Docker umístěný v hello složky projektu:</span><span class="sxs-lookup"><span data-stu-id="8c57d-180">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="8c57d-181">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="8c57d-181">ProductsApi</span></span>
* <span data-ttu-id="8c57d-182">Proxy server</span><span class="sxs-lookup"><span data-stu-id="8c57d-182">Proxy</span></span>
* <span data-ttu-id="8c57d-183">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="8c57d-183">RatingsApi</span></span>
* <span data-ttu-id="8c57d-184">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="8c57d-184">RecommandationsApi</span></span>
* <span data-ttu-id="8c57d-185">ShopFront</span><span class="sxs-lookup"><span data-stu-id="8c57d-185">ShopFront</span></span>

<span data-ttu-id="8c57d-186">Budete potřebovat dva kroky Docker každé bitové kopie, jednu toobuild hello image a jednu image hello toopush v registru kontejner Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-186">You need two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="8c57d-187">Klikněte na tlačítko tooadd krok v postupu sestavení hello **+ přidat krok sestavení** a vyberte **Docker**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-187">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. <span data-ttu-id="8c57d-189">Pro každé bitové kopie, nakonfigurovat jeden krok, který používá hello `docker build` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8c57d-189">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    <span data-ttu-id="8c57d-191">Pro hello sestavení operaci, vyberte kontejner registr Azure, hello **vytvořit bitovou kopii** akce a hello soubor Docker, která definuje každé bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8c57d-191">For hello build operation, select your Azure Container Registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="8c57d-192">Sada hello **pracovní adresář** jako hello soubor Docker kořenový adresář, definovat hello **název bitové kopie**a vyberte **zahrnují nejnovější značky**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-192">Set hello **Working Directory** as hello Dockerfile root directory, define hello **Image Name**, and select **Include Latest Tag**.</span></span>
    
    <span data-ttu-id="8c57d-193">Hello název bitové kopie má toobe v tomto formátu: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span><span class="sxs-lookup"><span data-stu-id="8c57d-193">hello Image Name has toobe in this format: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span></span> <span data-ttu-id="8c57d-194">Nahraďte **[NAME]** s hello název bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="8c57d-194">Replace **[NAME]** with hello image name:</span></span>
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. <span data-ttu-id="8c57d-195">Pro každé bitové kopie, nakonfigurujte druhý krok, který používá hello `docker push` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8c57d-195">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    <span data-ttu-id="8c57d-197">Pro hello pomocí operace push, vyberte kontejner Azure registr, hello **Push bitovou kopii** akce, zadejte hello **název bitové kopie** který je vytvořen v předchozím kroku hello a vyberte **zahrnout nejnovější značky** .</span><span class="sxs-lookup"><span data-stu-id="8c57d-197">For hello push operation, select your Azure container registry, hello **Push an image** action, enter hello **Image Name** that is built in hello previous step and select **Include Latest Tag**.</span></span>

4. <span data-ttu-id="8c57d-198">Po dokončení můžete konfigurovat hello sestavení a push kroky pro každý hello pět bitových kopií, přidejte že tři další kroky v hello sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="8c57d-198">After you configure hello build and push steps for each of hello five images, add three more steps in hello build workflow.</span></span>

   ![Visual Studio Team Services - přidat úlohu příkazového řádku](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. <span data-ttu-id="8c57d-200">Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *RegistryURL* výskyt v soubor docker-compose.yml hello s proměnnou RegistryURL hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-200">A command-line task that uses a bash script tooreplace hello *RegistryURL* occurrence in hello docker-compose.yml file with hello RegistryURL variable.</span></span> 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - vytvářené aktualizace souboru s adresou URL registru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. <span data-ttu-id="8c57d-202">Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *AgentURL* výskyt v soubor docker-compose.yml hello s proměnnou AgentURL hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-202">A command-line task that uses a bash script tooreplace hello *AgentURL* occurrence in hello docker-compose.yml file with hello AgentURL variable.</span></span>
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. <span data-ttu-id="8c57d-203">Úloha, která zahodí hello aktualizovat vytvářené souboru, protože sestavení artefaktů, je možné v hello verzi.</span><span class="sxs-lookup"><span data-stu-id="8c57d-203">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="8c57d-204">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8c57d-204">See hello following screen for details.</span></span>

         ![Visual Studio Team Services - publikování artefaktů](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. <span data-ttu-id="8c57d-207">Klikněte na tlačítko **Uložit & fronty** tootest vaší definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c57d-207">Click **Save & queue** tootest your build definition.</span></span>

   ![Visual Studio Team Services - uložit a fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services – nové fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. <span data-ttu-id="8c57d-210">Pokud hello **sestavení** je správný, máte toosee Tato obrazovka:</span><span class="sxs-lookup"><span data-stu-id="8c57d-210">If hello **Build** is correct, you have toosee this screen:</span></span>

  ![Visual Studio Team Services - sestavení bylo úspěšně dokončeno](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="8c57d-212">Krok 3: Vytvoření definice verze hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-212">Step 3: Create hello release definition</span></span>

<span data-ttu-id="8c57d-213">Visual Studio Team Services umožňuje příliš[Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="8c57d-213">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="8c57d-214">Průběžné nasazování toomake nasazení aplikace na vašich různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="8c57d-214">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="8c57d-215">Můžete vytvořit prostředí, které představuje váš cluster Azure Container Service Docker Swarm režimu.</span><span class="sxs-lookup"><span data-stu-id="8c57d-215">You can create an environment that represents your Azure Container Service Docker Swarm Mode cluster.</span></span>

![Visual Studio Team Services - tooACS verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="8c57d-217">Instalační program původní verze</span><span class="sxs-lookup"><span data-stu-id="8c57d-217">Initial release setup</span></span>

1. <span data-ttu-id="8c57d-218">Klikněte na tlačítko toocreate definici verze **verze** > **+ verze**</span><span class="sxs-lookup"><span data-stu-id="8c57d-218">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="8c57d-219">tooconfigure hello artefaktů zdroje, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-219">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="8c57d-220">Zde propojte tento nový definice toohello sestavení pro vydání který jste zadali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-220">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="8c57d-221">Potom je k dispozici v procesu verze hello soubor docker-compose.yml hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-221">After that, hello docker-compose.yml file is available in hello release process.</span></span>

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. <span data-ttu-id="8c57d-223">tooconfigure hello verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-223">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="8c57d-224">Aktivační událost hello sadu pro hello stejný zdroj artefaktů.</span><span class="sxs-lookup"><span data-stu-id="8c57d-224">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="8c57d-225">Toto nastavení zajistí, že nová verze začne po úspěšném dokončení sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-225">This setting ensures that a new release starts when hello build completes successfully.</span></span>

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. <span data-ttu-id="8c57d-227">Klikněte na tlačítko tooconfigure hello verze proměnné, **proměnné** a vyberte **+ proměnné** toocreate tři nové proměnné s informacemi hello hello registru: **docker.username**, **docker.password**, a **docker.registry**.</span><span class="sxs-lookup"><span data-stu-id="8c57d-227">tooconfigure hello release variables, click **Variables** and select **+Variable** toocreate three new variables with hello info of hello registry: **docker.username**, **docker.password**, and **docker.registry**.</span></span> <span data-ttu-id="8c57d-228">Vložte hello hodnoty registru a DNS agenty clusteru.</span><span class="sxs-lookup"><span data-stu-id="8c57d-228">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > <span data-ttu-id="8c57d-230">Jak je znázorněno na předchozí úvodní obrazovka, klikněte na tlačítko hello **zámku** zaškrtnout políčko docker.password.</span><span class="sxs-lookup"><span data-stu-id="8c57d-230">As shown on hello previous screen, click hello **Lock** checkbox in docker.password.</span></span> <span data-ttu-id="8c57d-231">Toto nastavení je důležité toorestrict hello heslo.</span><span class="sxs-lookup"><span data-stu-id="8c57d-231">This setting is important toorestrict hello password.</span></span>
    >

### <a name="define-hello-release-workflow"></a><span data-ttu-id="8c57d-232">Definice pracovního postupu verze hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-232">Define hello release workflow</span></span>

<span data-ttu-id="8c57d-233">verze Hello pracovní postup se skládá z dvě úlohy, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="8c57d-233">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="8c57d-234">Konfigurace úloh toosecurely kopie hello tvoří soubor tooa *nasazení* složky na hello Docker Swarm hlavního uzlu, pomocí připojení SSH hello jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="8c57d-234">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="8c57d-235">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8c57d-235">See hello following screen for details.</span></span>
    
    <span data-ttu-id="8c57d-236">Zdrojová složka:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span><span class="sxs-lookup"><span data-stu-id="8c57d-236">Source folder: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span></span>

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. <span data-ttu-id="8c57d-238">Nakonfigurujte druhý tooexecute úloh příkaz toorun bash `docker` a `docker stack deploy` příkazů na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-238">Configure a second task tooexecute a bash command toorun `docker` and `docker stack deploy` commands on hello master node.</span></span> <span data-ttu-id="8c57d-239">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8c57d-239">See hello following screen for details.</span></span>

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    <span data-ttu-id="8c57d-241">příkaz Hello proveden na hlavní server hello používá hello příkazového řádku Dockeru a hello Docker Compose CLI toodo hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="8c57d-241">hello command executed on hello master uses hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="8c57d-242">Protokolu v registru toohello kontejner Azure (používá tři proměnné sestavení, které jsou definovány v hello **proměnné** karty)</span><span class="sxs-lookup"><span data-stu-id="8c57d-242">Log in toohello Azure container registry (it uses three build variables that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="8c57d-243">Definování hello **DOCKER_HOST** proměnné toowork koncovému bodu Swarm hello (: 2375)</span><span class="sxs-lookup"><span data-stu-id="8c57d-243">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="8c57d-244">Přejděte toohello *nasazení* složky, který byl vytvořen hello předcházející úlohy zabezpečené kopie a který obsahuje soubor docker-compose.yml hello</span><span class="sxs-lookup"><span data-stu-id="8c57d-244">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="8c57d-245">Spuštění `docker stack deploy` příkazy, které hello nových bitových kopií pro vyžádání obsahu a vytvoření kontejnerů hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-245">Execute `docker stack deploy` commands that pull hello new images and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="8c57d-246">Jak je znázorněno na předchozí obrazovce hello, nechte hello **nezdaří do datového proudu STDERR** nezaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="8c57d-246">As shown on hello previous screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="8c57d-247">Toto nastavení umožňuje nám toocomplete hello verzi z důvodu příliš`docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo se na výstup hello standardní chyba odstraněn.</span><span class="sxs-lookup"><span data-stu-id="8c57d-247">This setting allows us toocomplete hello release process due too`docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="8c57d-248">Pokud zaškrtnete políčko hello, hlásí Visual Studio Team Services nastaly chyby během hello verzi, i pokud všechno proběhne správně.</span><span class="sxs-lookup"><span data-stu-id="8c57d-248">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="8c57d-249">Uložte tuto novou verzi definici.</span><span class="sxs-lookup"><span data-stu-id="8c57d-249">Save this new release definition.</span></span>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="8c57d-250">Krok 4: Test hello CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="8c57d-250">Step 4: Test hello CI/CD pipeline</span></span>

<span data-ttu-id="8c57d-251">Teď, když jste hotovi s konfigurací hello, je čas tootest tento nový kanál CI/CD.</span><span class="sxs-lookup"><span data-stu-id="8c57d-251">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="8c57d-252">Hello nejjednodušší způsob, jak tootest je hello tooupdate hello zdrojového kódu a provést změny do úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="8c57d-252">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="8c57d-253">Několik sekund poté, co push hello kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8c57d-253">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="8c57d-254">Po úspěšném dokončení novou verzi je aktivována a nasazena hello nové verze aplikace hello na clusteru Azure Container Service hello.</span><span class="sxs-lookup"><span data-stu-id="8c57d-254">Once completed successfully, a new release is triggered and deployed hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c57d-255">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c57d-255">Next steps</span></span>

* <span data-ttu-id="8c57d-256">Další informace o CI/CD s Visual Studio Team Services najdete v tématu hello [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="8c57d-256">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
* <span data-ttu-id="8c57d-257">Další informace o modulu služby ACS, najdete v části hello [úložiště GitHub modul ACS](https://github.com/Azure/acs-engine).</span><span class="sxs-lookup"><span data-stu-id="8c57d-257">For more information about ACS Engine, see hello [ACS Engine GitHub repo](https://github.com/Azure/acs-engine).</span></span>
* <span data-ttu-id="8c57d-258">Další informace o Docker Swarm režimu najdete v tématu hello [Docker Swarm přehled režimu](https://docs.docker.com/engine/swarm/).</span><span class="sxs-lookup"><span data-stu-id="8c57d-258">For more information about Docker Swarm mode, see hello [Docker Swarm mode overview](https://docs.docker.com/engine/swarm/).</span></span>
