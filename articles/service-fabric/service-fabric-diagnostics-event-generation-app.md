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
# <a name="application-and-service-level-event-and-log-generation"></a><span data-ttu-id="583a0-103">Aplikace a služby na úrovni generování událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="583a0-103">Application and service level event and log generation</span></span>

## <a name="instrumenting-hello-code-with-custom-events"></a><span data-ttu-id="583a0-104">Instrumentace hello kódu s použitím vlastních událostí</span><span class="sxs-lookup"><span data-stu-id="583a0-104">Instrumenting hello code with custom events</span></span>

<span data-ttu-id="583a0-105">Instrumentace hello kód je základem hello nejvíce dalších aspektů vaší služby monitorování.</span><span class="sxs-lookup"><span data-stu-id="583a0-105">Instrumenting hello code is hello basis for most other aspects of monitoring your services.</span></span> <span data-ttu-id="583a0-106">Instrumentace je hello jediným způsobem, jak můžete víte, že je něco špatně a toodiagnose toho, co je toobe pevné.</span><span class="sxs-lookup"><span data-stu-id="583a0-106">Instrumentation is hello only way you can know that something is wrong, and toodiagnose what needs toobe fixed.</span></span> <span data-ttu-id="583a0-107">Přestože je technicky možné tooconnect služby produkční tooa ladicí program, není běžnou praxi.</span><span class="sxs-lookup"><span data-stu-id="583a0-107">Although technically it's possible tooconnect a debugger tooa production service, it's not a common practice.</span></span> <span data-ttu-id="583a0-108">Nutnosti podrobně data instrumentace je tedy důležité.</span><span class="sxs-lookup"><span data-stu-id="583a0-108">So, having detailed instrumentation data is important.</span></span>

<span data-ttu-id="583a0-109">Některé produkty, automaticky instrumentace vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="583a0-109">Some products automatically instrument your code.</span></span> <span data-ttu-id="583a0-110">I když tato řešení můžete dobře fungovat, je vyžadováno téměř vždy ruční instrumentace.</span><span class="sxs-lookup"><span data-stu-id="583a0-110">Although these solutions can work well, manual instrumentation is almost always required.</span></span> <span data-ttu-id="583a0-111">V hello end musí mít dostatek tooforensically informace ladění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="583a0-111">In hello end, you must have enough information tooforensically debug hello application.</span></span> <span data-ttu-id="583a0-112">Tento dokument popisuje různý přístup tooinstrumenting kódu, a když toochoose jeden přístupu oproti jinému.</span><span class="sxs-lookup"><span data-stu-id="583a0-112">This document describes different approaches tooinstrumenting your code, and when toochoose one approach over another.</span></span>

## <a name="eventsource"></a><span data-ttu-id="583a0-113">EventSource</span><span class="sxs-lookup"><span data-stu-id="583a0-113">EventSource</span></span>
<span data-ttu-id="583a0-114">Když vytvoříte řešení Service Fabric ze šablony v sadě Visual Studio **EventSource**-odvozené třídy (**ServiceEventSource** nebo **ActorEventSource**) se vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="583a0-114">When you create a Service Fabric solution from a template in Visual Studio, an **EventSource**-derived class (**ServiceEventSource** or **ActorEventSource**) is generated.</span></span> <span data-ttu-id="583a0-115">Je vytvořit šablonu, ve kterém můžete přidat události pro vaše aplikace nebo služba.</span><span class="sxs-lookup"><span data-stu-id="583a0-115">A template is created, in which you can add events for your application or service.</span></span> <span data-ttu-id="583a0-116">Hello **EventSource** název **musí** být jedinečný a musí být přejmenován ze hello výchozí šablony řetězec Moje_firma -&lt;řešení&gt; - &lt; projekt&gt;.</span><span class="sxs-lookup"><span data-stu-id="583a0-116">hello **EventSource** name **must** be unique, and should be renamed from hello default template string MyCompany-&lt;solution&gt;-&lt;project&gt;.</span></span> <span data-ttu-id="583a0-117">S více **EventSource** hello definice, které používají stejný název příčiny problému na dobu běhu.</span><span class="sxs-lookup"><span data-stu-id="583a0-117">Having multiple **EventSource** definitions that use hello same name causes an issue at run time.</span></span> <span data-ttu-id="583a0-118">Každé definované události, musí mít jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="583a0-118">Each defined event must have a unique identifier.</span></span> <span data-ttu-id="583a0-119">Pokud není jedinečný identifikátor, dojde k selhání běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="583a0-119">If an identifier is not unique, a runtime failure occurs.</span></span> <span data-ttu-id="583a0-120">Některé organizace preassign rozsahy hodnoty pro identifikátory tooavoid konflikty mezi samostatnou vývojové týmy.</span><span class="sxs-lookup"><span data-stu-id="583a0-120">Some organizations preassign ranges of values for identifiers tooavoid conflicts between separate development teams.</span></span> <span data-ttu-id="583a0-121">Další informace najdete v tématu [hovou na blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) nebo hello [dokumentace MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span><span class="sxs-lookup"><span data-stu-id="583a0-121">For more information, see [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) or hello [MSDN documentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span></span>

### <a name="using-structured-eventsource-events"></a><span data-ttu-id="583a0-122">Používání strukturovaného událostí EventSource</span><span class="sxs-lookup"><span data-stu-id="583a0-122">Using structured EventSource events</span></span>

<span data-ttu-id="583a0-123">Každá z hello událostí v hello příklady kódu v této části jsou definovány pro konkrétní případ, například při registraci typu služby.</span><span class="sxs-lookup"><span data-stu-id="583a0-123">Each of hello events in hello code examples in this section are defined for a specific case, for example, when a service type is registered.</span></span> <span data-ttu-id="583a0-124">Když definujete zprávy případu použití, data se dá zabalit s textem hello hello chyby a můžete více snadno hledat a filtr na základě hello názvy nebo hodnoty hello zadané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="583a0-124">When you define messages by use case, data can be packaged with hello text of hello error, and you can more easily search and filter based on hello names or values of hello specified properties.</span></span> <span data-ttu-id="583a0-125">Strukturování výstup instrumentace hello umožňuje snazší tooconsume, ale vyžaduje další myšlenku a čas toodefine novou událost pro každý případ použití.</span><span class="sxs-lookup"><span data-stu-id="583a0-125">Structuring hello instrumentation output makes it easier tooconsume, but requires more thought and time toodefine a new event for each use case.</span></span> <span data-ttu-id="583a0-126">Některé události definice mohlo sdílet víc hello celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="583a0-126">Some event definitions can be shared across hello entire application.</span></span> <span data-ttu-id="583a0-127">Například metoda spuštění nebo zastavení by být událostí opětovně použít napříč mnoha služeb v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="583a0-127">For example, a method start or stop event would be reused across many services within an application.</span></span> <span data-ttu-id="583a0-128">Může mít specifické pro doménu služby, jako jsou systému pořadí **CreateOrder** událost, která má svůj vlastní jedinečný událostí.</span><span class="sxs-lookup"><span data-stu-id="583a0-128">A domain-specific service, like an order system, might have a **CreateOrder** event, which has its own unique event.</span></span> <span data-ttu-id="583a0-129">Tento přístup může generovat mnoho událostí a potenciálně vyžadují koordinaci identifikátory v rámci projektové týmy.</span><span class="sxs-lookup"><span data-stu-id="583a0-129">This approach might generate many events, and potentially require coordination of identifiers across project teams.</span></span> 

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

### <a name="using-eventsource-generically"></a><span data-ttu-id="583a0-130">Pomocí EventSource obecně</span><span class="sxs-lookup"><span data-stu-id="583a0-130">Using EventSource generically</span></span>

<span data-ttu-id="583a0-131">Protože definování určité události může být složité, definujte Spousta lidí několik událostí s společnou sadu parametrů, které obvykle výstup své informace jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="583a0-131">Because defining specific events can be difficult, many people define a few events with a common set of parameters that generally output their information as a string.</span></span> <span data-ttu-id="583a0-132">Velká část hello strukturovaná aspekt dojde ke ztrátě, a je obtížnější toosearch a filtrování výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="583a0-132">Much of hello structured aspect is lost, and it's more difficult toosearch and filter hello results.</span></span> <span data-ttu-id="583a0-133">V tomto přístupu jsou definovány několik události, které obvykle odpovídají úrovně protokolování toohello.</span><span class="sxs-lookup"><span data-stu-id="583a0-133">In this approach, a few events that usually correspond toohello logging levels are defined.</span></span> <span data-ttu-id="583a0-134">Následující fragment kódu Hello definuje ladění a chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="583a0-134">hello following snippet defines a debug and error message:</span></span>

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

<span data-ttu-id="583a0-135">Pomocí hybridní strukturovaných a obecná instrumentace můžete použít také.</span><span class="sxs-lookup"><span data-stu-id="583a0-135">Using a hybrid of structured and generic instrumentation also can work well.</span></span> <span data-ttu-id="583a0-136">Strukturované instrumentace se používá pro vytváření sestav chyb a metriky.</span><span class="sxs-lookup"><span data-stu-id="583a0-136">Structured instrumentation is used for reporting errors and metrics.</span></span> <span data-ttu-id="583a0-137">Obecné události lze použít pro hello podrobné protokolování, který je spotřebovávají techniky pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="583a0-137">Generic events can be used for hello detailed logging that is consumed by engineers for troubleshooting.</span></span>

## <a name="aspnet-core-logging"></a><span data-ttu-id="583a0-138">ASP.NET Core protokolování</span><span class="sxs-lookup"><span data-stu-id="583a0-138">ASP.NET Core logging</span></span>

<span data-ttu-id="583a0-139">Důležité toocarefully naplánujte, jak bude instrumentace vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="583a0-139">It's important toocarefully plan how you will instrument your code.</span></span> <span data-ttu-id="583a0-140">plán správné instrumentace Hello vám může pomoct vyhnout potenciálně destabilizing základní kódu a pak nutnosti tooreinstrument hello kódu.</span><span class="sxs-lookup"><span data-stu-id="583a0-140">hello right instrumentation plan can help you avoid potentially destabilizing your code base, and then needing tooreinstrument hello code.</span></span> <span data-ttu-id="583a0-141">riziko tooreduce, můžete knihovna nástrojů, jako je [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), který je součástí Microsoft ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="583a0-141">tooreduce risk, you can choose an instrumentation library like [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), which is part of Microsoft ASP.NET Core.</span></span> <span data-ttu-id="583a0-142">ASP.NET Core má [objektu ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) rozhraní, které můžete použít u poskytovatele hello podle vaší volby, a současně minimalizujete její hello vliv na existující kód.</span><span class="sxs-lookup"><span data-stu-id="583a0-142">ASP.NET Core has an [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface that you can use with hello provider of your choice, while minimizing hello effect on existing code.</span></span> <span data-ttu-id="583a0-143">Můžete použít kód hello v ASP.NET Core v systému Windows a Linux a v hello úplné rozhraní .NET Framework, proto je standardizovaný kód instrumentace.</span><span class="sxs-lookup"><span data-stu-id="583a0-143">You can use hello code in ASP.NET Core on Windows and Linux, and in hello full .NET Framework, so your instrumentation code is standardized.</span></span> <span data-ttu-id="583a0-144">To je dále zkoumat níže:</span><span class="sxs-lookup"><span data-stu-id="583a0-144">This is further explored below:</span></span>

### <a name="using-microsoftextensionslogging-in-service-fabric"></a><span data-ttu-id="583a0-145">Použití Microsoft.Extensions.Logging v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="583a0-145">Using Microsoft.Extensions.Logging in Service Fabric</span></span>

1. <span data-ttu-id="583a0-146">Přidejte hello projektu toohello balíček Microsoft.Extensions.Logging NuGet chcete tooinstrument.</span><span class="sxs-lookup"><span data-stu-id="583a0-146">Add hello Microsoft.Extensions.Logging NuGet package toohello project you want tooinstrument.</span></span> <span data-ttu-id="583a0-147">Navíc přidat všechny balíčky zprostředkovatele (pro balíček třetích stran, viz následující ukázka hello).</span><span class="sxs-lookup"><span data-stu-id="583a0-147">Also, add any provider packages (for a third-party package, see hello following example).</span></span> <span data-ttu-id="583a0-148">Další informace najdete v tématu [protokolování v ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="583a0-148">For more information, see [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span></span>
2. <span data-ttu-id="583a0-149">Přidat **pomocí** direktivy pro soubor Microsoft.Extensions.Logging tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="583a0-149">Add a **using** directive for Microsoft.Extensions.Logging tooyour service file.</span></span>
3. <span data-ttu-id="583a0-150">Definování soukromé proměnné v třídě služby.</span><span class="sxs-lookup"><span data-stu-id="583a0-150">Define a private variable within your service class.</span></span>

  ```csharp
  private ILogger _logger = null;

  ```
4. <span data-ttu-id="583a0-151">V konstruktoru hello vaší třídy služeb přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="583a0-151">In hello constructor of your service class, add this code:</span></span>

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. <span data-ttu-id="583a0-152">Spuštění kódu v vaše metody instrumentace.</span><span class="sxs-lookup"><span data-stu-id="583a0-152">Start instrumenting your code in your methods.</span></span> <span data-ttu-id="583a0-153">Tady je několik ukázky:</span><span class="sxs-lookup"><span data-stu-id="583a0-153">Here are a few samples:</span></span>

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a><span data-ttu-id="583a0-154">Použití dalších zprostředkovatelů protokolování</span><span class="sxs-lookup"><span data-stu-id="583a0-154">Using other logging providers</span></span>

<span data-ttu-id="583a0-155">Někteří poskytovatelé třetích stran používají hello metody uvedené v předcházející části hello včetně [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), a [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span><span class="sxs-lookup"><span data-stu-id="583a0-155">Some third-party providers use hello approach described in hello preceding section, including [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), and [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span></span> <span data-ttu-id="583a0-156">Každá z těchto do ASP.NET Core protokolování je možné připojit nebo je můžete použít samostatně.</span><span class="sxs-lookup"><span data-stu-id="583a0-156">You can plug each of these into ASP.NET Core logging, or you can use them separately.</span></span> <span data-ttu-id="583a0-157">Serilog obsahuje funkce, která vylepšuje všech zpráv odeslaných z protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="583a0-157">Serilog has a feature that enriches all messages sent from a logger.</span></span> <span data-ttu-id="583a0-158">Tato funkce může být užitečné toooutput hello služby název, typ a informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="583a0-158">This feature can be useful toooutput hello service name, type, and partition information.</span></span> <span data-ttu-id="583a0-159">toouse tuto funkci hello ASP.NET základní infrastruktury, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="583a0-159">toouse this capability in hello ASP.NET Core infrastructure, do these steps:</span></span>

1. <span data-ttu-id="583a0-160">Přidejte hello Serilog, Serilog.Extensions.Logging, a balíčky Serilog.Sinks.Observable NuGet toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="583a0-160">Add hello Serilog, Serilog.Extensions.Logging, and Serilog.Sinks.Observable NuGet packages toohello project.</span></span> <span data-ttu-id="583a0-161">Například hello další také přidáte Serilog.Sinks.Literate.</span><span class="sxs-lookup"><span data-stu-id="583a0-161">For hello next example, also add Serilog.Sinks.Literate.</span></span> <span data-ttu-id="583a0-162">Lepší přístup se zobrazí později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="583a0-162">A better approach is shown later in this article.</span></span>
2. <span data-ttu-id="583a0-163">V Serilog vytvořte instanci LoggerConfiguration a hello protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="583a0-163">In Serilog, create a LoggerConfiguration and hello logger instance.</span></span>

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. <span data-ttu-id="583a0-164">Přidejte konstruktor Serilog.ILogger argument toohello služby a předejte hello nově vytvořený protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="583a0-164">Add a Serilog.ILogger argument toohello service constructor, and pass hello newly created logger.</span></span>

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. <span data-ttu-id="583a0-165">V konstruktoru hello služby, přidejte následující kód, který vytvoří hello enrichers vlastnost pro hello hello **ServiceTypeName**, **ServiceName**, **PartitionId**a  **Identifikátor InstanceId** vlastnosti služby hello.</span><span class="sxs-lookup"><span data-stu-id="583a0-165">In hello service constructor, add hello following code, which creates hello property enrichers for hello **ServiceTypeName**, **ServiceName**, **PartitionId**, and **InstanceId** properties of hello service.</span></span> <span data-ttu-id="583a0-166">Také přidá vlastnost enricher toohello protokolování ASP.NET Core objekt pro vytváření, abyste je mohli používat Microsoft.Extensions.Logging.ILogger ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="583a0-166">It also adds a property enricher toohello ASP.NET Core logging factory, so you can use Microsoft.Extensions.Logging.ILogger in your code.</span></span>

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

5. <span data-ttu-id="583a0-167">Kód hello nástrojích hello stejné, jako kdyby byly pomocí ASP.NET Core bez Serilog.</span><span class="sxs-lookup"><span data-stu-id="583a0-167">Instrument hello code hello same as if you were using ASP.NET Core without Serilog.</span></span>

  >[!NOTE]
  ><span data-ttu-id="583a0-168">Doporučujeme vám, že nepoužíváte hello statické Log.Logger s předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="583a0-168">We recommend that you don't use hello static Log.Logger with hello preceding example.</span></span> <span data-ttu-id="583a0-169">Service Fabric může být hostitelem více instancí z hello stejný typ v rámci jednoho procesu služby.</span><span class="sxs-lookup"><span data-stu-id="583a0-169">Service Fabric can host multiple instances of hello same service type within a single process.</span></span> <span data-ttu-id="583a0-170">Pokud používáte hello statické Log.Logger, hello poslední zapisovač systému hello vlastnost enrichers zobrazí hodnoty pro všechny instance, které jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="583a0-170">If you use hello static Log.Logger, hello last writer of hello property enrichers will show values for all instances that are running.</span></span> <span data-ttu-id="583a0-171">Toto je jedním z důvodů, proč hello _logger proměnné je proměnná privátního člena třídy služeb, hello.</span><span class="sxs-lookup"><span data-stu-id="583a0-171">This is one reason why hello _logger variable is a private member variable of hello service class.</span></span> <span data-ttu-id="583a0-172">Navíc musíte nastavit hello _logger dostupné toocommon kód, který může být použit ve službách.</span><span class="sxs-lookup"><span data-stu-id="583a0-172">Also, you must make hello _logger available toocommon code, which might be used across services.</span></span>

## <a name="choosing-a-logging-provider"></a><span data-ttu-id="583a0-173">Výběr zprostředkovatele protokolování</span><span class="sxs-lookup"><span data-stu-id="583a0-173">Choosing a logging provider</span></span>

<span data-ttu-id="583a0-174">Pokud vaše aplikace využívá vysoký výkon, **EventSource** je obvykle dobrou přístup.</span><span class="sxs-lookup"><span data-stu-id="583a0-174">If your application relies on high performance, **EventSource** is usually a good approach.</span></span> <span data-ttu-id="583a0-175">**EventSource** *obecně* využívá méně prostředků a provede lépe než ASP.NET Core protokolování nebo některého z hello k dispozici řešení třetí strany.</span><span class="sxs-lookup"><span data-stu-id="583a0-175">**EventSource** *generally* uses fewer resources and performs better than ASP.NET Core logging or any of hello available third-party solutions.</span></span>  <span data-ttu-id="583a0-176">Tato akce není problém pro mnoho služby, ale pokud je vaše služba orientovaných na výkon, pomocí **EventSource** může být vhodnější.</span><span class="sxs-lookup"><span data-stu-id="583a0-176">This isn't an issue for many services, but if your service is performance-oriented, using **EventSource** might be a better choice.</span></span> <span data-ttu-id="583a0-177">Ale tooget tyto výhody strukturovaná protokolování, **EventSource** vyžaduje větší investice z technickému týmu.</span><span class="sxs-lookup"><span data-stu-id="583a0-177">However, tooget these benefits of structured logging, **EventSource** requires a larger investment from your engineering team.</span></span> <span data-ttu-id="583a0-178">Pokud je to možné, proveďte rychlé prototyp několik možností protokolování a poté zvolte hello ten, který nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="583a0-178">If possible, do a quick prototype of a few logging options, and then choose hello one that best meets your needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="583a0-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="583a0-179">Next steps</span></span>

<span data-ttu-id="583a0-180">Po zvolení vaší tooinstrument zprostředkovatele protokolování vašim aplikacím a službám, protokolů a událostí třeba toobe agregovat před jejich odesláním tooany analysis platformy.</span><span class="sxs-lookup"><span data-stu-id="583a0-180">Once you have chosen your logging provider tooinstrument your applications and services, your logs and events need toobe aggregated before they can be sent tooany analysis platform.</span></span> <span data-ttu-id="583a0-181">Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter pochopit hello doporučené možnosti.</span><span class="sxs-lookup"><span data-stu-id="583a0-181">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter understand some of hello recommended options.</span></span>
