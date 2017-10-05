---
title: "CI/CD z volaných pro virtuální počítače Azure se Team Services | Microsoft Docs"
description: "Nastavte průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí volaných k virtuálním počítačům Azure ze správy verzí v aplikaci Visual Studio Team Services (VSTS) nebo Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="4e45c-103">Nasazení aplikace do virtuální počítače s Linuxem pomocí volaných a Team Services</span><span class="sxs-lookup"><span data-stu-id="4e45c-103">Deploy your app to Linux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="4e45c-104">Průběžnou integraci (CI) a průběžné nasazování (CD) je kanál, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód.</span><span class="sxs-lookup"><span data-stu-id="4e45c-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="4e45c-105">Team Services poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="4e45c-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment to Azure.</span></span> <span data-ttu-id="4e45c-106">Volaných je Oblíbené 3. stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace.</span><span class="sxs-lookup"><span data-stu-id="4e45c-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="4e45c-107">Můžete i společně k přizpůsobení jak poskytovat cloudové aplikace nebo služby.</span><span class="sxs-lookup"><span data-stu-id="4e45c-107">You can use both together to customize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="4e45c-108">V tomto kurzu použijete k sestavení volaných **webové aplikace Node.js**a Visual Studio Team Services k jeho nasazení [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4e45c-108">In this tutorial, you use Jenkins to build a **Node.js web app**, and Visual Studio Team Services to deploy it to a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="4e45c-109">Vaším úkolem je:</span><span class="sxs-lookup"><span data-stu-id="4e45c-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4e45c-110">Sestavení aplikace ve volaných</span><span class="sxs-lookup"><span data-stu-id="4e45c-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="4e45c-111">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="4e45c-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="4e45c-112">Vytvořit skupinu nasazení pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4e45c-112">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="4e45c-113">Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci</span><span class="sxs-lookup"><span data-stu-id="4e45c-113">Create a release definition that configures the VMs and deploys the app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4e45c-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4e45c-114">Before you begin</span></span>

* <span data-ttu-id="4e45c-115">Potřebujete přístup k účtu volaných.</span><span class="sxs-lookup"><span data-stu-id="4e45c-115">You need access to a Jenkins account.</span></span> <span data-ttu-id="4e45c-116">Pokud jste ještě nevytvořili volaných server, přečtěte si téma [volaných dokumentaci](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="4e45c-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="4e45c-117">Přihlaste se ke svému účtu Team Services (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="4e45c-117">Sign in to your Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="4e45c-118">Můžete získat [bezplatný účet Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="4e45c-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="4e45c-119">Další informace najdete v tématu [připojit k Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="4e45c-119">For more information, see [Connect to Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="4e45c-120">Pokud jste ještě nemáte, vytvořte osobní přístupový token (Jan) ve vašem účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="4e45c-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="4e45c-121">Volaných vyžaduje zadání těchto informací pro přístup k účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="4e45c-121">Jenkins requires this information to access your Team Services account.</span></span>
  <span data-ttu-id="4e45c-122">Čtení [vytvoření osobního přístupového tokenu pro Team Services a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) se dozvíte, jak jej vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="4e45c-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to learn how to generate one.</span></span>

## <a name="get-the-sample-app"></a><span data-ttu-id="4e45c-123">Načíst ukázková aplikace</span><span class="sxs-lookup"><span data-stu-id="4e45c-123">Get the sample app</span></span>

<span data-ttu-id="4e45c-124">Je nutné aplikaci k nasazení uložené v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="4e45c-124">You need an app to deploy stored in a Git repository.</span></span>
<span data-ttu-id="4e45c-125">V tomto kurzu doporučujeme, abyste použili [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="4e45c-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="4e45c-126">Vytvoření větve tuto aplikaci a poznamenejte si umístění (URL) pro použití v dalších krocích tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-126">Create a fork of this app and take note of the location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="4e45c-127">Ujistěte se, rozvětvení **veřejné** ke zjednodušení připojení ke Githubu později.</span><span class="sxs-lookup"><span data-stu-id="4e45c-127">Make the fork **public** to simplify connecting to GitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="4e45c-128">Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/) a [nastavíte privátní úložiště jako veřejné](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="4e45c-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="4e45c-129">Aplikace bylo vytvořeno prostřednictvím [Yeoman](http://yeoman.io/learning/index.html); používá **Express**, **bower**, a **grunt**; a má některé **npm** balíčky jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="4e45c-129">The app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="4e45c-130">Ukázková aplikace obsahuje sadu [šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) používané dynamicky vytváření virtuálních počítačů pro nasazení v Azure.</span><span class="sxs-lookup"><span data-stu-id="4e45c-130">The sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used to dynamically create the virtual machines for deployment on Azure.</span></span> <span data-ttu-id="4e45c-131">Tyto šablony jsou používány úlohy v [Team Services verze definice](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="4e45c-131">These templates are used by tasks in the [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="4e45c-132">Hlavní šablona vytvoří skupinu zabezpečení sítě, virtuální počítač a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4e45c-132">The main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="4e45c-133">Ho přiřadí veřejnou IP adresu a otevře příchozí port 80.</span><span class="sxs-lookup"><span data-stu-id="4e45c-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="4e45c-134">Také přidá značku, který je používán skupiny nasazení vybrat počítače pro nasazení obdrží.</span><span class="sxs-lookup"><span data-stu-id="4e45c-134">It also adds a tag that is used by the deployment group to select the machines to receive the deployment.</span></span>
>
> <span data-ttu-id="4e45c-135">Ukázka také obsahuje skript, který nastaví Nginx a nasadí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e45c-135">The sample also contains a script that sets up Nginx and deploys the app.</span></span> <span data-ttu-id="4e45c-136">Spuštění na každý z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e45c-136">It is executed on each of the virtual machines.</span></span> <span data-ttu-id="4e45c-137">Konkrétně skript nainstaluje uzlu, Nginx a PM2; Nakonfiguruje Nginx a PM2; pak spustí aplikaci uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-137">Specifically, the script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts the Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="4e45c-138">Konfigurace volaných moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="4e45c-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="4e45c-139">Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in pro **NodeJS** a **integraci se službou Team Services**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="4e45c-140">Otevřete váš účet volaných a zvolte **spravovat volaných**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="4e45c-141">V **spravovat volaných** vyberte **Správa modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-141">In the **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="4e45c-142">Filtrování seznamu a vyhledejte **NodeJS** modulů plug-in a nainstalovat bez restartování.</span><span class="sxs-lookup"><span data-stu-id="4e45c-142">Filter the list to locate the **NodeJS** plugin and install it without restart.</span></span>

   ![Přidání modulu plug-in NodeJS do volaných](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="4e45c-144">Filtrovat seznam a vyhledat **Team Foundation Server** modulů plug-in a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="4e45c-144">Filter the list to find the **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="4e45c-145">(Tento modul plug-in funguje pro Team Services a serveru Team Foundation Server.) Restartování volaných není nutné.</span><span class="sxs-lookup"><span data-stu-id="4e45c-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="4e45c-146">Konfigurace sestavení volaných pro Node.js</span><span class="sxs-lookup"><span data-stu-id="4e45c-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="4e45c-147">Volaných vytvořte nový projekt sestavení a nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e45c-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="4e45c-148">V **Obecné** zadejte název pro sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-148">In the **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="4e45c-149">V **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o úložišti a větev obsahující kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e45c-149">In the **Source Code Management** tab, select **Git** and enter the details of the repository and the branch containing your app code.</span></span>

   ![Přidat úložišti do vaší sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="4e45c-151">Pokud úložiště není veřejný, zvolte **přidat** a zadejte pověření pro připojení k němu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-151">If your repository is not public, choose **Add** and provide credentials to connect to it.</span></span>

1. <span data-ttu-id="4e45c-152">V **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán `H/03 * * * *` k dotazování úložiště Git pro změny každé tři minuty.</span><span class="sxs-lookup"><span data-stu-id="4e45c-152">In the **Build Triggers** tab, select **Poll SCM** and enter the schedule `H/03 * * * *` to poll the Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="4e45c-153">V **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a zadejte `NodeJS` pro hodnotu instalace JS uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-153">In the **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for the Node JS Installation value.</span></span> <span data-ttu-id="4e45c-154">Nechte **npmrc souboru** nastavena na "použít výchozí systémové nastavení."</span><span class="sxs-lookup"><span data-stu-id="4e45c-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="4e45c-155">V **sestavení** kartě, zadejte příkaz `npm install` zajistit jsou aktualizovány všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="4e45c-155">In the **Build** tab, enter the command `npm install` to ensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="4e45c-156">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="4e45c-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="4e45c-157">V **akce po sestavení** kartě pro **soubory k archivaci**, zadejte `**/*` zahrnout všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="4e45c-157">In the **Post-build Actions** tab, for **Files to archive**, enter `**/*` to include all files.</span></span>

1. <span data-ttu-id="4e45c-158">Pro **aktivovat verze v TFS/Team Services**, zadejte úplnou adresu URL vašeho účtu (například `https://your-account-name.visualstudio.com`), název projektu, název pro definici verze (vytvořit později) a pověření pro připojení k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-158">For **Trigger release in TFS/Team Services**, enter the full URL of your account (such as `https://your-account-name.visualstudio.com`), the project name, a name for the release definition (created later), and the credentials to connect to your account.</span></span>
   <span data-ttu-id="4e45c-159">Budete potřebovat vaše uživatelské jméno a Jana jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="4e45c-159">You need your user name and the PAT you created earlier.</span></span> 

   ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="4e45c-161">Uložte sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-161">Save the build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="4e45c-162">Vytvoření koncového bodu služby volaných</span><span class="sxs-lookup"><span data-stu-id="4e45c-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="4e45c-163">Koncový bod služby umožňuje Team Services pro připojení k volaných.</span><span class="sxs-lookup"><span data-stu-id="4e45c-163">A service endpoint allows Team Services to connect to Jenkins.</span></span>

1. <span data-ttu-id="4e45c-164">Otevřete **služby** stránky v Team Services, otevřete **nový koncový bod služby** seznam a vyberte **volaných**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-164">Open the **Services** page in Team Services, open the **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="4e45c-166">Zadejte název, který budete používat k odkazování na toto připojení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-166">Enter a name you will use to refer to this connection.</span></span>

1. <span data-ttu-id="4e45c-167">Zadejte adresu URL vašeho serveru volaných a značek **přijetí nedůvěryhodných certifikátů SSL** možnost.</span><span class="sxs-lookup"><span data-stu-id="4e45c-167">Enter the URL of your Jenkins server, and tick the **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="4e45c-168">Zadejte uživatelské jméno a heslo účtu volaných.</span><span class="sxs-lookup"><span data-stu-id="4e45c-168">Enter the user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="4e45c-169">Zvolte **ověření připojení** zkontrolujte správnost informací.</span><span class="sxs-lookup"><span data-stu-id="4e45c-169">Choose **Verify connection** to check that the information is correct.</span></span>

1. <span data-ttu-id="4e45c-170">Zvolte **OK** vytvořit koncový bod služby.</span><span class="sxs-lookup"><span data-stu-id="4e45c-170">Choose **OK** to create the service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="4e45c-171">Vytvořte skupinu nasazení</span><span class="sxs-lookup"><span data-stu-id="4e45c-171">Create a deployment group</span></span>

<span data-ttu-id="4e45c-172">Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) tak, aby obsahovala virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e45c-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) to  contain the virtual machines.</span></span>

1. <span data-ttu-id="4e45c-173">Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete **nasazení skupiny** a klikněte na příkaz **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-173">Open the **Releases** tab of the **Build &amp; Release** hub, then open the **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="4e45c-174">Zadejte název skupiny nasazení a volitelný popis.</span><span class="sxs-lookup"><span data-stu-id="4e45c-174">Enter a name for the deployment group, and an optional description.</span></span>
   <span data-ttu-id="4e45c-175">Zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-175">Then choose **Create**.</span></span>

<span data-ttu-id="4e45c-176">Úloha nasazení skupiny prostředků Azure vytvoří a zaregistruje virtuální počítače, když je spuštěna pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4e45c-176">The Azure Resource Group Deployment task creates and registers the VMs when it runs using the Azure Resource Manager template.</span></span>
<span data-ttu-id="4e45c-177">Nemusíte vytvářet a registraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e45c-177">You don't need to create and register the virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="4e45c-178">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="4e45c-178">Create a release definition</span></span>

<span data-ttu-id="4e45c-179">Verze definice určuje, že proces Team Services, budou spuštěny při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e45c-179">A release definition specifies the process Team Services will execute to deploy the app.</span></span>
<span data-ttu-id="4e45c-180">K vytvoření definice verze v Team Services:</span><span class="sxs-lookup"><span data-stu-id="4e45c-180">To create the release definition in Team Services:</span></span>

1. <span data-ttu-id="4e45c-181">Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete  **+**  rozevírací seznam v seznamu definice vydání a vyberte  **Vytvoření verze definice**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-181">Open the **Releases** tab of the **Build &amp; Release** hub, open the **+** drop-down in the list of release definitions, and choose the **Create release definition**.</span></span> 

1. <span data-ttu-id="4e45c-182">Vyberte **prázdný** šablony a zvolte **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-182">Select the **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="4e45c-183">V **artefakty** části, klikněte na **odkaz artefakt** a zvolte **volaných**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-183">In the **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="4e45c-184">Vyberte připojení volaných koncový bod služby.</span><span class="sxs-lookup"><span data-stu-id="4e45c-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="4e45c-185">Pak vyberte úlohu volaných zdroje a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-185">Then select the Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="4e45c-186">V nové verzi definice, zvolte **+ přidat úlohy** a přidejte **nasazení skupiny prostředků Azure** úloh do výchozí prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e45c-186">In the new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task to the default environment.</span></span>

1. <span data-ttu-id="4e45c-187">Vyberte šipku rozevíracího seznamu vedle položky **+ přidat úlohy** propojení a přidejte do definice skupiny fáze nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-187">Choose the drop down arrow next to the **+ Add tasks** link and add a deployment group phase to the definition.</span></span>

   ![Přidání skupiny fáze nasazení](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="4e45c-189">V katalogu úloh, otevřete **nástroj** a přidejte instance **skript prostředí** úloh.</span><span class="sxs-lookup"><span data-stu-id="4e45c-189">In the Task catalog, open the **Utility** section and add an instance of the **Shell Script** task.</span></span>

1. <span data-ttu-id="4e45c-190">K šabloně parametry použité při nasazení skupiny prostředků Azure úloh nastavuje heslo správce používá k připojení k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="4e45c-190">The parameters template used in the Azure Resource Group Deployment task sets the admin password used to connect to the VMs.</span></span>
   <span data-ttu-id="4e45c-191">Zadejte toto heslo s proměnnou **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="4e45c-191">You provide this password with the variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="4e45c-192">Otevřete **proměnné** kartě a v **proměnné** části, zadejte název `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="4e45c-192">Open the **Variables** tab and, in the **Variables** section, enter the name `adminpassword`.</span></span>

   - <span data-ttu-id="4e45c-193">Zadejte heslo správce.</span><span class="sxs-lookup"><span data-stu-id="4e45c-193">Enter the administrator password.</span></span>

   - <span data-ttu-id="4e45c-194">Zvolte ikonu "visacího zámku" vedle textové pole hodnoty k ochraně heslo.</span><span class="sxs-lookup"><span data-stu-id="4e45c-194">Choose the "padlock" icon next to the value textbox to protect the password.</span></span> 

## <a name="configure-the-azure-resource-group-deployment-task"></a><span data-ttu-id="4e45c-195">Konfigurace úloh nasazení skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="4e45c-195">Configure the Azure Resource Group Deployment task</span></span>

<span data-ttu-id="4e45c-196">**Nasazení skupiny prostředků Azure** úloha se používá k vytvoření skupiny nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-196">The **Azure Resource Group Deployment** task is used to create the deployment group.</span></span> <span data-ttu-id="4e45c-197">Nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e45c-197">Configure it as follows:</span></span>

* <span data-ttu-id="4e45c-198">**Předplatné:** vyberte připojení ze seznamu v části **dostupné připojení služby Azure**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-198">**Azure Subscription:** Select a connection from the list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="4e45c-199">Pokud žádné připojení k dispozici, zvolte **spravovat**, vyberte **nový koncový bod služby** pak **Azure Resource Manager**a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="4e45c-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow the prompts.</span></span>
  <span data-ttu-id="4e45c-200">Vrátit do vaší definice vydání, aktualizujte **AzureRM předplatné** seznam a vyberte připojení, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4e45c-200">Return to your release definition, refresh the **AzureRM Subscription** list, and select the connection you created.</span></span>

* <span data-ttu-id="4e45c-201">**Skupina prostředků**: Zadejte název skupiny prostředků, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="4e45c-201">**Resource group**: Enter a name of the resource group you created earlier.</span></span>

* <span data-ttu-id="4e45c-202">**Umístění**: Vyberte oblast pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-202">**Location**: Select a region for the deployment.</span></span>

  ![Vytvořit novou skupinu prostředků](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="4e45c-204">**Umístění šablon**:`URL of the file`</span><span class="sxs-lookup"><span data-stu-id="4e45c-204">**Template location**: `URL of the file`</span></span>

* <span data-ttu-id="4e45c-205">**Šablona odkaz**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="4e45c-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="4e45c-206">**Odkaz parametry šablony**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="4e45c-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="4e45c-207">**Přepsání parametry šablony**: seznam přepsání hodnoty, například: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="4e45c-207">**Override template parameters**: A list of the override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="4e45c-208">Vložení vlastní konkrétní hodnoty pro {zástupné symboly}.</span><span class="sxs-lookup"><span data-stu-id="4e45c-208">Insert your own specific values for the {placeholders}.</span></span> 

* <span data-ttu-id="4e45c-209">**Povolit požadavky**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="4e45c-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="4e45c-210">**Koncový bod služby sady TFS nebo VSTS**: Zvolte **přidat** a v dialogovém okně "Přidat nové Team Foundation Server/Team Services připojení" vyberte **tokenu ověřování na základě**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-210">**TFS/VSTS endpoint**: Choose **Add** and, in the "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="4e45c-211">Zadejte název připojení a adresu URL týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-211">Enter a name of the connection and the URL of your team project.</span></span> <span data-ttu-id="4e45c-212">Potom generovat a zadejte [Personal Access Token (Jan)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) k ověření připojení k vašemu týmovému projektu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to authenticate the connection to your team project.</span></span>

  ![Vytvořit osobní přístupový Token](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="4e45c-214">**Týmový projekt**: Vyberte aktuálního projektu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="4e45c-215">**Nasazení skupiny**: Zadejte stejný název skupiny pro nasazení, jako jste použili při **skupiny prostředků** parametr.</span><span class="sxs-lookup"><span data-stu-id="4e45c-215">**Deployment Group**: Enter the same deployment group name as you used for the **Resource group** parameter.</span></span>

<span data-ttu-id="4e45c-216">Výchozí nastavení pro úlohu nasazení skupiny prostředků Azure jsou vytvořit nebo aktualizovat prostředek a to udělat postupně.</span><span class="sxs-lookup"><span data-stu-id="4e45c-216">The default settings for the Azure Resource Group Deployment task are to create or update a resource, and to do so incrementally.</span></span> <span data-ttu-id="4e45c-217">Úloha vytvoří virtuální počítače poprvé se spustí a následně právě provede jejich aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="4e45c-217">The task creates the VMs the first time it runs, and subsequently just update them.</span></span>

## <a name="configure-the-shell-script-task"></a><span data-ttu-id="4e45c-218">Nakonfigurujte úlohu skript prostředí</span><span class="sxs-lookup"><span data-stu-id="4e45c-218">Configure the Shell Script task</span></span>

<span data-ttu-id="4e45c-219">**Skript prostředí** úloha slouží k poskytování konfigurace pro skript běžet na každém serveru nainstalovat Node.js a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e45c-219">The **Shell Script** task is used to provide the configuration for a script to run on each server to install Node.js and start the app.</span></span> <span data-ttu-id="4e45c-220">Nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e45c-220">Configure it as follows:</span></span>

* <span data-ttu-id="4e45c-221">**Skript cesta**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="4e45c-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="4e45c-222">**Zadat pracovní adresář**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="4e45c-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="4e45c-223">**Pracovní adresář**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="4e45c-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-the-release-definition"></a><span data-ttu-id="4e45c-224">Přejmenujte a uložte definici verze</span><span class="sxs-lookup"><span data-stu-id="4e45c-224">Rename and save the release definition</span></span>

1. <span data-ttu-id="4e45c-225">Upravit název definice verze na název, který jste zadali v **akce po sestavení** kartě sestavení v volaných.</span><span class="sxs-lookup"><span data-stu-id="4e45c-225">Edit the name of the release definition to the name you specified in the **Post-build Actions** tab of the build in Jenkins.</span></span> <span data-ttu-id="4e45c-226">Volaných vyžaduje tento název moct aktivovat novou verzi, když jsou aktualizovány zdrojové artefakty.</span><span class="sxs-lookup"><span data-stu-id="4e45c-226">Jenkins requires this name to be able to trigger a new release when the source artifacts are updated.</span></span>

1. <span data-ttu-id="4e45c-227">Volitelně můžete změníte název prostředí kliknutím na název.</span><span class="sxs-lookup"><span data-stu-id="4e45c-227">Optionally, change the name of the environment by clicking on the name.</span></span> 

1. <span data-ttu-id="4e45c-228">Zvolte **Uložit**a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="4e45c-229">Spustit ruční nasazení</span><span class="sxs-lookup"><span data-stu-id="4e45c-229">Start a manual deployment</span></span>

1. <span data-ttu-id="4e45c-230">Zvolte **+ verze** a vyberte **vytvořit verzi**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="4e45c-231">Vyberte sestavení, můžete dokončit v zvýrazněná rozevíracího seznamu a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e45c-231">Select the build you completed in the highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="4e45c-232">Vyberte odkaz verze v místní zpráva.</span><span class="sxs-lookup"><span data-stu-id="4e45c-232">Choose the release link in the popup message.</span></span> <span data-ttu-id="4e45c-233">Například: "verze **verze 1** byla vytvořena."</span><span class="sxs-lookup"><span data-stu-id="4e45c-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="4e45c-234">Otevřete **protokoly** a podívejte se na výstup konzoly verze.</span><span class="sxs-lookup"><span data-stu-id="4e45c-234">Open the **Logs** tab to watch the release console output.</span></span>

1. <span data-ttu-id="4e45c-235">V prohlížeči otevřete adresu URL jednoho ze serverů, které jste přidali do vaší skupiny nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-235">In your browser, open the URL of one of the servers you added to your deployment group.</span></span> <span data-ttu-id="4e45c-236">Zadejte například`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="4e45c-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="4e45c-237">Spuštění nasazení CI/CD</span><span class="sxs-lookup"><span data-stu-id="4e45c-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="4e45c-238">V definici verze, zrušte zaškrtnutí políčka **povoleno** zaškrtnout políčko **možnosti řízení** část nastavení pro úlohu nasazení skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4e45c-238">In the release definition, uncheck the **Enabled** checkbox in the **Control Options** section of the settings for the Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="4e45c-239">Pro budoucí nasazení do existující skupiny nasazení není potřeba znovu spustit tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="4e45c-239">For future deployments to the existing deployment group, you do not need to re-execute this task.</span></span>

1. <span data-ttu-id="4e45c-240">Přejít na zdrojové úložiště Git a upravovat obsah **h1** záhlaví v souboru [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="4e45c-240">Go to the source Git repository and modify the contents of the **h1** heading in the file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="4e45c-241">Potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="4e45c-241">Commit your change.</span></span>

1. <span data-ttu-id="4e45c-242">Po několika minutách, zobrazí se nová verze vytvořené v **verze** Team Services nebo TFS.</span><span class="sxs-lookup"><span data-stu-id="4e45c-242">After a few minutes, you will see a new release created in the **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="4e45c-243">Otevřete verzi zobrazíte probíhající nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e45c-243">Open the release to see the deployment taking place.</span></span> <span data-ttu-id="4e45c-244">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="4e45c-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e45c-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e45c-245">Next Steps</span></span>

<span data-ttu-id="4e45c-246">V tomto kurzu automatické nasazení aplikace do Azure pomocí volaných sestavení a Team Services pro verzi.</span><span class="sxs-lookup"><span data-stu-id="4e45c-246">In this tutorial, you automated the deployment of an app to Azure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="4e45c-247">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="4e45c-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4e45c-248">Sestavení aplikace ve volaných</span><span class="sxs-lookup"><span data-stu-id="4e45c-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="4e45c-249">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="4e45c-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="4e45c-250">Vytvořit skupinu nasazení pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4e45c-250">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="4e45c-251">Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci</span><span class="sxs-lookup"><span data-stu-id="4e45c-251">Create a release definition that configures the VMs and deploys the app</span></span>

<span data-ttu-id="4e45c-252">Přechod na další kurzu se dozvíte další informace o tom, jak nasadit SVÍTILNU zásobníku (Linux, Apache, MySQL a PHP,).</span><span class="sxs-lookup"><span data-stu-id="4e45c-252">Advance to the next tutorial to learn more about how to deploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4e45c-253">Nasazení zásobníku LAMP</span><span class="sxs-lookup"><span data-stu-id="4e45c-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)