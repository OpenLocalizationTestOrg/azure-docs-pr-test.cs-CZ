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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Shromažďovat protokoly přímo ze procesu služby Azure Service Fabric
## <a name="in-process-log-collection"></a>Proces shromažďování protokolů
Aplikace pro shromažďování protokolů pomocí [rozšíření Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) je vhodný pro **Azure Service Fabric** služby pokud sada protokolu zdroje a cíle je malý, nemění často a je přehledné mapování mezi zdroji a cíli. Pokud ne, alternativou je tak, aby měl služby odeslat protokoly přímo do centrálního umístění. Tento proces se označuje jako **shromáždění protokolů v rámci procesu** a má několik potenciálních výhod:

* *Jednoduchá konfigurace a nasazení*

    * Konfigurace shromažďování diagnostických dat je právě součástí konfigurace služby. Je snadné vždy synchronizujte jej "" s zbývající aplikace.
    * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.
        * Kolekce založené na agentovi protokolu obvykle vyžaduje samostatné nasazení a konfigurace diagnostiky agenta, který je velmi Správce úloh a potenciální příčinu chyby. Často existuje pouze jedna instance agenta povolena na virtuální počítač (uzel) a konfigurace agenta je sdílen mezi všechny aplikace a služby spuštěné v tomto uzlu. 

* *Flexibilita*
   
    * Aplikace může posílat data, bez ohledu na účelu, také je zde knihovna klienta, který podporuje systém cílových dat úložiště. Nový port nelze přidat podle potřeby.
    * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.
    * Kolekce založené na agentovi protokolu je často omezena jímky dat, které podporuje agenta. Někteří agenti jsou extensible.

* *Přístup k datům interní aplikace a kontext*
   
    * Subsystém diagnostiky běžících v rámci procesu aplikace nebo služba může snadno posílení trasování s kontextové informace.
    * S kolekce založené na agentovi protokolu musí být data zaslány agentovi prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows. Tento mechanismus může použít další omezení.

Je možné kombinovat a těžit z obou metod kolekce. Ve skutečnosti může být nejlepším řešením pro mnoho aplikací. Kolekce založené na agentovi je přirozené řešení pro shromažďování protokolů týkající se celý cluster a jednotlivé uzly clusteru. Je mnohem větší spolehlivost způsobem, než v procesu protokolu kolekce k diagnostikování problémů spuštění služby a dojde k chybě. Navíc s mnoha služeb běžících v rámci clusteru Service Fabric, každá služba provádění svou vlastní kolekci v procesu protokolu výsledkem mnoha odchozí připojení z clusteru. Velký počet odchozích připojení je zdanění pro subsystém sítě i pro cíl protokolu. Agent například [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) můžete shromažďovat data z více služeb a odeslat veškerá data prostřednictvím několik připojení, zvýšení propustnosti. 

V tomto článku ukážeme, jak nastavit kolekce v procesu protokolu pomocí [ **knihovny open-source EventFlow**](https://github.com/Azure/diagnostics-eventflow). K tomuto účelu může používat další knihovny, ale EventFlow má výhodu s byly navrženy speciálně pro kolekce v procesu protokolu a k podpoře služby Service Fabric. Používáme [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) jako cíl protokolu. Jiným cílům, jako [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) nebo [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) jsou také podporovány. Je právě dotaz odpovídající balíček NuGet instalaci a konfiguraci cílového v konfiguračním souboru EventFlow. Další informace o protokolu cíle než Application Insights, najdete v části [EventFlow dokumentaci](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a>Přidávání EventFlow knihovny do projektu služby Service Fabric
Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet. Pokud chcete přidat do projektu služby Service Fabric EventFlow, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte "Balíčky NuGet spravovat". Přejděte na kartu "Vyhledat" a vyhledejte "`Diagnostics.EventFlow`":

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet][1]

Služby hostování EventFlow by měla obsahovat příslušné balíčky v závislosti na zdroj a cíl pro protokoly aplikací. Přidejte následující balíčky: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (k zaznamenání dat od služby EventSource – třída a od standardní EventSources například *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (přidáme poslat protokoly prostředek Azure Application Insights)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (umožňuje inicializaci kanálu EventFlow z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`balíček vyžaduje projekt služby, který má cílové rozhraní .NET Framework 4.6 nebo novější. Ujistěte se, že nastavíte příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku. 

Po instalaci všech balíčků, dalším krokem je ke konfiguraci a povolení EventFlow ve službě.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurace a povolení shromažďování protokolů
EventFlow kanálu, která je odpovědná za zasílání protokolů, je vytvořený z specifikaci uložené v konfiguračním souboru. `Microsoft.Diagnostics.EventFlow.ServiceFabric`balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složce řešení. Název souboru je `eventFlowConfig.json`. Tento konfigurační soubor je potřeba upravit tak, aby zaznamenání dat z výchozí služba `EventSource` třídy a posílat data do služby Application Insights.

> [!NOTE]
> Předpokládáme, že jste obeznámeni s **Azure Application Insights** služby a zda máte prostředek Application Insights, který chcete použít k monitorování služby Service Fabric. Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).

Otevřete `eventFlowConfig.json` souboru v editoru a změňte jeho obsah, jak je uvedeno níže. Nezapomeňte nahradit název ServiceEventSource a klíč instrumentace Application Insights podle komentáře. 

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
> Název služby ServiceEventSource je hodnota vlastnosti název `EventSourceAttribute` třídy ServiceEventSource použít. Všechny se vyskytuje v `ServiceEventSource.cs` souboru, který je součástí kódu služby. Například v následující fragment kódu je název ServiceEventSource *Moje_firma. Application1 Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace. Změny tohoto souboru můžou být součástí úplné nebo konfigurace jen upgrady služby, podstoupí kontroly upgradu stavu Service Fabric a automatického vrácení zpět, dojde-li k selhání při upgradu. Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).

Posledním krokem je vytvoření instance EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru. V následujícím příkladu jsou označené související EventFlow přidání komentáře počínaje `****`:

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

Název předány jako parametr `CreatePipeline` metodu `ServiceFabricDiagnosticsPipelineFactory` je název *stavu entity* představující kanálu EventFlow protokolu kolekce. Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím subsystém stavu Service Fabric.

## <a name="verification"></a>Ověření
Spuštění služby a sledovat ladění výstup – okno v sadě Visual Studio. Po spuštění služby, měli byste začít zobrazuje důkaz, že vaše služba odesílá záznamů "Telemetrii Application Insights". Otevřete webový prohlížeč a přejděte přejděte do zdroje Application Insights. Otevřete kartu "Vyhledat" (v horní části okna výchozí "Přehled"). Po chvíli trvat, byste měli začít zobrazuje vaše trasování v portálu služby Application Insights:

![Aplikace portálu Statistika zobrazující protokoly z aplikace Service Fabric][2]

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostice a monitorování služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentace](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
