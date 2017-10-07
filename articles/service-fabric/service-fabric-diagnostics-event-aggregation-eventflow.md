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
# <a name="event-aggregation-and-collection-using-eventflow"></a>Seskupení událostí a kolekce pomocí EventFlow

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) může směrovat události z uzlu tooone nebo další monitorování cílů. Vzhledem k tomu, že je zahrnut jako balíčku NuGet ve vašem projektu service, EventFlow kódu a konfigurace přenosu službou hello odstraňuje problém konfigurace podle uzlu hello již bylo zmíněno dříve o Azure Diagnostics. EventFlow běží v procesu služby a připojuje přímo výstupy toohello nakonfigurované. Z důvodu hello přímé připojení, EventFlow funguje pro Azure, kontejneru a nasazení služby v místě. Dávejte pozor, pokud spustíte EventFlow v podobě výkonných scénáře, jako třeba v kontejneru, protože každý kanál EventFlow vytvoří externí připojení. Takže pokud hostujete několik procesy, můžete načítat několik odchozí připojení! Tato akce není tolik důležité aplikace Service Fabric, protože všechny repliky `ServiceType` spustit v hello stejný proces, a to omezuje počet hello odchozí připojení. EventFlow také nabízí filtrování událostí tak, aby se odesílají pouze hello události, které odpovídají hello zadaný filtr.

## <a name="setting-up-eventflow"></a>Nastavení EventFlow

Binární soubory EventFlow jsou k dispozici jako sada balíčků NuGet. tooadd EventFlow tooa Service Fabric služby projektu, klikněte pravým tlačítkem na projekt hello v hello Průzkumníku řešení a zvolte "Balíčky NuGet spravovat". Přepínač toohello "Browse" kartě a vyhledejte "`Diagnostics.EventFlow`":

![Balíčky EventFlow NuGet v uživatelské rozhraní Správce balíčků Visual Studio NuGet](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

Zobrazí se seznam různých balíčků objeví, s popiskem "Vstup" a "Výstupy". EventFlow podporuje různé zprostředkovatelé různých protokolování a analyzátory. hostování EventFlow Hello služby by měla obsahovat příslušné balíčky v závislosti na hello zdroj a cíl pro protokoly aplikací hello. Kromě toohello základní ServiceFabric balíček, je také nutné mít alespoň jeden vstup a výstup nakonfigurované. Pro exmaple můžete přidat hello následující balíčky toosent EventSource události tooApplication statistiky:

* `Microsoft.Diagnostics.EventFlow.Input.EventSource`toocapture data ze služby hello EventSource – třída a z standardní EventSources jako *služby společnosti Microsoft ServiceFabric* a *Microsoft-ServiceFabric-aktéři*)
* `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(přidáme prostředek služby Azure Application Insights toosend hello protokoly tooan)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(umožňuje inicializaci kanálu EventFlow hello z konfigurace služby Service Fabric a ohlásí případné potíže s odesílání diagnostických dat jako sestav stavu Service Fabric)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Input.EventSource`balíček vyžaduje hello služby projektu tootarget rozhraní .NET Framework 4.6 nebo novější. Ujistěte se, že nastavíte hello příslušné cílové rozhraní v okně Vlastnosti projektu před instalací tohoto balíčku.

Po všech hello balíčky jsou nainstalovány, hello dalším krokem je tooconfigure a povolte EventFlow ve službě hello.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurace a povolení shromažďování protokolů
kanál EventFlow Hello odpovědná za zasílání protokolů hello je vytvořený z specifikaci uložené v konfiguračním souboru. Hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` balíček nainstaluje výchozí konfigurační soubor EventFlow pod `PackageRoot\Config` složku řešení s názvem `eventFlowConfig.json`. Tento konfigurační soubor vyžaduje toobe upravit toocapture data ze služby výchozí hello `EventSource` třídy a všechny ostatní vstupy chcete tooconfigure a odesílat data toohello příslušné místo.

Zde je ukázka *eventFlowConfig.json* podle hello balíčků NuGet uvedených výše:
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

název služby ServiceEventSource Hello je hodnota hello hello vlastnost názvu hello `EventSourceAttribute` použít toohello ServiceEventSource třídy. Všechny se vyskytuje v hello `ServiceEventSource.cs` souboru, který je součástí kódu služby hello. Například v hello je fragment kódu následující hello název hello ServiceEventSource *Moje_firma. Application1 Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Všimněte si, že `eventFlowConfig.json` soubor je součástí balíčku služby konfigurace. Soubor toothis změny můžou být součástí úplné nebo konfigurace jen upgrady hello služby, předmět tooService Fabric upgrade kontroly stavu a automatického vrácení zpět dojde-li k selhání při upgradu. Další informace najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).

Hello *filtry* část konfigurace hello vám umožní toofurther přizpůsobit hello informace, které se má toogo prostřednictvím hello EventFlow kanálu toohello výstupů, což vám toodrop obsahovat určité informace nebo změnit hello Struktura dat událostí hello. Další informace o filtrování najdete v tématu [EventFlow filtry](https://github.com/Azure/diagnostics-eventflow#filters).

posledním krokem Hello je tooinstantiate EventFlow kanálu v kódu spuštění vaší služby, umístěný v `Program.cs` souboru:

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

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a>Pomocí nastavení Service Fabric a eventFlowConfig tooin parametry aplikace

Podporuje EventFlow pomocí Service Fabric nastavení a nastavení EventFlow tooconfigure parametry aplikace. Můžete odkazovat pomocí této syntaxe speciální hodnoty parametrů pro nastavení tooService prostředků infrastruktury:

```json
servicefabric:/<section-name>/<setting-name>
``` 

`<section-name>`je název hello hello Service Fabric konfigurační oddíl, a `<setting-name>` je nastavení konfigurace hello poskytování hello hodnotu, která bude použité tooconfigure na EventFlow nastavení. tooread více o tom, toodo se přejděte příliš[podporu pro Service Fabric nastavení a aplikace parametry](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Ověření

Spuštění služby a sledovat hello ladění výstup – okno v sadě Visual Studio. Jakmile je spuštěna služba hello, měli byste začít zobrazuje, že důkaz, že vaše služba odesílá zaznamenává toohello výstupu, který jste nakonfigurovali. Přejděte tooyour platformy analýzy a vizualizace událostí a zkontrolujte, zda protokoly spustili tooshow nahoru (může trvat několik minut).

## <a name="next-steps"></a>Další kroky

* [Analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analýza události a vizualizace s OMS](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow dokumentace](https://github.com/Azure/diagnostics-eventflow)