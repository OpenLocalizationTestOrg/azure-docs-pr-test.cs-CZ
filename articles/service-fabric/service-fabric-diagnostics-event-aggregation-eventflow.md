---
title: "Azure Service Fabric událostí agregace s EventFlow | Microsoft Docs"
description: "Další informace o agregaci a shromažďování událostí pomocí EventFlow pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 90d26a77b749e70de3a7d910f15820653e2ef39b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a><span data-ttu-id="3b73e-103">Seskupení událostí a kolekce pomocí EventFlow</span><span class="sxs-lookup"><span data-stu-id="3b73e-103">Event aggregation and collection using EventFlow</span></span>

<span data-ttu-id="3b73e-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) může směrovat události z uzlu na jeden nebo více monitorování místa určení.</span><span class="sxs-lookup"><span data-stu-id="3b73e-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) can route events from a node to one or more monitoring destinations.</span></span> <span data-ttu-id="3b73e-105">Vzhledem k tomu, že je zahrnut jako balíčku NuGet ve vašem projektu service, se službou, odstraňuje problém konfigurace podle uzlu, již bylo zmíněno dříve o Azure Diagnostics přenosu EventFlow kódu a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3b73e-105">Because it is included as a NuGet package in your service project, EventFlow code and configuration travel with the service, eliminating the per-node configuration issue mentioned earlier about Azure Diagnostics.</span></span> <span data-ttu-id="3b73e-106">EventFlow běží v rámci procesu vaší služby a připojuje přímo k nakonfigurované výstupy.</span><span class="sxs-lookup"><span data-stu-id="3b73e-106">EventFlow runs within your service process, and connects directly to the configured outputs.</span></span> <span data-ttu-id="3b73e-107">Z důvodu přímé připojení EventFlow funguje pro Azure, kontejneru a nasazení služby v místě.</span><span class="sxs-lookup"><span data-stu-id="3b73e-107">Because of the direct connection, EventFlow works for Azure, container, and on-premises service deployments.</span></span> <span data-ttu-id="3b73e-108">Dávejte pozor, pokud spustíte EventFlow v podobě výkonných scénáře, jako třeba v kontejneru, protože každý kanál EventFlow vytvoří externí připojení.</span><span class="sxs-lookup"><span data-stu-id="3b73e-108">Be careful if you run EventFlow in high-density scenarios, such as in a container, because each EventFlow pipeline makes an external connection.</span></span> <span data-ttu-id="3b73e-109">Takže pokud hostujete několik procesy, můžete načítat několik odchozí připojení!</span><span class="sxs-lookup"><span data-stu-id="3b73e-109">So, if you host several processes, you get several outbound connections!</span></span> <span data-ttu-id="3b73e-110">Tato akce není tolik důležité aplikace Service Fabric, protože všechny repliky `ServiceType` spustit ve stejném procesu, a to omezuje počet odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="3b73e-110">This isn't as much a concern for Service Fabric applications, because all replicas of a `ServiceType` run in the same process, and this limits the number of outbound connections.</span></span> <span data-ttu-id="3b73e-111">EventFlow také nabízí filtrování událostí tak, aby se odesílají jenom události, které odpovídají zadané filtru.</span><span class="sxs-lookup"><span data-stu-id="3b73e-111">EventFlow also offers event filtering, so that only the events that match the specified filter are sent.</span></span>

## <a name="setting-up-eventflow"></a><span data-ttu-id="3b73e-112">Nastavení EventFlow</span><span class="sxs-lookup"><span data-stu-id="3b73e-112">Setting up EventFlow</span></span>

<span data-ttu-id="3b73e-113">Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b73e-113">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="3b73e-114">Pokud chcete přidat do projektu služby Service Fabric EventFlow, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte "Balíčky NuGet spravovat".</span><span class="sxs-lookup"><span data-stu-id="3b73e-114">To add EventFlow to a Service Fabric service project, right-click the project in the Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="3b73e-115">Přejděte na kartu "Vyhledat" a vyhledejte "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="3b73e-115">Switch to the "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

<span data-ttu-id="3b73e-117">Zobrazí se seznam různých balíčků objeví, s popiskem "Vstup" a "Výstupy".</span><span class="sxs-lookup"><span data-stu-id="3b73e-117">You will see a list of various packages show up, labeled with "Inputs" and "Outputs".</span></span> <span data-ttu-id="3b73e-118">EventFlow podporuje různé zprostředkovatelé různých protokolování a analyzátory.</span><span class="sxs-lookup"><span data-stu-id="3b73e-118">EventFlow supports various different logging providers and analyzers.</span></span> <span data-ttu-id="3b73e-119">Služby hostování EventFlow by měla obsahovat příslušné balíčky v závislosti na zdroj a cíl pro protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="3b73e-119">The service hosting EventFlow should include appropriate packages depending on the source and destination for the application logs.</span></span> <span data-ttu-id="3b73e-120">Kromě základní ServiceFabric balíčku musíte také aspoň jeden vstup a výstup nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="3b73e-120">In addition to the core ServiceFabric package, you also need at least one Input and Output configured.</span></span> <span data-ttu-id="3b73e-121">Pro exmaple přidáním následujících balíčků do odeslané událostí EventSource Application insights:</span><span class="sxs-lookup"><span data-stu-id="3b73e-121">For exmaple, you can add the following packages to sent EventSource events to Application Insights:</span></span>

* <span data-ttu-id="3b73e-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource`k zaznamenání dat od služby EventSource – třída a od standardní EventSources například *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)</span><span class="sxs-lookup"><span data-stu-id="3b73e-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource` to capture data from the service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* <span data-ttu-id="3b73e-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(přidáme poslat protokoly prostředek Azure Application Insights)</span><span class="sxs-lookup"><span data-stu-id="3b73e-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` (we are going to send the logs to an Azure Application Insights resource)</span></span>
* <span data-ttu-id="3b73e-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(umožňuje inicializaci kanálu EventFlow z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)</span><span class="sxs-lookup"><span data-stu-id="3b73e-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(enables initialization of the EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

>[!NOTE]
><span data-ttu-id="3b73e-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource`balíček vyžaduje projekt služby, který má cílové rozhraní .NET Framework 4.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3b73e-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource` package requires the service project to target .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="3b73e-126">Ujistěte se, že nastavíte příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="3b73e-126">Make sure you set the appropriate target framework in project properties before installing this package.</span></span>

<span data-ttu-id="3b73e-127">Po instalaci všech balíčků, dalším krokem je ke konfiguraci a povolení EventFlow ve službě.</span><span class="sxs-lookup"><span data-stu-id="3b73e-127">After all the packages are installed, the next step is to configure and enable EventFlow in the service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="3b73e-128">Konfigurace a povolení shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="3b73e-128">Configuring and enabling log collection</span></span>
<span data-ttu-id="3b73e-129">Odpovědná za zasílání protokolů EventFlow kanálu je vytvořený z specifikaci uložené v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="3b73e-129">The EventFlow pipeline responsible for sending the logs is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="3b73e-130">`Microsoft.Diagnostics.EventFlow.ServiceFabric` Balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složku řešení s názvem `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="3b73e-130">The `Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder, named `eventFlowConfig.json`.</span></span> <span data-ttu-id="3b73e-131">Tento konfigurační soubor je potřeba upravit tak, aby zaznamenání dat z výchozí služba `EventSource` třídy a všechny ostatní vstupy chcete konfigurovat a odesílat data na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="3b73e-131">This configuration file needs to be modified to capture data from the default service `EventSource` class, and any other inputs you want to configure, and send data to the appropriate place.</span></span>

<span data-ttu-id="3b73e-132">Zde je ukázka *eventFlowConfig.json* založené na balíčky NuGet uvedených výše:</span><span class="sxs-lookup"><span data-stu-id="3b73e-132">Here is a sample *eventFlowConfig.json* based on the NuGet packages mentioned above:</span></span>
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

<span data-ttu-id="3b73e-133">Název služby ServiceEventSource je hodnota vlastnosti název `EventSourceAttribute` třídy ServiceEventSource použít.</span><span class="sxs-lookup"><span data-stu-id="3b73e-133">The name of service's ServiceEventSource is the value of the Name property of the `EventSourceAttribute` applied to the ServiceEventSource class.</span></span> <span data-ttu-id="3b73e-134">Všechny se vyskytuje v `ServiceEventSource.cs` souboru, který je součástí kódu služby.</span><span class="sxs-lookup"><span data-stu-id="3b73e-134">It is all specified in the `ServiceEventSource.cs` file, which is part of the service code.</span></span> <span data-ttu-id="3b73e-135">Například v následující fragment kódu je název ServiceEventSource *Moje_firma. Application1 Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="3b73e-135">For example, in the following code snippet the name of the ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

<span data-ttu-id="3b73e-136">Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3b73e-136">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="3b73e-137">Změny tohoto souboru můžou být součástí úplné nebo konfigurace jen upgrady služby, podstoupí kontroly upgradu stavu Service Fabric a automatického vrácení zpět, dojde-li k selhání při upgradu.</span><span class="sxs-lookup"><span data-stu-id="3b73e-137">Changes to this file can be included in full- or configuration-only upgrades of the service, subject to Service Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="3b73e-138">Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="3b73e-138">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="3b73e-139">*Filtry* část konfigurace můžete dále přizpůsobit informace, které se bude předat kanálem EventFlow výstupy umožňuje vyřadit nebo obsahovat určité informace, nebo změnit strukturu data události.</span><span class="sxs-lookup"><span data-stu-id="3b73e-139">The *filters* section of the config allows you to further customize the information that is going to go through the EventFlow pipeline to the outputs, allowing you to drop or include certain information, or change the structure of the event data.</span></span> <span data-ttu-id="3b73e-140">Další informace o filtrování najdete v tématu [EventFlow filtry](https://github.com/Azure/diagnostics-eventflow#filters).</span><span class="sxs-lookup"><span data-stu-id="3b73e-140">For more information on filtering, see [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).</span></span>

<span data-ttu-id="3b73e-141">Posledním krokem je vytvoření instance EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="3b73e-141">The final step is to instantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

<span data-ttu-id="3b73e-142">Název předány jako parametr `CreatePipeline` metodu `ServiceFabricDiagnosticsPipelineFactory` je název *stavu entity* představující kanálu EventFlow protokolu kolekce.</span><span class="sxs-lookup"><span data-stu-id="3b73e-142">The name passed as the parameter of the `CreatePipeline` method of the `ServiceFabricDiagnosticsPipelineFactory` is the name of the *health entity* representing the EventFlow log collection pipeline.</span></span> <span data-ttu-id="3b73e-143">Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím subsystém stavu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3b73e-143">This name is used if EventFlow encounters and error and reports it through the Service Fabric health subsystem.</span></span>

### <a name="using-service-fabric-settings-and-application-parameters-to-in-eventflowconfig"></a><span data-ttu-id="3b73e-144">Pomocí nastavení Service Fabric a parametry aplikace v eventFlowConfig</span><span class="sxs-lookup"><span data-stu-id="3b73e-144">Using Service Fabric settings and application parameters to in eventFlowConfig</span></span>

<span data-ttu-id="3b73e-145">EventFlow podporuje konfiguraci nastavení EventFlow pomocí Service Fabric nastavení a aplikace parametry.</span><span class="sxs-lookup"><span data-stu-id="3b73e-145">EventFlow supports using Service Fabric settings and application paremeters to configure EventFlow settings.</span></span> <span data-ttu-id="3b73e-146">Můžete se podívat do Service Fabric nastavení parametrů pomocí této syntaxe speciální hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3b73e-146">You can refer to Service Fabric settings parameters using this special syntax for values:</span></span>

```json
servicefabric:/<section-name>/<setting-name>
``` 

<span data-ttu-id="3b73e-147">`<section-name>`představuje název konfiguračního oddílu Service Fabric a `<setting-name>` je zajištění hodnotu, která se použije ke konfiguraci nastavení EventFlow nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3b73e-147">`<section-name>` is the name of the Service Fabric configuration section, and `<setting-name>` is the configuration setting providing the value that will be used to configure an EventFlow setting.</span></span> <span data-ttu-id="3b73e-148">Číst informace o tom, jak to provést, přejděte na [podporu pro Service Fabric nastavení a aplikace parametry](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span><span class="sxs-lookup"><span data-stu-id="3b73e-148">To read more about how to do this, go to [Support for Service Fabric settings and application parameters](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span></span>

## <a name="verification"></a><span data-ttu-id="3b73e-149">Ověření</span><span class="sxs-lookup"><span data-stu-id="3b73e-149">Verification</span></span>

<span data-ttu-id="3b73e-150">Spuštění služby a sledovat ladění výstup – okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b73e-150">Start your service and observe the Debug output window in Visual Studio.</span></span> <span data-ttu-id="3b73e-151">Po spuštění služby, měli byste začít zobrazuje důkaz, že vaše služba odesílá záznamy do výstupu, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="3b73e-151">After the service is started, you should start seeing evidence that your service is sending records to the output that you have configured.</span></span> <span data-ttu-id="3b73e-152">Přejděte k platformě analýzy a vizualizace událostí a ověřte, že protokoly spustili zobrazíte nahoru (může trvat několik minut).</span><span class="sxs-lookup"><span data-stu-id="3b73e-152">Navigate to your event analysis and visualization platform and confirm that logs have started to show up (could take a few minutes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b73e-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b73e-153">Next steps</span></span>

* [<span data-ttu-id="3b73e-154">Analýza události a vizualizace s Application Insights</span><span class="sxs-lookup"><span data-stu-id="3b73e-154">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="3b73e-155">Analýza události a vizualizace s OMS</span><span class="sxs-lookup"><span data-stu-id="3b73e-155">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)
* [<span data-ttu-id="3b73e-156">EventFlow dokumentace</span><span class="sxs-lookup"><span data-stu-id="3b73e-156">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)