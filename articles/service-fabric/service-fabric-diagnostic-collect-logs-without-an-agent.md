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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Shromažďovat protokoly přímo ze procesu služby Azure Service Fabric
## <a name="in-process-log-collection"></a>Proces shromažďování protokolů
Aplikace pro shromažďování protokolů pomocí [rozšíření Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) je vhodný pro **Azure Service Fabric** služby pokud hello sadu protokolu zdroje a cíle je malý, nemění často a existuje je přehledné mapování mezi zdroji hello a jejich cíle. Pokud ne, alternativou je, že služby toohave odeslat protokoly přímo tooa centrálního umístění. Tento proces se označuje jako **shromáždění protokolů v rámci procesu** a má několik potenciálních výhod:

* *Jednoduchá konfigurace a nasazení*

    * Konfigurace Hello shromažďování diagnostických dat je právě součástí konfigurace služby hello. Je snadno tooalways zachovat ji "synchronizována" s hello rest aplikace hello.
    * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.
        * Kolekce založené na agentovi protokolu obvykle vyžaduje samostatné nasazení a konfiguraci diagnostiky agenta hello, což je velmi Správce úloh a potenciální příčinu chyby. Často existuje pouze jedna instance hello agenta povolena na virtuální počítač (uzel) a konfigurace agenta hello je sdílen mezi všechny aplikace a služby spuštěné v tomto uzlu. 

* *Flexibilita*
   
    * Hello aplikace může odesílat hello data bez ohledu na potřebuje toogo, dokud není klientské knihovny, která podporuje systém úložiště dat hello cílové. Nový port nelze přidat podle potřeby.
    * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.
    * Kolekce založené na agentovi protokolu je často omezena jímky hello dat, které hello agent podporuje. Někteří agenti jsou extensible.

* *Data aplikací toointernal přístup a kontext*
   
    * diagnostické subsystému Hello běžících v rámci procesu aplikace nebo služby hello můžete snadno posílení hello trasování s kontextové informace.
    * S kolekce založené na agentovi protokolu hello data musí být odeslána tooan agent prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows. Tento mechanismus může použít další omezení.

Je možné toocombine a benefit z obou metod kolekce. Ve skutečnosti může být hello nejlepší řešení pro mnoho aplikací. Kolekce založené na agentovi je přirozené řešení pro shromažďování protokolů souvisejících toohello celý cluster a jednotlivé uzly clusteru. Je mnohem větší spolehlivost způsobem, než kolekce protokolu v procesu, potíže se spouštěním služby toodiagnose a dojde k chybě. Navíc s mnoha služeb běžících v rámci clusteru Service Fabric, každá služba provádění svou vlastní kolekci v procesu protokolu výsledkem mnoha odchozí připojení z clusteru hello. Velký počet odchozích připojení je zdanění pro subsystém hello sítě i pro cíl protokolu hello. Agent například [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) můžete shromažďovat data z více služeb a odeslat veškerá data prostřednictvím několik připojení, zvýšení propustnosti. 

V tomto článku jsme ukazují, jak tooset nahoru v procesu protokolu pomocí kolekce [ **knihovny open-source EventFlow**](https://github.com/Azure/diagnostics-eventflow). Může používat další knihovny pro hello stejný účel, ale EventFlow má výhodu hello s byl určený speciálně pro v procesu protokolu kolekce a toosupport Service Fabric služby. Používáme [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) jako cíl protokolu hello. Jiným cílům, jako [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) nebo [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) jsou také podporovány. Je právě dotaz odpovídající balíček NuGet instalaci a konfiguraci cílového hello ve hello EventFlow konfigurační soubor. Další informace o protokolu cíle než Application Insights, najdete v části [EventFlow dokumentaci](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a>Přidání projektu služby EventFlow knihovny tooa Service Fabric
Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet. tooadd EventFlow tooa Service Fabric služby projektu, klikněte pravým tlačítkem na projekt hello v hello Průzkumníku řešení a zvolte "Balíčky NuGet spravovat". Přepínač toohello "Browse" kartě a vyhledejte "`Diagnostics.EventFlow`":

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet][1]

hostování EventFlow Hello služby by měla obsahovat příslušné balíčky v závislosti na hello zdroj a cíl pro protokoly aplikací hello. Přidejte hello následující balíčky: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (toocapture data ze služby hello EventSource – třída a z standardní EventSources jako *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (přidáme prostředek služby Azure Application Insights toosend hello protokoly tooan)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (umožňuje inicializaci kanálu EventFlow hello z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`balíček vyžaduje hello služby projektu tootarget rozhraní .NET Framework 4.6 nebo novější. Ujistěte se, že nastavíte hello příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku. 

Po všech hello balíčky jsou nainstalovány, hello dalším krokem je tooconfigure a povolte EventFlow ve službě hello.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurace a povolení shromažďování protokolů
EventFlow kanálu, která je odpovědná za zasílání protokolů hello je vytvořený z specifikaci uložené v konfiguračním souboru. `Microsoft.Diagnostics.EventFlow.ServiceFabric`balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složce řešení. Název souboru Hello je `eventFlowConfig.json`. Tento konfigurační soubor vyžaduje toobe upravit toocapture data ze služby výchozí hello `EventSource` třídy a odeslání dat službě Statistika tooApplication.

> [!NOTE]
> Předpokládáme, že jste obeznámeni s **Azure Application Insights** služby a zda máte prostředek Application Insights, abyste naplánovali toouse toomonitor služby Service Fabric. Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).

Otevřete hello `eventFlowConfig.json` souboru v editoru hello a změňte jeho obsah, jak je uvedeno níže. Ujistěte se, že tooreplace hello ServiceEventSource název a klíč instrumentace Application Insights podle toocomments. 

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
> název služby ServiceEventSource Hello je hodnota hello hello vlastnost názvu hello `EventSourceAttribute` použít toohello ServiceEventSource třídy. Všechny se vyskytuje v hello `ServiceEventSource.cs` souboru, který je součástí kódu služby hello. Například v hello je fragment kódu následující hello název hello ServiceEventSource *Moje_firma. Application1 Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace. Soubor toothis změny můžou být součástí úplné nebo konfigurace jen upgrady hello služby, předmět tooService Fabric upgrade kontroly stavu a automatického vrácení zpět dojde-li k selhání při upgradu. Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).

posledním krokem Hello je tooinstantiate EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru. V hello jsou označené těmito přídavky EventFlow související příklad komentáře počínaje `****`:

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

Název Hello předány jako parametr hello hello `CreatePipeline` metoda hello `ServiceFabricDiagnosticsPipelineFactory` je název hello hello *stavu entity* představující hello EventFlow protokolu kolekce kanálu. Tento název se používá, pokud EventFlow zaznamená a chybu a sestavy prostřednictvím hello subsystému stavu Service Fabric.

## <a name="verification"></a>Ověření
Spuštění služby a sledovat hello ladění výstup – okno v sadě Visual Studio. Jakmile je spuštěna služba hello, měli byste začít zobrazuje důkaz, že vaše služba odesílá záznamů "Telemetrii Application Insights". Otevřete webový prohlížeč a přejděte přejděte tooyour prostředek Application Insights. Otevřete kartu "Vyhledat" (v hello horní části okna "Přehled" výchozí hello). Po chvíli trvat, byste měli začít zobrazuje vaše trasování hello Application Insights portálu:

![Aplikace portálu Statistika zobrazující protokoly z aplikace Service Fabric][2]

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostice a monitorování služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentace](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
