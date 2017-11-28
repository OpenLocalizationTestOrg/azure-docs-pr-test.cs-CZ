---
title: "Ladění aplikace v sadě Visual Studio | Microsoft Docs"
description: "Vylepšit spolehlivost a výkon vašich služeb vývoji a ladění je v sadě Visual Studio na místní vývojový cluster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 2459025899a7f5ffebf44fa104ed112c0eb99dfa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="48517-103">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48517-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48517-104">Visual Studio nebo CSharp</span><span class="sxs-lookup"><span data-stu-id="48517-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="48517-105">Eclipse nebo Java</span><span class="sxs-lookup"><span data-stu-id="48517-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="48517-106">Ladění místní aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="48517-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="48517-107">Nasazení a ladění aplikace Azure Service Fabric v místním počítači vývoj clusteru můžete ušetřit čas a peníze.</span><span class="sxs-lookup"><span data-stu-id="48517-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="48517-108">Visual Studio 2017 nebo Visual Studio 2015 můžete nasadit aplikaci k místnímu clusteru a automaticky připojit ladicí program na všechny instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-108">Visual Studio 2017 or Visual Studio 2015 can deploy the application to the local cluster and automatically connect the debugger to all instances of your application.</span></span>

1. <span data-ttu-id="48517-109">Spuštění místního vývojového clusteru podle kroků v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="48517-109">Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="48517-110">Stiskněte klávesu **F5** nebo klikněte na tlačítko **ladění** > **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="48517-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Spuštění ladění aplikace][startdebugging]
3. <span data-ttu-id="48517-112">Nastavte zarážky v kódu a krok prostřednictvím aplikace klepnutím na příkazy v **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="48517-112">Set breakpoints in your code and step through the application by clicking commands in the **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="48517-113">Visual Studio se připojí k všech instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-113">Visual Studio attaches to all instances of your application.</span></span> <span data-ttu-id="48517-114">Když jste krokování kódu, může získat zarážky více procesy, které jsou výsledkem souběžných relací.</span><span class="sxs-lookup"><span data-stu-id="48517-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="48517-115">Zkuste, zákaz zarážce poté, že máte, tím, že každé zarážky podmíněného na ID vlákna nebo pomocí diagnostických událostí.</span><span class="sxs-lookup"><span data-stu-id="48517-115">Try disabling the breakpoints after they're hit, by making each breakpoint conditional on the thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="48517-116">**Diagnostických událostí** okno se automaticky otevře, takže si můžete prohlédnout diagnostických událostí v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="48517-116">The **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Zobrazení diagnostických událostí v reálném čase][diagnosticevents]
5. <span data-ttu-id="48517-118">Můžete také otevřít **diagnostických událostí** okno v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="48517-118">You can also open the **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="48517-119">V části **Service Fabric**, klikněte pravým tlačítkem na libovolný uzel a vyberte možnost **zobrazení streamování trasování**.</span><span class="sxs-lookup"><span data-stu-id="48517-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![Otevřete okno diagnostických událostí][viewdiagnosticevents]
   
    <span data-ttu-id="48517-121">Pokud chcete filtrovat vaší trasování pro konkrétní službu nebo aplikaci, jednoduše povolte streamování trasování na této konkrétní služby nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-121">If you want to filter your traces to a specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="48517-122">Diagnostické události si můžete prohlédnout v automaticky generované **ServiceEventSource.cs** souborů a jsou volat z kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-122">The diagnostic events can be seen in the automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="48517-123">**Diagnostických událostí** okno podporuje filtrování, pozastavení a kontrole událostí v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="48517-123">The **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="48517-124">Filtr je hledání jednoduchého řetězce událostí zprávy, včetně jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="48517-124">The filter is a simple string search of the event message, including its contents.</span></span>
   
    ![Filtrovat, pozastavení a obnovení nebo zkontrolujte události v reálném čase][diagnosticeventsactions]
8. <span data-ttu-id="48517-126">Ladění služeb je jako ladění žádnou jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48517-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="48517-127">Bude obvykle nastavit zarážky pomocí sady Visual Studio pro snadné ladění.</span><span class="sxs-lookup"><span data-stu-id="48517-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="48517-128">I když spolehlivé kolekce replikují mezi několika uzly, se stále implementovat rozhraní IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="48517-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="48517-129">To znamená, že můžete zobrazit výsledky v sadě Visual Studio při ladění najdete v článku jste uložený v.</span><span class="sxs-lookup"><span data-stu-id="48517-129">This means that you can use the Results View in Visual Studio while debugging to see what you've stored inside.</span></span> <span data-ttu-id="48517-130">Jednoduše zarážku kdekoli v kódu.</span><span class="sxs-lookup"><span data-stu-id="48517-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Spuštění ladění aplikace][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="48517-132">Ladění vzdáleného aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="48517-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="48517-133">Pokud vaše aplikace Service Fabric běží na clusteru Service Fabric v Azure, budete moci vzdáleně ladit tyto přímo ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48517-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able to remotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="48517-134">Funkce vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="48517-134">The feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="48517-135">Vzdálené ladění je určená pro scénáře vývoje/testování a není určen k použití v produkčním prostředí, protože dopad na spuštěné aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-135">Remote debugging is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> 
> 

1. <span data-ttu-id="48517-136">Přejít na cluster v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit ladění**</span><span class="sxs-lookup"><span data-stu-id="48517-136">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Povolit vzdálené ladění][enableremotedebugging]
   
    <span data-ttu-id="48517-138">To bude ji proces povolení vzdáleného ladění rozšíření na vaše uzly clusteru, jakož i Konfigurace požadované sítě.</span><span class="sxs-lookup"><span data-stu-id="48517-138">This will kick off the process of enabling the remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="48517-139">Klikněte pravým tlačítkem na uzel clusteru v **Průzkumník cloudu**a zvolte **připojit ladicí program**</span><span class="sxs-lookup"><span data-stu-id="48517-139">Right-click the cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Připojit ladicí program][attachdebugger]
3. <span data-ttu-id="48517-141">V **připojit k procesu** dialogové okno, zvolte proces chcete ladit a klikněte na tlačítko **připojit**</span><span class="sxs-lookup"><span data-stu-id="48517-141">In the **Attach to process** dialog, choose the process you want to debug, and click **Attach**</span></span>
   
    ![Vyberte proces][chooseprocess]
   
    <span data-ttu-id="48517-143">Název procesu, který chcete připojit, se rovná názvu název sestavení projektu služby.</span><span class="sxs-lookup"><span data-stu-id="48517-143">The name of the process you want to attach to, equals the name of your service project assembly name.</span></span>
   
    <span data-ttu-id="48517-144">Připojí ladicí program se na všech uzlech spuštěním procesu.</span><span class="sxs-lookup"><span data-stu-id="48517-144">The debugger will attach to all nodes running the process.</span></span>
   
   * <span data-ttu-id="48517-145">Všechny instance služby na všech uzlech v případě, kde jsou ladění bezstavové služby, jsou součástí relace ladění.</span><span class="sxs-lookup"><span data-stu-id="48517-145">In the case where you are debugging a stateless service, all instances of the service on all nodes are part of the debug session.</span></span>
   * <span data-ttu-id="48517-146">Pokud ladíte stavové služby, bude pouze primární replika libovolného oddílu aktivní a proto zachycení pomocí ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="48517-146">If you are debugging a stateful service, only the primary replica of any partition will be active and therefore caught by the debugger.</span></span> <span data-ttu-id="48517-147">Pokud se během ladicí relace přesune primární replikou, zpracování této repliky bude stále součástí relace ladění.</span><span class="sxs-lookup"><span data-stu-id="48517-147">If the primary replica moves during the debug session, the processing of that replica will still be part of the debug session.</span></span>
   * <span data-ttu-id="48517-148">Chcete-li pouze zachytit příslušné oddíly nebo instance dané služby, můžete v podmíněné zarážky pouze rozdělit na konkrétní oddíl nebo instance.</span><span class="sxs-lookup"><span data-stu-id="48517-148">In order to only catch relevant partitions or instances of a given service, you can use conditional breakpoints to only break a specific partition or instance.</span></span>
     
     ![Podmíněné zarážky][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="48517-150">Ladění clusteru Service Fabric s více instancemi se stejným názvem spustitelného souboru služby aktuálně nepodporujeme.</span><span class="sxs-lookup"><span data-stu-id="48517-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of the same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="48517-151">Jakmile dokončíte ladění aplikace, můžete zakázat vzdáleného ladění rozšíření kliknutím pravým tlačítkem myši na cluster ve **Průzkumník cloudu** a zvolte **zakázat ladění**</span><span class="sxs-lookup"><span data-stu-id="48517-151">Once you finish debugging your application, you can disable the remote debugging extension by right-clicking the cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Zakázání vzdálené ladění][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="48517-153">Streamování trasování z uzlu vzdáleného clusteru</span><span class="sxs-lookup"><span data-stu-id="48517-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="48517-154">Je také možné do datového proudu trasování přímo z uzlu vzdáleného clusteru k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48517-154">You are also able to stream traces directly from a remote cluster node to Visual Studio.</span></span> <span data-ttu-id="48517-155">Tato funkce umožňuje události trasování ETW datového proudu, vytváří na uzlu clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="48517-155">This feature allows you to stream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="48517-156">Tato funkce vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="48517-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="48517-157">Tato funkce podporuje pouze clustery spuštěná v Azure.</span><span class="sxs-lookup"><span data-stu-id="48517-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="48517-158">Streamování trasování je určená pro scénáře vývoje/testování a není určen k použití v produkčním prostředí, protože dopad na spuštěné aplikace.</span><span class="sxs-lookup"><span data-stu-id="48517-158">Streaming traces is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> <span data-ttu-id="48517-159">V produkční scénář měli byste tedy spoléhat na předávání událostí pomocí diagnostiky Azure.</span><span class="sxs-lookup"><span data-stu-id="48517-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="48517-160">Přejít na cluster v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit trasování streamování**</span><span class="sxs-lookup"><span data-stu-id="48517-160">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Povolit vzdálené streamování trasování][enablestreamingtraces]
   
    <span data-ttu-id="48517-162">To bude ji proces povolení streamování rozšíření trasování na vaše uzly clusteru, jakož i Konfigurace požadované sítě.</span><span class="sxs-lookup"><span data-stu-id="48517-162">This will kick off the process of enabling the streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="48517-163">Rozbalte **uzly** element v **Průzkumník cloudu**, klikněte pravým tlačítkem na uzel chcete trasování z datového proudu a zvolte **zobrazení streamování trasování**</span><span class="sxs-lookup"><span data-stu-id="48517-163">Expand the **Nodes** element in **Cloud Explorer**, right-click the node you want to stream traces from and choose **View Streaming Traces**</span></span>
   
    ![Zobrazení vzdáleného streamování trasování][viewremotestreamingtraces]
   
    <span data-ttu-id="48517-165">Opakujte krok 2 pro libovolný počet uzlů, jak chcete zobrazit trasování z.</span><span class="sxs-lookup"><span data-stu-id="48517-165">Repeat step 2 for as many nodes as you want to see traces from.</span></span> <span data-ttu-id="48517-166">Každý datový proud uzly se zobrazí v okně vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="48517-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="48517-167">Nyní jste moci zobrazit trasování vysílaných Service Fabric a služeb.</span><span class="sxs-lookup"><span data-stu-id="48517-167">You are now able to see the traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="48517-168">Pokud chcete filtrovat události, které chcete zobrazit pouze na konkrétní aplikaci, jednoduše zadejte název aplikace ve filtru.</span><span class="sxs-lookup"><span data-stu-id="48517-168">If you want to filter the events to only show a specific application, simply type in the name of the application in the filter.</span></span>
   
    ![Zobrazení, streamování trasování][viewingstreamingtraces]
3. <span data-ttu-id="48517-170">Jakmile dokončíte streamování trasování z clusteru, můžete zakázat vzdálené streamování trasování, kliknutím pravým tlačítkem myši na cluster ve **Průzkumník cloudu** a zvolte **zakázat streamování trasování**</span><span class="sxs-lookup"><span data-stu-id="48517-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking the cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Zakázání vzdálené streamování trasování][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="48517-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48517-172">Next steps</span></span>
* <span data-ttu-id="48517-173">[Testování služby Service Fabric](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="48517-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="48517-174">[Spravujte svoje aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="48517-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
