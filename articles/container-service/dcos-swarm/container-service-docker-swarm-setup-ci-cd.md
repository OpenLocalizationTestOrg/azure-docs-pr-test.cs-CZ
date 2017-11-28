---
title: aaaCI/CD s Azure Container Service a Swarm | Microsoft Docs
description: "Pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services toodeliver nepřetržitě aplikace .NET Core více kontejnerů Azure Container Service"
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
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="a3bb5-104">Úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="a3bb5-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="a3bb5-105">Jeden z hello nejnáročnějších úkolů při vývoj moderních aplikací pro hello cloud se může toodeliver tyto aplikace průběžně.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-105">One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="a3bb5-106">V tomto článku se dozvíte, jak vytvářet tooimplement úplné průběžnou integraci a nasazení (CI/CD) kanálu Azure Container Service pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services a správa verzí.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-106">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="a3bb5-107">Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/acs-docs), vyvinuté pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="a3bb5-108">Hello aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-108">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="a3bb5-110">cíl Hello je toodeliver tuto aplikaci nepřetržitě v clusteru Docker Swarm, pomocí sady Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-110">hello objective is toodeliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="a3bb5-111">Hello následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-111">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="a3bb5-113">Následuje stručné vysvětlení hello kroky:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-113">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="a3bb5-114">Změny kódu jsou potvrzené toohello úložiště zdrojového kódu (tady Githubu)</span><span class="sxs-lookup"><span data-stu-id="a3bb5-114">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="a3bb5-115">GitHub aktivuje build ve Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="a3bb5-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="a3bb5-116">Visual Studio Team Services získá hello nejnovější verzi hello zdroje a vytvoří všechny hello bitové kopie, které tvoří aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-116">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="a3bb5-117">Visual Studio Team Services doručí registru Docker tooa každé bitové kopie vytvořené pomocí služby Azure kontejneru registru hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-117">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="a3bb5-118">Visual Studio Team Services aktivuje novou verzí</span><span class="sxs-lookup"><span data-stu-id="a3bb5-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="a3bb5-119">verze Hello spouští některé příkazy hlavního uzlu clusteru serveru hello Azure container service pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="a3bb5-119">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="a3bb5-120">Docker Swarm hello clusteru si hello nejnovější verzi hello obrázků</span><span class="sxs-lookup"><span data-stu-id="a3bb5-120">Docker Swarm on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="a3bb5-121">Hello nové verze aplikace hello se nasazuje pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="a3bb5-121">hello new version of hello application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a3bb5-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a3bb5-122">Prerequisites</span></span>

<span data-ttu-id="a3bb5-123">Před zahájením tohoto kurzu potřebujete toocomplete hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-123">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="a3bb5-124">Vytvoření clusteru Swarm v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="a3bb5-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="a3bb5-125">Propojení s clusterem Swarm hello v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="a3bb5-125">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="a3bb5-126">Vytvoření služby Azure kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="a3bb5-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="a3bb5-127">Máte účet a tým projekt Visual Studio Team Services vytvořen</span><span class="sxs-lookup"><span data-stu-id="a3bb5-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="a3bb5-128">Rozvětvení hello Githubu úložiště tooyour účet GitHub</span><span class="sxs-lookup"><span data-stu-id="a3bb5-128">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="a3bb5-129">Budete také potřebovat počítače Ubuntu (14.04 a 16.04) s Docker nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="a3bb5-130">Tento počítač se používá ve Visual Studio Team Services během hello sestavení a verze procesy.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-130">This machine is used by Visual Studio Team Services during hello build and release processes.</span></span> <span data-ttu-id="a3bb5-131">Jedním ze způsobů toocreate tento počítač je k dispozici v hello image hello toouse [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="a3bb5-131">One way toocreate this machine is toouse hello image available in hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="a3bb5-132">Krok 1: Konfigurace účtu Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="a3bb5-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="a3bb5-133">V této části můžete nakonfigurovat účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="a3bb5-134">Konfigurace agenta pro Visual Studio Team Services Linux sestavení</span><span class="sxs-lookup"><span data-stu-id="a3bb5-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="a3bb5-135">toocreate imagí Dockeru a nabízených sestavení těchto bitových kopií do služby Azure kontejneru registru z Visual Studio Team Services, musíte tooregister agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-135">toocreate Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need tooregister a Linux agent.</span></span> <span data-ttu-id="a3bb5-136">Máte tyto možnosti instalace:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-136">You have these installation options:</span></span>

* [<span data-ttu-id="a3bb5-137">Nasazení agenta v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a3bb5-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="a3bb5-138">Pomocí Docker toorun hello služby VSTS agenta</span><span class="sxs-lookup"><span data-stu-id="a3bb5-138">Use Docker toorun hello VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a><span data-ttu-id="a3bb5-139">Instalace rozšíření služby VSTS integrace Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-139">Install hello Docker Integration VSTS extension</span></span>

<span data-ttu-id="a3bb5-140">Společnost Microsoft poskytuje toowork služby VSTS rozšíření s Docker v sestavení a verze procesy.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-140">Microsoft provides a VSTS extension toowork with Docker in build and release processes.</span></span> <span data-ttu-id="a3bb5-141">Toto rozšíření je k dispozici v hello [služby VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="a3bb5-141">This extension is available in hello [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="a3bb5-142">Klikněte na tlačítko **nainstalovat** tooadd tento účet služby VSTS tooyour rozšíření:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-142">Click **Install** tooadd this extension tooyour VSTS account:</span></span>

![Nainstalujte hello integrace Dockeru](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="a3bb5-144">Zobrazí se výzva tooconnect tooyour služby VSTS účtu pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-144">You are asked tooconnect tooyour VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="a3bb5-145">Připojit Visual Studio Team Services a GitHub</span><span class="sxs-lookup"><span data-stu-id="a3bb5-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="a3bb5-146">Nastavte připojení mezi projektu služby VSTS a účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="a3bb5-147">V projektu Visual Studio Team Services, klikněte na tlačítko hello **nastavení** v hello nástrojů a vyberte ikonu **služby**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-147">In your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

    ![Visual Studio Team Services - externí připojení](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="a3bb5-149">Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Githubu**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-149">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="a3bb5-151">služby VSTS toowork tooauthorize s vaším účtem Githubu, klikněte na tlačítko **Authorize** a použijte postup hello v otevřeném okně hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-151">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="a3bb5-153">Připojení služby VSTS tooyour kontejner Azure registru a cluster Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="a3bb5-153">Connect VSTS tooyour Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="a3bb5-154">Hello poslední kroky před získáním do kanálu hello CI/CD jsou tooconfigure externí připojení tooyour kontejneru registru a cluster Docker Swarm v Azure.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-154">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="a3bb5-155">V hello **služby** nastavení projektu Visual Studio Team Services přidat koncový bod služby typu **Docker registru**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-155">In hello **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="a3bb5-156">V překryvném hello, které se otevře zadejte hello URL a pověření hello registru systému Windows Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-156">In hello popup that opens, enter hello URL and hello credentials of your Azure container registry.</span></span>

    ![Visual Studio Team Services - Docker registru](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="a3bb5-158">Pro cluster hello Docker Swarm, přidání koncového bodu typu **SSH**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-158">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="a3bb5-159">Zadejte informace o připojení SSH hello clusteru Swarm.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-159">Then enter hello SSH connection information of your Swarm cluster.</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="a3bb5-161">Všechny konfigurace hello je nyní provádí.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-161">All hello configuration is done now.</span></span> <span data-ttu-id="a3bb5-162">V dalších krocích hello můžete vytvořit hello CI/CD kanálu, který vytvoří a nasadí hello aplikace toohello Docker Swarm clusteru.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-162">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="a3bb5-163">Krok 2: Vytvoření definici sestavení hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-163">Step 2: Create hello build definition</span></span>

<span data-ttu-id="a3bb5-164">V tomto kroku můžete nastavit definitionfor sestavení projektu služby VSTS a definovat hello sestavení pracovního postupu pro kontejner obrázků</span><span class="sxs-lookup"><span data-stu-id="a3bb5-164">In this step, you set up a build definitionfor your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="a3bb5-165">Definice počáteční instalace</span><span class="sxs-lookup"><span data-stu-id="a3bb5-165">Initial definition setup</span></span>

1. <span data-ttu-id="a3bb5-166">toocreate definice sestavení, připojit tooyour Visual Studio Team Services projektu a klikněte na tlačítko **sestavení a verze**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-166">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="a3bb5-167">V hello **sestavení definice** klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-167">In hello **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="a3bb5-168">Vyberte hello **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-168">Select hello **Empty** template.</span></span>

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="a3bb5-170">Konfigurace nového sestavení hello se zdrojem úložiště Githubu, zkontrolujte **průběžnou integraci**a vyberte hello agenta fronty, kde jste zaregistrovali Linux agent.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-170">Configure hello new build with a GitHub repository source, check **Continuous integration**, and select hello agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="a3bb5-171">Klikněte na tlačítko **vytvořit** toocreate hello sestavení definice.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-171">Click **Create** toocreate hello build definition.</span></span>

    ![Visual Studio Team Services – vytvoření definice sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="a3bb5-173">Na hello **definice sestavení** stránky, poprvé otevřete hello **úložiště** a nakonfigurujte hello sestavení toouse hello rozvětvení hello MyShop projektu, který jste vytvořili v hello požadavky.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-173">On hello **Build Definitions** page, first open hello **Repository** tab and configure hello build toouse hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="a3bb5-174">Ujistěte se, že jste vybrali *acs dokumentace* jako hello **výchozí větev**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-174">Make sure that you select *acs-docs* as hello **Default branch**.</span></span>

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="a3bb5-176">Na hello **aktivační události** nakonfigurujte hello sestavení toobe spustí po každém potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-176">On hello **Triggers** tab, configure hello build toobe triggered after each commit.</span></span> <span data-ttu-id="a3bb5-177">Vyberte **průběžnou integraci** a **dávky změny**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - konfigurace aktivační události sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="a3bb5-179">Definování hello sestavení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="a3bb5-179">Define hello build workflow</span></span>
<span data-ttu-id="a3bb5-180">Další kroky Hello definovat hello sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-180">hello next steps define hello build workflow.</span></span> <span data-ttu-id="a3bb5-181">Existují pět toobuild bitové kopie kontejner pro hello *MyShop* aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-181">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="a3bb5-182">Každé bitové kopie je sestaven pomocí hello soubor Docker umístěný v hello složky projektu:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-182">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="a3bb5-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="a3bb5-183">ProductsApi</span></span>
* <span data-ttu-id="a3bb5-184">Proxy server</span><span class="sxs-lookup"><span data-stu-id="a3bb5-184">Proxy</span></span>
* <span data-ttu-id="a3bb5-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="a3bb5-185">RatingsApi</span></span>
* <span data-ttu-id="a3bb5-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="a3bb5-186">RecommandationsApi</span></span>
* <span data-ttu-id="a3bb5-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="a3bb5-187">ShopFront</span></span>

<span data-ttu-id="a3bb5-188">Budete potřebovat dva kroky Docker tooadd každé bitové kopie, jednu toobuild hello image a jednu image hello toopush v registru kontejner Azure hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-188">You need tooadd two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="a3bb5-189">Klikněte na tlačítko tooadd krok v postupu sestavení hello **+ přidat krok sestavení** a vyberte **Docker**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-189">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="a3bb5-191">Pro každé bitové kopie, nakonfigurovat jeden krok, který používá hello `docker build` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-191">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="a3bb5-193">Pro hello sestavení operaci, vyberte kontejner Azure registr, hello **vytvořit bitovou kopii** akce a hello soubor Docker, která definuje každé bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-193">For hello build operation, select your Azure container registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="a3bb5-194">Sada hello **sestavení kontextu** jako hello soubor Docker kořenový adresář a definujte hello **název bitové kopie**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-194">Set hello **Build context** as hello Dockerfile root directory, and define hello **Image Name**.</span></span> 
    
    <span data-ttu-id="a3bb5-195">Jak je vidět na předchozím obrazovky hello, začněte název bitové kopie hello hello URI registru systému Windows Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-195">As shown on hello preceding screen, start hello image name with hello URI of your Azure container registry.</span></span> <span data-ttu-id="a3bb5-196">(Můžete také použít značku sestavení proměnné tooparameterize hello hello Image, jako je například identifikátor hello sestavení v tomto příkladu.)</span><span class="sxs-lookup"><span data-stu-id="a3bb5-196">(You can also use a build variable tooparameterize hello tag of hello image, such as hello build identifier in this example.)</span></span>

3. <span data-ttu-id="a3bb5-197">Pro každé bitové kopie, nakonfigurujte druhý krok, který používá hello `docker push` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-197">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="a3bb5-199">Pro hello pomocí operace push, vyberte kontejner Azure registr, hello **Push bitovou kopii** akce a zadejte hello **název bitové kopie** který je vytvořen v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-199">For hello push operation, select your Azure container registry, hello **Push an image** action, and enter hello **Image Name** that is built in hello previous step.</span></span>

4. <span data-ttu-id="a3bb5-200">Po dokončení můžete konfigurovat hello sestavení a push kroky pro každý hello pět bitových kopií, přidejte že dva další kroky v hello sestavení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-200">After you configure hello build and push steps for each of hello five images, add two more steps in hello build workflow.</span></span>

    <span data-ttu-id="a3bb5-201">a.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-201">a.</span></span> <span data-ttu-id="a3bb5-202">Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *BuildNumber* výskyt v soubor docker-compose.yml hello s hello aktuální sestavení ID. V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-202">A command-line task that uses a bash script tooreplace hello *BuildNumber* occurrence in hello docker-compose.yml file with hello current build Id. See hello following screen for details.</span></span>

    ![Visual Studio Team Services - vytvářené aktualizace souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="a3bb5-204">b.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-204">b.</span></span> <span data-ttu-id="a3bb5-205">Úloha, která zahodí hello aktualizovat vytvářené souboru, protože sestavení artefaktů, je možné v hello verzi.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-205">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="a3bb5-206">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-206">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="a3bb5-208">Klikněte na tlačítko **Uložit** a název vaší definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="a3bb5-209">Krok 3: Vytvoření definice verze hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-209">Step 3: Create hello release definition</span></span>

<span data-ttu-id="a3bb5-210">Visual Studio Team Services umožňuje příliš[Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="a3bb5-210">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="a3bb5-211">Průběžné nasazování toomake nasazení aplikace na vašich různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-211">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="a3bb5-212">Můžete vytvořit nové prostředí, které představuje váš cluster Azure Container Service Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![Visual Studio Team Services - tooACS verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="a3bb5-214">Instalační program původní verze</span><span class="sxs-lookup"><span data-stu-id="a3bb5-214">Initial release setup</span></span>

1. <span data-ttu-id="a3bb5-215">Klikněte na tlačítko toocreate definici verze **verze** > **+ verze**</span><span class="sxs-lookup"><span data-stu-id="a3bb5-215">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="a3bb5-216">tooconfigure hello artefaktů zdroje, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-216">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="a3bb5-217">Zde propojte tento nový definice toohello sestavení pro vydání který jste zadali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-217">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="a3bb5-218">Díky tomu je k dispozici v procesu verze hello soubor docker-compose.yml hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-218">By doing this, hello docker-compose.yml file is available in hello release process.</span></span>

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="a3bb5-220">tooconfigure hello verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-220">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="a3bb5-221">Aktivační událost hello sadu pro hello stejný zdroj artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-221">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="a3bb5-222">Toto nastavení zajistí, že začne novou verzi také hello sestavení se dokončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-222">This setting ensures that a new release starts as soon as hello build completes successfully.</span></span>

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a><span data-ttu-id="a3bb5-224">Definice pracovního postupu verze hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-224">Define hello release workflow</span></span>

<span data-ttu-id="a3bb5-225">verze Hello pracovní postup se skládá z dvě úlohy, které přidáte.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-225">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="a3bb5-226">Konfigurace úloh toosecurely kopie hello tvoří soubor tooa *nasazení* složky na hello Docker Swarm hlavního uzlu, pomocí připojení SSH hello jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-226">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="a3bb5-227">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-227">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="a3bb5-229">Nakonfigurujte druhý tooexecute úloh příkaz toorun bash `docker` a `docker-compose` příkazů na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-229">Configure a second task tooexecute a bash command toorun `docker` and `docker-compose` commands on hello master node.</span></span> <span data-ttu-id="a3bb5-230">V tématu hello následující obrazovka podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-230">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="a3bb5-232">příkaz Hello proveden na hello hello hlavní pomocí příkazového řádku Dockeru a hello Docker Compose CLI toodo hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="a3bb5-232">hello command executed on hello master use hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="a3bb5-233">Přihlášení toohello kontejner Azure registru (používá tři variab'les sestavení, která jsou definována v hello **proměnné** karty)</span><span class="sxs-lookup"><span data-stu-id="a3bb5-233">Login toohello Azure container registry (it uses three build variab\`les that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="a3bb5-234">Definování hello **DOCKER_HOST** proměnné toowork koncovému bodu Swarm hello (: 2375)</span><span class="sxs-lookup"><span data-stu-id="a3bb5-234">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="a3bb5-235">Přejděte toohello *nasazení* složky, který byl vytvořen hello předcházející úlohy zabezpečené kopie a který obsahuje soubor docker-compose.yml hello</span><span class="sxs-lookup"><span data-stu-id="a3bb5-235">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="a3bb5-236">Spuštění `docker-compose` příkazy, které hello nové bitové kopie, pro vyžádání obsahu zastavení služeb hello, odeberte hello služby a vytvoření kontejnerů hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-236">Execute `docker-compose` commands that pull hello new images, stop hello services, remove hello services, and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="a3bb5-237">Jak je vidět na předchozím obrazovky hello, nechte hello **nezdaří do datového proudu STDERR** nezaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-237">As shown on hello preceding screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="a3bb5-238">To je důležité nastavit, protože `docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo se na výstup hello standardní chyba odstraněn.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="a3bb5-239">Pokud zaškrtnete políčko hello, hlásí Visual Studio Team Services nastaly chyby během hello verzi, i pokud všechno proběhne správně.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-239">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="a3bb5-240">Uložte tuto novou verzi definici.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="a3bb5-241">Toto nasazení obsahuje výpadky, protože jsme zastavení služeb staré hello a systémem hello nový.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-241">This deployment includes some downtime because we are stopping hello old services and running hello new one.</span></span> <span data-ttu-id="a3bb5-242">Ho je možné tooavoid to pomocí tohoto postupu nasazení blue zeleně.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-242">It is possible tooavoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="a3bb5-243">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-243">Step 4.</span></span> <span data-ttu-id="a3bb5-244">Test hello CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="a3bb5-244">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="a3bb5-245">Teď, když jste hotovi s konfigurací hello, je čas tootest tento nový kanál CI/CD.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-245">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="a3bb5-246">Hello nejjednodušší způsob, jak tootest je hello tooupdate hello zdrojového kódu a provést změny do úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-246">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="a3bb5-247">Několik sekund poté, co push hello kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-247">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="a3bb5-248">Po úspěšném dokončení novou verzi se spustí a provede nasazení nové verze aplikace hello na clusteru Azure Container Service hello hello.</span><span class="sxs-lookup"><span data-stu-id="a3bb5-248">Once completed successfully, a new release will be triggered and will deploy hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3bb5-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3bb5-249">Next Steps</span></span>

* <span data-ttu-id="a3bb5-250">Další informace o CI/CD s Visual Studio Team Services najdete v tématu hello [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="a3bb5-250">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
