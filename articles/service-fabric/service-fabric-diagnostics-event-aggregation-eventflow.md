---
title: "aaaAzure Service Fabric událostí agregace s EventFlow | Microsoft Docs"
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
ms.openlocfilehash: c0141d3ed72d835139250af3589e298fd22d8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a><span data-ttu-id="89386-103">Seskupení událostí a kolekce pomocí EventFlow</span><span class="sxs-lookup"><span data-stu-id="89386-103">Event aggregation and collection using EventFlow</span></span>

<span data-ttu-id="89386-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) může směrovat události z uzlu tooone nebo další monitorování cílů.</span><span class="sxs-lookup"><span data-stu-id="89386-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) can route events from a node tooone or more monitoring destinations.</span></span> <span data-ttu-id="89386-105">Vzhledem k tomu, že je zahrnut jako balíčku NuGet ve vašem projektu service, EventFlow kódu a konfigurace přenosu službou hello odstraňuje problém konfigurace podle uzlu hello již bylo zmíněno dříve o Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="89386-105">Because it is included as a NuGet package in your service project, EventFlow code and configuration travel with hello service, eliminating hello per-node configuration issue mentioned earlier about Azure Diagnostics.</span></span> <span data-ttu-id="89386-106">EventFlow běží v procesu služby a připojuje přímo výstupy toohello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="89386-106">EventFlow runs within your service process, and connects directly toohello configured outputs.</span></span> <span data-ttu-id="89386-107">Z důvodu hello přímé připojení, EventFlow funguje pro Azure, kontejneru a nasazení služby v místě.</span><span class="sxs-lookup"><span data-stu-id="89386-107">Because of hello direct connection, EventFlow works for Azure, container, and on-premises service deployments.</span></span> <span data-ttu-id="89386-108">Dávejte pozor, pokud spustíte EventFlow v podobě výkonných scénáře, jako třeba v kontejneru, protože každý kanál EventFlow vytvoří externí připojení.</span><span class="sxs-lookup"><span data-stu-id="89386-108">Be careful if you run EventFlow in high-density scenarios, such as in a container, because each EventFlow pipeline makes an external connection.</span></span> <span data-ttu-id="89386-109">Takže pokud hostujete několik procesy, můžete načítat několik odchozí připojení!</span><span class="sxs-lookup"><span data-stu-id="89386-109">So, if you host several processes, you get several outbound connections!</span></span> <span data-ttu-id="89386-110">Tato akce není tolik důležité aplikace Service Fabric, protože všechny repliky `ServiceType` spustit v hello stejný proces, a to omezuje počet hello odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="89386-110">This isn't as much a concern for Service Fabric applications, because all replicas of a `ServiceType` run in hello same process, and this limits hello number of outbound connections.</span></span> <span data-ttu-id="89386-111">EventFlow také nabízí filtrování událostí tak, aby se odesílají pouze hello události, které odpovídají hello zadaný filtr.</span><span class="sxs-lookup"><span data-stu-id="89386-111">EventFlow also offers event filtering, so that only hello events that match hello specified filter are sent.</span></span>

## <a name="setting-up-eventflow"></a><span data-ttu-id="89386-112">Nastavení EventFlow</span><span class="sxs-lookup"><span data-stu-id="89386-112">Setting up EventFlow</span></span>

<span data-ttu-id="89386-113">Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="89386-113">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="89386-114">tooadd EventFlow tooa Service Fabric služby projektu, klikněte pravým tlačítkem na projekt hello v hello Průzkumníku řešení a zvolte "Balíčky NuGet spravovat".</span><span class="sxs-lookup"><span data-stu-id="89386-114">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="89386-115">Přepínač toohello "Browse" kartě a vyhledejte "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="89386-115">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

<span data-ttu-id="89386-117">Zobrazí se seznam různých balíčků objeví, s popiskem "Vstup" a "Výstupy".</span><span class="sxs-lookup"><span data-stu-id="89386-117">You will see a list of various packages show up, labeled with "Inputs" and "Outputs".</span></span> <span data-ttu-id="89386-118">EventFlow podporuje různé zprostředkovatelé různých protokolování a analyzátory.</span><span class="sxs-lookup"><span data-stu-id="89386-118">EventFlow supports various different logging providers and analyzers.</span></span> <span data-ttu-id="89386-119">hostování EventFlow Hello služby by měla obsahovat příslušné balíčky v závislosti na hello zdroj a cíl pro protokoly aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="89386-119">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="89386-120">Kromě toohello základní ServiceFabric balíček, je také nutné mít alespoň jeden vstup a výstup nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="89386-120">In addition toohello core ServiceFabric package, you also need at least one Input and Output configured.</span></span> <span data-ttu-id="89386-121">Pro exmaple můžete přidat hello následující balíčky toosent EventSource události tooApplication statistiky:</span><span class="sxs-lookup"><span data-stu-id="89386-121">For exmaple, you can add hello following packages toosent EventSource events tooApplication Insights:</span></span>

* <span data-ttu-id="89386-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource`toocapture data ze služby hello EventSource – třída a z standardní EventSources jako *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)</span><span class="sxs-lookup"><span data-stu-id="89386-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource` toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* <span data-ttu-id="89386-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(přidáme prostředek služby Azure Application Insights toosend hello protokoly tooan)</span><span class="sxs-lookup"><span data-stu-id="89386-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` (we are going toosend hello logs tooan Azure Application Insights resource)</span></span>
* <span data-ttu-id="89386-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(umožňuje inicializaci kanálu EventFlow hello z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)</span><span class="sxs-lookup"><span data-stu-id="89386-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

>[!NOTE]
><span data-ttu-id="89386-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource`balíček vyžaduje hello služby projektu tootarget rozhraní .NET Framework 4.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="89386-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="89386-126">Ujistěte se, že nastavíte hello příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku.</span><span class="sxs-lookup"><span data-stu-id="89386-126">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span>

<span data-ttu-id="89386-127">Po všech hello balíčky jsou nainstalovány, hello dalším krokem je tooconfigure a povolte EventFlow ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="89386-127">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="89386-128">Konfigurace a povolení shromažďování protokolů</span><span class="sxs-lookup"><span data-stu-id="89386-128">Configuring and enabling log collection</span></span>
<span data-ttu-id="89386-129">kanál EventFlow Hello odpovědná za zasílání protokolů hello je vytvořený z specifikaci uložené v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="89386-129">hello EventFlow pipeline responsible for sending hello logs is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="89386-130">Hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složku řešení s názvem `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="89386-130">hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder, named `eventFlowConfig.json`.</span></span> <span data-ttu-id="89386-131">Tento konfigurační soubor vyžaduje toobe upravit toocapture data ze služby výchozí hello `EventSource` třídy a všechny ostatní vstupy chcete tooconfigure a odesílat data toohello příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="89386-131">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class, and any other inputs you want tooconfigure, and send data toohello appropriate place.</span></span>

<span data-ttu-id="89386-132">Zde je ukázka *eventFlowConfig.json* podle hello balíčků NuGet uvedených výše:</span><span class="sxs-lookup"><span data-stu-id="89386-132">Here is a sample *eventFlowConfig.json* based on hello NuGet packages mentioned above:</span></span>
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

<span data-ttu-id="89386-133">název služby ServiceEventSource Hello je hodnota hello hello vlastnost názvu hello `EventSourceAttribute` použít toohello ServiceEventSource třídy.</span><span class="sxs-lookup"><span data-stu-id="89386-133">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="89386-134">Všechny se vyskytuje v hello `ServiceEventSource.cs` souboru, který je součástí kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="89386-134">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="89386-135">Například v hello je fragment kódu následující hello název hello ServiceEventSource *Moje_firma. Application1 Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="89386-135">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

<span data-ttu-id="89386-136">Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="89386-136">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="89386-137">Soubor toothis změny můžou být součástí úplné nebo konfigurace jen upgrady hello služby, předmět tooService Fabric upgrade kontroly stavu a automatického vrácení zpět dojde-li k selhání při upgradu.</span><span class="sxs-lookup"><span data-stu-id="89386-137">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="89386-138">Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="89386-138">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="89386-139">Hello *filtry* část konfigurace hello vám umožní toofurther přizpůsobit hello informace, které se má toogo prostřednictvím hello EventFlow kanálu toohello výstupů, což vám toodrop obsahovat určité informace nebo změnit hello Struktura dat událostí hello.</span><span class="sxs-lookup"><span data-stu-id="89386-139">hello *filters* section of hello config allows you toofurther customize hello information that is going toogo through hello EventFlow pipeline toohello outputs, allowing you toodrop or include certain information, or change hello structure of hello event data.</span></span> <span data-ttu-id="89386-140">Další informace o filtrování najdete v tématu [EventFlow filtry](https://github.com/Azure/diagnostics-eventflow#filters).</span><span class="sxs-lookup"><span data-stu-id="89386-140">For more information on filtering, see [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).</span></span>

<span data-ttu-id="89386-141">posledním krokem Hello je tooinstantiate EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="89386-141">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file:</span></span>

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

<span data-ttu-id="89386-142">Název Hello předány jako parametr hello hello `CreatePipeline` metoda hello `ServiceFabricDiagnosticsPipelineFactory` je název hello hello *stavu entity* představující hello EventFlow protokolu kolekce kanálu.</span><span class="sxs-lookup"><span data-stu-id="89386-142">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="89386-143">Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím hello subsystému stavu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="89386-143">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a><span data-ttu-id="89386-144">Pomocí nastavení Service Fabric a eventFlowConfig tooin parametry aplikace</span><span class="sxs-lookup"><span data-stu-id="89386-144">Using Service Fabric settings and application parameters tooin eventFlowConfig</span></span>

<span data-ttu-id="89386-145">Podporuje EventFlow pomocí Service Fabric nastavení a nastavení EventFlow tooconfigure parametry aplikace.</span><span class="sxs-lookup"><span data-stu-id="89386-145">EventFlow supports using Service Fabric settings and application paremeters tooconfigure EventFlow settings.</span></span> <span data-ttu-id="89386-146">Můžete odkazovat pomocí této syntaxe speciální hodnoty parametrů pro nastavení tooService prostředků infrastruktury:</span><span class="sxs-lookup"><span data-stu-id="89386-146">You can refer tooService Fabric settings parameters using this special syntax for values:</span></span>

```json
servicefabric:/<section-name>/<setting-name>
``` 

<span data-ttu-id="89386-147">`<section-name>`je název hello hello Service Fabric konfigurační oddíl, a `<setting-name>` je nastavení konfigurace hello poskytování hello hodnotu, která bude použité tooconfigure na EventFlow nastavení.</span><span class="sxs-lookup"><span data-stu-id="89386-147">`<section-name>` is hello name of hello Service Fabric configuration section, and `<setting-name>` is hello configuration setting providing hello value that will be used tooconfigure an EventFlow setting.</span></span> <span data-ttu-id="89386-148">tooread více o tom, toodo se přejděte příliš[podporu pro Service Fabric nastavení a aplikace parametry](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span><span class="sxs-lookup"><span data-stu-id="89386-148">tooread more about how toodo this, go too[Support for Service Fabric settings and application parameters](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span></span>

## <a name="verification"></a><span data-ttu-id="89386-149">Ověření</span><span class="sxs-lookup"><span data-stu-id="89386-149">Verification</span></span>

<span data-ttu-id="89386-150">Spuštění služby a sledovat hello ladění výstup – okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89386-150">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="89386-151">Jakmile je spuštěna služba hello, měli byste začít zobrazuje, že důkaz, že vaše služba odesílá zaznamenává toohello výstupu, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="89386-151">After hello service is started, you should start seeing evidence that your service is sending records toohello output that you have configured.</span></span> <span data-ttu-id="89386-152">Přejděte tooyour platformy analýzy a vizualizace událostí a zkontrolujte, zda protokoly spustili tooshow nahoru (může trvat několik minut).</span><span class="sxs-lookup"><span data-stu-id="89386-152">Navigate tooyour event analysis and visualization platform and confirm that logs have started tooshow up (could take a few minutes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="89386-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89386-153">Next steps</span></span>

* [<span data-ttu-id="89386-154">Analýza události a vizualizace s Application Insights</span><span class="sxs-lookup"><span data-stu-id="89386-154">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="89386-155">Analýza události a vizualizace s OMS</span><span class="sxs-lookup"><span data-stu-id="89386-155">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)
* [<span data-ttu-id="89386-156">EventFlow dokumentace</span><span class="sxs-lookup"><span data-stu-id="89386-156">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)