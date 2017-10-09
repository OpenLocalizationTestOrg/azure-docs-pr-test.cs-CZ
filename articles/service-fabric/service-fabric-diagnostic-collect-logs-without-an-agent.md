---
title: "aaaCollect protokoly přímo ze Azure Service Fabric procesu služby | Microsoft Azure"
description: "Popisuje Service Fabric aplikace můžete odeslat protokoly přímo tooa centrální umístění, například Azure Application Insights nebo Elasticsearch, bez nutnosti spoléhat se na agentovi Azure Diagnostics."
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
ms.openlocfilehash: d0681a2a6aaa76028d7cb469c31c006f24bbe954
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="4129e-103">Shromažďovat protokoly přímo ze procesu služby Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4129e-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="4129e-104">Proces shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="4129e-104">In-process log collection</span></span>
<span data-ttu-id="4129e-105">Aplikace pro shromažďování protokolů pomocí [rozšíření Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) je vhodný pro **Azure Service Fabric** služby pokud hello sadu protokolu zdroje a cíle je malý, nemění často a existuje je přehledné mapování mezi zdroji hello a jejich cíle.</span><span class="sxs-lookup"><span data-stu-id="4129e-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if hello set of log sources and destinations is small, does not change often, and there is a straightforward mapping between hello sources and their destinations.</span></span> <span data-ttu-id="4129e-106">Pokud ne, alternativou je, že služby toohave odeslat protokoly přímo tooa centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="4129e-106">If not, an alternative is toohave services send their logs directly tooa central location.</span></span> <span data-ttu-id="4129e-107">Tento proces se označuje jako **shromáždění protokolů v rámci procesu** a má několik potenciálních výhod:</span><span class="sxs-lookup"><span data-stu-id="4129e-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="4129e-108">*Jednoduchá konfigurace a nasazení*</span><span class="sxs-lookup"><span data-stu-id="4129e-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="4129e-109">Konfigurace Hello shromažďování diagnostických dat je právě součástí konfigurace služby hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-109">hello configuration of diagnostic data collection is just part of hello service configuration.</span></span> <span data-ttu-id="4129e-110">Je snadno tooalways zachovat ji "synchronizována" s hello rest aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-110">It is easy tooalways keep it "in sync" with hello rest of hello application.</span></span>
    * <span data-ttu-id="4129e-111">Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.</span><span class="sxs-lookup"><span data-stu-id="4129e-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="4129e-112">Kolekce založené na agentovi protokolu obvykle vyžaduje samostatné nasazení a konfiguraci diagnostiky agenta hello, což je velmi Správce úloh a potenciální příčinu chyby.</span><span class="sxs-lookup"><span data-stu-id="4129e-112">Agent-based log collection usually requires a separate deployment and configuration of hello diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="4129e-113">Často existuje pouze jedna instance hello agenta povolena na virtuální počítač (uzel) a konfigurace agenta hello je sdílen mezi všechny aplikace a služby spuštěné v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="4129e-113">Often there is only one instance of hello agent allowed per virtual machine (node) and hello agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="4129e-114">*Flexibilita*</span><span class="sxs-lookup"><span data-stu-id="4129e-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="4129e-115">Hello aplikace může odesílat hello data bez ohledu na potřebuje toogo, dokud není klientské knihovny, která podporuje systém úložiště dat hello cílové.</span><span class="sxs-lookup"><span data-stu-id="4129e-115">hello application can send hello data wherever it needs toogo, as long as there is a client library that supports hello targeted data storage system.</span></span> <span data-ttu-id="4129e-116">Nový port nelze přidat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4129e-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="4129e-117">Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.</span><span class="sxs-lookup"><span data-stu-id="4129e-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="4129e-118">Kolekce založené na agentovi protokolu je často omezena jímky hello dat, které hello agent podporuje.</span><span class="sxs-lookup"><span data-stu-id="4129e-118">Agent-based log collection is often limited by hello data sinks that hello agent supports.</span></span> <span data-ttu-id="4129e-119">Někteří agenti jsou extensible.</span><span class="sxs-lookup"><span data-stu-id="4129e-119">Some agents are extensible.</span></span>

* <span data-ttu-id="4129e-120">*Data aplikací toointernal přístup a kontext*</span><span class="sxs-lookup"><span data-stu-id="4129e-120">*Access toointernal application data and context*</span></span>
   
    * <span data-ttu-id="4129e-121">diagnostické subsystému Hello běžících v rámci procesu aplikace nebo služby hello můžete snadno posílení hello trasování s kontextové informace.</span><span class="sxs-lookup"><span data-stu-id="4129e-121">hello diagnostic subsystem running inside hello application/service process can easily augment hello traces with contextual information.</span></span>
    * <span data-ttu-id="4129e-122">S kolekce založené na agentovi protokolu hello data musí být odeslána tooan agent prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="4129e-122">With agent-based log collection, hello data must be sent tooan agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="4129e-123">Tento mechanismus může použít další omezení.</span><span class="sxs-lookup"><span data-stu-id="4129e-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="4129e-124">Je možné toocombine a benefit z obou metod kolekce.</span><span class="sxs-lookup"><span data-stu-id="4129e-124">It is possible toocombine and benefit from both collection methods.</span></span> <span data-ttu-id="4129e-125">Ve skutečnosti může být hello nejlepší řešení pro mnoho aplikací.</span><span class="sxs-lookup"><span data-stu-id="4129e-125">Indeed, it might be hello best solution for many applications.</span></span> <span data-ttu-id="4129e-126">Kolekce založené na agentovi je přirozené řešení pro shromažďování protokolů souvisejících toohello celý cluster a jednotlivé uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="4129e-126">Agent-based collection is a natural solution for collecting logs related toohello whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="4129e-127">Je mnohem větší spolehlivost způsobem, než kolekce protokolu v procesu, potíže se spouštěním služby toodiagnose a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4129e-127">It is much more reliable way, than in-process log collection, toodiagnose service startup problems and crashes.</span></span> <span data-ttu-id="4129e-128">Navíc s mnoha služeb běžících v rámci clusteru Service Fabric, každá služba provádění svou vlastní kolekci v procesu protokolu výsledkem mnoha odchozí připojení z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from hello cluster.</span></span> <span data-ttu-id="4129e-129">Velký počet odchozích připojení je zdanění pro subsystém hello sítě i pro cíl protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-129">Large number of outgoing connections is taxing both for hello network subsystem and for hello log destination.</span></span> <span data-ttu-id="4129e-130">Agent například [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) můžete shromažďovat data z více služeb a odeslat veškerá data prostřednictvím několik připojení, zvýšení propustnosti.</span><span class="sxs-lookup"><span data-stu-id="4129e-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="4129e-131">V tomto článku jsme ukazují, jak tooset nahoru v procesu protokolu pomocí kolekce [ **knihovny open-source EventFlow**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="4129e-131">In this article, we show how tooset up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="4129e-132">Může používat další knihovny pro hello stejný účel, ale EventFlow má výhodu hello s byl určený speciálně pro v procesu protokolu kolekce a toosupport Service Fabric služby.</span><span class="sxs-lookup"><span data-stu-id="4129e-132">Other libraries might be used for hello same purpose, but EventFlow has hello benefit of having been designed specifically for in-process log collection and toosupport Service Fabric services.</span></span> <span data-ttu-id="4129e-133">Používáme [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) jako cíl protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as hello log destination.</span></span> <span data-ttu-id="4129e-134">Jiným cílům, jako [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) nebo [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="4129e-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="4129e-135">Je právě dotaz odpovídající balíček NuGet instalaci a konfiguraci cílového hello ve hello EventFlow konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4129e-135">It is just a question of installing appropriate NuGet package and configuring hello destination in hello EventFlow configuration file.</span></span> <span data-ttu-id="4129e-136">Další informace o protokolu cíle než Application Insights, najdete v části [EventFlow dokumentaci](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="4129e-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a><span data-ttu-id="4129e-137">Přidání projektu služby EventFlow knihovny tooa Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4129e-137">Adding EventFlow library tooa Service Fabric service project</span></span>
<span data-ttu-id="4129e-138">Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="4129e-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="4129e-139">tooadd EventFlow tooa Service Fabric služby projektu, klikněte pravým tlačítkem na projekt hello v hello Průzkumníku řešení a zvolte "Balíčky NuGet spravovat".</span><span class="sxs-lookup"><span data-stu-id="4129e-139">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="4129e-140">Přepínač toohello "Browse" kartě a vyhledejte "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="4129e-140">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet][1]

<span data-ttu-id="4129e-142">hostování EventFlow Hello služby by měla obsahovat příslušné balíčky v závislosti na hello zdroj a cíl pro protokoly aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-142">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="4129e-143">Přidejte hello následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="4129e-143">Add hello following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="4129e-144">(toocapture data ze služby hello EventSource – třída a z standardní EventSources jako *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)</span><span class="sxs-lookup"><span data-stu-id="4129e-144">(toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="4129e-145">(přidáme prostředek služby Azure Application Insights toosend hello protokoly tooan)</span><span class="sxs-lookup"><span data-stu-id="4129e-145">(we are going toosend hello logs tooan Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="4129e-146">(umožňuje inicializaci kanálu EventFlow hello z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)</span><span class="sxs-lookup"><span data-stu-id="4129e-146">(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="4129e-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`balíček vyžaduje hello služby projektu tootarget rozhraní .NET Framework 4.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4129e-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="4129e-148">Ujistěte se, že nastavíte hello příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="4129e-148">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="4129e-149">Po všech hello balíčky jsou nainstalovány, hello dalším krokem je tooconfigure a povolte EventFlow ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-149">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="4129e-150">Konfigurace a povolení shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="4129e-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="4129e-151">EventFlow kanálu, která je odpovědná za zasílání protokolů hello je vytvořený z specifikaci uložené v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="4129e-151">EventFlow pipeline, responsible for sending hello logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="4129e-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složce řešení.</span><span class="sxs-lookup"><span data-stu-id="4129e-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="4129e-153">Název souboru Hello je `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="4129e-153">hello file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="4129e-154">Tento konfigurační soubor vyžaduje toobe upravit toocapture data ze služby výchozí hello `EventSource` třídy a odeslání dat službě Statistika tooApplication.</span><span class="sxs-lookup"><span data-stu-id="4129e-154">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class and send data tooApplication Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="4129e-155">Předpokládáme, že jste obeznámeni s **Azure Application Insights** služby a zda máte prostředek Application Insights, abyste naplánovali toouse toomonitor služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4129e-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan toouse toomonitor your Service Fabric service.</span></span> <span data-ttu-id="4129e-156">Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="4129e-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="4129e-157">Otevřete hello `eventFlowConfig.json` souboru v editoru hello a změňte jeho obsah, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4129e-157">Open hello `eventFlowConfig.json` file in hello editor and change its content as shown below.</span></span> <span data-ttu-id="4129e-158">Ujistěte se, že tooreplace hello ServiceEventSource název a klíč instrumentace Application Insights podle toocomments.</span><span class="sxs-lookup"><span data-stu-id="4129e-158">Make sure tooreplace hello ServiceEventSource name and Application Insights instrumentation key according toocomments.</span></span> 

```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace hello following value with your service's ServiceEventSource name)
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
      // (replace hello following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

> [!NOTE]
> <span data-ttu-id="4129e-159">název služby ServiceEventSource Hello je hodnota hello hello vlastnost názvu hello `EventSourceAttribute` použít toohello ServiceEventSource třídy.</span><span class="sxs-lookup"><span data-stu-id="4129e-159">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="4129e-160">Všechny se vyskytuje v hello `ServiceEventSource.cs` souboru, který je součástí kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="4129e-160">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="4129e-161">Například v hello je fragment kódu následující hello název hello ServiceEventSource *Moje_firma. Application1 Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="4129e-161">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="4129e-162">Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4129e-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="4129e-163">Soubor toothis změny můžou být součástí úplné nebo konfigurace jen upgrady hello služby, předmět tooService Fabric upgrade kontroly stavu a automatického vrácení zpět dojde-li k selhání při upgradu.</span><span class="sxs-lookup"><span data-stu-id="4129e-163">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="4129e-164">Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="4129e-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="4129e-165">posledním krokem Hello je tooinstantiate EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="4129e-165">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="4129e-166">V hello jsou označené těmito přídavky EventFlow související příklad komentáře počínaje `****`:</span><span class="sxs-lookup"><span data-stu-id="4129e-166">In hello following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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
        /// This is hello entry point of hello service host process.
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

<span data-ttu-id="4129e-167">Název Hello předány jako parametr hello hello `CreatePipeline` metoda hello `ServiceFabricDiagnosticsPipelineFactory` je název hello hello *stavu entity* představující hello EventFlow protokolu kolekce kanálu.</span><span class="sxs-lookup"><span data-stu-id="4129e-167">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="4129e-168">Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím hello subsystému stavu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4129e-168">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="4129e-169">Ověření</span><span class="sxs-lookup"><span data-stu-id="4129e-169">Verification</span></span>
<span data-ttu-id="4129e-170">Spuštění služby a sledovat hello ladění výstup – okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4129e-170">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="4129e-171">Jakmile je spuštěna služba hello, měli byste začít zobrazuje důkaz, že vaše služba odesílá záznamů "Telemetrii Application Insights".</span><span class="sxs-lookup"><span data-stu-id="4129e-171">After hello service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="4129e-172">Otevřete webový prohlížeč a přejděte přejděte tooyour prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4129e-172">Open a web browser and navigate go tooyour Application Insights resource.</span></span> <span data-ttu-id="4129e-173">Otevřete kartu "Vyhledat" (v hello horní části okna "Přehled" výchozí hello).</span><span class="sxs-lookup"><span data-stu-id="4129e-173">Open "Search" tab (at hello top of hello default "Overview" blade).</span></span> <span data-ttu-id="4129e-174">Po chvíli trvat, byste měli začít zobrazuje vaše trasování hello Application Insights portálu:</span><span class="sxs-lookup"><span data-stu-id="4129e-174">After a short delay you should start seeing your traces in hello Application Insights portal:</span></span>

![Aplikace portálu Statistika zobrazující protokoly z aplikace Service Fabric][2]

## <a name="next-steps"></a><span data-ttu-id="4129e-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4129e-176">Next steps</span></span>
* [<span data-ttu-id="4129e-177">Další informace o diagnostice a monitorování služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4129e-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="4129e-178">EventFlow dokumentace</span><span class="sxs-lookup"><span data-stu-id="4129e-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
