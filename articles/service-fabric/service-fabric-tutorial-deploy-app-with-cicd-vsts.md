---
title: "Nasazení aplikace Azure Service Fabric pomocí průběžnou integraci (Team Services) | Microsoft Docs"
description: "Zjistěte, jak nastavit průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services.  Nasazení aplikace do clusteru Service Fabric v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: 631f9794994530092d05a33b06ebf8c07f331649
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a><span data-ttu-id="6fd48-104">Nasazení aplikace s CI nebo CD pro cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6fd48-104">Deploy an application with CI/CD to a Service Fabric cluster</span></span>
<span data-ttu-id="6fd48-105">V tomto kurzu je součástí tři řady a popisuje, jak nastavit průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="6fd48-105">This tutorial is part three of a series and describes how to set up continuous integration and deployment for an Azure Service Fabric application using Visual Studio Team Services.</span></span>  <span data-ttu-id="6fd48-106">Je potřeba stávající aplikace Service Fabric, aplikace vytvořené v [sestavení aplikace .NET](service-fabric-tutorial-create-dotnet-app.md) slouží jako příklad.</span><span class="sxs-lookup"><span data-stu-id="6fd48-106">An existing Service Fabric application is needed, the application created in [Build a .NET application](service-fabric-tutorial-create-dotnet-app.md) is used as an example.</span></span>

<span data-ttu-id="6fd48-107">V rámci tři řady, zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="6fd48-107">In part three of the series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fd48-108">Do projektu přidejte zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="6fd48-108">Add source control to your project</span></span>
> * <span data-ttu-id="6fd48-109">Vytvořit definici sestavení v Team Services</span><span class="sxs-lookup"><span data-stu-id="6fd48-109">Create a build definition in Team Services</span></span>
> * <span data-ttu-id="6fd48-110">Vytvoření definice verze v Team Services</span><span class="sxs-lookup"><span data-stu-id="6fd48-110">Create a release definition in Team Services</span></span>
> * <span data-ttu-id="6fd48-111">Automatické nasazení a upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="6fd48-111">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="6fd48-112">V této série kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="6fd48-112">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="6fd48-113">Sestavení aplikace .NET Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6fd48-113">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * [<span data-ttu-id="6fd48-114">Nasazení aplikace na vzdálený cluster</span><span class="sxs-lookup"><span data-stu-id="6fd48-114">Deploy the application to a remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * <span data-ttu-id="6fd48-115">Konfigurace CI/CD pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="6fd48-115">Configure CI/CD using Visual Studio Team Services</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fd48-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6fd48-116">Prerequisites</span></span>
<span data-ttu-id="6fd48-117">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="6fd48-117">Before you begin this tutorial:</span></span>
- <span data-ttu-id="6fd48-118">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="6fd48-118">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="6fd48-119">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="6fd48-119">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="6fd48-120">Instalace Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="6fd48-120">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)
- <span data-ttu-id="6fd48-121">Vytvoření aplikace Service Fabric, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-dotnet-app.md).</span><span class="sxs-lookup"><span data-stu-id="6fd48-121">Create a Service Fabric application, for example by [following this tutorial](service-fabric-tutorial-create-dotnet-app.md).</span></span> 
- <span data-ttu-id="6fd48-122">Vytvoření clusteru s podporou Windows Service Fabric v Azure, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-cluster-azure-ps.md)</span><span class="sxs-lookup"><span data-stu-id="6fd48-122">Create a Windows Service Fabric cluster on Azure, for example by [following this tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)</span></span>
- <span data-ttu-id="6fd48-123">Vytvoření [účtu Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="6fd48-123">Create a [Team Services account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span></span>

## <a name="download-the-voting-sample-application"></a><span data-ttu-id="6fd48-124">Stažení ukázkové aplikace Voting</span><span class="sxs-lookup"><span data-stu-id="6fd48-124">Download the Voting sample application</span></span>
<span data-ttu-id="6fd48-125">Pokud není sestavení Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout.</span><span class="sxs-lookup"><span data-stu-id="6fd48-125">If you did not build the Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="6fd48-126">V příkazovém okně spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="6fd48-126">In a command window, run the following command to clone the sample app repository to your local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a><span data-ttu-id="6fd48-127">Příprava profilu publikování</span><span class="sxs-lookup"><span data-stu-id="6fd48-127">Prepare a publish profile</span></span>
<span data-ttu-id="6fd48-128">Teď, když jste [vytvořili aplikaci](service-fabric-tutorial-create-dotnet-app.md) a mít [nasadit aplikaci do Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), jste připraveni nastavte průběžnou integraci.</span><span class="sxs-lookup"><span data-stu-id="6fd48-128">Now that you've [created an application](service-fabric-tutorial-create-dotnet-app.md) and have [deployed the application to Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), you're ready to set up continuous integration.</span></span>  <span data-ttu-id="6fd48-129">Nejdřív připravte profil publikování v rámci vaší aplikace pro použití procesu nasazení, který spouští v rámci Team Services.</span><span class="sxs-lookup"><span data-stu-id="6fd48-129">First, prepare a publish profile within your application for use by the deployment process that executes within Team Services.</span></span>  <span data-ttu-id="6fd48-130">Profil publikování, musí být nakonfigurovaný pro cluster, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6fd48-130">The publish profile should be configured to target the cluster that you've previously created.</span></span>  <span data-ttu-id="6fd48-131">Spuštění sady Visual Studio a otevřít existující projekt aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6fd48-131">Start Visual Studio and open an existing Service Fabric application project.</span></span>  <span data-ttu-id="6fd48-132">V **Průzkumníku řešení**, klikněte pravým tlačítkem na aplikaci a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="6fd48-132">In **Solution Explorer**, right-click the application and select **Publish...**.</span></span>

<span data-ttu-id="6fd48-133">Zvolte profil cíl v rámci projektu aplikace použít pro pracovní postup průběžnou integraci, například v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6fd48-133">Choose a target profile within your application project to use for your continuous integration workflow, for example Cloud.</span></span>  <span data-ttu-id="6fd48-134">Zadejte koncový bod připojení clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-134">Specify the cluster connection endpoint.</span></span>  <span data-ttu-id="6fd48-135">Zkontrolujte **upgradu aplikace** políčko tak, aby vaše aplikace upgrady pro každé nasazení v Team Services.</span><span class="sxs-lookup"><span data-stu-id="6fd48-135">Check the **Upgrade the Application** checkbox so that your application upgrades for each deployment in Team Services.</span></span>  <span data-ttu-id="6fd48-136">Klikněte **Uložit** hypertextový odkaz na Uložit nastavení do profil publikování a pak klikněte na **zrušit** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6fd48-136">Click the **Save** hyperlink to save the settings to the publish profile and then click **Cancel** to close the dialog box.</span></span>  

![Push profilu][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a><span data-ttu-id="6fd48-138">Sdílet řešení sady Visual Studio do nového úložiště Team Services Git</span><span class="sxs-lookup"><span data-stu-id="6fd48-138">Share your Visual Studio solution to a new Team Services Git repo</span></span>
<span data-ttu-id="6fd48-139">Sdílejte vaše zdrojové soubory aplikací do týmového projektu v Team Services, můžete vygenerovat sestavení.</span><span class="sxs-lookup"><span data-stu-id="6fd48-139">Share your application source files to a team project in Team Services so you can generate builds.</span></span>  

<span data-ttu-id="6fd48-140">Vytvoření nové místní úložiště Git pro svůj projekt tak, že vyberete **přidat do správy zdrojového kódu** -> **Git** na stavovém řádku v pravém dolním rohu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fd48-140">Create a new local Git repo for your project by selecting **Add to Source Control** -> **Git** on the status bar in the lower right-hand corner of Visual Studio.</span></span> 

<span data-ttu-id="6fd48-141">V **Push** zobrazit v **Team Explorer**, vyberte **publikování úložiště Git** tlačítko pod **nabízená Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-141">In the **Push** view in **Team Explorer**, select the **Publish Git Repo** button under **Push to Visual Studio Team Services**.</span></span>

![Úložiště Git push][push-git-repo]

<span data-ttu-id="6fd48-143">Ověřit e-mailu a vyberte svůj účet za **Team Services domény** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="6fd48-143">Verify your email and select your account in the **Team Services Domain** drop-down.</span></span> <span data-ttu-id="6fd48-144">Zadejte název úložiště a vyberte **publikovat úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-144">Enter your repository name and select **Publish repository**.</span></span>

![Úložiště Git push][publish-code]

<span data-ttu-id="6fd48-146">Publikování úložišti vytvoří nového týmového projektu v účtu se stejným názvem jako místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="6fd48-146">Publishing the repo creates a new team project in your account with the same name as the local repo.</span></span> <span data-ttu-id="6fd48-147">Chcete-li vytvořit úložišti v existující týmového projektu, klikněte na tlačítko **Upřesnit** vedle **úložiště** název a vyberte týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="6fd48-147">To create the repo in an existing team project, click **Advanced** next to **Repository** name and select a team project.</span></span> <span data-ttu-id="6fd48-148">Váš kód na webu můžete zobrazit výběrem **najdete v článku na webu**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-148">You can view your code on the web by selecting **See it on the web**.</span></span>

## <a name="configure-continuous-delivery-with-vsts"></a><span data-ttu-id="6fd48-149">Služby VSTS nakonfigurovat nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="6fd48-149">Configure Continuous Delivery with VSTS</span></span>
<span data-ttu-id="6fd48-150">Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně.</span><span class="sxs-lookup"><span data-stu-id="6fd48-150">A Team Services build definition describes a workflow that is composed of a set of build steps that are executed sequentially.</span></span> <span data-ttu-id="6fd48-151">Vytvoření definice sestavení, která vytvoří balíček aplikace Service Fabric a artefaktů, k nasazení na cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6fd48-151">Create a build definition that that produces a Service Fabric application package, and other artifacts, to deploy to a Service Fabric cluster.</span></span> <span data-ttu-id="6fd48-152">Další informace o [definice sestavení Team Services](https://www.visualstudio.com/docs/build/define/create).</span><span class="sxs-lookup"><span data-stu-id="6fd48-152">Learn more about [Team Services build definitions](https://www.visualstudio.com/docs/build/define/create).</span></span> 

<span data-ttu-id="6fd48-153">Verze definice Team Services popisuje pracovní postup, který nasadí balíček aplikace do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-153">A Team Services release definition describes a workflow that deploys an application package to a cluster.</span></span> <span data-ttu-id="6fd48-154">Při použití společně, definici sestavení a verze definice spustit celý pracovní postup od zdrojové soubory k konče spuštěné aplikace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-154">When used together, the build definition and release definition execute the entire workflow starting with source files to ending with a running application in your cluster.</span></span> <span data-ttu-id="6fd48-155">Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span><span class="sxs-lookup"><span data-stu-id="6fd48-155">Learn more about Team Services [release definitions](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span></span>

### <a name="create-a-build-definition"></a><span data-ttu-id="6fd48-156">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="6fd48-156">Create a build definition</span></span>
<span data-ttu-id="6fd48-157">Otevřete webový prohlížeč a přejděte do nového týmového projektu v: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting.</span><span class="sxs-lookup"><span data-stu-id="6fd48-157">Open a web browser and navigate to your new team project at: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting .</span></span> 

<span data-ttu-id="6fd48-158">Vyberte **sestavení a verze** kartě pak **sestavení**, pak **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-158">Select the **Build & Release** tab, then **Builds**, then **+ New definition**.</span></span>  <span data-ttu-id="6fd48-159">V **vyberte šablonu**, vyberte **aplikace Azure Service Fabric** šablonu a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-159">In **Select a template**, select the **Azure Service Fabric Application** template and click **Apply**.</span></span> 

![Výběr šablony sestavení][select-build-template] 

<span data-ttu-id="6fd48-161">Hlasovací aplikaci obsahuje projektu .NET Core, proto přidat úloha, která obnoví závislosti.</span><span class="sxs-lookup"><span data-stu-id="6fd48-161">The voting application contains a .NET Core project, so add a task that restores the dependencies.</span></span> <span data-ttu-id="6fd48-162">V **úlohy** zobrazit, vyberte možnost **+ přidat úloha** dole vlevo.</span><span class="sxs-lookup"><span data-stu-id="6fd48-162">In the **Tasks** view, select **+ Add Task** in the bottom left.</span></span> <span data-ttu-id="6fd48-163">Vyhledávání v "Příkazový řádek" najít úlohu příkazového řádku a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-163">Search on "Command Line" to find the command-line task, then click **Add**.</span></span> 

![Přidat úloha][add-task] 

<span data-ttu-id="6fd48-165">Nová úloha, zadejte "Spustit dotnet.exe" v **zobrazovaný název**, "dotnet.exe" v **nástroj**a "obnovení" v **argumenty**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-165">In the new task, enter "Run dotnet.exe" in **Display name**, "dotnet.exe" in **Tool**, and "restore" in **Arguments**.</span></span> 

![Nová úloha][new-task] 

<span data-ttu-id="6fd48-167">V **aktivační události** zobrazit, klikněte na tlačítko **povolení této aktivační události** přepínače v rámci **průběžnou integraci**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-167">In the **Triggers** view, click the **Enable this trigger** switch under **Continuous Integration**.</span></span> 

<span data-ttu-id="6fd48-168">Vyberte **Uložit & fronty** a zadejte "Hostované VS2017" jako **fronty agenta**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-168">Select **Save & queue** and enter "Hosted VS2017" as the **Agent queue**.</span></span> <span data-ttu-id="6fd48-169">Vyberte **fronty** ruční spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="6fd48-169">Select **Queue** to manually start a build.</span></span>  <span data-ttu-id="6fd48-170">Vytvoří také aktivační události nabízené nebo vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="6fd48-170">Builds also triggers upon push or check-in.</span></span>

<span data-ttu-id="6fd48-171">Chcete-li zkontrolovat průběh sestavení, přepněte na **sestavení** kartě.  Jakmile ověříte úspěšně spustí sestavení, definujte definici verze, která nasadí aplikaci do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-171">To check your build progress, switch to the **Builds** tab.  Once you verify that the build executes successfully, define a release definition that deploys your application to a cluster.</span></span> 

### <a name="create-a-release-definition"></a><span data-ttu-id="6fd48-172">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="6fd48-172">Create a release definition</span></span>  

<span data-ttu-id="6fd48-173">Vyberte **sestavení a verze** kartě pak **verze**, pak **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-173">Select the **Build & Release** tab, then **Releases**, then **+ New definition**.</span></span>  <span data-ttu-id="6fd48-174">V **vytvořit verze definice**, vyberte **Azure Service Fabric nasazení** šablonu ze seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-174">In **Create release definition**, select the **Azure Service Fabric Deployment** template from the list and click **Next**.</span></span>  <span data-ttu-id="6fd48-175">Vyberte **sestavení** zdroje, zkontrolujte **průběžné nasazování** pole a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-175">Select the **Build** source, check the **Continuous deployment** box, and click **Create**.</span></span> 

<span data-ttu-id="6fd48-176">V **prostředí** zobrazit, klikněte na tlačítko **přidat** napravo od **připojení clusteru**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-176">In the **Environments** view, click **Add** to the right of **Cluster Connection**.</span></span>  <span data-ttu-id="6fd48-177">Zadejte název připojení "mysftestcluster", koncový bod clusteru z "tcp://mysftestcluster.westus.cloudapp.azure.com:19000" a Azure Active Directory nebo přihlašovací údaje certifikátu pro cluster.</span><span class="sxs-lookup"><span data-stu-id="6fd48-177">Specify a connection name of "mysftestcluster", a cluster endpoint of "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", and the Azure Active Directory or certificate credentials for the cluster.</span></span> <span data-ttu-id="6fd48-178">Zadání přihlašovacích údajů Azure Active Directory, zadejte pověření, kterou chcete použít pro připojení k clusteru, **uživatelské jméno** a **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6fd48-178">For Azure Active Directory credentials, define the credentials you want to use to connect to the cluster in the **Username** and **Password** fields.</span></span> <span data-ttu-id="6fd48-179">Pro ověřování pomocí certifikátů, zadejte kódování Base64 souboru certifikátu klienta v **klientský certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="6fd48-179">For certificate-based authentication, define the Base64 encoding of the client certificate file in the **Client Certificate** field.</span></span>  <span data-ttu-id="6fd48-180">Naleznete v nápovědě automaticky otevírané okno na toto pole informace o tom, jak získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6fd48-180">See the help pop-up on that field for info on how to get that value.</span></span>  <span data-ttu-id="6fd48-181">Pokud váš certifikát je chráněný heslem, zadejte heslo do **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6fd48-181">If your certificate is password-protected, define the password in the **Password** field.</span></span>  <span data-ttu-id="6fd48-182">Klikněte na tlačítko **Uložit** se uložit definici verze.</span><span class="sxs-lookup"><span data-stu-id="6fd48-182">Click **Save** to save the release definition.</span></span>

![Přidat připojení clusteru][add-cluster-connection] 

<span data-ttu-id="6fd48-184">Klikněte na tlačítko **spustit na agentovi**, pak vyberte **hostované VS2017** pro **nasazení fronty**.</span><span class="sxs-lookup"><span data-stu-id="6fd48-184">Click **Run on agent**, then select **Hosted VS2017** for **Deployment queue**.</span></span> <span data-ttu-id="6fd48-185">Klikněte na tlačítko **Uložit** se uložit definici verze.</span><span class="sxs-lookup"><span data-stu-id="6fd48-185">Click **Save** to save the release definition.</span></span>

![Spustit u agenta][run-on-agent]

<span data-ttu-id="6fd48-187">Vyberte **+ verze** -> **vytvořit verzi** -> **vytvořit** ručně vytvořit verze.</span><span class="sxs-lookup"><span data-stu-id="6fd48-187">Select **+Release** -> **Create Release** -> **Create** to manually create a release.</span></span>  <span data-ttu-id="6fd48-188">Ověřte, zda nasazení bylo úspěšné a aplikace běží v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-188">Verify that the deployment succeeded and the application is running in the cluster.</span></span>  <span data-ttu-id="6fd48-189">Otevřete webový prohlížeč a přejděte do [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="6fd48-189">Open a web browser and navigate to [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="6fd48-190">Poznámka: verze aplikace, v tomto příkladu je "1.0.0.20170616.3".</span><span class="sxs-lookup"><span data-stu-id="6fd48-190">Note the application version, in this example it is "1.0.0.20170616.3".</span></span> 

## <a name="commit-and-push-changes-trigger-a-release"></a><span data-ttu-id="6fd48-191">Potvrzení a doručte změny, aktivace vydání verze</span><span class="sxs-lookup"><span data-stu-id="6fd48-191">Commit and push changes, trigger a release</span></span>
<span data-ttu-id="6fd48-192">K ověření, že kanál průběžnou integraci funguje kontrolou v některé změny kódu Team Services.</span><span class="sxs-lookup"><span data-stu-id="6fd48-192">To verify that the continuous integration pipeline is functioning by checking in some code changes to Team Services.</span></span>    

<span data-ttu-id="6fd48-193">Při psaní kódu, změny jsou sledovány automaticky Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fd48-193">As you write your code, your changes are automatically tracked by Visual Studio.</span></span> <span data-ttu-id="6fd48-194">Potvrzení změn místní úložiště Git tak, že vyberete ikonu (čekajících změn</span><span class="sxs-lookup"><span data-stu-id="6fd48-194">Commit changes to your local Git repository by selecting the pending changes icon (</span></span>![Čekající na vyřízení][pending]<span data-ttu-id="6fd48-196">) ze stavového řádku v vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="6fd48-196">) from the status bar in the bottom right.</span></span>

<span data-ttu-id="6fd48-197">Na **změny** zobrazení v Team Exploreru, přidat zprávu s popisem aktualizace a provést změny.</span><span class="sxs-lookup"><span data-stu-id="6fd48-197">On the **Changes** view in Team Explorer, add a message describing your update and commit your changes.</span></span>

![Potvrdit všechny][changes]

<span data-ttu-id="6fd48-199">Vyberte ikonu nepublikované změny stavu panelu (![Nepublikováno změny][unpublished-changes]) nebo synchronizace zobrazení v Team Exploreru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-199">Select the unpublished changes status bar icon (![Unpublished changes][unpublished-changes]) or the Sync view in Team Explorer.</span></span> <span data-ttu-id="6fd48-200">Vyberte **Push** aktualizovat kódu v produktu Team Services nebo TFS.</span><span class="sxs-lookup"><span data-stu-id="6fd48-200">Select **Push** to update your code in Team Services/TFS.</span></span>

![Doručte změny][push]

<span data-ttu-id="6fd48-202">Když zavedete změny Team Services automaticky aktivuje build.</span><span class="sxs-lookup"><span data-stu-id="6fd48-202">Pushing the changes to Team Services automatically triggers a build.</span></span>  <span data-ttu-id="6fd48-203">Pokud definici sestavení úspěšně dokončí, verze se automaticky vytvoří a spustí upgrade aplikace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-203">When the build definition successfully completes, a release is automatically created and starts upgrading the application on the cluster.</span></span>

<span data-ttu-id="6fd48-204">Chcete-li zkontrolovat průběh sestavení, přepněte na **sestavení** kartě v **Team Explorer** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fd48-204">To check your build progress, switch to the **Builds** tab in **Team Explorer** in Visual Studio.</span></span>  <span data-ttu-id="6fd48-205">Jakmile ověříte úspěšně spustí sestavení, definujte definici verze, která nasadí aplikaci do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-205">Once you verify that the build executes successfully, define a release definition that deploys your application to a cluster.</span></span>

<span data-ttu-id="6fd48-206">Ověřte, zda nasazení bylo úspěšné a aplikace běží v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fd48-206">Verify that the deployment succeeded and the application is running in the cluster.</span></span>  <span data-ttu-id="6fd48-207">Otevřete webový prohlížeč a přejděte do [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="6fd48-207">Open a web browser and navigate to [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="6fd48-208">Poznámka: verze aplikace, v tomto příkladu je "1.0.0.20170815.3".</span><span class="sxs-lookup"><span data-stu-id="6fd48-208">Note the application version, in this example it is "1.0.0.20170815.3".</span></span>

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a><span data-ttu-id="6fd48-210">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="6fd48-210">Update the application</span></span>
<span data-ttu-id="6fd48-211">Proveďte změny kódu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6fd48-211">Make code changes in the application.</span></span>  <span data-ttu-id="6fd48-212">Uložte a provedení změn, podle předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="6fd48-212">Save and commit the changes, following the previous steps.</span></span>

<span data-ttu-id="6fd48-213">Po upgradu aplikace, můžete sledovat průběh upgradu v Service Fabric Exploreru:</span><span class="sxs-lookup"><span data-stu-id="6fd48-213">Once the upgrade of the application begins, you can watch the upgrade progress in Service Fabric Explorer:</span></span>

![Service Fabric Explorer][sfx2]

<span data-ttu-id="6fd48-215">Upgrade aplikace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6fd48-215">The application upgrade may take several minutes.</span></span> <span data-ttu-id="6fd48-216">Po dokončení upgradu bude aplikace spuštěna další verze.</span><span class="sxs-lookup"><span data-stu-id="6fd48-216">When the upgrade is complete, the application will be running the next version.</span></span>  <span data-ttu-id="6fd48-217">V tomto příkladu "1.0.0.20170815.4".</span><span class="sxs-lookup"><span data-stu-id="6fd48-217">In this example "1.0.0.20170815.4".</span></span>

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a><span data-ttu-id="6fd48-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fd48-219">Next steps</span></span>
<span data-ttu-id="6fd48-220">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="6fd48-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fd48-221">Do projektu přidejte zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="6fd48-221">Add source control to your project</span></span>
> * <span data-ttu-id="6fd48-222">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="6fd48-222">Create a build definition</span></span>
> * <span data-ttu-id="6fd48-223">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="6fd48-223">Create a release definition</span></span>
> * <span data-ttu-id="6fd48-224">Automatické nasazení a upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="6fd48-224">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="6fd48-225">Teď, když máte nasazené aplikace a nakonfigurovali průběžnou integraci, zkuste následující postup:</span><span class="sxs-lookup"><span data-stu-id="6fd48-225">Now that you have deployed an application and configured continuous integration, try the following:</span></span>
- [<span data-ttu-id="6fd48-226">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="6fd48-226">Upgrade an app</span></span>](service-fabric-application-upgrade.md)
- [<span data-ttu-id="6fd48-227">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="6fd48-227">Test an app</span></span>](service-fabric-testability-overview.md) 
- [<span data-ttu-id="6fd48-228">Monitorování a Diagnostika</span><span class="sxs-lookup"><span data-stu-id="6fd48-228">Monitor and diagnose</span></span>](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png