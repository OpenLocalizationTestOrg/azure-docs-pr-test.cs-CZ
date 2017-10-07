---
title: "aaaDeploy aplikace Azure Service Fabric pomocí průběžnou integraci (Team Services) | Microsoft Docs"
description: "Zjistěte, jak tooset průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services.  Nasazení clusteru Service Fabric tooa aplikace v Azure."
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
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a><span data-ttu-id="646b2-104">Nasazení aplikace se cluster Service Fabric tooa CI/CD</span><span class="sxs-lookup"><span data-stu-id="646b2-104">Deploy an application with CI/CD tooa Service Fabric cluster</span></span>
<span data-ttu-id="646b2-105">V tomto kurzu je součástí tři řady a popisuje, jak tooset průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="646b2-105">This tutorial is part three of a series and describes how tooset up continuous integration and deployment for an Azure Service Fabric application using Visual Studio Team Services.</span></span>  <span data-ttu-id="646b2-106">Je potřeba stávající aplikace Service Fabric, hello aplikace vytvořené v [sestavení aplikace .NET](service-fabric-tutorial-create-dotnet-app.md) slouží jako příklad.</span><span class="sxs-lookup"><span data-stu-id="646b2-106">An existing Service Fabric application is needed, hello application created in [Build a .NET application](service-fabric-tutorial-create-dotnet-app.md) is used as an example.</span></span>

<span data-ttu-id="646b2-107">V rámci tři řady hello, zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="646b2-107">In part three of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="646b2-108">Přidat projekt tooyour správy zdrojů</span><span class="sxs-lookup"><span data-stu-id="646b2-108">Add source control tooyour project</span></span>
> * <span data-ttu-id="646b2-109">Vytvořit definici sestavení v Team Services</span><span class="sxs-lookup"><span data-stu-id="646b2-109">Create a build definition in Team Services</span></span>
> * <span data-ttu-id="646b2-110">Vytvoření definice verze v Team Services</span><span class="sxs-lookup"><span data-stu-id="646b2-110">Create a release definition in Team Services</span></span>
> * <span data-ttu-id="646b2-111">Automatické nasazení a upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="646b2-111">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="646b2-112">V této série kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="646b2-112">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="646b2-113">Sestavení aplikace .NET Service Fabric</span><span class="sxs-lookup"><span data-stu-id="646b2-113">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * [<span data-ttu-id="646b2-114">Nasazení vzdáleného clusteru tooa hello aplikace</span><span class="sxs-lookup"><span data-stu-id="646b2-114">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * <span data-ttu-id="646b2-115">Konfigurace CI/CD pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="646b2-115">Configure CI/CD using Visual Studio Team Services</span></span>

## <a name="prerequisites"></a><span data-ttu-id="646b2-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="646b2-116">Prerequisites</span></span>
<span data-ttu-id="646b2-117">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="646b2-117">Before you begin this tutorial:</span></span>
- <span data-ttu-id="646b2-118">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="646b2-118">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="646b2-119">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte hello **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="646b2-119">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="646b2-120">Nainstalujte hello Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="646b2-120">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)
- <span data-ttu-id="646b2-121">Vytvoření aplikace Service Fabric, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-dotnet-app.md).</span><span class="sxs-lookup"><span data-stu-id="646b2-121">Create a Service Fabric application, for example by [following this tutorial](service-fabric-tutorial-create-dotnet-app.md).</span></span> 
- <span data-ttu-id="646b2-122">Vytvoření clusteru s podporou Windows Service Fabric v Azure, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-cluster-azure-ps.md)</span><span class="sxs-lookup"><span data-stu-id="646b2-122">Create a Windows Service Fabric cluster on Azure, for example by [following this tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)</span></span>
- <span data-ttu-id="646b2-123">Vytvoření [účtu Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="646b2-123">Create a [Team Services account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span></span>

## <a name="download-hello-voting-sample-application"></a><span data-ttu-id="646b2-124">Stáhněte si ukázkovou aplikaci Voting hello</span><span class="sxs-lookup"><span data-stu-id="646b2-124">Download hello Voting sample application</span></span>
<span data-ttu-id="646b2-125">Pokud není sestavení hello Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout.</span><span class="sxs-lookup"><span data-stu-id="646b2-125">If you did not build hello Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="646b2-126">V příkazovém okně spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-126">In a command window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a><span data-ttu-id="646b2-127">Příprava profilu publikování</span><span class="sxs-lookup"><span data-stu-id="646b2-127">Prepare a publish profile</span></span>
<span data-ttu-id="646b2-128">Teď, když jste [vytvořili aplikaci](service-fabric-tutorial-create-dotnet-app.md) a mít [nasazené aplikace tooAzure hello](service-fabric-tutorial-deploy-app-to-party-cluster.md), jste připravené tooset až průběžnou integraci.</span><span class="sxs-lookup"><span data-stu-id="646b2-128">Now that you've [created an application](service-fabric-tutorial-create-dotnet-app.md) and have [deployed hello application tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md), you're ready tooset up continuous integration.</span></span>  <span data-ttu-id="646b2-129">Nejdřív připravte profil publikování v rámci vaší aplikace pro použití hello procesu nasazení, který spouští v rámci Team Services.</span><span class="sxs-lookup"><span data-stu-id="646b2-129">First, prepare a publish profile within your application for use by hello deployment process that executes within Team Services.</span></span>  <span data-ttu-id="646b2-130">profil publikování se Hello by měla být nakonfigurované tootarget hello clusteru, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="646b2-130">hello publish profile should be configured tootarget hello cluster that you've previously created.</span></span>  <span data-ttu-id="646b2-131">Spuštění sady Visual Studio a otevřít existující projekt aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="646b2-131">Start Visual Studio and open an existing Service Fabric application project.</span></span>  <span data-ttu-id="646b2-132">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello aplikace a vyberte **publikování...** .</span><span class="sxs-lookup"><span data-stu-id="646b2-132">In **Solution Explorer**, right-click hello application and select **Publish...**.</span></span>

<span data-ttu-id="646b2-133">Zvolte profil cílového v rámci vaší toouse projekt aplikace pro pracovní postup průběžnou integraci, například v cloudu.</span><span class="sxs-lookup"><span data-stu-id="646b2-133">Choose a target profile within your application project toouse for your continuous integration workflow, for example Cloud.</span></span>  <span data-ttu-id="646b2-134">Zadejte hello koncového bodu připojení clusteru.</span><span class="sxs-lookup"><span data-stu-id="646b2-134">Specify hello cluster connection endpoint.</span></span>  <span data-ttu-id="646b2-135">Zkontrolujte hello **upgradu hello aplikace** políčko tak, aby vaše aplikace upgrady pro každé nasazení v Team Services.</span><span class="sxs-lookup"><span data-stu-id="646b2-135">Check hello **Upgrade hello Application** checkbox so that your application upgrades for each deployment in Team Services.</span></span>  <span data-ttu-id="646b2-136">Klikněte na tlačítko hello **Uložit** hypertextový odkaz toosave hello nastavení toohello profil publikování a pak klikněte na **zrušit** dialogové okno tooclose hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-136">Click hello **Save** hyperlink toosave hello settings toohello publish profile and then click **Cancel** tooclose hello dialog box.</span></span>  

![Push profilu][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a><span data-ttu-id="646b2-138">Sdílet vaše sady Visual Studio řešení tooa nové Team Services úložiště Git</span><span class="sxs-lookup"><span data-stu-id="646b2-138">Share your Visual Studio solution tooa new Team Services Git repo</span></span>
<span data-ttu-id="646b2-139">Sdílejte zdrojových souborech aplikace, které tooa týmového projektu v Team Services, můžete vygenerovat sestavení.</span><span class="sxs-lookup"><span data-stu-id="646b2-139">Share your application source files tooa team project in Team Services so you can generate builds.</span></span>  

<span data-ttu-id="646b2-140">Vytvoření nové místní úložiště Git pro svůj projekt tak, že vyberete **přidat tooSource řízení** -> **Git** na stavovém řádku hello v hello pravém dolním rohu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="646b2-140">Create a new local Git repo for your project by selecting **Add tooSource Control** -> **Git** on hello status bar in hello lower right-hand corner of Visual Studio.</span></span> 

<span data-ttu-id="646b2-141">V hello **Push** zobrazit v **Team Explorer**, vyberte hello **publikování úložiště Git** tlačítko pod **Push tooVisual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="646b2-141">In hello **Push** view in **Team Explorer**, select hello **Publish Git Repo** button under **Push tooVisual Studio Team Services**.</span></span>

![Úložiště Git push][push-git-repo]

<span data-ttu-id="646b2-143">Ověřit e-mailu a vyberte svůj účet v hello **Team Services domény** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="646b2-143">Verify your email and select your account in hello **Team Services Domain** drop-down.</span></span> <span data-ttu-id="646b2-144">Zadejte název úložiště a vyberte **publikovat úložiště**.</span><span class="sxs-lookup"><span data-stu-id="646b2-144">Enter your repository name and select **Publish repository**.</span></span>

![Úložiště Git push][publish-code]

<span data-ttu-id="646b2-146">Ve vašem účtu s hello stejný název jako místní úložiště hello publikování hello úložišti vytvoří nového týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="646b2-146">Publishing hello repo creates a new team project in your account with hello same name as hello local repo.</span></span> <span data-ttu-id="646b2-147">toocreate hello úložišti v existující týmového projektu, klikněte na tlačítko **Upřesnit** další příliš**úložiště** název a vyberte týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="646b2-147">toocreate hello repo in an existing team project, click **Advanced** next too**Repository** name and select a team project.</span></span> <span data-ttu-id="646b2-148">Váš kód můžete zobrazit na webu hello výběrem **najdete v článku na webu hello**.</span><span class="sxs-lookup"><span data-stu-id="646b2-148">You can view your code on hello web by selecting **See it on hello web**.</span></span>

## <a name="configure-continuous-delivery-with-vsts"></a><span data-ttu-id="646b2-149">Služby VSTS nakonfigurovat nastavené průběžné doručování</span><span class="sxs-lookup"><span data-stu-id="646b2-149">Configure Continuous Delivery with VSTS</span></span>
<span data-ttu-id="646b2-150">Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně.</span><span class="sxs-lookup"><span data-stu-id="646b2-150">A Team Services build definition describes a workflow that is composed of a set of build steps that are executed sequentially.</span></span> <span data-ttu-id="646b2-151">Vytvoření definice sestavení, která vytvoří balíček aplikace Service Fabric a další artefakty, cluster Service Fabric tooa toodeploy.</span><span class="sxs-lookup"><span data-stu-id="646b2-151">Create a build definition that that produces a Service Fabric application package, and other artifacts, toodeploy tooa Service Fabric cluster.</span></span> <span data-ttu-id="646b2-152">Další informace o [definice sestavení Team Services](https://www.visualstudio.com/docs/build/define/create).</span><span class="sxs-lookup"><span data-stu-id="646b2-152">Learn more about [Team Services build definitions](https://www.visualstudio.com/docs/build/define/create).</span></span> 

<span data-ttu-id="646b2-153">Verze definice Team Services popisuje pracovní postup, který nasadí cluster tooa balíček služby aplikací.</span><span class="sxs-lookup"><span data-stu-id="646b2-153">A Team Services release definition describes a workflow that deploys an application package tooa cluster.</span></span> <span data-ttu-id="646b2-154">Při použití společně, hello sestavení definice a verze definice provést hello celý pracovní postup, počínaje tooending zdrojové soubory s běžící aplikací v clusteru.</span><span class="sxs-lookup"><span data-stu-id="646b2-154">When used together, hello build definition and release definition execute hello entire workflow starting with source files tooending with a running application in your cluster.</span></span> <span data-ttu-id="646b2-155">Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span><span class="sxs-lookup"><span data-stu-id="646b2-155">Learn more about Team Services [release definitions](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span></span>

### <a name="create-a-build-definition"></a><span data-ttu-id="646b2-156">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="646b2-156">Create a build definition</span></span>
<span data-ttu-id="646b2-157">Otevřete webový prohlížeč a přejděte tooyour nového týmového projektu v: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting.</span><span class="sxs-lookup"><span data-stu-id="646b2-157">Open a web browser and navigate tooyour new team project at: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting .</span></span> 

<span data-ttu-id="646b2-158">Vyberte hello **sestavení a verze** kartě pak **sestavení**, pak **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="646b2-158">Select hello **Build & Release** tab, then **Builds**, then **+ New definition**.</span></span>  <span data-ttu-id="646b2-159">V **vyberte šablonu**, vyberte hello **aplikace Azure Service Fabric** šablonu a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="646b2-159">In **Select a template**, select hello **Azure Service Fabric Application** template and click **Apply**.</span></span> 

![Výběr šablony sestavení][select-build-template] 

<span data-ttu-id="646b2-161">Hello hlasování aplikace obsahuje aplikace .NET Core, proto přidat úloha, která obnoví hello závislosti.</span><span class="sxs-lookup"><span data-stu-id="646b2-161">hello voting application contains a .NET Core project, so add a task that restores hello dependencies.</span></span> <span data-ttu-id="646b2-162">V hello **úlohy** zobrazit, vyberte možnost **+ přidat úloha** hello dole vlevo.</span><span class="sxs-lookup"><span data-stu-id="646b2-162">In hello **Tasks** view, select **+ Add Task** in hello bottom left.</span></span> <span data-ttu-id="646b2-163">Hledání "Příkazový řádek" toofind hello úlohu příkazového řádku a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="646b2-163">Search on "Command Line" toofind hello command-line task, then click **Add**.</span></span> 

![Přidat úloha][add-task] 

<span data-ttu-id="646b2-165">V hello novou úlohu, zadejte "Spustit dotnet.exe" v **zobrazovaný název**, "dotnet.exe" v **nástroj**a "obnovení" v **argumenty**.</span><span class="sxs-lookup"><span data-stu-id="646b2-165">In hello new task, enter "Run dotnet.exe" in **Display name**, "dotnet.exe" in **Tool**, and "restore" in **Arguments**.</span></span> 

![Nová úloha][new-task] 

<span data-ttu-id="646b2-167">V hello **aktivační události** zobrazit, klikněte na tlačítko hello **povolení této aktivační události** přepínače v rámci **průběžnou integraci**.</span><span class="sxs-lookup"><span data-stu-id="646b2-167">In hello **Triggers** view, click hello **Enable this trigger** switch under **Continuous Integration**.</span></span> 

<span data-ttu-id="646b2-168">Vyberte **Uložit & fronty** a zadejte "Hostované VS2017" jako hello **fronty agenta**.</span><span class="sxs-lookup"><span data-stu-id="646b2-168">Select **Save & queue** and enter "Hosted VS2017" as hello **Agent queue**.</span></span> <span data-ttu-id="646b2-169">Vyberte **fronty** toomanually spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="646b2-169">Select **Queue** toomanually start a build.</span></span>  <span data-ttu-id="646b2-170">Vytvoří také aktivační události nabízené nebo vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="646b2-170">Builds also triggers upon push or check-in.</span></span>

<span data-ttu-id="646b2-171">toocheck sestavení průběh, přepínač toohello **sestavení** kartě.  Po ověření úspěšně spustí hello sestavení, zadejte definici verze, která nasadí cluster tooa aplikace.</span><span class="sxs-lookup"><span data-stu-id="646b2-171">toocheck your build progress, switch toohello **Builds** tab.  Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span> 

### <a name="create-a-release-definition"></a><span data-ttu-id="646b2-172">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="646b2-172">Create a release definition</span></span>  

<span data-ttu-id="646b2-173">Vyberte hello **sestavení a verze** kartě pak **verze**, pak **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="646b2-173">Select hello **Build & Release** tab, then **Releases**, then **+ New definition**.</span></span>  <span data-ttu-id="646b2-174">V **vytvořit verze definice**, vyberte hello **Azure Service Fabric nasazení** šablonu ze seznamu hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="646b2-174">In **Create release definition**, select hello **Azure Service Fabric Deployment** template from hello list and click **Next**.</span></span>  <span data-ttu-id="646b2-175">Vyberte hello **sestavení** zdroje, zkontrolujte hello **průběžné nasazování** pole a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="646b2-175">Select hello **Build** source, check hello **Continuous deployment** box, and click **Create**.</span></span> 

<span data-ttu-id="646b2-176">V hello **prostředí** zobrazit, klikněte na tlačítko **přidat** toohello napravo od **připojení clusteru**.</span><span class="sxs-lookup"><span data-stu-id="646b2-176">In hello **Environments** view, click **Add** toohello right of **Cluster Connection**.</span></span>  <span data-ttu-id="646b2-177">Zadejte název připojení "mysftestcluster", "tcp://mysftestcluster.westus.cloudapp.azure.com:19000" a hello Azure Active Directory nebo přihlašovací údaje certifikátu koncový bod clusteru pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="646b2-177">Specify a connection name of "mysftestcluster", a cluster endpoint of "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", and hello Azure Active Directory or certificate credentials for hello cluster.</span></span> <span data-ttu-id="646b2-178">U přihlašovacích údajů Azure Active Directory, definovat hello pověření, které chcete cluster toohello tooconnect toouse v hello **uživatelské jméno** a **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="646b2-178">For Azure Active Directory credentials, define hello credentials you want toouse tooconnect toohello cluster in hello **Username** and **Password** fields.</span></span> <span data-ttu-id="646b2-179">Pro ověřování pomocí certifikátů, zadejte hello Base64 kódování souboru certifikátu klienta hello v hello **klientský certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="646b2-179">For certificate-based authentication, define hello Base64 encoding of hello client certificate file in hello **Client Certificate** field.</span></span>  <span data-ttu-id="646b2-180">Zobrazit automaticky otevírané okno nápovědy hello na informace o tom, že pole tooget, která hodnota.</span><span class="sxs-lookup"><span data-stu-id="646b2-180">See hello help pop-up on that field for info on how tooget that value.</span></span>  <span data-ttu-id="646b2-181">Pokud váš certifikát je chráněný heslem, zadejte heslo hello v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="646b2-181">If your certificate is password-protected, define hello password in hello **Password** field.</span></span>  <span data-ttu-id="646b2-182">Klikněte na tlačítko **Uložit** toosave hello verze definice.</span><span class="sxs-lookup"><span data-stu-id="646b2-182">Click **Save** toosave hello release definition.</span></span>

![Přidat připojení clusteru][add-cluster-connection] 

<span data-ttu-id="646b2-184">Klikněte na tlačítko **spustit na agentovi**, pak vyberte **hostované VS2017** pro **nasazení fronty**.</span><span class="sxs-lookup"><span data-stu-id="646b2-184">Click **Run on agent**, then select **Hosted VS2017** for **Deployment queue**.</span></span> <span data-ttu-id="646b2-185">Klikněte na tlačítko **Uložit** toosave hello verze definice.</span><span class="sxs-lookup"><span data-stu-id="646b2-185">Click **Save** toosave hello release definition.</span></span>

![Spustit u agenta][run-on-agent]

<span data-ttu-id="646b2-187">Vyberte **+ verze** -> **vytvořit verzi** -> **vytvořit** toomanually vytvořit verzi.</span><span class="sxs-lookup"><span data-stu-id="646b2-187">Select **+Release** -> **Create Release** -> **Create** toomanually create a release.</span></span>  <span data-ttu-id="646b2-188">Ověřte, zda text hello nasazení bylo úspěšné a hello aplikace běží v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-188">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="646b2-189">Otevřete webový prohlížeč a přejděte příliš[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="646b2-189">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="646b2-190">Poznámka: verze aplikace hello, v tomto příkladu je "1.0.0.20170616.3".</span><span class="sxs-lookup"><span data-stu-id="646b2-190">Note hello application version, in this example it is "1.0.0.20170616.3".</span></span> 

## <a name="commit-and-push-changes-trigger-a-release"></a><span data-ttu-id="646b2-191">Potvrzení a doručte změny, aktivace vydání verze</span><span class="sxs-lookup"><span data-stu-id="646b2-191">Commit and push changes, trigger a release</span></span>
<span data-ttu-id="646b2-192">v některých změn kódu kontrolou tooTeam služby funguje tooverify, který hello průběžnou integraci kanálu.</span><span class="sxs-lookup"><span data-stu-id="646b2-192">tooverify that hello continuous integration pipeline is functioning by checking in some code changes tooTeam Services.</span></span>    

<span data-ttu-id="646b2-193">Při psaní kódu, změny jsou sledovány automaticky Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="646b2-193">As you write your code, your changes are automatically tracked by Visual Studio.</span></span> <span data-ttu-id="646b2-194">Potvrdit změny tooyour místní úložiště Git tak, že vyberete hello čekající změny ikonu (</span><span class="sxs-lookup"><span data-stu-id="646b2-194">Commit changes tooyour local Git repository by selecting hello pending changes icon (</span></span>![Čekající na vyřízení][pending]<span data-ttu-id="646b2-196">) z hello stavového řádku v hello vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="646b2-196">) from hello status bar in hello bottom right.</span></span>

<span data-ttu-id="646b2-197">Na hello **změny** zobrazení v Team Exploreru, přidat zprávu s popisem aktualizace a provést změny.</span><span class="sxs-lookup"><span data-stu-id="646b2-197">On hello **Changes** view in Team Explorer, add a message describing your update and commit your changes.</span></span>

![Potvrdit všechny][changes]

<span data-ttu-id="646b2-199">Ikona vyberte hello nepublikované změny stavového řádku (![Nepublikováno změny][unpublished-changes]) nebo hello synchronizace zobrazení v Team Exploreru.</span><span class="sxs-lookup"><span data-stu-id="646b2-199">Select hello unpublished changes status bar icon (![Unpublished changes][unpublished-changes]) or hello Sync view in Team Explorer.</span></span> <span data-ttu-id="646b2-200">Vyberte **Push** tooupdate kódu v produktu Team Services nebo TFS.</span><span class="sxs-lookup"><span data-stu-id="646b2-200">Select **Push** tooupdate your code in Team Services/TFS.</span></span>

![Doručte změny][push]

<span data-ttu-id="646b2-202">Když zavedete hello změny tooTeam služby automaticky aktivační události sestavení.</span><span class="sxs-lookup"><span data-stu-id="646b2-202">Pushing hello changes tooTeam Services automatically triggers a build.</span></span>  <span data-ttu-id="646b2-203">Pokud definici sestavení hello úspěšně dokončí, verze se automaticky vytvoří a spustí upgrade hello aplikace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-203">When hello build definition successfully completes, a release is automatically created and starts upgrading hello application on hello cluster.</span></span>

<span data-ttu-id="646b2-204">toocheck sestavení průběh, přepínač toohello **sestavení** kartě v **Team Explorer** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="646b2-204">toocheck your build progress, switch toohello **Builds** tab in **Team Explorer** in Visual Studio.</span></span>  <span data-ttu-id="646b2-205">Po ověření úspěšně spustí hello sestavení, zadejte definici verze, která nasadí cluster tooa aplikace.</span><span class="sxs-lookup"><span data-stu-id="646b2-205">Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span>

<span data-ttu-id="646b2-206">Ověřte, zda text hello nasazení bylo úspěšné a hello aplikace běží v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-206">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="646b2-207">Otevřete webový prohlížeč a přejděte příliš[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="646b2-207">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="646b2-208">Poznámka: verze aplikace hello, v tomto příkladu je "1.0.0.20170815.3".</span><span class="sxs-lookup"><span data-stu-id="646b2-208">Note hello application version, in this example it is "1.0.0.20170815.3".</span></span>

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a><span data-ttu-id="646b2-210">Aktualizovat aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="646b2-210">Update hello application</span></span>
<span data-ttu-id="646b2-211">Proveďte změny kódu v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="646b2-211">Make code changes in hello application.</span></span>  <span data-ttu-id="646b2-212">Uložte a provedení změn hello, hello předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="646b2-212">Save and commit hello changes, following hello previous steps.</span></span>

<span data-ttu-id="646b2-213">Jakmile hello upgrade aplikace hello začne, můžete sledovat průběh upgradu hello v Service Fabric Exploreru:</span><span class="sxs-lookup"><span data-stu-id="646b2-213">Once hello upgrade of hello application begins, you can watch hello upgrade progress in Service Fabric Explorer:</span></span>

![Service Fabric Explorer][sfx2]

<span data-ttu-id="646b2-215">upgrade aplikace Hello může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="646b2-215">hello application upgrade may take several minutes.</span></span> <span data-ttu-id="646b2-216">Po dokončení upgradu hello hello bude spuštěna aplikace hello další verze.</span><span class="sxs-lookup"><span data-stu-id="646b2-216">When hello upgrade is complete, hello application will be running hello next version.</span></span>  <span data-ttu-id="646b2-217">V tomto příkladu "1.0.0.20170815.4".</span><span class="sxs-lookup"><span data-stu-id="646b2-217">In this example "1.0.0.20170815.4".</span></span>

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a><span data-ttu-id="646b2-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="646b2-219">Next steps</span></span>
<span data-ttu-id="646b2-220">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="646b2-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="646b2-221">Přidat projekt tooyour správy zdrojů</span><span class="sxs-lookup"><span data-stu-id="646b2-221">Add source control tooyour project</span></span>
> * <span data-ttu-id="646b2-222">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="646b2-222">Create a build definition</span></span>
> * <span data-ttu-id="646b2-223">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="646b2-223">Create a release definition</span></span>
> * <span data-ttu-id="646b2-224">Automatické nasazení a upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="646b2-224">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="646b2-225">Teď, když máte nasazené aplikace a nakonfigurovali průběžnou integraci, zkuste následující hello:</span><span class="sxs-lookup"><span data-stu-id="646b2-225">Now that you have deployed an application and configured continuous integration, try hello following:</span></span>
- [<span data-ttu-id="646b2-226">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="646b2-226">Upgrade an app</span></span>](service-fabric-application-upgrade.md)
- [<span data-ttu-id="646b2-227">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="646b2-227">Test an app</span></span>](service-fabric-testability-overview.md) 
- [<span data-ttu-id="646b2-228">Monitorování a Diagnostika</span><span class="sxs-lookup"><span data-stu-id="646b2-228">Monitor and diagnose</span></span>](service-fabric-diagnostics-overview.md)


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