---
title: "aaaCI/CD z volaných tooAzure virtuálních počítačů s Team Services | Microsoft Docs"
description: "Nastavte průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí virtuálních počítačů tooAzure volaných z správy verzí v aplikaci Visual Studio Team Services (VSTS) nebo Microsoft Team Foundation Server (TFS)"
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
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="37c5c-103">Nasazení vaší aplikace tooLinux virtuálních počítačů pomocí volaných a Team Services</span><span class="sxs-lookup"><span data-stu-id="37c5c-103">Deploy your app tooLinux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="37c5c-104">Průběžnou integraci (CI) a průběžné nasazování (CD) je kanál, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód.</span><span class="sxs-lookup"><span data-stu-id="37c5c-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="37c5c-105">Team Services poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro tooAzure nasazení.</span><span class="sxs-lookup"><span data-stu-id="37c5c-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment tooAzure.</span></span> <span data-ttu-id="37c5c-106">Volaných je Oblíbené 3. stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace.</span><span class="sxs-lookup"><span data-stu-id="37c5c-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="37c5c-107">Můžete použít obě společně toocustomize jak poskytovat cloudové aplikace nebo služby.</span><span class="sxs-lookup"><span data-stu-id="37c5c-107">You can use both together toocustomize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="37c5c-108">V tomto kurzu použijete volaných toobuild **webové aplikace Node.js**a Visual Studio Team Services toodeploy ho tooa [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="37c5c-108">In this tutorial, you use Jenkins toobuild a **Node.js web app**, and Visual Studio Team Services toodeploy it tooa [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="37c5c-109">Vaším úkolem je:</span><span class="sxs-lookup"><span data-stu-id="37c5c-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37c5c-110">Sestavení aplikace ve volaných</span><span class="sxs-lookup"><span data-stu-id="37c5c-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="37c5c-111">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="37c5c-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="37c5c-112">Vytvořit skupinu nasazení pro hello virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="37c5c-112">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="37c5c-113">Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů hello a nasadí aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="37c5c-113">Create a release definition that configures hello VMs and deploys hello app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="37c5c-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="37c5c-114">Before you begin</span></span>

* <span data-ttu-id="37c5c-115">Budete potřebovat účet pro přístup k tooa volaných.</span><span class="sxs-lookup"><span data-stu-id="37c5c-115">You need access tooa Jenkins account.</span></span> <span data-ttu-id="37c5c-116">Pokud jste ještě nevytvořili volaných server, přečtěte si téma [volaných dokumentaci](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="37c5c-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="37c5c-117">Přihlaste se tooyour účtu Team Services (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="37c5c-117">Sign in tooyour Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="37c5c-118">Můžete získat [bezplatný účet Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="37c5c-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="37c5c-119">Další informace najdete v tématu [připojení služby tooTeam](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="37c5c-119">For more information, see [Connect tooTeam Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="37c5c-120">Pokud jste ještě nemáte, vytvořte osobní přístupový token (Jan) ve vašem účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="37c5c-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="37c5c-121">Volaných vyžaduje tento tooaccess informace vašeho účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="37c5c-121">Jenkins requires this information tooaccess your Team Services account.</span></span>
  <span data-ttu-id="37c5c-122">Čtení [vytvoření osobního přístupového tokenu pro Team Services a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn jak toogenerate jeden.</span><span class="sxs-lookup"><span data-stu-id="37c5c-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn how toogenerate one.</span></span>

## <a name="get-hello-sample-app"></a><span data-ttu-id="37c5c-123">Získat hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="37c5c-123">Get hello sample app</span></span>

<span data-ttu-id="37c5c-124">Budete potřebovat aplikaci toodeploy uložené v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="37c5c-124">You need an app toodeploy stored in a Git repository.</span></span>
<span data-ttu-id="37c5c-125">V tomto kurzu doporučujeme, abyste použili [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="37c5c-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="37c5c-126">Vytvoření větve tuto aplikaci a poznamenejte si umístění hello (URL) pro použití v dalších krocích tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-126">Create a fork of this app and take note of hello location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="37c5c-127">Ujistěte se, hello rozvětvení **veřejné** toosimplify tooGitHub připojení později.</span><span class="sxs-lookup"><span data-stu-id="37c5c-127">Make hello fork **public** toosimplify connecting tooGitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="37c5c-128">Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/) a [nastavíte privátní úložiště jako veřejné](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="37c5c-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="37c5c-129">aplikace Hello bylo vytvořeno prostřednictvím [Yeoman](http://yeoman.io/learning/index.html); používá **Express**, **bower**, a **grunt**; a má některé **npm**balíčky jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="37c5c-129">hello app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="37c5c-130">Ukázková aplikace Hello obsahuje sadu [šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) , jsou použité toodynamically vytváření hello virtuálních počítačů pro nasazení v Azure.</span><span class="sxs-lookup"><span data-stu-id="37c5c-130">hello sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used toodynamically create hello virtual machines for deployment on Azure.</span></span> <span data-ttu-id="37c5c-131">Tyto šablony jsou používány úlohy v hello [Team Services verze definice](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="37c5c-131">These templates are used by tasks in hello [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="37c5c-132">Hello hlavní šablona vytvoří skupinu zabezpečení sítě, virtuální počítač a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="37c5c-132">hello main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="37c5c-133">Ho přiřadí veřejnou IP adresu a otevře příchozí port 80.</span><span class="sxs-lookup"><span data-stu-id="37c5c-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="37c5c-134">Také přidá značku, který je používán nasazení hello tooreceive příliš vyberte hello hello nasazení skupiny počítačů.</span><span class="sxs-lookup"><span data-stu-id="37c5c-134">It also adds a tag that is used by hello deployment group too select hello machines tooreceive hello deployment.</span></span>
>
> <span data-ttu-id="37c5c-135">Ukázka Hello také obsahuje skript, který nastaví Nginx a nasadí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-135">hello sample also contains a script that sets up Nginx and deploys hello app.</span></span> <span data-ttu-id="37c5c-136">Spuštění na každý z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37c5c-136">It is executed on each of hello virtual machines.</span></span> <span data-ttu-id="37c5c-137">Konkrétně hello skript nainstaluje uzlu, Nginx a PM2; Nakonfiguruje Nginx a PM2; pak spustí aplikaci hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-137">Specifically, hello script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts hello Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="37c5c-138">Konfigurace volaných moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="37c5c-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="37c5c-139">Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in pro **NodeJS** a **integraci se službou Team Services**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="37c5c-140">Otevřete váš účet volaných a zvolte **spravovat volaných**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="37c5c-141">V hello **spravovat volaných** vyberte **Správa modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-141">In hello **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="37c5c-142">Filtr hello seznamu toolocate hello **NodeJS** modulů plug-in a nainstalovat bez restartování.</span><span class="sxs-lookup"><span data-stu-id="37c5c-142">Filter hello list toolocate hello **NodeJS** plugin and install it without restart.</span></span>

   ![Přidání tooJenkins modulu plug-in hello NodeJS](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="37c5c-144">Filtr hello seznamu toofind hello **Team Foundation Server** modulů plug-in a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="37c5c-144">Filter hello list toofind hello **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="37c5c-145">(Tento modul plug-in funguje pro Team Services a serveru Team Foundation Server.) Restartování volaných není nutné.</span><span class="sxs-lookup"><span data-stu-id="37c5c-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="37c5c-146">Konfigurace sestavení volaných pro Node.js</span><span class="sxs-lookup"><span data-stu-id="37c5c-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="37c5c-147">Volaných vytvořte nový projekt sestavení a nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="37c5c-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="37c5c-148">V hello **Obecné** zadejte název pro sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-148">In hello **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="37c5c-149">V hello **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o hello hello úložiště a větve hello obsahující kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="37c5c-149">In hello **Source Code Management** tab, select **Git** and enter hello details of hello repository and hello branch containing your app code.</span></span>

   ![Přidání úložiště tooyour sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="37c5c-151">Pokud úložiště není veřejný, zvolte **přidat** a zadejte přihlašovací údaje tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="37c5c-151">If your repository is not public, choose **Add** and provide credentials tooconnect tooit.</span></span>

1. <span data-ttu-id="37c5c-152">V hello **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán hello `H/03 * * * *` toopoll hello úložiště Git pro změny každé tři minuty.</span><span class="sxs-lookup"><span data-stu-id="37c5c-152">In hello **Build Triggers** tab, select **Poll SCM** and enter hello schedule `H/03 * * * *` toopoll hello Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="37c5c-153">V hello **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a zadejte `NodeJS` pro hello hodnota instalace JS uzlu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-153">In hello **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for hello Node JS Installation value.</span></span> <span data-ttu-id="37c5c-154">Nechte **npmrc souboru** nastavena na "použít výchozí systémové nastavení."</span><span class="sxs-lookup"><span data-stu-id="37c5c-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="37c5c-155">V hello **sestavení** kartě, zadejte příkaz hello `npm install` tooensure jsou aktualizovány všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="37c5c-155">In hello **Build** tab, enter hello command `npm install` tooensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="37c5c-156">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="37c5c-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="37c5c-157">V hello **akce po sestavení** kartě pro **soubory tooarchive**, zadejte `**/*` tooinclude všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="37c5c-157">In hello **Post-build Actions** tab, for **Files tooarchive**, enter `**/*` tooinclude all files.</span></span>

1. <span data-ttu-id="37c5c-158">Pro **aktivovat verze v TFS/Team Services**, zadejte úplnou adresu URL hello účtu (například `https://your-account-name.visualstudio.com`), hello název pro definici verze hello (vytvořit později), jako název projektu a hello účet tooyour tooconnect přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="37c5c-158">For **Trigger release in TFS/Team Services**, enter hello full URL of your account (such as `https://your-account-name.visualstudio.com`), hello project name, a name for hello release definition (created later), and hello credentials tooconnect tooyour account.</span></span>
   <span data-ttu-id="37c5c-159">Je třeba, uživatelské jméno a hello Jan jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="37c5c-159">You need your user name and hello PAT you created earlier.</span></span> 

   ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="37c5c-161">Uložte hello sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-161">Save hello build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="37c5c-162">Vytvoření koncového bodu služby volaných</span><span class="sxs-lookup"><span data-stu-id="37c5c-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="37c5c-163">Koncový bod služby umožňuje tooJenkins tooconnect Team Services.</span><span class="sxs-lookup"><span data-stu-id="37c5c-163">A service endpoint allows Team Services tooconnect tooJenkins.</span></span>

1. <span data-ttu-id="37c5c-164">Otevřete hello **služby** stránky v Team Services, otevřete hello **nový koncový bod služby** seznam a vyberte **volaných**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-164">Open hello **Services** page in Team Services, open hello **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="37c5c-166">Zadejte název budete používat toorefer toothis připojení.</span><span class="sxs-lookup"><span data-stu-id="37c5c-166">Enter a name you will use toorefer toothis connection.</span></span>

1. <span data-ttu-id="37c5c-167">Zadejte adresu URL hello volaných serveru a osové hello **přijetí nedůvěryhodných certifikátů SSL** možnost.</span><span class="sxs-lookup"><span data-stu-id="37c5c-167">Enter hello URL of your Jenkins server, and tick hello **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="37c5c-168">Zadejte hello uživatelské jméno a heslo účtu volaných.</span><span class="sxs-lookup"><span data-stu-id="37c5c-168">Enter hello user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="37c5c-169">Zvolte **ověření připojení** toocheck, který hello informace je správná.</span><span class="sxs-lookup"><span data-stu-id="37c5c-169">Choose **Verify connection** toocheck that hello information is correct.</span></span>

1. <span data-ttu-id="37c5c-170">Zvolte **OK** koncový bod služby toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-170">Choose **OK** toocreate hello service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="37c5c-171">Vytvořte skupinu nasazení</span><span class="sxs-lookup"><span data-stu-id="37c5c-171">Create a deployment group</span></span>

<span data-ttu-id="37c5c-172">Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) příliš obsahovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37c5c-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) too contain hello virtual machines.</span></span>

1. <span data-ttu-id="37c5c-173">Otevřete hello **verze** kartě hello **sestavení &amp; verze** rozbočovače a pak otevřete hello **nasazení skupiny** a klikněte na příkaz **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-173">Open hello **Releases** tab of hello **Build &amp; Release** hub, then open hello **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="37c5c-174">Zadejte název pro skupinu nasazení hello a volitelný popis.</span><span class="sxs-lookup"><span data-stu-id="37c5c-174">Enter a name for hello deployment group, and an optional description.</span></span>
   <span data-ttu-id="37c5c-175">Zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-175">Then choose **Create**.</span></span>

<span data-ttu-id="37c5c-176">Úloha nasazení skupiny prostředků Azure Hello vytvoří a zaregistruje hello virtuální počítače, když je spuštěna pomocí šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-176">hello Azure Resource Group Deployment task creates and registers hello VMs when it runs using hello Azure Resource Manager template.</span></span>
<span data-ttu-id="37c5c-177">Nebudete potřebovat toocreate a hello virtuální počítače zaregistrovat sami.</span><span class="sxs-lookup"><span data-stu-id="37c5c-177">You don't need toocreate and register hello virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="37c5c-178">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="37c5c-178">Create a release definition</span></span>

<span data-ttu-id="37c5c-179">Verze definice určuje, že proces hello Team Services, budou spuštěny toodeploy hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37c5c-179">A release definition specifies hello process Team Services will execute toodeploy hello app.</span></span>
<span data-ttu-id="37c5c-180">toocreate hello verze definice v Team Services:</span><span class="sxs-lookup"><span data-stu-id="37c5c-180">toocreate hello release definition in Team Services:</span></span>

1. <span data-ttu-id="37c5c-181">Otevřete hello **verze** kartě hello **sestavení &amp; verze** rozbočovače, otevřete hello  **+**  rozevírací seznam v seznamu hello Definice vydání a vyberte Hello **vytvořit verze definice**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-181">Open hello **Releases** tab of hello **Build &amp; Release** hub, open hello **+** drop-down in hello list of release definitions, and choose hello **Create release definition**.</span></span> 

1. <span data-ttu-id="37c5c-182">Vyberte hello **prázdný** šablony a zvolte **Další**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-182">Select hello **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="37c5c-183">V hello **artefakty** části, klikněte na **odkaz artefakt** a zvolte **volaných**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-183">In hello **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="37c5c-184">Vyberte připojení volaných koncový bod služby.</span><span class="sxs-lookup"><span data-stu-id="37c5c-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="37c5c-185">Potom vyberte hello volaných zdrojového projektu a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-185">Then select hello Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="37c5c-186">V hello nové verze definice, zvolte **+ přidat úlohy** a přidejte **nasazení skupiny prostředků Azure** prostředí výchozí toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="37c5c-186">In hello new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task toohello default environment.</span></span>

1. <span data-ttu-id="37c5c-187">Zvolte hello rozevírací šipku další toohello **+ přidat úlohy** propojit a přidejte definici toohello skupiny fáze nasazení.</span><span class="sxs-lookup"><span data-stu-id="37c5c-187">Choose hello drop down arrow next toohello **+ Add tasks** link and add a deployment group phase toohello definition.</span></span>

   ![Přidání skupiny fáze nasazení](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="37c5c-189">V katalogu hello úloh, otevřete hello **nástroj** a přidejte instance hello **skript prostředí** úloh.</span><span class="sxs-lookup"><span data-stu-id="37c5c-189">In hello Task catalog, open hello **Utility** section and add an instance of hello **Shell Script** task.</span></span>

1. <span data-ttu-id="37c5c-190">Hello parametry šablony použité v úloze nasazení skupiny prostředků Azure hello nastaví hello správce heslo použité tooconnect toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="37c5c-190">hello parameters template used in hello Azure Resource Group Deployment task sets hello admin password used tooconnect toohello VMs.</span></span>
   <span data-ttu-id="37c5c-191">Zadejte toto heslo se proměnná hello **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="37c5c-191">You provide this password with hello variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="37c5c-192">Otevřete hello **proměnné** kartě a v hello **proměnné** části, zadejte název hello `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="37c5c-192">Open hello **Variables** tab and, in hello **Variables** section, enter hello name `adminpassword`.</span></span>

   - <span data-ttu-id="37c5c-193">Zadejte heslo správce hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-193">Enter hello administrator password.</span></span>

   - <span data-ttu-id="37c5c-194">Zvolte hello "visacího zámku" ikonu další toohello hodnota textbox tooprotect hello heslo.</span><span class="sxs-lookup"><span data-stu-id="37c5c-194">Choose hello "padlock" icon next toohello value textbox tooprotect hello password.</span></span> 

## <a name="configure-hello-azure-resource-group-deployment-task"></a><span data-ttu-id="37c5c-195">Konfigurace úloh hello nasazení skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="37c5c-195">Configure hello Azure Resource Group Deployment task</span></span>

<span data-ttu-id="37c5c-196">Hello **nasazení skupiny prostředků Azure** úloha je použité toocreate hello nasazení skupiny.</span><span class="sxs-lookup"><span data-stu-id="37c5c-196">hello **Azure Resource Group Deployment** task is used toocreate hello deployment group.</span></span> <span data-ttu-id="37c5c-197">Nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="37c5c-197">Configure it as follows:</span></span>

* <span data-ttu-id="37c5c-198">**Předplatné:** vyberte připojení hello seznamu v části **dostupné připojení služby Azure**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-198">**Azure Subscription:** Select a connection from hello list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="37c5c-199">Pokud žádné připojení k dispozici, zvolte **spravovat**, vyberte **nový koncový bod služby** pak **Azure Resource Manager**a budete postupovat podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow hello prompts.</span></span>
  <span data-ttu-id="37c5c-200">Vrátí tooyour definice vydání, aktualizace hello **AzureRM předplatné** seznam a vyberte hello připojení, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="37c5c-200">Return tooyour release definition, refresh hello **AzureRM Subscription** list, and select hello connection you created.</span></span>

* <span data-ttu-id="37c5c-201">**Skupina prostředků**: Zadejte název skupiny prostředků hello jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="37c5c-201">**Resource group**: Enter a name of hello resource group you created earlier.</span></span>

* <span data-ttu-id="37c5c-202">**Umístění**: Vyberte oblast pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-202">**Location**: Select a region for hello deployment.</span></span>

  ![Vytvoření nové skupiny prostředků](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="37c5c-204">**Umístění šablon**:`URL of hello file`</span><span class="sxs-lookup"><span data-stu-id="37c5c-204">**Template location**: `URL of hello file`</span></span>

* <span data-ttu-id="37c5c-205">**Šablona odkaz**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="37c5c-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="37c5c-206">**Odkaz parametry šablony**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="37c5c-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="37c5c-207">**Přepsání parametry šablony**: seznam hello přepsat hodnoty, například: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="37c5c-207">**Override template parameters**: A list of hello override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="37c5c-208">Vložení vlastní konkrétní hodnoty pro hello {zástupné symboly}.</span><span class="sxs-lookup"><span data-stu-id="37c5c-208">Insert your own specific values for hello {placeholders}.</span></span> 

* <span data-ttu-id="37c5c-209">**Povolit požadavky**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="37c5c-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="37c5c-210">**Koncový bod služby sady TFS nebo VSTS**: Zvolte **přidat** a v dialogu "Přidat nové Team Foundation Server/Team Services připojení" hello, vyberte **tokenu ověřování na základě**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-210">**TFS/VSTS endpoint**: Choose **Add** and, in hello "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="37c5c-211">Zadejte název připojení hello a adresu URL hello týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-211">Enter a name of hello connection and hello URL of your team project.</span></span> <span data-ttu-id="37c5c-212">Potom generovat a zadejte [Personal Access Token (Jan)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello připojení tooyour týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello connection tooyour team project.</span></span>

  ![Vytvořit osobní přístupový Token](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="37c5c-214">**Týmový projekt**: Vyberte aktuálního projektu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="37c5c-215">**Nasazení skupiny**: Zadejte hello stejný název skupiny pro nasazení, jako jste použili pro hello **skupiny prostředků** parametr.</span><span class="sxs-lookup"><span data-stu-id="37c5c-215">**Deployment Group**: Enter hello same deployment group name as you used for hello **Resource group** parameter.</span></span>

<span data-ttu-id="37c5c-216">Hello výchozí nastavení pro úlohu hello nasazení skupiny prostředků Azure jsou toocreate nebo aktualizovat prostředek a toodo tak postupně.</span><span class="sxs-lookup"><span data-stu-id="37c5c-216">hello default settings for hello Azure Resource Group Deployment task are toocreate or update a resource, and toodo so incrementally.</span></span> <span data-ttu-id="37c5c-217">Hello úloh vytvoří virtuální počítače hello hello poprvé spustí a následně právě aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="37c5c-217">hello task creates hello VMs hello first time it runs, and subsequently just update them.</span></span>

## <a name="configure-hello-shell-script-task"></a><span data-ttu-id="37c5c-218">Konfigurace úloh skript prostředí hello</span><span class="sxs-lookup"><span data-stu-id="37c5c-218">Configure hello Shell Script task</span></span>

<span data-ttu-id="37c5c-219">Hello **skript prostředí** úloh je použité tooprovide hello konfigurace pro skript toorun na každý server tooinstall Node.js a spusťte hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37c5c-219">hello **Shell Script** task is used tooprovide hello configuration for a script toorun on each server tooinstall Node.js and start hello app.</span></span> <span data-ttu-id="37c5c-220">Nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="37c5c-220">Configure it as follows:</span></span>

* <span data-ttu-id="37c5c-221">**Skript cesta**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="37c5c-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="37c5c-222">**Zadat pracovní adresář**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="37c5c-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="37c5c-223">**Pracovní adresář**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="37c5c-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-hello-release-definition"></a><span data-ttu-id="37c5c-224">Přejmenovat a Uložit definici verze hello</span><span class="sxs-lookup"><span data-stu-id="37c5c-224">Rename and save hello release definition</span></span>

1. <span data-ttu-id="37c5c-225">Upravit hello název hello verze definice toohello jste zadali v **akce po sestavení** kartě hello sestavení v volaných.</span><span class="sxs-lookup"><span data-stu-id="37c5c-225">Edit hello name of hello release definition toohello name you specified in the **Post-build Actions** tab of hello build in Jenkins.</span></span> <span data-ttu-id="37c5c-226">Volaných vyžaduje tento název toobe možné tootrigger novou verzi, když jsou aktualizovány hello zdroj artefakty.</span><span class="sxs-lookup"><span data-stu-id="37c5c-226">Jenkins requires this name toobe able tootrigger a new release when hello source artifacts are updated.</span></span>

1. <span data-ttu-id="37c5c-227">Volitelně můžete změníte název hello hello prostředí kliknutím na název hello.</span><span class="sxs-lookup"><span data-stu-id="37c5c-227">Optionally, change hello name of hello environment by clicking on hello name.</span></span> 

1. <span data-ttu-id="37c5c-228">Zvolte **Uložit**a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="37c5c-229">Spustit ruční nasazení</span><span class="sxs-lookup"><span data-stu-id="37c5c-229">Start a manual deployment</span></span>

1. <span data-ttu-id="37c5c-230">Zvolte **+ verze** a vyberte **vytvořit verzi**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="37c5c-231">Vyberte hello sestavení jste dokončili v hello zvýrazněná rozevíracího seznamu a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="37c5c-231">Select hello build you completed in hello highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="37c5c-232">Vyberte odkaz verze hello v hello místní zpráva.</span><span class="sxs-lookup"><span data-stu-id="37c5c-232">Choose hello release link in hello popup message.</span></span> <span data-ttu-id="37c5c-233">Například: "verze **verze 1** byla vytvořena."</span><span class="sxs-lookup"><span data-stu-id="37c5c-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="37c5c-234">Otevřete hello **protokoly** kartě výstup toowatch hello verze konzoly.</span><span class="sxs-lookup"><span data-stu-id="37c5c-234">Open hello **Logs** tab toowatch hello release console output.</span></span>

1. <span data-ttu-id="37c5c-235">V prohlížeči otevřete adresu URL hello jednoho ze serverů hello jste přidali skupinu tooyour nasazení.</span><span class="sxs-lookup"><span data-stu-id="37c5c-235">In your browser, open hello URL of one of hello servers you added tooyour deployment group.</span></span> <span data-ttu-id="37c5c-236">Zadejte například`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="37c5c-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="37c5c-237">Spuštění nasazení CI/CD</span><span class="sxs-lookup"><span data-stu-id="37c5c-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="37c5c-238">V hello vydání definice a zrušte zaškrtnutí políčka hello **povoleno** zaškrtnout políčko hello **možnosti řízení** části hello nastavení pro úlohu hello nasazení skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="37c5c-238">In hello release definition, uncheck hello **Enabled** checkbox in hello **Control Options** section of hello settings for hello Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="37c5c-239">Pro budoucí nasazení toohello existující nasazení skupiny, není nutné toore-provedení tohoto úkolu.</span><span class="sxs-lookup"><span data-stu-id="37c5c-239">For future deployments toohello existing deployment group, you do not need toore-execute this task.</span></span>

1. <span data-ttu-id="37c5c-240">Přejděte úložiště Git toohello zdroje a upravovat obsah hello hello **h1** záhlaví v souboru hello [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="37c5c-240">Go toohello source Git repository and modify hello contents of hello **h1** heading in hello file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="37c5c-241">Potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="37c5c-241">Commit your change.</span></span>

1. <span data-ttu-id="37c5c-242">Po několika minutách, zobrazí se nová verze vytvořené v hello **verze** Team Services nebo TFS.</span><span class="sxs-lookup"><span data-stu-id="37c5c-242">After a few minutes, you will see a new release created in hello **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="37c5c-243">Otevřete hello verze toosee hello nasazení probíhající.</span><span class="sxs-lookup"><span data-stu-id="37c5c-243">Open hello release toosee hello deployment taking place.</span></span> <span data-ttu-id="37c5c-244">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="37c5c-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="37c5c-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37c5c-245">Next Steps</span></span>

<span data-ttu-id="37c5c-246">V tomto kurzu automatizované hello nasazení tooAzure aplikaci pomocí volaných sestavení a Team Services pro verzi.</span><span class="sxs-lookup"><span data-stu-id="37c5c-246">In this tutorial, you automated hello deployment of an app tooAzure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="37c5c-247">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="37c5c-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37c5c-248">Sestavení aplikace ve volaných</span><span class="sxs-lookup"><span data-stu-id="37c5c-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="37c5c-249">Konfigurace volaných pro integraci produktů Team Services</span><span class="sxs-lookup"><span data-stu-id="37c5c-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="37c5c-250">Vytvořit skupinu nasazení pro hello virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="37c5c-250">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="37c5c-251">Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů hello a nasadí aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="37c5c-251">Create a release definition that configures hello VMs and deploys hello app</span></span>

<span data-ttu-id="37c5c-252">Posunutí toohello další kurz toolearn informace o tom, jak zásobníku toodeploy svítilna (Linux, Apache, MySQL a PHP,).</span><span class="sxs-lookup"><span data-stu-id="37c5c-252">Advance toohello next tutorial toolearn more about how toodeploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="37c5c-253">Nasazení zásobníku LAMP</span><span class="sxs-lookup"><span data-stu-id="37c5c-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)