---
title: "Shromažďovat protokoly přímo ze procesu služby Azure Service Fabric | Microsoft Azure"
description: "Popisuje Service Fabric aplikace mohou zasílat protokoly přímo do centrálního umístění, jako je Azure Application Insights nebo Elasticsearch, bez nutnosti spoléhat se na agentovi Azure Diagnostics."
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: rwike77
editor: 
ms.assetid: ab92c99b-1edd-4677-8c28-4e591d909b47
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/18/2017
ms.author: karolz
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: b7d2541928f4248750417a77d99033c8b4354dcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="88b03-103">Shromažďovat protokoly přímo ze procesu služby Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="88b03-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="88b03-104">Proces shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="88b03-104">In-process log collection</span></span>
<span data-ttu-id="88b03-105">Aplikace pro shromažďování protokolů pomocí [rozšíření Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) je vhodný pro **Azure Service Fabric** služby pokud sada protokolu zdroje a cíle je malý, nemění často a je přehledné mapování mezi zdroji a cíli.</span><span class="sxs-lookup"><span data-stu-id="88b03-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if the set of log sources and destinations is small, does not change often, and there is a straightforward mapping between the sources and their destinations.</span></span> <span data-ttu-id="88b03-106">Pokud ne, alternativou je tak, aby měl služby odeslat protokoly přímo do centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="88b03-106">If not, an alternative is to have services send their logs directly to a central location.</span></span> <span data-ttu-id="88b03-107">Tento proces se označuje jako **shromáždění protokolů v rámci procesu** a má několik potenciálních výhod:</span><span class="sxs-lookup"><span data-stu-id="88b03-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="88b03-108">*Jednoduchá konfigurace a nasazení*</span><span class="sxs-lookup"><span data-stu-id="88b03-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="88b03-109">Konfigurace shromažďování diagnostických dat je právě součástí konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="88b03-109">The configuration of diagnostic data collection is just part of the service configuration.</span></span> <span data-ttu-id="88b03-110">Je snadné vždy synchronizujte jej "" s zbývající aplikace.</span><span class="sxs-lookup"><span data-stu-id="88b03-110">It is easy to always keep it "in sync" with the rest of the application.</span></span>
    * <span data-ttu-id="88b03-111">Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.</span><span class="sxs-lookup"><span data-stu-id="88b03-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="88b03-112">Kolekce založené na agentovi protokolu obvykle vyžaduje samostatné nasazení a konfigurace diagnostiky agenta, který je velmi Správce úloh a potenciální příčinu chyby.</span><span class="sxs-lookup"><span data-stu-id="88b03-112">Agent-based log collection usually requires a separate deployment and configuration of the diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="88b03-113">Často existuje pouze jedna instance agenta povolena na virtuální počítač (uzel) a konfigurace agenta je sdílen mezi všechny aplikace a služby spuštěné v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="88b03-113">Often there is only one instance of the agent allowed per virtual machine (node) and the agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="88b03-114">*Flexibilita*</span><span class="sxs-lookup"><span data-stu-id="88b03-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="88b03-115">Aplikace může posílat data, bez ohledu na účelu, také je zde knihovna klienta, který podporuje systém cílových dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="88b03-115">The application can send the data wherever it needs to go, as long as there is a client library that supports the targeted data storage system.</span></span> <span data-ttu-id="88b03-116">Nový port nelze přidat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="88b03-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="88b03-117">Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.</span><span class="sxs-lookup"><span data-stu-id="88b03-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="88b03-118">Kolekce založené na agentovi protokolu je často omezena jímky dat, které podporuje agenta.</span><span class="sxs-lookup"><span data-stu-id="88b03-118">Agent-based log collection is often limited by the data sinks that the agent supports.</span></span> <span data-ttu-id="88b03-119">Někteří agenti jsou extensible.</span><span class="sxs-lookup"><span data-stu-id="88b03-119">Some agents are extensible.</span></span>

* <span data-ttu-id="88b03-120">*Přístup k datům interní aplikace a kontext*</span><span class="sxs-lookup"><span data-stu-id="88b03-120">*Access to internal application data and context*</span></span>
   
    * <span data-ttu-id="88b03-121">Subsystém diagnostiky běžících v rámci procesu aplikace nebo služba může snadno posílení trasování s kontextové informace.</span><span class="sxs-lookup"><span data-stu-id="88b03-121">The diagnostic subsystem running inside the application/service process can easily augment the traces with contextual information.</span></span>
    * <span data-ttu-id="88b03-122">S kolekce založené na agentovi protokolu musí být data zaslány agentovi prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="88b03-122">With agent-based log collection, the data must be sent to an agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="88b03-123">Tento mechanismus může použít další omezení.</span><span class="sxs-lookup"><span data-stu-id="88b03-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="88b03-124">Je možné kombinovat a těžit z obou metod kolekce.</span><span class="sxs-lookup"><span data-stu-id="88b03-124">It is possible to combine and benefit from both collection methods.</span></span> <span data-ttu-id="88b03-125">Ve skutečnosti může být nejlepším řešením pro mnoho aplikací.</span><span class="sxs-lookup"><span data-stu-id="88b03-125">Indeed, it might be the best solution for many applications.</span></span> <span data-ttu-id="88b03-126">Kolekce založené na agentovi je přirozené řešení pro shromažďování protokolů týkající se celý cluster a jednotlivé uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="88b03-126">Agent-based collection is a natural solution for collecting logs related to the whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="88b03-127">Je mnohem větší spolehlivost způsobem, než v procesu protokolu kolekce k diagnostikování problémů spuštění služby a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="88b03-127">It is much more reliable way, than in-process log collection, to diagnose service startup problems and crashes.</span></span> <span data-ttu-id="88b03-128">Navíc s mnoha služeb běžících v rámci clusteru Service Fabric, každá služba provádění svou vlastní kolekci v procesu protokolu výsledkem mnoha odchozí připojení z clusteru.</span><span class="sxs-lookup"><span data-stu-id="88b03-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from the cluster.</span></span> <span data-ttu-id="88b03-129">Velký počet odchozích připojení je zdanění pro subsystém sítě i pro cíl protokolu.</span><span class="sxs-lookup"><span data-stu-id="88b03-129">Large number of outgoing connections is taxing both for the network subsystem and for the log destination.</span></span> <span data-ttu-id="88b03-130">Agent například [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) můžete shromažďovat data z více služeb a odeslat veškerá data prostřednictvím několik připojení, zvýšení propustnosti.</span><span class="sxs-lookup"><span data-stu-id="88b03-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="88b03-131">V tomto článku ukážeme, jak nastavit kolekce v procesu protokolu pomocí [ **knihovny open-source EventFlow**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="88b03-131">In this article, we show how to set up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="88b03-132">K tomuto účelu může používat další knihovny, ale EventFlow má výhodu s byly navrženy speciálně pro kolekce v procesu protokolu a k podpoře služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="88b03-132">Other libraries might be used for the same purpose, but EventFlow has the benefit of having been designed specifically for in-process log collection and to support Service Fabric services.</span></span> <span data-ttu-id="88b03-133">Používáme [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) jako cíl protokolu.</span><span class="sxs-lookup"><span data-stu-id="88b03-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as the log destination.</span></span> <span data-ttu-id="88b03-134">Jiným cílům, jako [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) nebo [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="88b03-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="88b03-135">Je právě dotaz odpovídající balíček NuGet instalaci a konfiguraci cílového v konfiguračním souboru EventFlow.</span><span class="sxs-lookup"><span data-stu-id="88b03-135">It is just a question of installing appropriate NuGet package and configuring the destination in the EventFlow configuration file.</span></span> <span data-ttu-id="88b03-136">Další informace o protokolu cíle než Application Insights, najdete v části [EventFlow dokumentaci](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="88b03-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a><span data-ttu-id="88b03-137">Přidávání EventFlow knihovny do projektu služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="88b03-137">Adding EventFlow library to a Service Fabric service project</span></span>
<span data-ttu-id="88b03-138">Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="88b03-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="88b03-139">Pokud chcete přidat do projektu služby Service Fabric EventFlow, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte "Balíčky NuGet spravovat".</span><span class="sxs-lookup"><span data-stu-id="88b03-139">To add EventFlow to a Service Fabric service project, right-click the project in the Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="88b03-140">Přejděte na kartu "Vyhledat" a vyhledejte "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="88b03-140">Switch to the "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet][1]

<span data-ttu-id="88b03-142">Služby hostování EventFlow by měla obsahovat příslušné balíčky v závislosti na zdroj a cíl pro protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="88b03-142">The service hosting EventFlow should include appropriate packages depending on the source and destination for the application logs.</span></span> <span data-ttu-id="88b03-143">Přidejte následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="88b03-143">Add the following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="88b03-144">(k zaznamenání dat od služby EventSource – třída a od standardní EventSources například *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)</span><span class="sxs-lookup"><span data-stu-id="88b03-144">(to capture data from the service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="88b03-145">(přidáme poslat protokoly prostředek Azure Application Insights)</span><span class="sxs-lookup"><span data-stu-id="88b03-145">(we are going to send the logs to an Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="88b03-146">(umožňuje inicializaci kanálu EventFlow z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)</span><span class="sxs-lookup"><span data-stu-id="88b03-146">(enables initialization of the EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="88b03-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`balíček vyžaduje projekt služby, který má cílové rozhraní .NET Framework 4.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="88b03-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires the service project to target .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="88b03-148">Ujistěte se, že nastavíte příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="88b03-148">Make sure you set the appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="88b03-149">Po instalaci všech balíčků, dalším krokem je ke konfiguraci a povolení EventFlow ve službě.</span><span class="sxs-lookup"><span data-stu-id="88b03-149">After all the packages are installed, the next step is to configure and enable EventFlow in the service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="88b03-150">Konfigurace a povolení shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="88b03-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="88b03-151">EventFlow kanálu, která je odpovědná za zasílání protokolů, je vytvořený z specifikaci uložené v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="88b03-151">EventFlow pipeline, responsible for sending the logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="88b03-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složce řešení.</span><span class="sxs-lookup"><span data-stu-id="88b03-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="88b03-153">Název souboru je `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="88b03-153">The file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="88b03-154">Tento konfigurační soubor je potřeba upravit tak, aby zaznamenání dat z výchozí služba `EventSource` třídy a posílat data do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="88b03-154">This configuration file needs to be modified to capture data from the default service `EventSource` class and send data to Application Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="88b03-155">Předpokládáme, že jste obeznámeni s **Azure Application Insights** služby a zda máte prostředek Application Insights, který chcete použít k monitorování služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="88b03-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan to use to monitor your Service Fabric service.</span></span> <span data-ttu-id="88b03-156">Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="88b03-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="88b03-157">Otevřete `eventFlowConfig.json` souboru v editoru a změňte jeho obsah, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="88b03-157">Open the `eventFlowConfig.json` file in the editor and change its content as shown below.</span></span> <span data-ttu-id="88b03-158">Nezapomeňte nahradit název ServiceEventSource a klíč instrumentace Application Insights podle komentáře.</span><span class="sxs-lookup"><span data-stu-id="88b03-158">Make sure to replace the ServiceEventSource name and Application Insights instrumentation key according to comments.</span></span> 

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

> [!NOTE]
> <span data-ttu-id="88b03-159">Název služby ServiceEventSource je hodnota vlastnosti název `EventSourceAttribute` třídy ServiceEventSource použít.</span><span class="sxs-lookup"><span data-stu-id="88b03-159">The name of service's ServiceEventSource is the value of the Name property of the `EventSourceAttribute` applied to the ServiceEventSource class.</span></span> <span data-ttu-id="88b03-160">Všechny se vyskytuje v `ServiceEventSource.cs` souboru, který je součástí kódu služby.</span><span class="sxs-lookup"><span data-stu-id="88b03-160">It is all specified in the `ServiceEventSource.cs` file, which is part of the service code.</span></span> <span data-ttu-id="88b03-161">Například v následující fragment kódu je název ServiceEventSource *Moje_firma. Application1 Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="88b03-161">For example, in the following code snippet the name of the ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="88b03-162">Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="88b03-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="88b03-163">Změny tohoto souboru můžou být součástí úplné nebo konfigurace jen upgrady služby, podstoupí kontroly upgradu stavu Service Fabric a automatického vrácení zpět, dojde-li k selhání při upgradu.</span><span class="sxs-lookup"><span data-stu-id="88b03-163">Changes to this file can be included in full- or configuration-only upgrades of the service, subject to Service Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="88b03-164">Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="88b03-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="88b03-165">Posledním krokem je vytvoření instance EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="88b03-165">The final step is to instantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="88b03-166">V následujícím příkladu jsou označené související EventFlow přidání komentáře počínaje `****`:</span><span class="sxs-lookup"><span data-stu-id="88b03-166">In the following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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

<span data-ttu-id="88b03-167">Název předány jako parametr `CreatePipeline` metodu `ServiceFabricDiagnosticsPipelineFactory` je název *stavu entity* představující kanálu EventFlow protokolu kolekce.</span><span class="sxs-lookup"><span data-stu-id="88b03-167">The name passed as the parameter of the `CreatePipeline` method of the `ServiceFabricDiagnosticsPipelineFactory` is the name of the *health entity* representing the EventFlow log collection pipeline.</span></span> <span data-ttu-id="88b03-168">Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím subsystém stavu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="88b03-168">This name is used if EventFlow encounters and error and reports it through the Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="88b03-169">Ověření</span><span class="sxs-lookup"><span data-stu-id="88b03-169">Verification</span></span>
<span data-ttu-id="88b03-170">Spuštění služby a sledovat ladění výstup – okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="88b03-170">Start your service and observe the Debug output window in Visual Studio.</span></span> <span data-ttu-id="88b03-171">Po spuštění služby, měli byste začít zobrazuje důkaz, že vaše služba odesílá záznamů "Telemetrii Application Insights".</span><span class="sxs-lookup"><span data-stu-id="88b03-171">After the service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="88b03-172">Otevřete webový prohlížeč a přejděte přejděte do zdroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="88b03-172">Open a web browser and navigate go to your Application Insights resource.</span></span> <span data-ttu-id="88b03-173">Otevřete kartu "Vyhledat" (v horní části okna výchozí "Přehled").</span><span class="sxs-lookup"><span data-stu-id="88b03-173">Open "Search" tab (at the top of the default "Overview" blade).</span></span> <span data-ttu-id="88b03-174">Po chvíli trvat, byste měli začít zobrazuje vaše trasování v portálu služby Application Insights:</span><span class="sxs-lookup"><span data-stu-id="88b03-174">After a short delay you should start seeing your traces in the Application Insights portal:</span></span>

![Aplikace portálu Statistika zobrazující protokoly z aplikace Service Fabric][2]

## <a name="next-steps"></a><span data-ttu-id="88b03-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88b03-176">Next steps</span></span>
* [<span data-ttu-id="88b03-177">Další informace o diagnostice a monitorování služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="88b03-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="88b03-178">EventFlow dokumentace</span><span class="sxs-lookup"><span data-stu-id="88b03-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
