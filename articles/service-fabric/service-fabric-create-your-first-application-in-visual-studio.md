---
title: "aaaCreate spolehlivé služby Azure Service Fabric pomocí C#"
description: "Vytvoření, nasazení a ladění aplikace spolehlivé služby postavené na Azure Service Fabric pomocí sady Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="da124-103">Vytvoření první aplikace spolehlivé stavové služby Service Fabric v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="da124-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="da124-104">Zjistěte, jak toodeploy vaší první aplikace Service Fabric pro .NET v systému Windows za několik minut.</span><span class="sxs-lookup"><span data-stu-id="da124-104">Learn how toodeploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="da124-105">Jakmile budete hotovi, budete mít funkční místní cluster s aplikací spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="da124-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da124-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="da124-106">Prerequisites</span></span>

<span data-ttu-id="da124-107">Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="da124-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="da124-108">To zahrnuje, instalací hello Service Fabric SDK a Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="da124-108">This includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="da124-109">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="da124-109">Create hello application</span></span>

<span data-ttu-id="da124-110">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="da124-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="da124-111">Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.</span><span class="sxs-lookup"><span data-stu-id="da124-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="da124-112">V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="da124-112">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="da124-113">Název aplikace hello **Moje_aplikace** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="da124-113">Name hello application **MyApplication** and press **OK**.</span></span>

   
![Dialogové okno Nový projekt ve Visual Studiu][1]

<span data-ttu-id="da124-115">Můžete vytvořit jakýkoli typ aplikace Service Fabric z dialogového okna Další hello.</span><span class="sxs-lookup"><span data-stu-id="da124-115">You can create any type of Service Fabric application from hello next dialog.</span></span> <span data-ttu-id="da124-116">Pro účely tohoto Rychlého startu zvolte **Stavová služba**.</span><span class="sxs-lookup"><span data-stu-id="da124-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="da124-117">Název služby hello **MyStatefulService** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="da124-117">Name hello service **MyStatefulService** and press **OK**.</span></span>

![Dialogové okno Nová služba ve Visual Studiu][2]


<span data-ttu-id="da124-119">Visual Studio vytvoří projekt aplikace hello a projekt stavové služby hello a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="da124-119">Visual Studio creates hello application project and hello stateful service project and displays them in Solution Explorer.</span></span>

![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

<span data-ttu-id="da124-121">projekt aplikace Hello (**Moje_aplikace**) přímo neobsahuje žádný kód.</span><span class="sxs-lookup"><span data-stu-id="da124-121">hello application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="da124-122">Odkazuje ale na sadu projektů služeb.</span><span class="sxs-lookup"><span data-stu-id="da124-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="da124-123">Kromě toho obsahuje další tři typy obsahu:</span><span class="sxs-lookup"><span data-stu-id="da124-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="da124-124">**Profily publikování**</span><span class="sxs-lookup"><span data-stu-id="da124-124">**Publish profiles**</span></span>  
<span data-ttu-id="da124-125">Profily pro nasazení toodifferent prostředí.</span><span class="sxs-lookup"><span data-stu-id="da124-125">Profiles for deploying toodifferent environments.</span></span>

* <span data-ttu-id="da124-126">**Skripty**</span><span class="sxs-lookup"><span data-stu-id="da124-126">**Scripts**</span></span>  
<span data-ttu-id="da124-127">Skript PowerShellu pro nasazení/upgrade aplikace.</span><span class="sxs-lookup"><span data-stu-id="da124-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="da124-128">**Definice aplikace**</span><span class="sxs-lookup"><span data-stu-id="da124-128">**Application definition**</span></span>  
<span data-ttu-id="da124-129">Zahrnuje hello ApplicationManifest.xml souboru pod *ApplicationPackageRoot* který popisuje složení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="da124-129">Includes hello ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="da124-130">Soubory parametrů přidružené aplikace jsou v části *ApplicationParameters*, které lze použít toospecify parametry konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="da124-130">Associated application parameter files are under *ApplicationParameters*, which can be used toospecify environment-specific parameters.</span></span> <span data-ttu-id="da124-131">Visual Studio vybere parametr souboru aplikace, který je uveden v hello přidružené profil publikování se během nasazení tooa konkrétní prostředí.</span><span class="sxs-lookup"><span data-stu-id="da124-131">Visual Studio selects an application parameter file that's specified in hello associated publish profile during deployment tooa specific environment.</span></span>
    
<span data-ttu-id="da124-132">Přehled hello obsah hello projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="da124-132">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-hello-application"></a><span data-ttu-id="da124-133">Nasazení a ladění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="da124-133">Deploy and debug hello application</span></span>

<span data-ttu-id="da124-134">Teď, když máte aplikaci, ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="da124-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="da124-135">V sadě Visual Studio, stiskněte klávesu `F5` aplikace hello toodeploy pro ladění.</span><span class="sxs-lookup"><span data-stu-id="da124-135">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="da124-136">Hello poprvé spustíte a nasadit hello aplikaci místně, Visual Studio vytvoří místní cluster pro ladění.</span><span class="sxs-lookup"><span data-stu-id="da124-136">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="da124-137">To může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="da124-137">This may take some time.</span></span> <span data-ttu-id="da124-138">Stav vytváření clusteru Hello se zobrazí v okně výstupu sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="da124-138">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="da124-139">Když hello clusteru je připraven, zobrazí oznámení z hello místní cluster systému panelu Správce aplikace součástí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="da124-139">When hello cluster is ready, you get a notification from hello local cluster system tray manager application included with hello SDK.</span></span>
   
![Upozornění na hlavním panelu systému místního clusteru][4]

<span data-ttu-id="da124-141">Jednou hello aplikace spustí, Visual Studio automaticky vyvolá hello **prohlížeč diagnostických událostí**, kde uvidíte výstup trasování z vašich služeb.</span><span class="sxs-lookup"><span data-stu-id="da124-141">Once hello application starts, Visual Studio automatically brings up hello **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Prohlížeč diagnostických událostí][5]

<span data-ttu-id="da124-143">Hello šablony stavové služby jsme použili jednoduše zobrazuje zvyšování hodnoty čítačů v hello `RunAsync` metodu **souboru MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="da124-143">hello stateful service template we used simply shows a counter value incrementing in hello `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="da124-144">Rozbalte jednu hello události toosee další podrobnosti, včetně hello uzlu, kde je spuštěn hello kód.</span><span class="sxs-lookup"><span data-stu-id="da124-144">Expand one of hello events toosee more details, including hello node where hello code is running.</span></span> <span data-ttu-id="da124-145">V tomto případě je to uzel \_Node\_2, to se ale může na vašem počítači lišit.</span><span class="sxs-lookup"><span data-stu-id="da124-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![Podrobnosti v prohlížeči diagnostických událostí][6]

<span data-ttu-id="da124-147">Hello místní cluster obsahuje pět uzlů hostovaných na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="da124-147">hello local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="da124-148">V produkčním prostředí je každý uzel hostovaný na odlišném fyzickém nebo virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="da124-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="da124-149">toosimulate hello ztrátě počítače při výkonu hello Visual Studio ladicího programu na hello stejný čas, Zkusme teď jeden z uzlů hello na místní cluster hello.</span><span class="sxs-lookup"><span data-stu-id="da124-149">toosimulate hello loss of a machine while exercising hello Visual Studio debugger at hello same time, let's take down one of hello nodes on hello local cluster.</span></span>

<span data-ttu-id="da124-150">V hello **Průzkumníku řešení** okně Otevřít **souboru MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="da124-150">In hello **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="da124-151">Najde hello `RunAsync` metoda a sadu zarážku na první řádek metody hello hello.</span><span class="sxs-lookup"><span data-stu-id="da124-151">Find hello `RunAsync` method and set a breakpoint on hello first line of hello method.</span></span>

![Zarážka v metodě RunAsync stavové služby][7]

<span data-ttu-id="da124-153">Spusťte hello **Service Fabric Explorer** nástroj kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a zvolte **spravovat místní Cluster**.</span><span class="sxs-lookup"><span data-stu-id="da124-153">Launch hello **Service Fabric Explorer** tool by right-clicking on hello **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![Spusťte Service Fabric Explorer z hello Local Cluster Manager][systray-launch-sfx]

<span data-ttu-id="da124-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) nabízí vizuální znázornění clusteru.</span><span class="sxs-lookup"><span data-stu-id="da124-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="da124-156">Obsahuje sadu hello aplikace nasazené tooit a hello sady fyzických uzlů, které ho tvoří.</span><span class="sxs-lookup"><span data-stu-id="da124-156">It includes hello set of applications deployed tooit and hello set of physical nodes that make it up.</span></span>

<span data-ttu-id="da124-157">V levém podokně hello rozbalte **Cluster > uzly** a najít hello uzlu, kde běží váš kód.</span><span class="sxs-lookup"><span data-stu-id="da124-157">In hello left pane, expand **Cluster > Nodes** and find hello node where your code is running.</span></span>

<span data-ttu-id="da124-158">Klikněte na tlačítko **akce > Deaktivovat (restartovat)** toosimulate restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="da124-158">Click **Actions > Deactivate (Restart)** toosimulate a machine restarting.</span></span>

![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

<span data-ttu-id="da124-160">Na okamžik, měli byste vidět vaší zarážce dosáhl v sadě Visual Studio jako výpočetní hello jste dělali v jednom uzlu plynule převezme tooanother.</span><span class="sxs-lookup"><span data-stu-id="da124-160">Momentarily, you should see your breakpoint hit in Visual Studio as hello computation you were doing on one node seamlessly fails over tooanother.</span></span>


<span data-ttu-id="da124-161">V dalším kroku vrátit toohello prohlížeče diagnostických událostí a sledovat hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="da124-161">Next, return toohello Diagnostic Events Viewer and observe hello messages.</span></span> <span data-ttu-id="da124-162">Hello čítače pořád roste, i když hello události ve skutečnosti přicházejí z jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="da124-162">hello counter has continued incrementing, even though hello events are actually coming from a different node.</span></span>

![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a><span data-ttu-id="da124-164">Čištění hello místní cluster (volitelné)</span><span class="sxs-lookup"><span data-stu-id="da124-164">Cleaning up hello local cluster (optional)</span></span>

<span data-ttu-id="da124-165">Pamatujte, že tento místní cluster je skutečný.</span><span class="sxs-lookup"><span data-stu-id="da124-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="da124-166">Zastavení ladicího programu hello odebere vaší instanci aplikace a zrušení registrace typu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="da124-166">Stopping hello debugger removes your application instance and unregisters hello application type.</span></span> <span data-ttu-id="da124-167">Hello cluster však stále toorun hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="da124-167">However, hello cluster continues toorun in hello background.</span></span> <span data-ttu-id="da124-168">Jakmile budete místní cluster připravený toostop hello, existuje několik možností.</span><span class="sxs-lookup"><span data-stu-id="da124-168">When you're ready toostop hello local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="da124-169">Zachování dat aplikace a trasování</span><span class="sxs-lookup"><span data-stu-id="da124-169">Keep application and trace data</span></span>

<span data-ttu-id="da124-170">Vypnout hello clusteru kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a potom zvolte **Stop Local Cluster**.</span><span class="sxs-lookup"><span data-stu-id="da124-170">Shut down hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-hello-cluster-and-all-data"></a><span data-ttu-id="da124-171">Odstranění clusteru hello a všechna data</span><span class="sxs-lookup"><span data-stu-id="da124-171">Delete hello cluster and all data</span></span>

<span data-ttu-id="da124-172">Odebrat hello cluster kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a potom zvolte **odebrat místní Cluster**.</span><span class="sxs-lookup"><span data-stu-id="da124-172">Remove hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="da124-173">Pokud zvolíte tuto možnost, Visual Studio bude znovu nasaďte hello clusteru hello příštím vaše práce hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="da124-173">If you choose this option, Visual Studio will redeploy hello cluster hello next time your run hello application.</span></span> <span data-ttu-id="da124-174">Tuto možnost zvolte, pokud toouse hello místní cluster nechystáte nějakou dobu, nebo pokud potřebujete tooreclaim prostředky.</span><span class="sxs-lookup"><span data-stu-id="da124-174">Choose this option if you don't intend toouse hello local cluster for some time or if you need tooreclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da124-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da124-175">Next steps</span></span>
<span data-ttu-id="da124-176">Další informace o [spolehlivých službách](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="da124-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
