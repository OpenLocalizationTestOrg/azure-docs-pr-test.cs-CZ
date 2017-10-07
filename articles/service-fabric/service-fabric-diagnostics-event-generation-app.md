---
title: "Monitorování služby infrastruktury aplikace úroveň aaaAzure | Microsoft Docs"
description: "Další informace o aplikaci a protokoly a události na úrovni služby použít toomonitor a diagnostikovat Azure Service Fabric clustery."
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
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 4f4da1eaad4b88428eaa3a2100ac25c8a285a727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Aplikace a služby na úrovni generování událostí a protokolů

## <a name="instrumenting-hello-code-with-custom-events"></a>Instrumentace hello kódu s použitím vlastních událostí

Instrumentace hello kód je základem hello nejvíce dalších aspektů vaší služby monitorování. Instrumentace je hello jediným způsobem, jak můžete víte, že je něco špatně a toodiagnose toho, co je toobe pevné. Přestože je technicky možné tooconnect služby produkční tooa ladicí program, není běžnou praxi. Nutnosti podrobně data instrumentace je tedy důležité.

Některé produkty, automaticky instrumentace vašeho kódu. I když tato řešení můžete dobře fungovat, je vyžadováno téměř vždy ruční instrumentace. V hello end musí mít dostatek tooforensically informace ladění aplikace hello. Tento dokument popisuje různý přístup tooinstrumenting kódu, a když toochoose jeden přístupu oproti jinému.

## <a name="eventsource"></a>EventSource
Když vytvoříte řešení Service Fabric ze šablony v sadě Visual Studio **EventSource**-odvozené třídy (**ServiceEventSource** nebo **ActorEventSource**) se vygeneruje. Je vytvořit šablonu, ve kterém můžete přidat události pro vaše aplikace nebo služba. Hello **EventSource** název **musí** být jedinečný a musí být přejmenován ze hello výchozí šablony řetězec Moje_firma -&lt;řešení&gt; - &lt; projekt&gt;. S více **EventSource** hello definice, které používají stejný název příčiny problému na dobu běhu. Každé definované události, musí mít jedinečný identifikátor. Pokud není jedinečný identifikátor, dojde k selhání běhového prostředí. Některé organizace preassign rozsahy hodnoty pro identifikátory tooavoid konflikty mezi samostatnou vývojové týmy. Další informace najdete v tématu [hovou na blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) nebo hello [dokumentace MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Používání strukturovaného událostí EventSource

Každá z hello událostí v hello příklady kódu v této části jsou definovány pro konkrétní případ, například při registraci typu služby. Když definujete zprávy případu použití, data se dá zabalit s textem hello hello chyby a můžete více snadno hledat a filtr na základě hello názvy nebo hodnoty hello zadané vlastnosti. Strukturování výstup instrumentace hello umožňuje snazší tooconsume, ale vyžaduje další myšlenku a čas toodefine novou událost pro každý případ použití. Některé události definice mohlo sdílet víc hello celou aplikaci. Například metoda spuštění nebo zastavení by být událostí opětovně použít napříč mnoha služeb v rámci aplikace. Může mít specifické pro doménu služby, jako jsou systému pořadí **CreateOrder** událost, která má svůj vlastní jedinečný událostí. Tento přístup může generovat mnoho událostí a potenciálně vyžadují koordinaci identifikátory v rámci projektové týmy. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello instance constructor is private tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // hello ServiceTypeRegistered event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // hello ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Pomocí EventSource obecně

Protože definování určité události může být složité, definujte Spousta lidí několik událostí s společnou sadu parametrů, které obvykle výstup své informace jako řetězec. Velká část hello strukturovaná aspekt dojde ke ztrátě, a je obtížnější toosearch a filtrování výsledků hello. V tomto přístupu jsou definovány několik události, které obvykle odpovídají úrovně protokolování toohello. Následující fragment kódu Hello definuje ladění a chybové zprávy:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello Instance constructor is private, tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Pomocí hybridní strukturovaných a obecná instrumentace můžete použít také. Strukturované instrumentace se používá pro vytváření sestav chyb a metriky. Obecné události lze použít pro hello podrobné protokolování, který je spotřebovávají techniky pro řešení potíží.

## <a name="aspnet-core-logging"></a>ASP.NET Core protokolování

Důležité toocarefully naplánujte, jak bude instrumentace vašeho kódu. plán správné instrumentace Hello vám může pomoct vyhnout potenciálně destabilizing základní kódu a pak nutnosti tooreinstrument hello kódu. riziko tooreduce, můžete knihovna nástrojů, jako je [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), který je součástí Microsoft ASP.NET Core. ASP.NET Core má [objektu ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) rozhraní, které můžete použít u poskytovatele hello podle vaší volby, a současně minimalizujete její hello vliv na existující kód. Můžete použít kód hello v ASP.NET Core v systému Windows a Linux a v hello úplné rozhraní .NET Framework, proto je standardizovaný kód instrumentace. To je dále zkoumat níže:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Použití Microsoft.Extensions.Logging v Service Fabric

1. Přidejte hello projektu toohello balíček Microsoft.Extensions.Logging NuGet chcete tooinstrument. Navíc přidat všechny balíčky zprostředkovatele (pro balíček třetích stran, viz následující ukázka hello). Další informace najdete v tématu [protokolování v ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Přidat **pomocí** direktivy pro soubor Microsoft.Extensions.Logging tooyour služby.
3. Definování soukromé proměnné v třídě služby.

  ```csharp
  private ILogger _logger = null;

  ```
4. V konstruktoru hello vaší třídy služeb přidejte tento kód:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Spuštění kódu v vaše metody instrumentace. Tady je několik ukázky:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Použití dalších zprostředkovatelů protokolování

Někteří poskytovatelé třetích stran používají hello metody uvedené v předcházející části hello včetně [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), a [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Každá z těchto do ASP.NET Core protokolování je možné připojit nebo je můžete použít samostatně. Serilog obsahuje funkce, která vylepšuje všech zpráv odeslaných z protokolovacího nástroje. Tato funkce může být užitečné toooutput hello služby název, typ a informace o oddílu. toouse tuto funkci hello ASP.NET základní infrastruktury, proveďte tyto kroky:

1. Přidejte hello Serilog, Serilog.Extensions.Logging, a balíčky Serilog.Sinks.Observable NuGet toohello projektu. Například hello další také přidáte Serilog.Sinks.Literate. Lepší přístup se zobrazí později v tomto článku.
2. V Serilog vytvořte instanci LoggerConfiguration a hello protokolovacího nástroje.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Přidejte konstruktor Serilog.ILogger argument toohello služby a předejte hello nově vytvořený protokolovacího nástroje.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. V konstruktoru hello služby, přidejte následující kód, který vytvoří hello enrichers vlastnost pro hello hello **ServiceTypeName**, **ServiceName**, **PartitionId**a  **Identifikátor InstanceId** vlastnosti služby hello. Také přidá vlastnost enricher toohello protokolování ASP.NET Core objekt pro vytváření, abyste je mohli používat Microsoft.Extensions.Logging.ILogger ve vašem kódu.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Kód hello nástrojích hello stejné, jako kdyby byly pomocí ASP.NET Core bez Serilog.

  >[!NOTE]
  >Doporučujeme vám, že nepoužíváte hello statické Log.Logger s předchozím příkladu hello. Service Fabric může být hostitelem více instancí z hello stejný typ v rámci jednoho procesu služby. Pokud používáte hello statické Log.Logger, hello poslední zapisovač systému hello vlastnost enrichers zobrazí hodnoty pro všechny instance, které jsou spuštěné. Toto je jedním z důvodů, proč hello _logger proměnné je proměnná privátního člena třídy služeb, hello. Navíc musíte nastavit hello _logger dostupné toocommon kód, který může být použit ve službách.

## <a name="choosing-a-logging-provider"></a>Výběr zprostředkovatele protokolování

Pokud vaše aplikace využívá vysoký výkon, **EventSource** je obvykle dobrou přístup. **EventSource** *obecně* využívá méně prostředků a provede lépe než ASP.NET Core protokolování nebo některého z hello k dispozici řešení třetí strany.  Tato akce není problém pro mnoho služby, ale pokud je vaše služba orientovaných na výkon, pomocí **EventSource** může být vhodnější. Ale tooget tyto výhody strukturovaná protokolování, **EventSource** vyžaduje větší investice z technickému týmu. Pokud je to možné, proveďte rychlé prototyp několik možností protokolování a poté zvolte hello ten, který nejlépe vyhovuje vašim potřebám.

## <a name="next-steps"></a>Další kroky

Po zvolení vaší tooinstrument zprostředkovatele protokolování vašim aplikacím a službám, protokolů a událostí třeba toobe agregovat před jejich odesláním tooany analysis platformy. Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter pochopit hello doporučené možnosti.
