---
title: "vlastní operace aaaTrack pomocí .NET SDK služby Azure Application Insights | Microsoft Docs"
description: "Sledování vlastní operace pomocí .NET SDK služby Azure Application Insights"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/31/2017
ms.author: sergkanz
ms.openlocfilehash: fe338d3e2b17a3dae43c96c60a19f57b3f46f0a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a><span data-ttu-id="e08f1-103">Sledování vlastní operace s Application Insights .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e08f1-103">Track custom operations with Application Insights .NET SDK</span></span>

<span data-ttu-id="e08f1-104">Azure sadách Application Insights SDK automaticky sledovat HTTP příchozí požadavky a volá toodependent služby, jako je například požadavků HTTP a dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="e08f1-104">Azure Application Insights SDKs automatically track incoming HTTP requests and calls toodependent services, such as HTTP requests and SQL queries.</span></span> <span data-ttu-id="e08f1-105">Sledování a korelace mezi požadavky a závislosti a získáte přehled o rychlost reakce a spolehlivost hello celou aplikaci přes všechny mikroslužeb, které spojují této aplikace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-105">Tracking and correlation of requests and dependencies give you visibility into hello whole application's responsiveness and reliability across all microservices that combine this application.</span></span> 

<span data-ttu-id="e08f1-106">Existuje třída schémat aplikace, které nepodporují se obecně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-106">There is a class of application patterns that can't be supported generically.</span></span> <span data-ttu-id="e08f1-107">Správné sledování tyto vzory vyžaduje ruční kód instrumentace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-107">Proper monitoring of such patterns requires manual code instrumentation.</span></span> <span data-ttu-id="e08f1-108">Tento článek se zabývá několik vzorů, které můžou vyžadovat ruční instrumentace, jako je například vlastní fronty zpracování a spuštění úlohy dlouho běžící na pozadí.</span><span class="sxs-lookup"><span data-stu-id="e08f1-108">This article covers a few patterns that might require manual instrumentation, such as custom queue processing and running long-running background tasks.</span></span>

<span data-ttu-id="e08f1-109">Tento dokument obsahuje pokyny k jak tootrack vlastní operace s hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="e08f1-109">This document provides guidance on how tootrack custom operations with hello Application Insights SDK.</span></span> <span data-ttu-id="e08f1-110">Tato dokumentace je důležité pro:</span><span class="sxs-lookup"><span data-stu-id="e08f1-110">This documentation is relevant for:</span></span>

- <span data-ttu-id="e08f1-111">Application Insights pro rozhraní .NET (také označované jako základní sady SDK) verze 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="e08f1-111">Application Insights for .NET (also known as Base SDK) version 2.4+.</span></span>
- <span data-ttu-id="e08f1-112">Application Insights pro webové aplikace (používající technologii ASP.NET) verze 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="e08f1-112">Application Insights for web applications (running ASP.NET) version 2.4+.</span></span>
- <span data-ttu-id="e08f1-113">Application Insights pro ASP.NET Core verze 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="e08f1-113">Application Insights for ASP.NET Core version 2.1+.</span></span>

## <a name="overview"></a><span data-ttu-id="e08f1-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="e08f1-114">Overview</span></span>
<span data-ttu-id="e08f1-115">Operace je logické část práce spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-115">An operation is a logical piece of work run by an application.</span></span> <span data-ttu-id="e08f1-116">Má název, čas spuštění, doba trvání, výsledek a kontext spuštění jako uživatelské jméno, vlastnosti a výsledek.</span><span class="sxs-lookup"><span data-stu-id="e08f1-116">It has a name, start time, duration, result, and a context of execution like user name, properties, and result.</span></span> <span data-ttu-id="e08f1-117">Pokud byla zahájena operace A operace B, pak operaci B je nastaven jako nadřazený pro A. Operace může mít jen jednu nadřazenou položku, ale může mít mnoho podřízené operací.</span><span class="sxs-lookup"><span data-stu-id="e08f1-117">If operation A was initiated by operation B, then operation B is set as a parent for A. An operation can have only one parent, but it can have many child operations.</span></span> <span data-ttu-id="e08f1-118">Další informace o operacích a telemetrie korelace najdete v tématu [Azure Application Insights telemetrie korelace](application-insights-correlation.md).</span><span class="sxs-lookup"><span data-stu-id="e08f1-118">For more information on operations and telemetry correlation, see [Azure Application Insights telemetry correlation](application-insights-correlation.md).</span></span>

<span data-ttu-id="e08f1-119">V hello Application Insights .NET SDK, je popsán hello operaci hello abstraktní třída [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) a jeho následníky [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) a [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span><span class="sxs-lookup"><span data-stu-id="e08f1-119">In hello Application Insights .NET SDK, hello operation is described by hello abstract class [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) and its descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) and [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span></span>

## <a name="incoming-operations-tracking"></a><span data-ttu-id="e08f1-120">Příchozí operace sledování</span><span class="sxs-lookup"><span data-stu-id="e08f1-120">Incoming operations tracking</span></span> 
<span data-ttu-id="e08f1-121">Hello webové služby Application Insights SDK automaticky shromažďuje požadavky protokolu HTTP pro aplikace ASP.NET, které běží v kanálu služby IIS a všechny aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e08f1-121">hello Application Insights web SDK automatically collects HTTP requests for ASP.NET applications that run in an IIS pipeline and all ASP.NET Core applications.</span></span> <span data-ttu-id="e08f1-122">Nejsou podporované komunity řešení pro jiné platformy a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e08f1-122">There are community-supported solutions for other platforms and frameworks.</span></span> <span data-ttu-id="e08f1-123">Ale pokud aplikace hello nepodporuje hello standard nebo podporovaná komunitou řešení, můžete instrumentovat ji ručně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-123">However, if hello application isn't supported by any of hello standard or community-supported solutions, you can instrument it manually.</span></span>

<span data-ttu-id="e08f1-124">Další příklad, který vyžaduje vlastní sledování je hello pracovního procesu, která přijímá položky z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="e08f1-124">Another example that requires custom tracking is hello worker that receives items from hello queue.</span></span> <span data-ttu-id="e08f1-125">Pro některé fronty hello volání tooadd zprávu, kterou toothis fronty je sledovat jako závislost.</span><span class="sxs-lookup"><span data-stu-id="e08f1-125">For some queues, hello call tooadd a message toothis queue is tracked as a dependency.</span></span> <span data-ttu-id="e08f1-126">Hello základní operace, která popisuje zpracování zprávy se však nejsou shromažďovány automaticky.</span><span class="sxs-lookup"><span data-stu-id="e08f1-126">However, hello high-level operation that describes message processing is not automatically collected.</span></span>

<span data-ttu-id="e08f1-127">Podívejme se, jak jsme můžete sledovat tyto operace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-127">Let's see how we can track such operations.</span></span>

<span data-ttu-id="e08f1-128">Na vysoké úrovni, úloha hello je toocreate `RequestTelemetry` a nastavte známých vlastností.</span><span class="sxs-lookup"><span data-stu-id="e08f1-128">On a high level, hello task is toocreate `RequestTelemetry` and set known properties.</span></span> <span data-ttu-id="e08f1-129">Po dokončení operace hello je sledovat hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e08f1-129">After hello operation is finished, you track hello telemetry.</span></span> <span data-ttu-id="e08f1-130">Hello následující příklad ukazuje tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-130">hello following example demonstrates this task.</span></span>

### <a name="http-request-in-owin-self-hosted-app"></a><span data-ttu-id="e08f1-131">Požadavek HTTP ve vlastním hostováním aplikace Owin</span><span class="sxs-lookup"><span data-stu-id="e08f1-131">HTTP request in Owin self-hosted app</span></span>
<span data-ttu-id="e08f1-132">V tomto příkladu jsme podle hello [protokolu HTTP pro korelačního](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="e08f1-132">In this example, we follow hello [HTTP Protocol for Correlation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span></span> <span data-ttu-id="e08f1-133">Byste měli očekávat tooreceive hlavičky, které jsou popsány existuje.</span><span class="sxs-lookup"><span data-stu-id="e08f1-133">You should expect tooreceive headers that are described there.</span></span>

``` C#
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from hello upstream service, set hello telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process hello request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time toostop hello operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns hello root ID from hello '|' toohello first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

<span data-ttu-id="e08f1-134">Hello protokolu HTTP pro korelačního také deklaruje hello `Correlation-Context` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e08f1-134">hello HTTP Protocol for Correlation also declares hello `Correlation-Context` header.</span></span> <span data-ttu-id="e08f1-135">Nicméně je vynechaný sem pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="e08f1-135">However, it's omitted here for simplicity.</span></span>

## <a name="queue-instrumentation"></a><span data-ttu-id="e08f1-136">Fronty instrumentace</span><span class="sxs-lookup"><span data-stu-id="e08f1-136">Queue instrumentation</span></span>
<span data-ttu-id="e08f1-137">Pro komunikaci pomocí protokolu HTTP vytvořili jsme protokol toopass korelace podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e08f1-137">For HTTP communication, we've created a protocol toopass correlation details.</span></span> <span data-ttu-id="e08f1-138">S protokoly některé fronty můžete předat další metadata, společně s uvítací zprávu a s ostatními, které není možné.</span><span class="sxs-lookup"><span data-stu-id="e08f1-138">With some queues' protocols, you can pass additional metadata along with hello message, and with others you can't.</span></span>

### <a name="service-bus-queue"></a><span data-ttu-id="e08f1-139">Fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="e08f1-139">Service Bus queue</span></span>
<span data-ttu-id="e08f1-140">S hello Azure [fronty Service Bus](../service-bus-messaging/index.md), můžete předat kontejneru objektů a dat společně s uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-140">With hello Azure [Service Bus queue](../service-bus-messaging/index.md), you can pass a property bag along with hello message.</span></span> <span data-ttu-id="e08f1-141">Použití ID toopass hello korelace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-141">We use it toopass hello correlation ID.</span></span>

<span data-ttu-id="e08f1-142">fronty Service Bus Hello používá protokoly založených na protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="e08f1-142">hello Service Bus queue uses TCP-based protocols.</span></span> <span data-ttu-id="e08f1-143">Application Insights nesleduje automaticky operace fronty, takže jsme sledovat ručně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-143">Application Insights doesn't automatically track queue operations, so we track them manually.</span></span> <span data-ttu-id="e08f1-144">dequeue – Hello operace je rozhraní API nabízené style a máme nelze tootrack ho.</span><span class="sxs-lookup"><span data-stu-id="e08f1-144">hello dequeue operation is a push-style API, and we're unable tootrack it.</span></span>

#### <a name="enqueue"></a><span data-ttu-id="e08f1-145">Zařazování</span><span class="sxs-lookup"><span data-stu-id="e08f1-145">Enqueue</span></span>

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes hello telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows hello property bag toopass along with hello message.
    // We will use them toopass our correlation identifiers (and other context)
    // toohello consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a><span data-ttu-id="e08f1-146">Proces</span><span class="sxs-lookup"><span data-stu-id="e08f1-146">Process</span></span>
```C#
public async Task Process(BrokeredMessage message)
{
    // After hello message is taken from hello queue, create RequestTelemetry tootrack its processing.
    // It might also make sense tooget hello name from hello message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a><span data-ttu-id="e08f1-147">Fronty Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e08f1-147">Azure Storage queue</span></span>
<span data-ttu-id="e08f1-148">Následující příklad ukazuje, jak Hello tootrack hello [fronty Azure Storage](../storage/queues/storage-dotnet-how-to-use-queues.md) operace a correlate telemetrie mezi hello producent, hello příjemce a Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e08f1-148">hello following example shows how tootrack hello [Azure Storage queue](../storage/queues/storage-dotnet-how-to-use-queues.md) operations and correlate telemetry between hello producer, hello consumer, and Azure Storage.</span></span> 

<span data-ttu-id="e08f1-149">fronty úložiště Hello používá rozhraní API HTTP.</span><span class="sxs-lookup"><span data-stu-id="e08f1-149">hello Storage queue has an HTTP API.</span></span> <span data-ttu-id="e08f1-150">Všechny fronty toohello volání jsou sledovány objektem hello Application Insights závislostí kolekce pro požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="e08f1-150">All calls toohello queue are tracked by hello Application Insights Dependency Collector for HTTP requests.</span></span>
<span data-ttu-id="e08f1-151">Zajistěte, aby byla `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` v `applicationInsights.config`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-151">Make sure you have `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`.</span></span> <span data-ttu-id="e08f1-152">Pokud ji nemáte, přidejte ho programově, jak je popsáno v [filtrování a předzpracování v hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="e08f1-152">If you don't have it, add it programmatically as described in [Filtering and Preprocessing in hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span></span>

<span data-ttu-id="e08f1-153">Pokud nakonfigurujete Application Insights ručně, ujistěte se, vytvoření a inicializace `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` podobně jako:</span><span class="sxs-lookup"><span data-stu-id="e08f1-153">If you configure Application Insights manually, make sure you create and initialize `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` similarly to:</span></span>
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

<span data-ttu-id="e08f1-154">Můžete také chtít toocorrelate hello Application Insights ID operace s ID hello úložiště požadavku.</span><span class="sxs-lookup"><span data-stu-id="e08f1-154">You also might want toocorrelate hello Application Insights operation ID with hello Storage request ID.</span></span> <span data-ttu-id="e08f1-155">Informace o tom, jak tooset a získání úložiště požadavků klienta a ID žádosti serveru najdete v tématu [monitorování, Diagnostika a řešení potíží s Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span><span class="sxs-lookup"><span data-stu-id="e08f1-155">For information on how tooset and get a Storage request client and a server request ID, see [Monitor, diagnose, and troubleshoot Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span></span>

#### <a name="enqueue"></a><span data-ttu-id="e08f1-156">Zařazování</span><span class="sxs-lookup"><span data-stu-id="e08f1-156">Enqueue</span></span>
<span data-ttu-id="e08f1-157">Fronty úložiště nepodporují hello rozhraní API HTTP, všechny operace s frontou hello sledovány automaticky pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-157">Because Storage queues support hello HTTP API, all operations with hello queue are automatically tracked by Application Insights.</span></span> <span data-ttu-id="e08f1-158">V mnoha případech tato instrumentace by vám měly dostatečně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-158">In many cases, this instrumentation should be enough.</span></span> <span data-ttu-id="e08f1-159">Ale toocorrelate trasování na straně příjemce hello s producent trasování, je nutné předat některé korelace kontextu podobně toohow jsme to provést hello protokolu HTTP pro korelačního.</span><span class="sxs-lookup"><span data-stu-id="e08f1-159">However, toocorrelate traces on hello consumer side with producer traces, you must pass some correlation context similarly toohow we do it in hello HTTP Protocol for Correlation.</span></span> 

<span data-ttu-id="e08f1-160">V tomto příkladu jsme sledovat hello volitelné `Enqueue` operaci.</span><span class="sxs-lookup"><span data-stu-id="e08f1-160">In this example, we track hello optional `Enqueue` operation.</span></span> <span data-ttu-id="e08f1-161">Můžete:</span><span class="sxs-lookup"><span data-stu-id="e08f1-161">You can:</span></span>

 - <span data-ttu-id="e08f1-162">**Korelovat opakovaných pokusů (pokud existuje)**: všechny mají jeden společným nadřazeným prvkem, který je hello `Enqueue` operaci.</span><span class="sxs-lookup"><span data-stu-id="e08f1-162">**Correlate retries (if any)**: They all have one common parent that's hello `Enqueue` operation.</span></span> <span data-ttu-id="e08f1-163">Jinak se sledují jako podřízené objekty hello příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="e08f1-163">Otherwise, they're tracked as children of hello incoming request.</span></span> <span data-ttu-id="e08f1-164">Pokud existují více logických požadavky toohello fronty, může být obtížné toofind výsledkem které volání opakování.</span><span class="sxs-lookup"><span data-stu-id="e08f1-164">If there are multiple logical requests toohello queue, it might be difficult toofind which call resulted in retries.</span></span>
 - <span data-ttu-id="e08f1-165">**Korelovat protokoly úložiště (Pokud je potřeba)**: jste korelační s telemetrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-165">**Correlate Storage logs (if and when needed)**: They're correlated with Application Insights telemetry.</span></span>

<span data-ttu-id="e08f1-166">Hello `Enqueue` operaci je podřízeným hello nadřazené operace (například příchozí žádosti HTTP).</span><span class="sxs-lookup"><span data-stu-id="e08f1-166">hello `Enqueue` operation is hello child of a parent operation (for example, an incoming HTTP request).</span></span> <span data-ttu-id="e08f1-167">volání závislostí Hello HTTP je podřízeným hello hello `Enqueue` operace a hello pod podřízená hello příchozích požadavků:</span><span class="sxs-lookup"><span data-stu-id="e08f1-167">hello HTTP dependency call is hello child of hello `Enqueue` operation and hello grandchild of hello incoming request:</span></span>

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose toopass payload serialized tooJSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message tooprocess'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id toohello OperationContext toocorrelate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

<span data-ttu-id="e08f1-168">sestavy aplikace tooreduce hello množství telemetrie nebo pokud nechcete, aby tootrack hello `Enqueue` operace z jiných důvodů, použijte hello `Activity` přímo rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e08f1-168">tooreduce hello amount of telemetry your application reports or if you don't want tootrack hello `Enqueue` operation for other reasons, use hello `Activity` API directly:</span></span>

- <span data-ttu-id="e08f1-169">Vytvořit (a spuštění) novou `Activity` místo spuštění operace hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-169">Create (and start) a new `Activity` instead of starting hello Application Insights operation.</span></span> <span data-ttu-id="e08f1-170">Provedete *není* potřebovat tooassign žádné vlastnosti v něm kromě název operace hello.</span><span class="sxs-lookup"><span data-stu-id="e08f1-170">You do *not* need tooassign any properties on it except hello operation name.</span></span>
- <span data-ttu-id="e08f1-171">Serializovat `yourActivity.Id` do datovou část zprávy hello místo `operation.Telemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-171">Serialize `yourActivity.Id` into hello message payload instead of `operation.Telemetry.Id`.</span></span> <span data-ttu-id="e08f1-172">Můžete také použít `Activity.Current.Id`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-172">You can also use `Activity.Current.Id`.</span></span>


#### <a name="dequeue"></a><span data-ttu-id="e08f1-173">Dequeue –</span><span class="sxs-lookup"><span data-stu-id="e08f1-173">Dequeue</span></span>
<span data-ttu-id="e08f1-174">Podobně příliš`Enqueue`, frontu úložiště služby skutečné HTTP žádost toohello automaticky sledován pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-174">Similarly too`Enqueue`, an actual HTTP request toohello Storage queue is automatically tracked by Application Insights.</span></span> <span data-ttu-id="e08f1-175">Ale hello `Enqueue` operaci pravděpodobně dojde v kontextu nadřazené hello, jako je například kontextu příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="e08f1-175">However, hello `Enqueue` operation presumably happens in hello parent context, such as an incoming request context.</span></span> <span data-ttu-id="e08f1-176">Sadách Application Insights SDK automaticky korelovat takovou operaci (a jeho součástí HTTP) s nadřazenou žádostí hello a další telemetrií uvedený v hello stejný obor.</span><span class="sxs-lookup"><span data-stu-id="e08f1-176">Application Insights SDKs automatically correlate such an operation (and its HTTP part) with hello parent request and other telemetry reported in hello same scope.</span></span>

<span data-ttu-id="e08f1-177">Hello `Dequeue` operace je složité.</span><span class="sxs-lookup"><span data-stu-id="e08f1-177">hello `Dequeue` operation is tricky.</span></span> <span data-ttu-id="e08f1-178">Hello Application Insights SDK automaticky sleduje požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="e08f1-178">hello Application Insights SDK automatically tracks HTTP requests.</span></span> <span data-ttu-id="e08f1-179">Ale neví hello korelace kontextu, dokud je analyzována uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-179">However, it doesn't know hello correlation context until hello message is parsed.</span></span> <span data-ttu-id="e08f1-180">Není možné toocorrelate hello HTTP žádost tooget uvítací zprávu se zbytkem hello hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e08f1-180">It's not possible toocorrelate hello HTTP request tooget hello message with hello rest of hello telemetry.</span></span>

<span data-ttu-id="e08f1-181">V mnoha případech může být užitečné toocorrelate toohello fronty požadavků HTTP hello se také další trasování.</span><span class="sxs-lookup"><span data-stu-id="e08f1-181">In many cases, it might be useful toocorrelate hello HTTP request toohello queue with other traces as well.</span></span> <span data-ttu-id="e08f1-182">Hello následující příklad ukazuje, jak toodo ho:</span><span class="sxs-lookup"><span data-stu-id="e08f1-182">hello following example demonstrates how toodo it:</span></span>

``` C#
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // If there is a message, we want toocorrelate hello Dequeue operation with processing.
            // However, we will only know what correlation ID toouse after we get it from hello message,
            // so we will report telemetry after we know hello IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete hello message.
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a><span data-ttu-id="e08f1-183">Proces</span><span class="sxs-lookup"><span data-stu-id="e08f1-183">Process</span></span>

<span data-ttu-id="e08f1-184">V následujícím příkladu hello, jsme příchozí zprávu trasování způsobem podobně toohow jsme trasování HTTP příchozí žádosti:</span><span class="sxs-lookup"><span data-stu-id="e08f1-184">In hello following example, we trace an incoming message in a manner similarly toohow we trace an incoming HTTP request:</span></span>

```C#
public async Task Process(MessagePayload message)
{
    // After hello message is dequeued from hello queue, create RequestTelemetry tootrack its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense tooget hello name from hello message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

<span data-ttu-id="e08f1-185">Podobně můžete instrumentovány jiné operace fronty.</span><span class="sxs-lookup"><span data-stu-id="e08f1-185">Similarly, other queue operations can be instrumented.</span></span> <span data-ttu-id="e08f1-186">Funkce Náhled operace by měla instrumentována podobným způsobem jako operace dequeue.</span><span class="sxs-lookup"><span data-stu-id="e08f1-186">A peek operation should be instrumented in a similar way as a dequeue operation.</span></span> <span data-ttu-id="e08f1-187">Instrumentace operace fronty správy není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="e08f1-187">Instrumenting queue management operations isn't necessary.</span></span> <span data-ttu-id="e08f1-188">Application Insights sleduje operací, jako je například HTTP a ve většině případů je dost.</span><span class="sxs-lookup"><span data-stu-id="e08f1-188">Application Insights tracks operations such as HTTP, and in most cases, it's enough.</span></span>

<span data-ttu-id="e08f1-189">Pokud jste instrumentace odstranění zprávy, ujistěte se, že nastavíte hello operaci identifikátory (korelace).</span><span class="sxs-lookup"><span data-stu-id="e08f1-189">When you instrument message deletion, make sure you set hello operation (correlation) identifiers.</span></span> <span data-ttu-id="e08f1-190">Alternativně můžete použít hello `Activity` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e08f1-190">Alternatively, you can use hello `Activity` API.</span></span> <span data-ttu-id="e08f1-191">Protože Application Insights provede za vás pak není třeba identifikátory operace tooset na hello telemetrie položky:</span><span class="sxs-lookup"><span data-stu-id="e08f1-191">Then you don't need tooset operation identifiers on hello telemetry items because Application Insights does it for you:</span></span>

- <span data-ttu-id="e08f1-192">Vytvořte novou `Activity` po vám položky z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="e08f1-192">Create a new `Activity` after you've got an item from hello queue.</span></span>
- <span data-ttu-id="e08f1-193">Použití `Activity.SetParentId(message.ParentId)` toocorrelate příjemce a producent protokoly.</span><span class="sxs-lookup"><span data-stu-id="e08f1-193">Use `Activity.SetParentId(message.ParentId)` toocorrelate consumer and producer logs.</span></span>
- <span data-ttu-id="e08f1-194">Spustit hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-194">Start hello `Activity`.</span></span>
- <span data-ttu-id="e08f1-195">Sledování dequeue – zpracovat a operace odstranění pomocí `Start/StopOperation` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="e08f1-195">Track dequeue, process, and delete operations by using `Start/StopOperation` helpers.</span></span> <span data-ttu-id="e08f1-196">Provést z hello stejné asynchronní řízení toku (kontext spuštění).</span><span class="sxs-lookup"><span data-stu-id="e08f1-196">Do it from hello same asynchronous control flow (execution context).</span></span> <span data-ttu-id="e08f1-197">Tímto způsobem že jste korelační správně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-197">In this way, they're correlated properly.</span></span>
- <span data-ttu-id="e08f1-198">Zastavení hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-198">Stop hello `Activity`.</span></span>
- <span data-ttu-id="e08f1-199">Použití `Start/StopOperation`, nebo volejte `Track` telemetrie ručně.</span><span class="sxs-lookup"><span data-stu-id="e08f1-199">Use `Start/StopOperation`, or call `Track` telemetry manually.</span></span>

### <a name="batch-processing"></a><span data-ttu-id="e08f1-200">Dávkové zpracování</span><span class="sxs-lookup"><span data-stu-id="e08f1-200">Batch processing</span></span>
<span data-ttu-id="e08f1-201">Některé fronty můžete dequeue – více zpráv s jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="e08f1-201">With some queues, you can dequeue multiple messages with one request.</span></span> <span data-ttu-id="e08f1-202">Zpracování takové zprávy je pravděpodobně z důvodu nezávislé a patří toohello různých logických operací.</span><span class="sxs-lookup"><span data-stu-id="e08f1-202">Processing such messages is presumably independent and belongs toohello different logical operations.</span></span> <span data-ttu-id="e08f1-203">V takovém případě není možné toocorrelate hello `Dequeue` zpracování zprávy tooparticular operaci.</span><span class="sxs-lookup"><span data-stu-id="e08f1-203">In this case, it's not possible toocorrelate hello `Dequeue` operation tooparticular message processing.</span></span>

<span data-ttu-id="e08f1-204">Každá zpráva, měla by být zpracována v jeho vlastní asynchronní řízení toku.</span><span class="sxs-lookup"><span data-stu-id="e08f1-204">Each message should be processed in its own asynchronous control flow.</span></span> <span data-ttu-id="e08f1-205">Další informace najdete v tématu hello [sledování závislosti odchozí](#outgoing-dependencies-tracking) části.</span><span class="sxs-lookup"><span data-stu-id="e08f1-205">For more information, see hello [Outgoing dependencies tracking](#outgoing-dependencies-tracking) section.</span></span>

## <a name="long-running-background-tasks"></a><span data-ttu-id="e08f1-206">Dlouho běžící úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="e08f1-206">Long-running background tasks</span></span>
<span data-ttu-id="e08f1-207">Některé aplikace spusťte dlouhotrvající operace, které mohou být způsobeny požadavků uživatele.</span><span class="sxs-lookup"><span data-stu-id="e08f1-207">Some applications start long-running operations that might be caused by user requests.</span></span> <span data-ttu-id="e08f1-208">Z hlediska hello trasování nebo instrumentace se neliší od instrumentace požadavku nebo závislost:</span><span class="sxs-lookup"><span data-stu-id="e08f1-208">From hello tracing/instrumentation perspective, it's not different from request or dependency instrumentation:</span></span> 

``` C#
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process hello task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

<span data-ttu-id="e08f1-209">V tomto příkladu používáme `telemetryClient.StartOperation` toocreate `RequestTelemetry` a výplně hello korelace kontextu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-209">In this example, we use `telemetryClient.StartOperation` toocreate `RequestTelemetry` and fill hello correlation context.</span></span> <span data-ttu-id="e08f1-210">Řekněme, že máte nadřazené operace, který byl vytvořen příchozích požadavků, které naplánovanou operaci hello.</span><span class="sxs-lookup"><span data-stu-id="e08f1-210">Let's say you have a parent operation that was created by incoming requests that scheduled hello operation.</span></span> <span data-ttu-id="e08f1-211">Tak dlouho, dokud `BackgroundTask` spustí v hello stejného asynchronní řízení toku jako příchozí žádosti, je vztažen v této operaci nadřazené.</span><span class="sxs-lookup"><span data-stu-id="e08f1-211">As long as `BackgroundTask` starts in hello same asynchronous control flow as an incoming request, it's correlated with that parent operation.</span></span> <span data-ttu-id="e08f1-212">`BackgroundTask`a všechny vnořené telemetrie položky se automaticky korelační s hello požadavek, který způsobil, i po končí hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e08f1-212">`BackgroundTask` and all nested telemetry items are automatically correlated with hello request that caused it, even after hello request ends.</span></span>

<span data-ttu-id="e08f1-213">Při spuštění úlohy hello z vlákna na pozadí hello, který nemá všechny operace (`Activity`) přidružený `BackgroundTask` nemá některé z nadřazených.</span><span class="sxs-lookup"><span data-stu-id="e08f1-213">When hello task starts from hello background thread that doesn't have any operation (`Activity`) associated with it, `BackgroundTask` doesn't have any parent.</span></span> <span data-ttu-id="e08f1-214">Ale ho můžete vnořené operace.</span><span class="sxs-lookup"><span data-stu-id="e08f1-214">However, it can have nested operations.</span></span> <span data-ttu-id="e08f1-215">Všechny položky telemetrie nahlásila hello úloh jsou korelační toohello `RequestTelemetry` vytvořené v `BackgroundTask`.</span><span class="sxs-lookup"><span data-stu-id="e08f1-215">All telemetry items reported from hello task are correlated toohello `RequestTelemetry` created in `BackgroundTask`.</span></span>

## <a name="outgoing-dependencies-tracking"></a><span data-ttu-id="e08f1-216">Odchozí závislosti sledování</span><span class="sxs-lookup"><span data-stu-id="e08f1-216">Outgoing dependencies tracking</span></span>
<span data-ttu-id="e08f1-217">Můžete sledovat vlastní typ závislosti nebo operace, který není podporován nástrojem Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-217">You can track your own dependency kind or an operation that's not supported by Application Insights.</span></span>

<span data-ttu-id="e08f1-218">Hello `Enqueue` metoda fronty Service Bus hello nebo fronty úložiště hello může sloužit jako příklady takových vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="e08f1-218">hello `Enqueue` method in hello Service Bus queue or hello Storage queue can serve as examples for such custom tracking.</span></span>

<span data-ttu-id="e08f1-219">Hello obecný přístup k vlastní závislost sledování je:</span><span class="sxs-lookup"><span data-stu-id="e08f1-219">hello general approach for custom dependency tracking is to:</span></span>

- <span data-ttu-id="e08f1-220">Volání hello `TelemetryClient.StartOperation` metoda (rozšíření), který vyplní celé hello `DependencyTelemetry` vlastnosti, které jsou potřeba ke korelaci a některé další vlastnosti (čas spuštění razítka, doba trvání).</span><span class="sxs-lookup"><span data-stu-id="e08f1-220">Call hello `TelemetryClient.StartOperation` (extension) method that fills hello `DependencyTelemetry` properties that are needed for correlation and some other properties (start  time stamp, duration).</span></span>
- <span data-ttu-id="e08f1-221">Nastavit další vlastnosti, vlastní na hello `DependencyTelemetry`, jako jsou například název hello a další kontext, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="e08f1-221">Set other custom properties on hello `DependencyTelemetry`, such as hello name and any other context you need.</span></span>
- <span data-ttu-id="e08f1-222">Ujistěte se, volání a počkejte na její závislosti.</span><span class="sxs-lookup"><span data-stu-id="e08f1-222">Make a dependency call and wait for it.</span></span>
- <span data-ttu-id="e08f1-223">Zastavte operaci hello s `StopOperation` po jeho dokončení.</span><span class="sxs-lookup"><span data-stu-id="e08f1-223">Stop hello operation with `StopOperation` when it's finished.</span></span>
- <span data-ttu-id="e08f1-224">Zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="e08f1-224">Handle exceptions.</span></span>

<span data-ttu-id="e08f1-225">`StopOperation`pouze zastaví hello operace, která byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="e08f1-225">`StopOperation` only stops hello operation that was started.</span></span> <span data-ttu-id="e08f1-226">Pokud aktuální běžící operace hello neodpovídá hello jeden chcete toostop, `StopOperation` se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="e08f1-226">If hello current running operation doesn't match hello one you want toostop, `StopOperation` does nothing.</span></span> <span data-ttu-id="e08f1-227">Tato situace může dojít, pokud spustíte více operací paralelně v hello stejný kontext spuštění:</span><span class="sxs-lookup"><span data-stu-id="e08f1-227">This situation might happen if you start multiple operations in parallel in hello same execution context:</span></span>

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for hello first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

<span data-ttu-id="e08f1-228">Ověřte, že je vždy volat `StartOperation` a spustit úlohu v jeho vlastní kontextu:</span><span class="sxs-lookup"><span data-stu-id="e08f1-228">Make sure you always call `StartOperation` and run your task in its own context:</span></span>
```C#
public async Task RunMyTaskAsync()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
    try 
    {
        var myTask = await StartMyTaskAsync();
        // Update status code and success as appropriate.
    }
    catch(...) 
    {
        // Update status code and success as appropriate.
    }
    finally 
    {
        telemetryClient.StopOperation(operation);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="e08f1-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e08f1-229">Next steps</span></span>

- <span data-ttu-id="e08f1-230">Další hello Základy [telemetrie korelace](application-insights-correlation.md) ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e08f1-230">Learn hello basics of [telemetry correlation](application-insights-correlation.md) in Application Insights.</span></span>
- <span data-ttu-id="e08f1-231">V tématu hello [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-231">See hello [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="e08f1-232">Sestavy vlastní [události a metriky](app-insights-api-custom-events-metrics.md) tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="e08f1-232">Report custom [events and metrics](app-insights-api-custom-events-metrics.md) tooApplication Insights.</span></span>
- <span data-ttu-id="e08f1-233">Podívejte se na standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) pro kolekci vlastností kontextu.</span><span class="sxs-lookup"><span data-stu-id="e08f1-233">Check out standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) for context properties collection.</span></span>
- <span data-ttu-id="e08f1-234">Zkontrolujte hello [System.Diagnostics.Activity uživatelská příručka](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee jak jsme korelovat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="e08f1-234">Check hello [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee how we correlate telemetry.</span></span>
