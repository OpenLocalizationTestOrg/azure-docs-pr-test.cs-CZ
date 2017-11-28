---
title: "aaaDeploy a upgradujte místně Azure mikroslužeb | Microsoft Docs"
description: "Zjistěte, jak nasadit existující aplikaci tooit tooset si místní cluster Service Fabric a potom tuto aplikaci upgradujte."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a><span data-ttu-id="d8d4d-103">Začínáme s nasazením a upgradem aplikací v místním clusteru</span><span class="sxs-lookup"><span data-stu-id="d8d4d-103">Get started with deploying and upgrading applications on your local cluster</span></span>
<span data-ttu-id="d8d4d-104">Hello Azure Service Fabric SDK zahrnuje úplné místní vývojové prostředí, který můžete použít tooquickly Začínáme s nasazením a správou aplikací v místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-104">hello Azure Service Fabric SDK includes a full local development environment that you can use tooquickly get started with deploying and managing applications on a local cluster.</span></span> <span data-ttu-id="d8d4d-105">V tomto článku vytvoříte místní cluster, nasazení tooit existující aplikaci a pak upgradovat aplikaci tooa nové verze, všechny z prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-105">In this article, you create a local cluster, deploy an existing application tooit, and then upgrade that application tooa new version, all from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="d8d4d-106">Tento článek vychází z předpokladu, že už máte [nastavené vývojové prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-106">This article assumes that you already [set up your development environment](service-fabric-get-started.md).</span></span>
> 
> 

## <a name="create-a-local-cluster"></a><span data-ttu-id="d8d4d-107">Vytvoření místního clusteru</span><span class="sxs-lookup"><span data-stu-id="d8d4d-107">Create a local cluster</span></span>
<span data-ttu-id="d8d4d-108">Cluster Service Fabric tvoří sada hardwarových prostředků, na které můžete nasazovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-108">A Service Fabric cluster represents a set of hardware resources that you can deploy applications to.</span></span> <span data-ttu-id="d8d4d-109">Obvykle clusteru je tvořen kdekoli z pěti toomany tisíc počítačů.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-109">Typically, a cluster is made up of anywhere from five toomany thousands of machines.</span></span> <span data-ttu-id="d8d4d-110">Hello Service Fabric SDK ale obsahuje konfiguraci clusteru, můžete spustit na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-110">However, hello Service Fabric SDK includes a cluster configuration that can run on a single machine.</span></span>

<span data-ttu-id="d8d4d-111">Je důležité, že toounderstand, který hello místní cluster Service Fabric není emulátor ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-111">It is important toounderstand that hello Service Fabric local cluster is not an emulator or simulator.</span></span> <span data-ttu-id="d8d4d-112">Ji spustí hello stejný kód platformy, která se nachází na víc počítačů clustery.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-112">It runs hello same platform code that is found on multi-machine clusters.</span></span> <span data-ttu-id="d8d4d-113">Hello jediným rozdílem je, že běží procesy platformy hello, které jsou normálně rozložené mezi pěti počítačů na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-113">hello only difference is that it runs hello platform processes that are normally spread across five machines on one machine.</span></span>

<span data-ttu-id="d8d4d-114">Hello SDK nabízí dva způsoby tooset si místní cluster: prostředí Windows PowerShell skriptu a hello Local Cluster Manager systému aplikaci na hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-114">hello SDK provides two ways tooset up a local cluster: a Windows PowerShell script and hello Local Cluster Manager system tray app.</span></span> <span data-ttu-id="d8d4d-115">V tomto kurzu používáme skript prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-115">In this tutorial, we use hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="d8d4d-116">Pokud jste už nasazením aplikace z Visual Studia vytvořili místní cluster, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-116">If you have already created a local cluster by deploying an application from Visual Studio, you can skip this section.</span></span>
> 
> 

1. <span data-ttu-id="d8d4d-117">Jako správce spusťte nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-117">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="d8d4d-118">Spusťte instalační skript clusteru hello ze složky sady SDK hello:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-118">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    <span data-ttu-id="d8d4d-119">Instalace clusteru bude chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-119">Cluster setup takes a few moments.</span></span> <span data-ttu-id="d8d4d-120">Po dokončení instalace byste měli vidět výstup podobný tomuhle:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-120">After setup is finished, you should see output similar to:</span></span>
   
    ![Výstup po instalaci clusteru][cluster-setup-success]
   
    <span data-ttu-id="d8d4d-122">Nyní je připraven tootry nasazení clusteru služby tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-122">You are now ready tootry deploying an application tooyour cluster.</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="d8d4d-123">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="d8d4d-123">Deploy an application</span></span>
<span data-ttu-id="d8d4d-124">Hello Service Fabric SDK zahrnuje celou řadu architektur a vývojářských nástrojů pro vytváření aplikací.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-124">hello Service Fabric SDK includes a rich set of frameworks and developer tooling for creating applications.</span></span> <span data-ttu-id="d8d4d-125">Pokud vás zajímá dozvědět, jak zjistit, toocreate aplikací v sadě Visual Studio, [vytvoření první aplikace Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-125">If you are interested in learning how toocreate applications in Visual Studio, see [Create your first Service Fabric application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>

<span data-ttu-id="d8d4d-126">V tomto kurzu budete používat existující ukázkovou aplikaci (s názvem WordCount) tak, aby se mohli zaměřit na aspekty správy hello hello platformy: nasazení, monitorování a upgradu.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-126">In this tutorial, you use an existing sample application (called WordCount) so that you can focus on hello management aspects of hello platform: deployment, monitoring, and upgrade.</span></span>

1. <span data-ttu-id="d8d4d-127">Jako správce spusťte nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-127">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="d8d4d-128">Importujte modul Service Fabric SDK PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-128">Import hello Service Fabric SDK PowerShell module.</span></span>
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. <span data-ttu-id="d8d4d-129">Vytvořte aplikaci hello toostore adresáře, která můžete stáhnout a nasadit, třeba C:\ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-129">Create a directory toostore hello application that you download and deploy, such as C:\ServiceFabric.</span></span>
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. <span data-ttu-id="d8d4d-130">[Stáhněte aplikaci WordCount hello](http://aka.ms/servicefabric-wordcountapp) toohello umístění, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-130">[Download hello WordCount application](http://aka.ms/servicefabric-wordcountapp) toohello location you created.</span></span>  <span data-ttu-id="d8d4d-131">Poznámka: prohlížeč Microsoft Edge hello uloží soubor hello s *.zip* rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-131">Note: hello Microsoft Edge browser saves hello file with a *.zip* extension.</span></span>  <span data-ttu-id="d8d4d-132">Změnit příponu souboru hello příliš*.sfpkg*.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-132">Change hello file extension too*.sfpkg*.</span></span>
5. <span data-ttu-id="d8d4d-133">Připojte toohello místní cluster:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-133">Connect toohello local cluster:</span></span>
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. <span data-ttu-id="d8d4d-134">Vytvoření nové aplikace pomocí příkazu hello SDK nasazení s názvem a balíček aplikace toohello cesta.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-134">Create a new application using hello SDK's deployment command with a name and a path toohello application package.</span></span>
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="d8d4d-135">Pokud všechno proběhne správně, měli byste vidět hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-135">If all goes well, you should see hello following output:</span></span>
   
    ![Nasazení místního clusteru toohello aplikace][deploy-app-to-local-cluster]
7. <span data-ttu-id="d8d4d-137">toosee hello aplikaci v akci, spusťte hello prohlížeče a přejděte příliš[http://localhost: 8081/WordCount/index.HTML](http://localhost:8081/wordcount/index.html).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-137">toosee hello application in action, launch hello browser and navigate too[http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span></span> <span data-ttu-id="d8d4d-138">Měli byste vidět tohle:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-138">You should see:</span></span>
   
    ![Uživatelské rozhraní nasazené aplikace][deployed-app-ui]
   
    <span data-ttu-id="d8d4d-140">Hello aplikace WordCount je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-140">hello WordCount application is simple.</span></span> <span data-ttu-id="d8d4d-141">Obsahuje klienta JavaScript kód toogenerate náhodných pěti znacích "slova", které jsou pak přes předávací službu toohello aplikace přes webové rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-141">It includes client-side JavaScript code toogenerate random five-character "words", which are then relayed toohello application via ASP.NET Web API.</span></span> <span data-ttu-id="d8d4d-142">Stavové služby sleduje hello počtu slov počítají.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-142">A stateful service tracks hello number of words counted.</span></span> <span data-ttu-id="d8d4d-143">Dělí do oddílů podle hello první znak hello slov.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-143">They are partitioned based on hello first character of hello word.</span></span> <span data-ttu-id="d8d4d-144">Můžete najít hello zdrojového kódu pro aplikaci WordCount hello v hello [classic Začínáme ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-144">You can find hello source code for hello WordCount app in hello [classic getting started samples](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span></span>
   
    <span data-ttu-id="d8d4d-145">Hello aplikace, kterou jsme nasadili obsahuje čtyři oddíly.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-145">hello application that we deployed contains four partitions.</span></span> <span data-ttu-id="d8d4d-146">Takže slova začínající písmeny A až G se ukládají do hello prvního oddílu, slova začínající písmeny H až N jsou uloženy v hello druhý oddíl a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-146">So words beginning with A through G are stored in hello first partition, words beginning with H through N are stored in hello second partition, and so on.</span></span>

## <a name="view-application-details-and-status"></a><span data-ttu-id="d8d4d-147">Zobrazení podrobností a stavu aplikace</span><span class="sxs-lookup"><span data-stu-id="d8d4d-147">View application details and status</span></span>
<span data-ttu-id="d8d4d-148">Teď, když jsme nasadili aplikaci hello, podíváme se na některé podrobnosti o aplikaci hello v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-148">Now that we have deployed hello application, let's look at some of hello app details in PowerShell.</span></span>

1. <span data-ttu-id="d8d4d-149">Dotaz na všechny nasazené aplikace v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-149">Query all deployed applications on hello cluster:</span></span>
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    <span data-ttu-id="d8d4d-150">Za předpokladu, že jste nasadili jenom aplikaci WordCount hello, zobrazí se o něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-150">Assuming that you have only deployed hello WordCount app, you see something similar to:</span></span>
   
    ![Dotaz na všechny nasazené aplikace v prostředí PowerShell][ps-getsfapp]
2. <span data-ttu-id="d8d4d-152">Přejděte další úroveň toohello dotazováním hello sadu služeb, které jsou součástí aplikace WordCount hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-152">Go toohello next level by querying hello set of services that are included in hello WordCount application.</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Výpis seznamu služeb aplikace hello v prostředí PowerShell][ps-getsfsvc]
   
    <span data-ttu-id="d8d4d-154">Hello aplikace se skládá z dvě služby, hello webového uživatelského rozhraní a hello stavové služby, která spravuje slova hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-154">hello application is made up of two services, hello web front end, and hello stateful service that manages hello words.</span></span>
3. <span data-ttu-id="d8d4d-155">Nakonec podívejte se na hello seznam oddílů služby WordCountService:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-155">Finally, look at hello list of partitions for WordCountService:</span></span>
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Zobrazení oddílů služby hello v prostředí PowerShell][ps-getsfpartitions]
   
    <span data-ttu-id="d8d4d-157">Dobrý den sadu příkazů, které jste použili, stejně jako všechny příkazy Service Fabric prostředí PowerShell, jsou k dispozici pro všechny clusteru, který jste se může připojit k místní nebo vzdálené.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-157">hello set of commands that you used, like all Service Fabric PowerShell commands, are available for any cluster that you might connect to, local or remote.</span></span>
   
    <span data-ttu-id="d8d4d-158">Pro víc vizuální způsob toointeract s hello clusteru, můžete použít hello webový nástroj Service Fabric Explorer tak, že přejdete příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer) v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-158">For a more visual way toointeract with hello cluster, you can use hello web-based Service Fabric Explorer tool by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer) in hello browser.</span></span>
   
    ![Zobrazení podrobností aplikace v Service Fabric Exploreru][sfx-service-overview]
   
   > [!NOTE]
   > <span data-ttu-id="d8d4d-160">toolearn Další informace o Service Fabric Explorer najdete v části [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-160">toolearn more about Service Fabric Explorer, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
   > 
   > 

## <a name="upgrade-an-application"></a><span data-ttu-id="d8d4d-161">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="d8d4d-161">Upgrade an application</span></span>
<span data-ttu-id="d8d4d-162">Service Fabric nabízí upgrady bez výpadků díky monitorování stavu hello aplikace hello během zavádění v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-162">Service Fabric provides no-downtime upgrades by monitoring hello health of hello application as it rolls out across hello cluster.</span></span> <span data-ttu-id="d8d4d-163">Proveďte upgrade aplikace WordCount hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-163">Perform an upgrade of hello WordCount application.</span></span>

<span data-ttu-id="d8d4d-164">Hello nové verze aplikace hello teď počty jenom slova, která začínají samohláskou.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-164">hello new version of hello application now counts only words that begin with a vowel.</span></span> <span data-ttu-id="d8d4d-165">Jako hello upgradu vidíme dvě změny v chování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-165">As hello upgrade rolls out, we see two changes in hello application's behavior.</span></span> <span data-ttu-id="d8d4d-166">Nejdřív by se měla zpomalit hello rychlost, jakou hello Zaprvé, protože se počítá míň slov.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-166">First, hello rate at which hello count grows should slow, since fewer words are being counted.</span></span> <span data-ttu-id="d8d4d-167">Druhý protože hello první oddíl obsahuje dvě samohlásky (A a E) a všechny ostatní oddíly jenom po jedné, jeho počet nakonec začněte toooutpace hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-167">Second, since hello first partition has two vowels (A and E) and all other partitions contain only one each, its count should eventually start toooutpace hello others.</span></span>

1. <span data-ttu-id="d8d4d-168">[Stáhněte balíček WordCount verze 2 hello](http://aka.ms/servicefabric-wordcountappv2) toohello stejné umístění, kam jste stáhli balíček verze 1 hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-168">[Download hello WordCount version 2 package](http://aka.ms/servicefabric-wordcountappv2) toohello same location where you downloaded hello version 1 package.</span></span>
2. <span data-ttu-id="d8d4d-169">Vrátí tooyour okno prostředí PowerShell a použít příkaz pro upgrade hello SDK tooregister hello novou verzi v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-169">Return tooyour PowerShell window and use hello SDK's upgrade command tooregister hello new version in hello cluster.</span></span> <span data-ttu-id="d8d4d-170">Pak začněte upgradovat hello fabric: / WordCount aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-170">Then begin upgrading hello fabric:/WordCount application.</span></span>
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    <span data-ttu-id="d8d4d-171">Měli byste vidět, že začne hello následující výstup v prostředí PowerShell jako hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-171">You should see hello following output in PowerShell as hello upgrade begins.</span></span>
   
    ![Průběh upgradu v prostředí PowerShell][ps-appupgradeprogress]
3. <span data-ttu-id="d8d4d-173">Při upgradu hello možná bude snazší toomonitor jeho stav v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-173">While hello upgrade is proceeding, you may find it easier toomonitor its status from Service Fabric Explorer.</span></span> <span data-ttu-id="d8d4d-174">Spusťte okno prohlížeče a přejděte příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-174">Launch a browser window and navigate too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="d8d4d-175">Rozbalte položku **aplikace** ve stromu hello na levé straně hello zvolte **WordCount**a v neposlední řadě **fabric: / WordCount**.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-175">Expand **Applications** in hello tree on hello left, then choose **WordCount**, and finally **fabric:/WordCount**.</span></span> <span data-ttu-id="d8d4d-176">Na kartě hello essentials je zobrazena hello stav upgradu hello pokračuje prostřednictvím domén upgradu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-176">In hello essentials tab, you see hello status of hello upgrade as it proceeds through hello cluster's upgrade domains.</span></span>
   
    ![Průběh upgradu v Service Fabric Exploreru][sfx-upgradeprogress]
   
    <span data-ttu-id="d8d4d-178">Hello upgradu v každé doméně, jsou kontroly stavu provádí tooensure, který aplikace hello nepracuje správně.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-178">As hello upgrade proceeds through each domain, health checks are performed tooensure that hello application is behaving properly.</span></span>
4. <span data-ttu-id="d8d4d-179">Pokud hello dříve dotazu hello sadu služeb v hello fabric: / WordCount aplikace, Všimněte si, že hello změnit verze WordCountService, ale verze WordCountWebService hello nebyla:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-179">If you rerun hello earlier query for hello set of services in hello fabric:/WordCount application, notice that hello WordCountService version changed but hello WordCountWebService version did not:</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Dotaz na aplikační služby po upgradu][ps-getsfsvc-postupgrade]
   
    <span data-ttu-id="d8d4d-181">Tento příklad ukazuje, jak Service Fabric spravuje upgrady aplikací.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-181">This example highlights how Service Fabric manages application upgrades.</span></span> <span data-ttu-id="d8d4d-182">Dotýká se jenom hello sada služeb (nebo balíčků kódu/konfigurace v těchto službách) změnily, což může proces hello upgrade rychlejší a spolehlivější.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-182">It touches only hello set of services (or code/configuration packages within those services) that have changed, which makes hello process of upgrading faster and more reliable.</span></span>
5. <span data-ttu-id="d8d4d-183">Nakonec se vraťte toohello prohlížeče tooobserve hello chování nové verze aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-183">Finally, return toohello browser tooobserve hello behavior of hello new application version.</span></span> <span data-ttu-id="d8d4d-184">Podle očekávání, hello počet postupuje pomaleji a hello první oddíl nakonec získá o něco větší hello svazku.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-184">As expected, hello count progresses more slowly, and hello first partition ends up with slightly more of hello volume.</span></span>
   
    ![Zobrazení hello nové verze aplikace hello v prohlížeči hello][deployed-app-ui-v2]

## <a name="cleaning-up"></a><span data-ttu-id="d8d4d-186">Čištění</span><span class="sxs-lookup"><span data-stu-id="d8d4d-186">Cleaning up</span></span>
<span data-ttu-id="d8d4d-187">Před zabalení je důležité, že jsou skutečná tooremember, který hello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-187">Before wrapping up, it's important tooremember that hello local cluster is real.</span></span> <span data-ttu-id="d8d4d-188">Aplikace pokračovat toorun hello pozadí, dokud je odebrat.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-188">Applications continue toorun in hello background until you remove them.</span></span>  <span data-ttu-id="d8d4d-189">V závislosti na povaze vašich aplikací hello spuštěné aplikaci může spotřebovávat značné množství prostředků na počítači.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-189">Depending on hello nature of your apps, a running app can take up significant resources on your machine.</span></span> <span data-ttu-id="d8d4d-190">Máte několik možností toomanage aplikace a hello clusteru:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-190">You have several options toomanage applications and hello cluster:</span></span>

1. <span data-ttu-id="d8d4d-191">tooremove jednotlivou aplikaci a všechny jeho je daty spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-191">tooremove an individual application and all it's data, run hello following command:</span></span>
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="d8d4d-192">Nebo odstranění aplikace hello z hello Service Fabric Explorer **akce** nabídky nebo hello místní nabídky v zobrazení seznamu levé aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-192">Or, delete hello application from hello Service Fabric Explorer **ACTIONS** menu or hello context menu in hello left-hand application list view.</span></span>
   
    ![Odstranění aplikace v Service Fabric Exploreru][sfe-delete-application]
2. <span data-ttu-id="d8d4d-194">Po odstranění aplikace hello z hello clusteru, zrušit registraci verze 1.0.0 a 2.0.0 hello typ aplikace WordCount.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-194">After deleting hello application from hello cluster, unregister versions 1.0.0 and 2.0.0 of hello WordCount application type.</span></span> <span data-ttu-id="d8d4d-195">Odstranění odebere hello balíčky aplikací, včetně hello kódu a konfigurace, z úložiště bitových kopií hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-195">Deletion removes hello application packages, including hello code and configuration, from hello cluster's image store.</span></span>
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    <span data-ttu-id="d8d4d-196">Nebo v Service Fabric Exploreru zvolte **Unprovision typ** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-196">Or, in Service Fabric Explorer, choose **Unprovision Type** for hello application.</span></span>
3. <span data-ttu-id="d8d4d-197">Klikněte na tlačítko tooshut dolů hello clusteru, ale data aplikací hello zachovat a trasování, **Stop Local Cluster** v aplikaci na hlavním panelu systému hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-197">tooshut down hello cluster but keep hello application data and traces, click **Stop Local Cluster** in hello system tray app.</span></span>
4. <span data-ttu-id="d8d4d-198">zcela, klikněte na tlačítko toodelete hello clusteru **odebrat místní Cluster** v aplikaci na hlavním panelu systému hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-198">toodelete hello cluster entirely, click **Remove Local Cluster** in hello system tray app.</span></span> <span data-ttu-id="d8d4d-199">Tato možnost způsobí jiný hello pomalé nasazení příštím stisknutí klávesy F5 ve Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-199">This option will result in another slow deployment hello next time you press F5 in Visual Studio.</span></span> <span data-ttu-id="d8d4d-200">Odeberte místní cluster hello pouze v případě, že nechystáte toouse, je po určitý čas nebo pokud potřebujete tooreclaim prostředky.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-200">Remove hello local cluster only if you don't intend toouse it for some time or if you need tooreclaim resources.</span></span>

## <a name="one-node-and-five-node-cluster-mode"></a><span data-ttu-id="d8d4d-201">Režim clusteru s jedním uzlem a s pěti uzly</span><span class="sxs-lookup"><span data-stu-id="d8d4d-201">One-node and five-node cluster mode</span></span>
<span data-ttu-id="d8d4d-202">Při vývoji aplikací často potřebujete rychle střídat psaní kódu, ladění, změny kódu a ladění.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-202">When developing applications, you often find yourself doing quick iterations of writing code, debugging, changing code, and debugging.</span></span> <span data-ttu-id="d8d4d-203">toohelp optimalizovat tento proces, hello místní cluster může běžet ve dvou režimech: jedním uzlem nebo pěti uzlů.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-203">toohelp optimize this process, hello local cluster can run in two modes: one-node or five-node.</span></span> <span data-ttu-id="d8d4d-204">Oba režimy mají své výhody.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-204">Both cluster modes have their benefits.</span></span> <span data-ttu-id="d8d4d-205">Režim pěti uzly clusteru umožňuje toowork s clusterem s podporou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-205">Five-node cluster mode enables you toowork with a real cluster.</span></span> <span data-ttu-id="d8d4d-206">Můžete testovat scénáře převzetí služeb při selhání, pracovat s více instancemi a replikami služeb.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-206">You can test failover scenarios, work with more instances and replicas of your services.</span></span> <span data-ttu-id="d8d4d-207">Režim jednouzlového clusteru je optimalizované toodo rychlé nasazení a registrace služby, toohelp rychle ověření kódu pomocí modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-207">One-node cluster mode is optimized toodo quick deployment and registration of services, toohelp you quickly validate code using hello Service Fabric runtime.</span></span>

<span data-ttu-id="d8d4d-208">Cluster s jedním uzlem ani cluster s pěti uzly není emulátor ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-208">Neither one-node cluster or five-node cluster modes are an emulator or simulator.</span></span> <span data-ttu-id="d8d4d-209">Hello místního vývojového clusteru spustí hello stejný kód platformy, která se nachází na víc počítačů clustery.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-209">hello local development cluster runs hello same platform code that is found on multi-machine clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="d8d4d-210">Při změně režimu clusteru hello aktuální cluster hello je odebrána z vašeho systému a vytvoření nového clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-210">When you change hello cluster mode, hello current cluster is removed from your system and a new cluster is created.</span></span> <span data-ttu-id="d8d4d-211">Hello data uložená v clusteru hello je odstraněna, když změníte režim clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-211">hello data stored in hello cluster is deleted when you change cluster mode.</span></span>
> 
> 

<span data-ttu-id="d8d4d-212">toochange hello režimu tooone uzlu clusteru, vyberte **přepínače clusteru režimu** v hello Service Fabric Local Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-212">toochange hello mode tooone-node cluster, select **Switch Cluster Mode** in hello Service Fabric Local Cluster Manager.</span></span>

![Přepnutí režimu clusteru][switch-cluster-mode]

<span data-ttu-id="d8d4d-214">Nebo změnit režim hello clusteru pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-214">Or, change hello cluster mode using PowerShell:</span></span>

1. <span data-ttu-id="d8d4d-215">Jako správce spusťte nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-215">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="d8d4d-216">Spusťte instalační skript clusteru hello ze složky sady SDK hello:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-216">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    <span data-ttu-id="d8d4d-217">Instalace clusteru bude chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-217">Cluster setup takes a few moments.</span></span> <span data-ttu-id="d8d4d-218">Po dokončení instalace byste měli vidět výstup podobný tomuhle:</span><span class="sxs-lookup"><span data-stu-id="d8d4d-218">After setup is finished, you should see output similar to:</span></span>
   
    ![Výstup po instalaci clusteru][cluster-setup-success-1-node]

## <a name="next-steps"></a><span data-ttu-id="d8d4d-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8d4d-220">Next steps</span></span>
* <span data-ttu-id="d8d4d-221">Provedli jste nasazení a upgrade některých předem sestavených aplikací a teď si můžete zkusit [sestavit vlastní aplikaci v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="d8d4d-221">Now that you have deployed and upgraded some pre-built applications, you can [try building your own in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>
* <span data-ttu-id="d8d4d-222">Všechny hello akce prováděné na místní cluster hello v tomto článku lze provést na [clusteru Azure](service-fabric-cluster-creation-via-portal.md) také.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-222">All hello actions performed on hello local cluster in this article can be performed on an [Azure cluster](service-fabric-cluster-creation-via-portal.md) as well.</span></span>
* <span data-ttu-id="d8d4d-223">Hello upgradu, které jsme provedli v tomto článku se základní.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-223">hello upgrade that we performed in this article was basic.</span></span> <span data-ttu-id="d8d4d-224">V tématu hello [dokumentaci k upgradu](service-fabric-application-upgrade.md) toolearn více informací o hello výkonu a flexibilitě upgradů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d8d4d-224">See hello [upgrade documentation](service-fabric-application-upgrade.md) toolearn more about hello power and flexibility of Service Fabric upgrades.</span></span>

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
