---
title: "Vytvoření spolehlivé služby Azure Service Fabric pomocí jazyka C#"
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
ms.openlocfilehash: f93298e6483fd8c9dfda835964aeebd1a430af69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="a414c-103">Vytvoření první aplikace spolehlivé stavové služby Service Fabric v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="a414c-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="a414c-104">Zjistěte, jak nasadit první aplikaci Service Fabric pro .NET v systému Windows během několika minut.</span><span class="sxs-lookup"><span data-stu-id="a414c-104">Learn how to deploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="a414c-105">Jakmile budete hotovi, budete mít funkční místní cluster s aplikací spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="a414c-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a414c-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a414c-106">Prerequisites</span></span>

<span data-ttu-id="a414c-107">Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a414c-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="a414c-108">To zahrnuje instalaci sady Service Fabric SDK a sady Visual Studio 2017 nebo 2015.</span><span class="sxs-lookup"><span data-stu-id="a414c-108">This includes installing the Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="a414c-109">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="a414c-109">Create the application</span></span>

<span data-ttu-id="a414c-110">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="a414c-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="a414c-111">Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.</span><span class="sxs-lookup"><span data-stu-id="a414c-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="a414c-112">V dialogovém okně **Nový projekt** zvolte **Cloud > Aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="a414c-112">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="a414c-113">Pojmenujte aplikaci **MyApplication** a stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a414c-113">Name the application **MyApplication** and press **OK**.</span></span>

   
![Dialogové okno Nový projekt ve Visual Studiu][1]

<span data-ttu-id="a414c-115">V dalším dialogovém okně můžete vytvořit jakýkoli typ aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a414c-115">You can create any type of Service Fabric application from the next dialog.</span></span> <span data-ttu-id="a414c-116">Pro účely tohoto Rychlého startu zvolte **Stavová služba**.</span><span class="sxs-lookup"><span data-stu-id="a414c-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="a414c-117">Pojmenujte službu **MyStatefulService** a stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a414c-117">Name the service **MyStatefulService** and press **OK**.</span></span>

![Dialogové okno Nová služba ve Visual Studiu][2]


<span data-ttu-id="a414c-119">Visual Studio vytvoří projekt aplikace a projekt stavové služby a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="a414c-119">Visual Studio creates the application project and the stateful service project and displays them in Solution Explorer.</span></span>

![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

<span data-ttu-id="a414c-121">Projekt aplikace (**MyApplication**) jako takový neobsahuje žádný kód.</span><span class="sxs-lookup"><span data-stu-id="a414c-121">The application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="a414c-122">Odkazuje ale na sadu projektů služeb.</span><span class="sxs-lookup"><span data-stu-id="a414c-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="a414c-123">Kromě toho obsahuje další tři typy obsahu:</span><span class="sxs-lookup"><span data-stu-id="a414c-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="a414c-124">**Profily publikování**</span><span class="sxs-lookup"><span data-stu-id="a414c-124">**Publish profiles**</span></span>  
<span data-ttu-id="a414c-125">Profily pro nasazování do různých prostředí.</span><span class="sxs-lookup"><span data-stu-id="a414c-125">Profiles for deploying to different environments.</span></span>

* <span data-ttu-id="a414c-126">**Skripty**</span><span class="sxs-lookup"><span data-stu-id="a414c-126">**Scripts**</span></span>  
<span data-ttu-id="a414c-127">Skript PowerShellu pro nasazení/upgrade aplikace.</span><span class="sxs-lookup"><span data-stu-id="a414c-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="a414c-128">**Definice aplikace**</span><span class="sxs-lookup"><span data-stu-id="a414c-128">**Application definition**</span></span>  
<span data-ttu-id="a414c-129">Zahrnuje soubor ApplicationManifest.xml ve složce *ApplicationPackageRoot*, který popisuje složení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a414c-129">Includes the ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="a414c-130">Ve složce *ApplicationParameters* se nachází související soubory parametrů aplikace, které můžete použít k zadání parametrů specifických pro prostředí.</span><span class="sxs-lookup"><span data-stu-id="a414c-130">Associated application parameter files are under *ApplicationParameters*, which can be used to specify environment-specific parameters.</span></span> <span data-ttu-id="a414c-131">Sada Visual Studio během nasazení do konkrétního prostředí vybere soubor parametrů aplikace, který je zadaný v přidruženém profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="a414c-131">Visual Studio selects an application parameter file that's specified in the associated publish profile during deployment to a specific environment.</span></span>
    
<span data-ttu-id="a414c-132">Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="a414c-132">For an overview of the contents of the service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-the-application"></a><span data-ttu-id="a414c-133">Nasazení a ladění aplikace</span><span class="sxs-lookup"><span data-stu-id="a414c-133">Deploy and debug the application</span></span>

<span data-ttu-id="a414c-134">Teď, když máte aplikaci, ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="a414c-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="a414c-135">Stisknutím klávesy `F5` v sadě Visual Studio aplikaci nasaďte pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="a414c-135">In Visual Studio, press `F5` to deploy the application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="a414c-136">Při prvním místním spuštění a nasazení aplikace sada Visual Studio vytvoří místní cluster pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="a414c-136">The first time you run and deploy the application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="a414c-137">To může nějakou dobu trvat.</span><span class="sxs-lookup"><span data-stu-id="a414c-137">This may take some time.</span></span> <span data-ttu-id="a414c-138">Stav vytváření clusteru se zobrazí v okně výstupu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a414c-138">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="a414c-139">Až bude cluster připravený, aplikace pro správu na hlavním panelu systému místního clusteru, která je součástí sady SDK, zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="a414c-139">When the cluster is ready, you get a notification from the local cluster system tray manager application included with the SDK.</span></span>
   
![Upozornění na hlavním panelu systému místního clusteru][4]

<span data-ttu-id="a414c-141">Až se aplikace spustí, Visual Studio automaticky zobrazí **Prohlížeč diagnostických událostí**, ve kterém uvidíte výstup trasování ze služeb.</span><span class="sxs-lookup"><span data-stu-id="a414c-141">Once the application starts, Visual Studio automatically brings up the **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Prohlížeč diagnostických událostí][5]

<span data-ttu-id="a414c-143">Šablona stavové služby, kterou jsme použili, uvádí jenom hodnotu čítače, která se zvyšuje v metodě `RunAsync` v souboru **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="a414c-143">The stateful service template we used simply shows a counter value incrementing in the `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="a414c-144">Pokud některou událost rozbalíte, zobrazí se další podrobnosti, včetně uzlu, na kterém kód běží.</span><span class="sxs-lookup"><span data-stu-id="a414c-144">Expand one of the events to see more details, including the node where the code is running.</span></span> <span data-ttu-id="a414c-145">V tomto případě je to uzel \_Node\_2, to se ale může na vašem počítači lišit.</span><span class="sxs-lookup"><span data-stu-id="a414c-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![Podrobnosti v prohlížeči diagnostických událostí][6]

<span data-ttu-id="a414c-147">Místní cluster obsahuje pět uzlů hostovaných na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="a414c-147">The local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="a414c-148">V produkčním prostředí je každý uzel hostovaný na odlišném fyzickém nebo virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a414c-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="a414c-149">Abychom napodobili situaci, kdy počítač přestane pracovat, zatímco je na něm spuštěn ladící program sady Visual Studio, zkusme teď jeden z uzlů místního clusteru zastavit.</span><span class="sxs-lookup"><span data-stu-id="a414c-149">To simulate the loss of a machine while exercising the Visual Studio debugger at the same time, let's take down one of the nodes on the local cluster.</span></span>

<span data-ttu-id="a414c-150">V okně **Průzkumník řešení** otevřete soubor **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="a414c-150">In the **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="a414c-151">Vyhledejte metodu `RunAsync` a na prvním řádku metody nastavte zarážku.</span><span class="sxs-lookup"><span data-stu-id="a414c-151">Find the `RunAsync` method and set a breakpoint on the first line of the method.</span></span>

![Zarážka v metodě RunAsync stavové služby][7]

<span data-ttu-id="a414c-153">Spusťte nástroj **Service Fabric Explorer** kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Manage Local Cluster** (Správa místního clusteru).</span><span class="sxs-lookup"><span data-stu-id="a414c-153">Launch the **Service Fabric Explorer** tool by right-clicking on the **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![Spuštění nástroje Service Fabric Explorer z aplikace Local Cluster Manager][systray-launch-sfx]

<span data-ttu-id="a414c-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) nabízí vizuální znázornění clusteru.</span><span class="sxs-lookup"><span data-stu-id="a414c-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="a414c-156">To zahrnujte sadu aplikací, které jsou v něm nasazené, a sadu fyzických uzlů, které ho tvoří.</span><span class="sxs-lookup"><span data-stu-id="a414c-156">It includes the set of applications deployed to it and the set of physical nodes that make it up.</span></span>

<span data-ttu-id="a414c-157">V levém podokně rozbalte položky **Cluster > Uzly** a najděte uzel, na kterém běží váš kód.</span><span class="sxs-lookup"><span data-stu-id="a414c-157">In the left pane, expand **Cluster > Nodes** and find the node where your code is running.</span></span>

<span data-ttu-id="a414c-158">Kliknutím na **Akce > Deaktivovat (restartovat)** simulujte restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="a414c-158">Click **Actions > Deactivate (Restart)** to simulate a machine restarting.</span></span>

![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

<span data-ttu-id="a414c-160">Na okamžik byste měli zahlédnout, jak Visual Studio dojde k vaší zarážce a výpočet probíhající na jednom uzlu plynule převezme jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="a414c-160">Momentarily, you should see your breakpoint hit in Visual Studio as the computation you were doing on one node seamlessly fails over to another.</span></span>


<span data-ttu-id="a414c-161">Dále se vraťte do prohlížeče diagnostických událostí a podívejte se na zprávy.</span><span class="sxs-lookup"><span data-stu-id="a414c-161">Next, return to the Diagnostic Events Viewer and observe the messages.</span></span> <span data-ttu-id="a414c-162">Hodnota čítače pořád roste, i když události ve skutečnosti přicházejí z jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="a414c-162">The counter has continued incrementing, even though the events are actually coming from a different node.</span></span>

![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a><span data-ttu-id="a414c-164">Vyčištění místního clusteru (volitelné)</span><span class="sxs-lookup"><span data-stu-id="a414c-164">Cleaning up the local cluster (optional)</span></span>

<span data-ttu-id="a414c-165">Pamatujte, že tento místní cluster je skutečný.</span><span class="sxs-lookup"><span data-stu-id="a414c-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="a414c-166">Při zastavení ladicího programu se odebere instance aplikace a zruší se registrace typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a414c-166">Stopping the debugger removes your application instance and unregisters the application type.</span></span> <span data-ttu-id="a414c-167">Cluster ale dál běží na pozadí.</span><span class="sxs-lookup"><span data-stu-id="a414c-167">However, the cluster continues to run in the background.</span></span> <span data-ttu-id="a414c-168">Až budete připraveni zastavit místní cluster, máte několik možností.</span><span class="sxs-lookup"><span data-stu-id="a414c-168">When you're ready to stop the local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="a414c-169">Zachování dat aplikace a trasování</span><span class="sxs-lookup"><span data-stu-id="a414c-169">Keep application and trace data</span></span>

<span data-ttu-id="a414c-170">Vypněte cluster kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Stop Local Cluster** (Zastavit místní cluster).</span><span class="sxs-lookup"><span data-stu-id="a414c-170">Shut down the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-the-cluster-and-all-data"></a><span data-ttu-id="a414c-171">Odstranění clusteru a veškerých dat</span><span class="sxs-lookup"><span data-stu-id="a414c-171">Delete the cluster and all data</span></span>

<span data-ttu-id="a414c-172">Odeberte cluster kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Remove Local Cluster** (Odebrat místní cluster).</span><span class="sxs-lookup"><span data-stu-id="a414c-172">Remove the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="a414c-173">Pokud zvolíte tuto možnost, sada Visual Studio cluster znovu nasadí při příštím spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a414c-173">If you choose this option, Visual Studio will redeploy the cluster the next time your run the application.</span></span> <span data-ttu-id="a414c-174">Zvolte tuto možnost v případě, že se místní cluster nechystáte nějakou dobu používat nebo potřebujete uvolnit prostředky.</span><span class="sxs-lookup"><span data-stu-id="a414c-174">Choose this option if you don't intend to use the local cluster for some time or if you need to reclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a414c-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a414c-175">Next steps</span></span>
<span data-ttu-id="a414c-176">Další informace o [spolehlivých službách](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a414c-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
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
