---
title: "aaaDebug aplikace v sadě Visual Studio | Microsoft Docs"
description: "Vylepšit hello spolehlivost a výkon vašich služeb vývoji a ladění je v sadě Visual Studio na místní vývojový cluster."
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
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="53eed-103">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53eed-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="53eed-104">Visual Studio nebo CSharp</span><span class="sxs-lookup"><span data-stu-id="53eed-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="53eed-105">Eclipse nebo Java</span><span class="sxs-lookup"><span data-stu-id="53eed-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="53eed-106">Ladění místní aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="53eed-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="53eed-107">Nasazení a ladění aplikace Azure Service Fabric v místním počítači vývoj clusteru můžete ušetřit čas a peníze.</span><span class="sxs-lookup"><span data-stu-id="53eed-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="53eed-108">Visual Studio 2017 nebo Visual Studio 2015 můžete nasadit hello aplikace toohello místní cluster a automaticky se připojí hello ladicí program tooall instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="53eed-108">Visual Studio 2017 or Visual Studio 2015 can deploy hello application toohello local cluster and automatically connect hello debugger tooall instances of your application.</span></span>

1. <span data-ttu-id="53eed-109">Spuštění místního vývojového clusteru podle následujících kroků hello v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="53eed-109">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="53eed-110">Stiskněte klávesu **F5** nebo klikněte na tlačítko **ladění** > **spustit ladění**.</span><span class="sxs-lookup"><span data-stu-id="53eed-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Spuštění ladění aplikace][startdebugging]
3. <span data-ttu-id="53eed-112">Nastavte zarážky v kódu a krok prostřednictvím aplikace hello klepnutím na příkazy v hello **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="53eed-112">Set breakpoints in your code and step through hello application by clicking commands in hello **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="53eed-113">Visual Studio připojí tooall instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="53eed-113">Visual Studio attaches tooall instances of your application.</span></span> <span data-ttu-id="53eed-114">Když jste krokování kódu, může získat zarážky více procesy, které jsou výsledkem souběžných relací.</span><span class="sxs-lookup"><span data-stu-id="53eed-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="53eed-115">Zkuste, zákaz hello zarážky poté, že máte, tím, že každé zarážky podmíněného na ID vlákna hello nebo pomocí diagnostických událostí.</span><span class="sxs-lookup"><span data-stu-id="53eed-115">Try disabling hello breakpoints after they're hit, by making each breakpoint conditional on hello thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="53eed-116">Hello **diagnostických událostí** okno se automaticky otevře, takže si můžete prohlédnout diagnostických událostí v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="53eed-116">hello **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Zobrazení diagnostických událostí v reálném čase][diagnosticevents]
5. <span data-ttu-id="53eed-118">Můžete také otevřít hello **diagnostických událostí** okno v Průzkumníku cloudu.</span><span class="sxs-lookup"><span data-stu-id="53eed-118">You can also open hello **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="53eed-119">V části **Service Fabric**, klikněte pravým tlačítkem na libovolný uzel a vyberte možnost **zobrazení streamování trasování**.</span><span class="sxs-lookup"><span data-stu-id="53eed-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![Okno otevřít hello diagnostických událostí][viewdiagnosticevents]
   
    <span data-ttu-id="53eed-121">Pokud chcete toofilter trasování tooa konkrétní službu nebo aplikaci, jednoduše povolte streamování trasování na této konkrétní služby nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="53eed-121">If you want toofilter your traces tooa specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="53eed-122">Hello diagnostických událostí se zobrazí v automaticky generovány hello **ServiceEventSource.cs** souborů a jsou volat z kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="53eed-122">hello diagnostic events can be seen in hello automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="53eed-123">Hello **diagnostických událostí** okno podporuje filtrování, pozastavení a kontrole událostí v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="53eed-123">hello **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="53eed-124">Filtr Hello je hledání jednoduchého řetězce hello událostí zprávy, včetně jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="53eed-124">hello filter is a simple string search of hello event message, including its contents.</span></span>
   
    ![Filtrovat, pozastavení a obnovení nebo zkontrolujte události v reálném čase][diagnosticeventsactions]
8. <span data-ttu-id="53eed-126">Ladění služeb je jako ladění žádnou jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53eed-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="53eed-127">Bude obvykle nastavit zarážky pomocí sady Visual Studio pro snadné ladění.</span><span class="sxs-lookup"><span data-stu-id="53eed-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="53eed-128">I když spolehlivé kolekce replikují mezi několika uzly, se stále implementovat rozhraní IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="53eed-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="53eed-129">To znamená, že můžete použít hello výsledky zobrazení v sadě Visual Studio při ladění toosee jste uložený v.</span><span class="sxs-lookup"><span data-stu-id="53eed-129">This means that you can use hello Results View in Visual Studio while debugging toosee what you've stored inside.</span></span> <span data-ttu-id="53eed-130">Jednoduše zarážku kdekoli v kódu.</span><span class="sxs-lookup"><span data-stu-id="53eed-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Spuštění ladění aplikace][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="53eed-132">Ladění vzdáleného aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="53eed-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="53eed-133">Pokud vaše aplikace Service Fabric běží na clusteru Service Fabric v Azure, máte možnost tooremotely ladění tyto přímo ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53eed-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able tooremotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="53eed-134">Funkce Hello vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="53eed-134">hello feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="53eed-135">Vzdálené ladění je určená pro scénáře vývoje/testování a není toobe použít v produkčním prostředí kvůli hello dopad na hello spuštěných aplikací.</span><span class="sxs-lookup"><span data-stu-id="53eed-135">Remote debugging is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> 
> 

1. <span data-ttu-id="53eed-136">Přejděte tooyour clusteru v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit ladění**</span><span class="sxs-lookup"><span data-stu-id="53eed-136">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Povolit vzdálené ladění][enableremotedebugging]
   
    <span data-ttu-id="53eed-138">To bude ji hello proces povolení hello vzdálené ladění rozšíření na uzly clusteru, a také vyžaduje konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="53eed-138">This will kick off hello process of enabling hello remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="53eed-139">Klikněte pravým tlačítkem na uzel clusteru hello v **Průzkumník cloudu**a zvolte **připojit ladicí program**</span><span class="sxs-lookup"><span data-stu-id="53eed-139">Right-click hello cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Připojit ladicí program][attachdebugger]
3. <span data-ttu-id="53eed-141">V hello **připojit tooprocess** dialogovém okně, vyberte hello proces toodebug a klikněte na **připojit**</span><span class="sxs-lookup"><span data-stu-id="53eed-141">In hello **Attach tooprocess** dialog, choose hello process you want toodebug, and click **Attach**</span></span>
   
    ![Vyberte proces][chooseprocess]
   
    <span data-ttu-id="53eed-143">Název Hello hello procesu, který má tooattach k, rovná hello název název sestavení projektu služby.</span><span class="sxs-lookup"><span data-stu-id="53eed-143">hello name of hello process you want tooattach to, equals hello name of your service project assembly name.</span></span>
   
    <span data-ttu-id="53eed-144">ladicí program Hello připojí uzly tooall spuštěn proces hello.</span><span class="sxs-lookup"><span data-stu-id="53eed-144">hello debugger will attach tooall nodes running hello process.</span></span>
   
   * <span data-ttu-id="53eed-145">Všechny instance služby hello na všech uzlech v případě hello, kde jsou ladění bezstavové služby, jsou součástí relace ladění hello.</span><span class="sxs-lookup"><span data-stu-id="53eed-145">In hello case where you are debugging a stateless service, all instances of hello service on all nodes are part of hello debug session.</span></span>
   * <span data-ttu-id="53eed-146">Pokud ladíte stavové služby, bude pouze hello primární repliky libovolného oddílu aktivní a proto zachycení pomocí ladicího programu hello.</span><span class="sxs-lookup"><span data-stu-id="53eed-146">If you are debugging a stateful service, only hello primary replica of any partition will be active and therefore caught by hello debugger.</span></span> <span data-ttu-id="53eed-147">Pokud se primární repliky hello přesune během relace ladění hello, zůstanou hello zpracování této repliky součástí relace ladění hello.</span><span class="sxs-lookup"><span data-stu-id="53eed-147">If hello primary replica moves during hello debug session, hello processing of that replica will still be part of hello debug session.</span></span>
   * <span data-ttu-id="53eed-148">V pořadí tooonly catch příslušné oddíly nebo instance dané služby můžete použít podmíněné zarážky tooonly přerušení na konkrétní oddíl nebo instance.</span><span class="sxs-lookup"><span data-stu-id="53eed-148">In order tooonly catch relevant partitions or instances of a given service, you can use conditional breakpoints tooonly break a specific partition or instance.</span></span>
     
     ![Podmíněné zarážky][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="53eed-150">Aktuálně nepodporujeme ladění clusteru Service Fabric s více instancemi hello stejný název spustitelného souboru služby.</span><span class="sxs-lookup"><span data-stu-id="53eed-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of hello same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="53eed-151">Jakmile dokončíte ladění aplikace, můžete zakázat hello vzdáleného ladění rozšíření kliknutím pravým tlačítkem na cluster hello v **Průzkumník cloudu** a zvolte **zakázat ladění**</span><span class="sxs-lookup"><span data-stu-id="53eed-151">Once you finish debugging your application, you can disable hello remote debugging extension by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Zakázání vzdálené ladění][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="53eed-153">Streamování trasování z uzlu vzdáleného clusteru</span><span class="sxs-lookup"><span data-stu-id="53eed-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="53eed-154">Můžete je taky možné toostream trasování přímo v uzlu tooVisual vzdálený cluster Studio.</span><span class="sxs-lookup"><span data-stu-id="53eed-154">You are also able toostream traces directly from a remote cluster node tooVisual Studio.</span></span> <span data-ttu-id="53eed-155">Tato funkce umožňuje události trasování ETW toostream, vytváří na uzlu clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="53eed-155">This feature allows you toostream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="53eed-156">Tato funkce vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="53eed-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="53eed-157">Tato funkce podporuje pouze clustery spuštěná v Azure.</span><span class="sxs-lookup"><span data-stu-id="53eed-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="53eed-158">Streamování trasování je určená pro scénáře vývoje/testování a není toobe použít v produkčním prostředí kvůli hello dopad na hello spuštěných aplikací.</span><span class="sxs-lookup"><span data-stu-id="53eed-158">Streaming traces is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> <span data-ttu-id="53eed-159">V produkční scénář měli byste tedy spoléhat na předávání událostí pomocí diagnostiky Azure.</span><span class="sxs-lookup"><span data-stu-id="53eed-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="53eed-160">Přejděte tooyour clusteru v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit trasování streamování**</span><span class="sxs-lookup"><span data-stu-id="53eed-160">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Povolit vzdálené streamování trasování][enablestreamingtraces]
   
    <span data-ttu-id="53eed-162">To bude ji hello proces povolení hello streamování rozšíření trasování na uzly clusteru, a také vyžaduje konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="53eed-162">This will kick off hello process of enabling hello streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="53eed-163">Rozbalte hello **uzly** element v **Průzkumník cloudu**, klikněte pravým tlačítkem na hello uzel má toostream trasování z a zvolte **zobrazení streamování trasování**</span><span class="sxs-lookup"><span data-stu-id="53eed-163">Expand hello **Nodes** element in **Cloud Explorer**, right-click hello node you want toostream traces from and choose **View Streaming Traces**</span></span>
   
    ![Zobrazení vzdáleného streamování trasování][viewremotestreamingtraces]
   
    <span data-ttu-id="53eed-165">Opakujte krok 2 pro libovolný počet uzlů tak, jak chcete toosee trasování z.</span><span class="sxs-lookup"><span data-stu-id="53eed-165">Repeat step 2 for as many nodes as you want toosee traces from.</span></span> <span data-ttu-id="53eed-166">Každý datový proud uzly se zobrazí v okně vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="53eed-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="53eed-167">Nyní je možné toosee hello trasování vysílaných Service Fabric a služeb.</span><span class="sxs-lookup"><span data-stu-id="53eed-167">You are now able toosee hello traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="53eed-168">Pokud chcete zobrazit tooonly události hello toofilter konkrétní aplikaci, jednoduše zadejte název hello aplikace hello ve filtru hello.</span><span class="sxs-lookup"><span data-stu-id="53eed-168">If you want toofilter hello events tooonly show a specific application, simply type in hello name of hello application in hello filter.</span></span>
   
    ![Zobrazení, streamování trasování][viewingstreamingtraces]
3. <span data-ttu-id="53eed-170">Jakmile dokončíte streamování trasování z clusteru, můžete zakázat vzdálené streamování trasování, kliknutím pravým tlačítkem na cluster hello v **Průzkumník cloudu** a zvolte **zakázat streamování trasování**</span><span class="sxs-lookup"><span data-stu-id="53eed-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Zakázání vzdálené streamování trasování][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="53eed-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53eed-172">Next steps</span></span>
* <span data-ttu-id="53eed-173">[Testování služby Service Fabric](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="53eed-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="53eed-174">[Spravujte svoje aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="53eed-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

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
