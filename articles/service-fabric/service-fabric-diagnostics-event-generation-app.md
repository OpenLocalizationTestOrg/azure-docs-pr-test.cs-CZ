---
title: "Úrovni aplikace Azure Service Fabric monitorování | Microsoft Docs"
description: "Další informace o aplikaci a události na úrovni služby a protokoly použít k monitorování a diagnostice Azure Service Fabric clustery."
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
ms.openlocfilehash: 3c472904641108b7383cd0f1416c47460f8de11a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a><span data-ttu-id="54aa2-103">Aplikace a služby na úrovni generování událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="54aa2-103">Application and service level event and log generation</span></span>

## <a name="instrumenting-the-code-with-custom-events"></a><span data-ttu-id="54aa2-104">Instrumentace kód vlastní události</span><span class="sxs-lookup"><span data-stu-id="54aa2-104">Instrumenting the code with custom events</span></span>

<span data-ttu-id="54aa2-105">Instrumentace kód je základem pro většině ostatních aspektů monitorování vašim službám.</span><span class="sxs-lookup"><span data-stu-id="54aa2-105">Instrumenting the code is the basis for most other aspects of monitoring your services.</span></span> <span data-ttu-id="54aa2-106">Instrumentace je jediným způsobem, jak můžete víte, že je něco špatně a ke zjištění toho, co je nutné opravit.</span><span class="sxs-lookup"><span data-stu-id="54aa2-106">Instrumentation is the only way you can know that something is wrong, and to diagnose what needs to be fixed.</span></span> <span data-ttu-id="54aa2-107">Přestože je technicky možné se připojit ladicí program k produkčním služby, není běžnou praxi.</span><span class="sxs-lookup"><span data-stu-id="54aa2-107">Although technically it's possible to connect a debugger to a production service, it's not a common practice.</span></span> <span data-ttu-id="54aa2-108">Nutnosti podrobně data instrumentace je tedy důležité.</span><span class="sxs-lookup"><span data-stu-id="54aa2-108">So, having detailed instrumentation data is important.</span></span>

<span data-ttu-id="54aa2-109">Některé produkty, automaticky instrumentace vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-109">Some products automatically instrument your code.</span></span> <span data-ttu-id="54aa2-110">I když tato řešení můžete dobře fungovat, je vyžadováno téměř vždy ruční instrumentace.</span><span class="sxs-lookup"><span data-stu-id="54aa2-110">Although these solutions can work well, manual instrumentation is almost always required.</span></span> <span data-ttu-id="54aa2-111">V části end musí mít dostatek informací k forensically ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="54aa2-111">In the end, you must have enough information to forensically debug the application.</span></span> <span data-ttu-id="54aa2-112">Tento dokument popisuje různé přístupy k instrumentaci kódu a kdy zvolit jeden ze způsobů oproti jinému.</span><span class="sxs-lookup"><span data-stu-id="54aa2-112">This document describes different approaches to instrumenting your code, and when to choose one approach over another.</span></span>

## <a name="eventsource"></a><span data-ttu-id="54aa2-113">EventSource</span><span class="sxs-lookup"><span data-stu-id="54aa2-113">EventSource</span></span>
<span data-ttu-id="54aa2-114">Když vytvoříte řešení Service Fabric ze šablony v sadě Visual Studio **EventSource**-odvozené třídy (**ServiceEventSource** nebo **ActorEventSource**) se vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="54aa2-114">When you create a Service Fabric solution from a template in Visual Studio, an **EventSource**-derived class (**ServiceEventSource** or **ActorEventSource**) is generated.</span></span> <span data-ttu-id="54aa2-115">Je vytvořit šablonu, ve kterém můžete přidat události pro vaše aplikace nebo služba.</span><span class="sxs-lookup"><span data-stu-id="54aa2-115">A template is created, in which you can add events for your application or service.</span></span> <span data-ttu-id="54aa2-116">**EventSource** název **musí** být jedinečné a musí jej přejmenovat z výchozí šablony řetězec Moje_firma -&lt;řešení&gt;-&lt;projektu&gt;.</span><span class="sxs-lookup"><span data-stu-id="54aa2-116">The **EventSource** name **must** be unique, and should be renamed from the default template string MyCompany-&lt;solution&gt;-&lt;project&gt;.</span></span> <span data-ttu-id="54aa2-117">S více **EventSource** definice, které používají stejný název způsobuje problém v době běhu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-117">Having multiple **EventSource** definitions that use the same name causes an issue at run time.</span></span> <span data-ttu-id="54aa2-118">Každé definované události, musí mít jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="54aa2-118">Each defined event must have a unique identifier.</span></span> <span data-ttu-id="54aa2-119">Pokud není jedinečný identifikátor, dojde k selhání běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="54aa2-119">If an identifier is not unique, a runtime failure occurs.</span></span> <span data-ttu-id="54aa2-120">Některé organizace preassign rozsahy hodnoty pro identifikátory, aby nedocházelo ke konfliktům mezi samostatnou vývojové týmy.</span><span class="sxs-lookup"><span data-stu-id="54aa2-120">Some organizations preassign ranges of values for identifiers to avoid conflicts between separate development teams.</span></span> <span data-ttu-id="54aa2-121">Další informace najdete v tématu [hovou na blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) nebo [dokumentace MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span><span class="sxs-lookup"><span data-stu-id="54aa2-121">For more information, see [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) or the [MSDN documentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span></span>

### <a name="using-structured-eventsource-events"></a><span data-ttu-id="54aa2-122">Používání strukturovaného událostí EventSource</span><span class="sxs-lookup"><span data-stu-id="54aa2-122">Using structured EventSource events</span></span>

<span data-ttu-id="54aa2-123">Každá z událostí v příklady kódu v této části jsou definovány pro konkrétní případ, například při registraci typu služby.</span><span class="sxs-lookup"><span data-stu-id="54aa2-123">Each of the events in the code examples in this section are defined for a specific case, for example, when a service type is registered.</span></span> <span data-ttu-id="54aa2-124">Když definujete zprávy případu použití, data se dá zabalit s text chyby a více můžete snadno hledat a filtr na základě názvy nebo hodnoty zadané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54aa2-124">When you define messages by use case, data can be packaged with the text of the error, and you can more easily search and filter based on the names or values of the specified properties.</span></span> <span data-ttu-id="54aa2-125">Strukturování díky výstup instrumentace usnadňují využívat, ale vyžaduje další myšlenku a čas pro definovat novou událost pro každý případ použití.</span><span class="sxs-lookup"><span data-stu-id="54aa2-125">Structuring the instrumentation output makes it easier to consume, but requires more thought and time to define a new event for each use case.</span></span> <span data-ttu-id="54aa2-126">Některé události definice mohlo sdílet napříč celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54aa2-126">Some event definitions can be shared across the entire application.</span></span> <span data-ttu-id="54aa2-127">Například metoda spuštění nebo zastavení by být událostí opětovně použít napříč mnoha služeb v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="54aa2-127">For example, a method start or stop event would be reused across many services within an application.</span></span> <span data-ttu-id="54aa2-128">Může mít specifické pro doménu služby, jako jsou systému pořadí **CreateOrder** událost, která má svůj vlastní jedinečný událostí.</span><span class="sxs-lookup"><span data-stu-id="54aa2-128">A domain-specific service, like an order system, might have a **CreateOrder** event, which has its own unique event.</span></span> <span data-ttu-id="54aa2-129">Tento přístup může generovat mnoho událostí a potenciálně vyžadují koordinaci identifikátory v rámci projektové týmy.</span><span class="sxs-lookup"><span data-stu-id="54aa2-129">This approach might generate many events, and potentially require coordination of identifiers across project teams.</span></span> 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a><span data-ttu-id="54aa2-130">Pomocí EventSource obecně</span><span class="sxs-lookup"><span data-stu-id="54aa2-130">Using EventSource generically</span></span>

<span data-ttu-id="54aa2-131">Protože definování určité události může být složité, definujte Spousta lidí několik událostí s společnou sadu parametrů, které obvykle výstup své informace jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="54aa2-131">Because defining specific events can be difficult, many people define a few events with a common set of parameters that generally output their information as a string.</span></span> <span data-ttu-id="54aa2-132">Velká část strukturovaných aspekt dojde ke ztrátě, a je obtížnější k vyhledávání a filtrování výsledků.</span><span class="sxs-lookup"><span data-stu-id="54aa2-132">Much of the structured aspect is lost, and it's more difficult to search and filter the results.</span></span> <span data-ttu-id="54aa2-133">V tomto přístupu jsou definovány několik události, které obvykle odpovídají úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="54aa2-133">In this approach, a few events that usually correspond to the logging levels are defined.</span></span> <span data-ttu-id="54aa2-134">Následující fragment kódu definuje ladění a chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="54aa2-134">The following snippet defines a debug and error message:</span></span>

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
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

<span data-ttu-id="54aa2-135">Pomocí hybridní strukturovaných a obecná instrumentace můžete použít také.</span><span class="sxs-lookup"><span data-stu-id="54aa2-135">Using a hybrid of structured and generic instrumentation also can work well.</span></span> <span data-ttu-id="54aa2-136">Strukturované instrumentace se používá pro vytváření sestav chyb a metriky.</span><span class="sxs-lookup"><span data-stu-id="54aa2-136">Structured instrumentation is used for reporting errors and metrics.</span></span> <span data-ttu-id="54aa2-137">Obecné události lze použít pro podrobné protokolování, který využívá techniky pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="54aa2-137">Generic events can be used for the detailed logging that is consumed by engineers for troubleshooting.</span></span>

## <a name="aspnet-core-logging"></a><span data-ttu-id="54aa2-138">ASP.NET Core protokolování</span><span class="sxs-lookup"><span data-stu-id="54aa2-138">ASP.NET Core logging</span></span>

<span data-ttu-id="54aa2-139">Je důležité pečlivě naplánovat, jak bude instrumentace vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-139">It's important to carefully plan how you will instrument your code.</span></span> <span data-ttu-id="54aa2-140">Plán správné instrumentace můžete vyhnout potenciálně destabilizing vaše základu kódu a pak se museli reinstrument kód.</span><span class="sxs-lookup"><span data-stu-id="54aa2-140">The right instrumentation plan can help you avoid potentially destabilizing your code base, and then needing to reinstrument the code.</span></span> <span data-ttu-id="54aa2-141">Aby se snížilo riziko, můžete knihovna nástrojů, jako je [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), který je součástí Microsoft ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54aa2-141">To reduce risk, you can choose an instrumentation library like [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), which is part of Microsoft ASP.NET Core.</span></span> <span data-ttu-id="54aa2-142">ASP.NET Core má [objektu ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) rozhraní, které můžete použít s poskytovatelem podle vaší volby, a současně minimalizujete její vliv na existující kód.</span><span class="sxs-lookup"><span data-stu-id="54aa2-142">ASP.NET Core has an [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface that you can use with the provider of your choice, while minimizing the effect on existing code.</span></span> <span data-ttu-id="54aa2-143">Můžete použít kód v ASP.NET Core v systému Windows a Linux, a v úplné rozhraní .NET Framework, takže je váš kód instrumentace standardizované.</span><span class="sxs-lookup"><span data-stu-id="54aa2-143">You can use the code in ASP.NET Core on Windows and Linux, and in the full .NET Framework, so your instrumentation code is standardized.</span></span> <span data-ttu-id="54aa2-144">To je dále zkoumat níže:</span><span class="sxs-lookup"><span data-stu-id="54aa2-144">This is further explored below:</span></span>

### <a name="using-microsoftextensionslogging-in-service-fabric"></a><span data-ttu-id="54aa2-145">Použití Microsoft.Extensions.Logging v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="54aa2-145">Using Microsoft.Extensions.Logging in Service Fabric</span></span>

1. <span data-ttu-id="54aa2-146">Do projektu, které chcete přidat balíček Microsoft.Extensions.Logging NuGet nástrojích.</span><span class="sxs-lookup"><span data-stu-id="54aa2-146">Add the Microsoft.Extensions.Logging NuGet package to the project you want to instrument.</span></span> <span data-ttu-id="54aa2-147">Navíc přidat všechny balíčky zprostředkovatele (pro balíček třetích stran, viz následující příklad).</span><span class="sxs-lookup"><span data-stu-id="54aa2-147">Also, add any provider packages (for a third-party package, see the following example).</span></span> <span data-ttu-id="54aa2-148">Další informace najdete v tématu [protokolování v ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="54aa2-148">For more information, see [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span></span>
2. <span data-ttu-id="54aa2-149">Přidat **pomocí** direktivy pro Microsoft.Extensions.Logging do souboru služby.</span><span class="sxs-lookup"><span data-stu-id="54aa2-149">Add a **using** directive for Microsoft.Extensions.Logging to your service file.</span></span>
3. <span data-ttu-id="54aa2-150">Definování soukromé proměnné v třídě služby.</span><span class="sxs-lookup"><span data-stu-id="54aa2-150">Define a private variable within your service class.</span></span>

  ```csharp
  private ILogger _logger = null;

  ```
4. <span data-ttu-id="54aa2-151">V konstruktoru třídy vaší služby přidejte tento kód:</span><span class="sxs-lookup"><span data-stu-id="54aa2-151">In the constructor of your service class, add this code:</span></span>

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. <span data-ttu-id="54aa2-152">Spuštění kódu v vaše metody instrumentace.</span><span class="sxs-lookup"><span data-stu-id="54aa2-152">Start instrumenting your code in your methods.</span></span> <span data-ttu-id="54aa2-153">Tady je několik ukázky:</span><span class="sxs-lookup"><span data-stu-id="54aa2-153">Here are a few samples:</span></span>

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a><span data-ttu-id="54aa2-154">Použití dalších zprostředkovatelů protokolování</span><span class="sxs-lookup"><span data-stu-id="54aa2-154">Using other logging providers</span></span>

<span data-ttu-id="54aa2-155">Některé použití poskytovatelů třetích stran přístupu popsané v předchozí části, včetně [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), a [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span><span class="sxs-lookup"><span data-stu-id="54aa2-155">Some third-party providers use the approach described in the preceding section, including [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), and [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span></span> <span data-ttu-id="54aa2-156">Každá z těchto do ASP.NET Core protokolování je možné připojit nebo je můžete použít samostatně.</span><span class="sxs-lookup"><span data-stu-id="54aa2-156">You can plug each of these into ASP.NET Core logging, or you can use them separately.</span></span> <span data-ttu-id="54aa2-157">Serilog obsahuje funkce, která vylepšuje všech zpráv odeslaných z protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="54aa2-157">Serilog has a feature that enriches all messages sent from a logger.</span></span> <span data-ttu-id="54aa2-158">Tato funkce může být užitečné k vypsání názvu služby, typ a informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-158">This feature can be useful to output the service name, type, and partition information.</span></span> <span data-ttu-id="54aa2-159">Chcete-li použít tuto funkci v infrastruktuře ASP.NET Core, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="54aa2-159">To use this capability in the ASP.NET Core infrastructure, do these steps:</span></span>

1. <span data-ttu-id="54aa2-160">Přidání balíčků Serilog, Serilog.Extensions.Logging a Serilog.Sinks.Observable NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-160">Add the Serilog, Serilog.Extensions.Logging, and Serilog.Sinks.Observable NuGet packages to the project.</span></span> <span data-ttu-id="54aa2-161">Další příklad také přidáte Serilog.Sinks.Literate.</span><span class="sxs-lookup"><span data-stu-id="54aa2-161">For the next example, also add Serilog.Sinks.Literate.</span></span> <span data-ttu-id="54aa2-162">Lepší přístup se zobrazí později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="54aa2-162">A better approach is shown later in this article.</span></span>
2. <span data-ttu-id="54aa2-163">V Serilog vytvořte LoggerConfiguration a instance protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="54aa2-163">In Serilog, create a LoggerConfiguration and the logger instance.</span></span>

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. <span data-ttu-id="54aa2-164">Přidejte Serilog.ILogger argument konstruktoru služby a předat nově vytvořený protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="54aa2-164">Add a Serilog.ILogger argument to the service constructor, and pass the newly created logger.</span></span>

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. <span data-ttu-id="54aa2-165">V konstruktoru služby, přidejte následující kód, který vytvoří enrichers vlastnost pro **ServiceTypeName**, **ServiceName**, **PartitionId**, a **identifikátor InstanceId** vlastnosti služby.</span><span class="sxs-lookup"><span data-stu-id="54aa2-165">In the service constructor, add the following code, which creates the property enrichers for the **ServiceTypeName**, **ServiceName**, **PartitionId**, and **InstanceId** properties of the service.</span></span> <span data-ttu-id="54aa2-166">Také přidá vlastnost enricher k objektu pro vytváření protokolování ASP.NET Core, abyste je mohli používat Microsoft.Extensions.Logging.ILogger ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-166">It also adds a property enricher to the ASP.NET Core logging factory, so you can use Microsoft.Extensions.Logging.ILogger in your code.</span></span>

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

5. <span data-ttu-id="54aa2-167">Instrumentace kód stejné jako kdyby byly pomocí ASP.NET Core bez Serilog.</span><span class="sxs-lookup"><span data-stu-id="54aa2-167">Instrument the code the same as if you were using ASP.NET Core without Serilog.</span></span>

  >[!NOTE]
  ><span data-ttu-id="54aa2-168">Doporučujeme vám, že nepoužíváte statické Log.Logger s v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-168">We recommend that you don't use the static Log.Logger with the preceding example.</span></span> <span data-ttu-id="54aa2-169">Service Fabric může být hostitelem více instancí stejného typu služby v rámci jednoho procesu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-169">Service Fabric can host multiple instances of the same service type within a single process.</span></span> <span data-ttu-id="54aa2-170">Pokud používáte statické Log.Logger, zobrazí poslednímu zapisujícímu enrichers vlastnost hodnoty pro všechny instance, které jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="54aa2-170">If you use the static Log.Logger, the last writer of the property enrichers will show values for all instances that are running.</span></span> <span data-ttu-id="54aa2-171">Toto je jedním z důvodů, proč proměnnou _logger je proměnná privátního člena třídy služeb.</span><span class="sxs-lookup"><span data-stu-id="54aa2-171">This is one reason why the _logger variable is a private member variable of the service class.</span></span> <span data-ttu-id="54aa2-172">Navíc musíte nastavit _logger k dispozici pro společný kód, který může být použit ve službách.</span><span class="sxs-lookup"><span data-stu-id="54aa2-172">Also, you must make the _logger available to common code, which might be used across services.</span></span>

## <a name="choosing-a-logging-provider"></a><span data-ttu-id="54aa2-173">Výběr zprostředkovatele protokolování</span><span class="sxs-lookup"><span data-stu-id="54aa2-173">Choosing a logging provider</span></span>

<span data-ttu-id="54aa2-174">Pokud vaše aplikace využívá vysoký výkon, **EventSource** je obvykle dobrou přístup.</span><span class="sxs-lookup"><span data-stu-id="54aa2-174">If your application relies on high performance, **EventSource** is usually a good approach.</span></span> <span data-ttu-id="54aa2-175">**EventSource** *obecně* využívá méně prostředků a provede lépe než ASP.NET Core protokolování ani žádný z dostupných řešení třetí strany.</span><span class="sxs-lookup"><span data-stu-id="54aa2-175">**EventSource** *generally* uses fewer resources and performs better than ASP.NET Core logging or any of the available third-party solutions.</span></span>  <span data-ttu-id="54aa2-176">Tato akce není problém pro mnoho služby, ale pokud je vaše služba orientovaných na výkon, pomocí **EventSource** může být vhodnější.</span><span class="sxs-lookup"><span data-stu-id="54aa2-176">This isn't an issue for many services, but if your service is performance-oriented, using **EventSource** might be a better choice.</span></span> <span data-ttu-id="54aa2-177">Však získat tyto výhody strukturovaná protokolování, **EventSource** vyžaduje větší investice z technickému týmu.</span><span class="sxs-lookup"><span data-stu-id="54aa2-177">However, to get these benefits of structured logging, **EventSource** requires a larger investment from your engineering team.</span></span> <span data-ttu-id="54aa2-178">Pokud je to možné nezadávejte rychlé prototyp několik možností protokolování a potom vyberte ten, který nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="54aa2-178">If possible, do a quick prototype of a few logging options, and then choose the one that best meets your needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54aa2-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54aa2-179">Next steps</span></span>

<span data-ttu-id="54aa2-180">Jakmile jste vybrali poskytovatele protokolování a instrumentace aplikací a služeb, protokolů a událostí muset agregovat před odesláním pro žádnou platformu analysis.</span><span class="sxs-lookup"><span data-stu-id="54aa2-180">Once you have chosen your logging provider to instrument your applications and services, your logs and events need to be aggregated before they can be sent to any analysis platform.</span></span> <span data-ttu-id="54aa2-181">Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) lépe pochopit některé doporučené možnosti.</span><span class="sxs-lookup"><span data-stu-id="54aa2-181">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) to better understand some of the recommended options.</span></span>
