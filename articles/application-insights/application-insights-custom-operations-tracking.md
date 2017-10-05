---
title: "Sledování vlastní operace pomocí .NET SDK služby Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: b31d38fe2f7060597956a1ee9c66f43ce39d7240
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a><span data-ttu-id="25237-103">Sledování vlastní operace s Application Insights .NET SDK</span><span class="sxs-lookup"><span data-stu-id="25237-103">Track custom operations with Application Insights .NET SDK</span></span>

<span data-ttu-id="25237-104">Azure sadách Application Insights SDK automaticky sledovat příchozí požadavky HTTP a volání závislých služeb, jako je například požadavků HTTP a dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="25237-104">Azure Application Insights SDKs automatically track incoming HTTP requests and calls to dependent services, such as HTTP requests and SQL queries.</span></span> <span data-ttu-id="25237-105">Sledování a korelace mezi požadavky a závislosti a získáte přehled o rychlost reakce a spolehlivost celou aplikaci přes všechny mikroslužeb, které spojují této aplikace.</span><span class="sxs-lookup"><span data-stu-id="25237-105">Tracking and correlation of requests and dependencies give you visibility into the whole application's responsiveness and reliability across all microservices that combine this application.</span></span> 

<span data-ttu-id="25237-106">Existuje třída schémat aplikace, které nepodporují se obecně.</span><span class="sxs-lookup"><span data-stu-id="25237-106">There is a class of application patterns that can't be supported generically.</span></span> <span data-ttu-id="25237-107">Správné sledování tyto vzory vyžaduje ruční kód instrumentace.</span><span class="sxs-lookup"><span data-stu-id="25237-107">Proper monitoring of such patterns requires manual code instrumentation.</span></span> <span data-ttu-id="25237-108">Tento článek se zabývá několik vzorů, které můžou vyžadovat ruční instrumentace, jako je například vlastní fronty zpracování a spuštění úlohy dlouho běžící na pozadí.</span><span class="sxs-lookup"><span data-stu-id="25237-108">This article covers a few patterns that might require manual instrumentation, such as custom queue processing and running long-running background tasks.</span></span>

<span data-ttu-id="25237-109">Tento dokument obsahuje pokyny o tom, jak sledovat vlastní operace s Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="25237-109">This document provides guidance on how to track custom operations with the Application Insights SDK.</span></span> <span data-ttu-id="25237-110">Tato dokumentace je důležité pro:</span><span class="sxs-lookup"><span data-stu-id="25237-110">This documentation is relevant for:</span></span>

- <span data-ttu-id="25237-111">Application Insights pro rozhraní .NET (také označované jako základní sady SDK) verze 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="25237-111">Application Insights for .NET (also known as Base SDK) version 2.4+.</span></span>
- <span data-ttu-id="25237-112">Application Insights pro webové aplikace (používající technologii ASP.NET) verze 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="25237-112">Application Insights for web applications (running ASP.NET) version 2.4+.</span></span>
- <span data-ttu-id="25237-113">Application Insights pro ASP.NET Core verze 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="25237-113">Application Insights for ASP.NET Core version 2.1+.</span></span>

## <a name="overview"></a><span data-ttu-id="25237-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="25237-114">Overview</span></span>
<span data-ttu-id="25237-115">Operace je logické část práce spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="25237-115">An operation is a logical piece of work run by an application.</span></span> <span data-ttu-id="25237-116">Má název, čas spuštění, doba trvání, výsledek a kontext spuštění jako uživatelské jméno, vlastnosti a výsledek.</span><span class="sxs-lookup"><span data-stu-id="25237-116">It has a name, start time, duration, result, and a context of execution like user name, properties, and result.</span></span> <span data-ttu-id="25237-117">Pokud byla zahájena operace A operace B, pak operaci B je nastaven jako nadřazený pro A. Operace může mít jen jednu nadřazenou položku, ale může mít mnoho podřízené operací.</span><span class="sxs-lookup"><span data-stu-id="25237-117">If operation A was initiated by operation B, then operation B is set as a parent for A. An operation can have only one parent, but it can have many child operations.</span></span> <span data-ttu-id="25237-118">Další informace o operacích a telemetrie korelace najdete v tématu [Azure Application Insights telemetrie korelace](application-insights-correlation.md).</span><span class="sxs-lookup"><span data-stu-id="25237-118">For more information on operations and telemetry correlation, see [Azure Application Insights telemetry correlation](application-insights-correlation.md).</span></span>

<span data-ttu-id="25237-119">V Application Insights .NET SDK, je popsán operaci abstraktní třída [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) a jeho následníky [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) a [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span><span class="sxs-lookup"><span data-stu-id="25237-119">In the Application Insights .NET SDK, the operation is described by the abstract class [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) and its descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) and [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span></span>

## <a name="incoming-operations-tracking"></a><span data-ttu-id="25237-120">Příchozí operace sledování</span><span class="sxs-lookup"><span data-stu-id="25237-120">Incoming operations tracking</span></span> 
<span data-ttu-id="25237-121">Webové služby Application Insights SDK automaticky shromažďuje požadavky protokolu HTTP pro aplikace ASP.NET, které běží v kanálu služby IIS a všechny aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25237-121">The Application Insights web SDK automatically collects HTTP requests for ASP.NET applications that run in an IIS pipeline and all ASP.NET Core applications.</span></span> <span data-ttu-id="25237-122">Nejsou podporované komunity řešení pro jiné platformy a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="25237-122">There are community-supported solutions for other platforms and frameworks.</span></span> <span data-ttu-id="25237-123">Ale pokud aplikace nepodporují žádné řešení standardní nebo podporována komunity, můžete instrumentovat ji ručně.</span><span class="sxs-lookup"><span data-stu-id="25237-123">However, if the application isn't supported by any of the standard or community-supported solutions, you can instrument it manually.</span></span>

<span data-ttu-id="25237-124">Další příklad, který vyžaduje vlastní sledování je pracovní proces, který přijme položky z fronty.</span><span class="sxs-lookup"><span data-stu-id="25237-124">Another example that requires custom tracking is the worker that receives items from the queue.</span></span> <span data-ttu-id="25237-125">Pro některé fronty volání přidání zprávy do této fronty sledován jako závislost.</span><span class="sxs-lookup"><span data-stu-id="25237-125">For some queues, the call to add a message to this queue is tracked as a dependency.</span></span> <span data-ttu-id="25237-126">Základní operace, která popisuje zpracování zprávy se však nejsou shromažďovány automaticky.</span><span class="sxs-lookup"><span data-stu-id="25237-126">However, the high-level operation that describes message processing is not automatically collected.</span></span>

<span data-ttu-id="25237-127">Podívejme se, jak jsme můžete sledovat tyto operace.</span><span class="sxs-lookup"><span data-stu-id="25237-127">Let's see how we can track such operations.</span></span>

<span data-ttu-id="25237-128">Na vysoké úrovni, úloha je vytvoření `RequestTelemetry` a nastavte známých vlastností.</span><span class="sxs-lookup"><span data-stu-id="25237-128">On a high level, the task is to create `RequestTelemetry` and set known properties.</span></span> <span data-ttu-id="25237-129">Po dokončení operace je sledovat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="25237-129">After the operation is finished, you track the telemetry.</span></span> <span data-ttu-id="25237-130">Následující příklad ukazuje tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="25237-130">The following example demonstrates this task.</span></span>

### <a name="http-request-in-owin-self-hosted-app"></a><span data-ttu-id="25237-131">Požadavek HTTP ve vlastním hostováním aplikace Owin</span><span class="sxs-lookup"><span data-stu-id="25237-131">HTTP request in Owin self-hosted app</span></span>
<span data-ttu-id="25237-132">V tomto příkladu jsme podle [protokolu HTTP pro korelačního](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="25237-132">In this example, we follow the [HTTP Protocol for Correlation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span></span> <span data-ttu-id="25237-133">Jste měli očekávat hlavičky, které jsou popsány existuje.</span><span class="sxs-lookup"><span data-stu-id="25237-133">You should expect to receive headers that are described there.</span></span>

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

        // If there is a Request-Id received from the upstream service, set the telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process the request.
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

            // Now it's time to stop the operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns the root ID from the '|' to the first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

<span data-ttu-id="25237-134">Protokol HTTP pro korelačního také deklaruje `Correlation-Context` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="25237-134">The HTTP Protocol for Correlation also declares the `Correlation-Context` header.</span></span> <span data-ttu-id="25237-135">Nicméně je vynechaný sem pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="25237-135">However, it's omitted here for simplicity.</span></span>

## <a name="queue-instrumentation"></a><span data-ttu-id="25237-136">Fronty instrumentace</span><span class="sxs-lookup"><span data-stu-id="25237-136">Queue instrumentation</span></span>
<span data-ttu-id="25237-137">Pro komunikaci pomocí protokolu HTTP vytvořili jsme protokol předat korelace podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25237-137">For HTTP communication, we've created a protocol to pass correlation details.</span></span> <span data-ttu-id="25237-138">S protokoly některé fronty můžete předat další metadata, společně s zprávu a s ostatními, které není možné.</span><span class="sxs-lookup"><span data-stu-id="25237-138">With some queues' protocols, you can pass additional metadata along with the message, and with others you can't.</span></span>

### <a name="service-bus-queue"></a><span data-ttu-id="25237-139">Fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="25237-139">Service Bus queue</span></span>
<span data-ttu-id="25237-140">S Azure [fronty Service Bus](../service-bus-messaging/index.md), můžete předat kontejneru objektů a dat společně s zprávy.</span><span class="sxs-lookup"><span data-stu-id="25237-140">With the Azure [Service Bus queue](../service-bus-messaging/index.md), you can pass a property bag along with the message.</span></span> <span data-ttu-id="25237-141">Můžeme použít k předání ID korelace.</span><span class="sxs-lookup"><span data-stu-id="25237-141">We use it to pass the correlation ID.</span></span>

<span data-ttu-id="25237-142">Fronty Service Bus používá protokoly založených na protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="25237-142">The Service Bus queue uses TCP-based protocols.</span></span> <span data-ttu-id="25237-143">Application Insights nesleduje automaticky operace fronty, takže jsme sledovat ručně.</span><span class="sxs-lookup"><span data-stu-id="25237-143">Application Insights doesn't automatically track queue operations, so we track them manually.</span></span> <span data-ttu-id="25237-144">Operace dequeue je rozhraní API nabízené style a nemohli jsme ji sledovat.</span><span class="sxs-lookup"><span data-stu-id="25237-144">The dequeue operation is a push-style API, and we're unable to track it.</span></span>

#### <a name="enqueue"></a><span data-ttu-id="25237-145">Zařazování</span><span class="sxs-lookup"><span data-stu-id="25237-145">Enqueue</span></span>

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes the telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows the property bag to pass along with the message.
    // We will use them to pass our correlation identifiers (and other context)
    // to the consumer.
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

#### <a name="process"></a><span data-ttu-id="25237-146">Proces</span><span class="sxs-lookup"><span data-stu-id="25237-146">Process</span></span>
```C#
public async Task Process(BrokeredMessage message)
{
    // After the message is taken from the queue, create RequestTelemetry to track its processing.
    // It might also make sense to get the name from the message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
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

### <a name="azure-storage-queue"></a><span data-ttu-id="25237-147">Fronty Azure Storage</span><span class="sxs-lookup"><span data-stu-id="25237-147">Azure Storage queue</span></span>
<span data-ttu-id="25237-148">Následující příklad ukazuje, jak sledovat [fronty Azure Storage](../storage/queues/storage-dotnet-how-to-use-queues.md) operace a correlate telemetrie mezi Autor, příjemce a Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="25237-148">The following example shows how to track the [Azure Storage queue](../storage/queues/storage-dotnet-how-to-use-queues.md) operations and correlate telemetry between the producer, the consumer, and Azure Storage.</span></span> 

<span data-ttu-id="25237-149">Fronty úložiště používá rozhraní API HTTP.</span><span class="sxs-lookup"><span data-stu-id="25237-149">The Storage queue has an HTTP API.</span></span> <span data-ttu-id="25237-150">Všechna volání do fronty jsou sledovány kolekce Application Insights závislostí pro požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="25237-150">All calls to the queue are tracked by the Application Insights Dependency Collector for HTTP requests.</span></span>
<span data-ttu-id="25237-151">Zajistěte, aby byla `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` v `applicationInsights.config`.</span><span class="sxs-lookup"><span data-stu-id="25237-151">Make sure you have `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`.</span></span> <span data-ttu-id="25237-152">Pokud ji nemáte, přidejte ho programově, jak je popsáno v [filtrování a předběžného zpracování v Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="25237-152">If you don't have it, add it programmatically as described in [Filtering and Preprocessing in the Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span></span>

<span data-ttu-id="25237-153">Pokud nakonfigurujete Application Insights ručně, ujistěte se, vytvoření a inicializace `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` podobně jako:</span><span class="sxs-lookup"><span data-stu-id="25237-153">If you configure Application Insights manually, make sure you create and initialize `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` similarly to:</span></span>
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

<span data-ttu-id="25237-154">Můžete také chtít korelovat Application Insights ID operace s ID úložiště požadavku.</span><span class="sxs-lookup"><span data-stu-id="25237-154">You also might want to correlate the Application Insights operation ID with the Storage request ID.</span></span> <span data-ttu-id="25237-155">Informace o tom, jak nastavit a získat požadavek klienta úložiště a ID požadavku serveru najdete v tématu [monitorování, Diagnostika a řešení potíží s Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span><span class="sxs-lookup"><span data-stu-id="25237-155">For information on how to set and get a Storage request client and a server request ID, see [Monitor, diagnose, and troubleshoot Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span></span>

#### <a name="enqueue"></a><span data-ttu-id="25237-156">Zařazování</span><span class="sxs-lookup"><span data-stu-id="25237-156">Enqueue</span></span>
<span data-ttu-id="25237-157">Protože toto rozhraní API HTTP podporují fronty úložiště, všechny operace s fronty sledovány automaticky pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-157">Because Storage queues support the HTTP API, all operations with the queue are automatically tracked by Application Insights.</span></span> <span data-ttu-id="25237-158">V mnoha případech tato instrumentace by vám měly dostatečně.</span><span class="sxs-lookup"><span data-stu-id="25237-158">In many cases, this instrumentation should be enough.</span></span> <span data-ttu-id="25237-159">Ale ke korelaci trasování na straně příjemce s producent trasování, je nutné předat některé korelace kontextu podobně do jak jsme se v protokolu HTTP pro korelačního.</span><span class="sxs-lookup"><span data-stu-id="25237-159">However, to correlate traces on the consumer side with producer traces, you must pass some correlation context similarly to how we do it in the HTTP Protocol for Correlation.</span></span> 

<span data-ttu-id="25237-160">V tomto příkladu jsme sledovat nepovinný `Enqueue` operaci.</span><span class="sxs-lookup"><span data-stu-id="25237-160">In this example, we track the optional `Enqueue` operation.</span></span> <span data-ttu-id="25237-161">Můžete:</span><span class="sxs-lookup"><span data-stu-id="25237-161">You can:</span></span>

 - <span data-ttu-id="25237-162">**Korelovat opakovaných pokusů (pokud existuje)**: budou všechny mít jeden společný nadřazených, který má `Enqueue` operaci.</span><span class="sxs-lookup"><span data-stu-id="25237-162">**Correlate retries (if any)**: They all have one common parent that's the `Enqueue` operation.</span></span> <span data-ttu-id="25237-163">Jinak se sledují jako podřízené objekty příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="25237-163">Otherwise, they're tracked as children of the incoming request.</span></span> <span data-ttu-id="25237-164">Pokud se několik logické požadavků do fronty, může být obtížné vyhledat, které volání výsledkem opakování.</span><span class="sxs-lookup"><span data-stu-id="25237-164">If there are multiple logical requests to the queue, it might be difficult to find which call resulted in retries.</span></span>
 - <span data-ttu-id="25237-165">**Korelovat protokoly úložiště (Pokud je potřeba)**: jste korelační s telemetrie Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-165">**Correlate Storage logs (if and when needed)**: They're correlated with Application Insights telemetry.</span></span>

<span data-ttu-id="25237-166">`Enqueue` Operaci je podřízeným nadřazené operace (například příchozí žádosti HTTP).</span><span class="sxs-lookup"><span data-stu-id="25237-166">The `Enqueue` operation is the child of a parent operation (for example, an incoming HTTP request).</span></span> <span data-ttu-id="25237-167">Závislost volání protokolu HTTP je podřízeným `Enqueue` operace a pod podřízená příchozích požadavků:</span><span class="sxs-lookup"><span data-stu-id="25237-167">The HTTP dependency call is the child of the `Enqueue` operation and the grandchild of the incoming request:</span></span>

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose to pass payload serialized to JSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Storage logs and Application Insights telemetry.
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

<span data-ttu-id="25237-168">Ke snížení objemu telemetrie aplikace sestavy nebo pokud nechcete, aby ke sledování `Enqueue` operace z jiných důvodů, použijte `Activity` přímo rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="25237-168">To reduce the amount of telemetry your application reports or if you don't want to track the `Enqueue` operation for other reasons, use the `Activity` API directly:</span></span>

- <span data-ttu-id="25237-169">Vytvořit (a spuštění) novou `Activity` místo spuštění operace Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-169">Create (and start) a new `Activity` instead of starting the Application Insights operation.</span></span> <span data-ttu-id="25237-170">Provedete *není* je třeba přiřadit žádné vlastnosti v něm kromě název operace.</span><span class="sxs-lookup"><span data-stu-id="25237-170">You do *not* need to assign any properties on it except the operation name.</span></span>
- <span data-ttu-id="25237-171">Serializovat `yourActivity.Id` do datové části zprávy místo `operation.Telemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="25237-171">Serialize `yourActivity.Id` into the message payload instead of `operation.Telemetry.Id`.</span></span> <span data-ttu-id="25237-172">Můžete také použít `Activity.Current.Id`.</span><span class="sxs-lookup"><span data-stu-id="25237-172">You can also use `Activity.Current.Id`.</span></span>


#### <a name="dequeue"></a><span data-ttu-id="25237-173">Dequeue –</span><span class="sxs-lookup"><span data-stu-id="25237-173">Dequeue</span></span>
<span data-ttu-id="25237-174">Podobně jako `Enqueue`, vlastní požadavek HTTP do fronty úložiště je automaticky sledován pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-174">Similarly to `Enqueue`, an actual HTTP request to the Storage queue is automatically tracked by Application Insights.</span></span> <span data-ttu-id="25237-175">Ale `Enqueue` operaci pravděpodobně dojde v kontextu nadřazené, jako je například kontextu příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="25237-175">However, the `Enqueue` operation presumably happens in the parent context, such as an incoming request context.</span></span> <span data-ttu-id="25237-176">Sadách Application Insights SDK automaticky korelovat takovou operaci (a jeho součástí HTTP) s nadřazené žádosti a další telemetrií hlášené ve stejném oboru.</span><span class="sxs-lookup"><span data-stu-id="25237-176">Application Insights SDKs automatically correlate such an operation (and its HTTP part) with the parent request and other telemetry reported in the same scope.</span></span>

<span data-ttu-id="25237-177">`Dequeue` Operace je složité.</span><span class="sxs-lookup"><span data-stu-id="25237-177">The `Dequeue` operation is tricky.</span></span> <span data-ttu-id="25237-178">Application Insights SDK automaticky sleduje požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="25237-178">The Application Insights SDK automatically tracks HTTP requests.</span></span> <span data-ttu-id="25237-179">Ale neví kontext korelace, dokud je analyzován zprávu.</span><span class="sxs-lookup"><span data-stu-id="25237-179">However, it doesn't know the correlation context until the message is parsed.</span></span> <span data-ttu-id="25237-180">Není možné ke korelaci požadavku HTTP k získání zprávy se zbytkem telemetrii.</span><span class="sxs-lookup"><span data-stu-id="25237-180">It's not possible to correlate the HTTP request to get the message with the rest of the telemetry.</span></span>

<span data-ttu-id="25237-181">V mnoha případech může být užitečné ke korelaci s také další trasování požadavku HTTP do fronty.</span><span class="sxs-lookup"><span data-stu-id="25237-181">In many cases, it might be useful to correlate the HTTP request to the queue with other traces as well.</span></span> <span data-ttu-id="25237-182">Následující příklad ukazuje, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="25237-182">The following example demonstrates how to do it:</span></span>

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

            // If there is a message, we want to correlate the Dequeue operation with processing.
            // However, we will only know what correlation ID to use after we get it from the message,
            // so we will report telemetry after we know the IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete the message.
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

#### <a name="process"></a><span data-ttu-id="25237-183">Proces</span><span class="sxs-lookup"><span data-stu-id="25237-183">Process</span></span>

<span data-ttu-id="25237-184">V následujícím příkladu jsme trasování příchozí zprávy způsobem podobně pro jak jsme trasování příchozí žádosti HTTP:</span><span class="sxs-lookup"><span data-stu-id="25237-184">In the following example, we trace an incoming message in a manner similarly to how we trace an incoming HTTP request:</span></span>

```C#
public async Task Process(MessagePayload message)
{
    // After the message is dequeued from the queue, create RequestTelemetry to track its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense to get the name from the message.
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

<span data-ttu-id="25237-185">Podobně můžete instrumentovány jiné operace fronty.</span><span class="sxs-lookup"><span data-stu-id="25237-185">Similarly, other queue operations can be instrumented.</span></span> <span data-ttu-id="25237-186">Funkce Náhled operace by měla instrumentována podobným způsobem jako operace dequeue.</span><span class="sxs-lookup"><span data-stu-id="25237-186">A peek operation should be instrumented in a similar way as a dequeue operation.</span></span> <span data-ttu-id="25237-187">Instrumentace operace fronty správy není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="25237-187">Instrumenting queue management operations isn't necessary.</span></span> <span data-ttu-id="25237-188">Application Insights sleduje operací, jako je například HTTP a ve většině případů je dost.</span><span class="sxs-lookup"><span data-stu-id="25237-188">Application Insights tracks operations such as HTTP, and in most cases, it's enough.</span></span>

<span data-ttu-id="25237-189">Když jste instrumentace odstranění zprávy, nezapomeňte že nastavit operaci identifikátory (korelace).</span><span class="sxs-lookup"><span data-stu-id="25237-189">When you instrument message deletion, make sure you set the operation (correlation) identifiers.</span></span> <span data-ttu-id="25237-190">Alternativně můžete použít `Activity` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25237-190">Alternatively, you can use the `Activity` API.</span></span> <span data-ttu-id="25237-191">Pak nemusíte na položky telemetrie nastavit identifikátory operaci, protože Application Insights provede za vás:</span><span class="sxs-lookup"><span data-stu-id="25237-191">Then you don't need to set operation identifiers on the telemetry items because Application Insights does it for you:</span></span>

- <span data-ttu-id="25237-192">Vytvořte novou `Activity` po vám položky z fronty.</span><span class="sxs-lookup"><span data-stu-id="25237-192">Create a new `Activity` after you've got an item from the queue.</span></span>
- <span data-ttu-id="25237-193">Použití `Activity.SetParentId(message.ParentId)` ke korelaci protokolů příjemce a výrobce.</span><span class="sxs-lookup"><span data-stu-id="25237-193">Use `Activity.SetParentId(message.ParentId)` to correlate consumer and producer logs.</span></span>
- <span data-ttu-id="25237-194">Spuštění `Activity`.</span><span class="sxs-lookup"><span data-stu-id="25237-194">Start the `Activity`.</span></span>
- <span data-ttu-id="25237-195">Sledování dequeue – zpracovat a operace odstranění pomocí `Start/StopOperation` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="25237-195">Track dequeue, process, and delete operations by using `Start/StopOperation` helpers.</span></span> <span data-ttu-id="25237-196">To proveďte z stejného asynchronní řízení toku (kontext spuštění).</span><span class="sxs-lookup"><span data-stu-id="25237-196">Do it from the same asynchronous control flow (execution context).</span></span> <span data-ttu-id="25237-197">Tímto způsobem že jste korelační správně.</span><span class="sxs-lookup"><span data-stu-id="25237-197">In this way, they're correlated properly.</span></span>
- <span data-ttu-id="25237-198">Zastavit `Activity`.</span><span class="sxs-lookup"><span data-stu-id="25237-198">Stop the `Activity`.</span></span>
- <span data-ttu-id="25237-199">Použití `Start/StopOperation`, nebo volejte `Track` telemetrie ručně.</span><span class="sxs-lookup"><span data-stu-id="25237-199">Use `Start/StopOperation`, or call `Track` telemetry manually.</span></span>

### <a name="batch-processing"></a><span data-ttu-id="25237-200">Dávkové zpracování</span><span class="sxs-lookup"><span data-stu-id="25237-200">Batch processing</span></span>
<span data-ttu-id="25237-201">Některé fronty můžete dequeue – více zpráv s jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="25237-201">With some queues, you can dequeue multiple messages with one request.</span></span> <span data-ttu-id="25237-202">Zpracování takové zprávy je pravděpodobně z důvodu nezávislé a patří do různých logických operací.</span><span class="sxs-lookup"><span data-stu-id="25237-202">Processing such messages is presumably independent and belongs to the different logical operations.</span></span> <span data-ttu-id="25237-203">V takovém případě není možné ke korelaci `Dequeue` na konkrétní zprávu zpracování operace.</span><span class="sxs-lookup"><span data-stu-id="25237-203">In this case, it's not possible to correlate the `Dequeue` operation to particular message processing.</span></span>

<span data-ttu-id="25237-204">Každá zpráva, měla by být zpracována v jeho vlastní asynchronní řízení toku.</span><span class="sxs-lookup"><span data-stu-id="25237-204">Each message should be processed in its own asynchronous control flow.</span></span> <span data-ttu-id="25237-205">Další informace najdete v tématu [sledování závislosti odchozí](#outgoing-dependencies-tracking) části.</span><span class="sxs-lookup"><span data-stu-id="25237-205">For more information, see the [Outgoing dependencies tracking](#outgoing-dependencies-tracking) section.</span></span>

## <a name="long-running-background-tasks"></a><span data-ttu-id="25237-206">Dlouho běžící úlohy na pozadí</span><span class="sxs-lookup"><span data-stu-id="25237-206">Long-running background tasks</span></span>
<span data-ttu-id="25237-207">Některé aplikace spusťte dlouhotrvající operace, které mohou být způsobeny požadavků uživatele.</span><span class="sxs-lookup"><span data-stu-id="25237-207">Some applications start long-running operations that might be caused by user requests.</span></span> <span data-ttu-id="25237-208">Z pohledu trasování nebo instrumentace se neliší od instrumentace požadavku nebo závislost:</span><span class="sxs-lookup"><span data-stu-id="25237-208">From the tracing/instrumentation perspective, it's not different from request or dependency instrumentation:</span></span> 

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
            // Process the task.
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

<span data-ttu-id="25237-209">V tomto příkladu používáme `telemetryClient.StartOperation` k vytvoření `RequestTelemetry` a vyplnění kontext korelace.</span><span class="sxs-lookup"><span data-stu-id="25237-209">In this example, we use `telemetryClient.StartOperation` to create `RequestTelemetry` and fill the correlation context.</span></span> <span data-ttu-id="25237-210">Řekněme, že máte nadřazené operace, který byl vytvořen příchozích požadavků, které naplánované operace.</span><span class="sxs-lookup"><span data-stu-id="25237-210">Let's say you have a parent operation that was created by incoming requests that scheduled the operation.</span></span> <span data-ttu-id="25237-211">Tak dlouho, dokud `BackgroundTask` spustí ve stejném asynchronní řízení toku jako příchozí žádosti, je vztažen v této operaci nadřazené.</span><span class="sxs-lookup"><span data-stu-id="25237-211">As long as `BackgroundTask` starts in the same asynchronous control flow as an incoming request, it's correlated with that parent operation.</span></span> <span data-ttu-id="25237-212">`BackgroundTask`a všechny vnořené telemetrie položky se automaticky korelační s požadavkem, který způsobuje její neočekávané, i po ukončení požadavku.</span><span class="sxs-lookup"><span data-stu-id="25237-212">`BackgroundTask` and all nested telemetry items are automatically correlated with the request that caused it, even after the request ends.</span></span>

<span data-ttu-id="25237-213">Při spuštění úlohy ze vlákně na pozadí, který nemá všechny operace (`Activity`) přidružený `BackgroundTask` nemá některé z nadřazených.</span><span class="sxs-lookup"><span data-stu-id="25237-213">When the task starts from the background thread that doesn't have any operation (`Activity`) associated with it, `BackgroundTask` doesn't have any parent.</span></span> <span data-ttu-id="25237-214">Ale ho můžete vnořené operace.</span><span class="sxs-lookup"><span data-stu-id="25237-214">However, it can have nested operations.</span></span> <span data-ttu-id="25237-215">Všechny položky telemetrie nahlásila úlohy jsou korelována `RequestTelemetry` vytvořené v `BackgroundTask`.</span><span class="sxs-lookup"><span data-stu-id="25237-215">All telemetry items reported from the task are correlated to the `RequestTelemetry` created in `BackgroundTask`.</span></span>

## <a name="outgoing-dependencies-tracking"></a><span data-ttu-id="25237-216">Odchozí závislosti sledování</span><span class="sxs-lookup"><span data-stu-id="25237-216">Outgoing dependencies tracking</span></span>
<span data-ttu-id="25237-217">Můžete sledovat vlastní typ závislosti nebo operace, který není podporován nástrojem Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-217">You can track your own dependency kind or an operation that's not supported by Application Insights.</span></span>

<span data-ttu-id="25237-218">`Enqueue` Metoda v Service Bus fronty nebo fronty úložiště může sloužit jako příklady takových vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="25237-218">The `Enqueue` method in the Service Bus queue or the Storage queue can serve as examples for such custom tracking.</span></span>

<span data-ttu-id="25237-219">Je obecný přístup k vlastní závislost sledování:</span><span class="sxs-lookup"><span data-stu-id="25237-219">The general approach for custom dependency tracking is to:</span></span>

- <span data-ttu-id="25237-220">Volání `TelemetryClient.StartOperation` metoda (rozšíření), který vyplní celé `DependencyTelemetry` vlastnosti, které jsou potřeba ke korelaci a některé další vlastnosti (čas spuštění razítka, doba trvání).</span><span class="sxs-lookup"><span data-stu-id="25237-220">Call the `TelemetryClient.StartOperation` (extension) method that fills the `DependencyTelemetry` properties that are needed for correlation and some other properties (start  time stamp, duration).</span></span>
- <span data-ttu-id="25237-221">Nastavit další vlastní vlastnosti, na `DependencyTelemetry`, třeba název a další kontext, je nutné.</span><span class="sxs-lookup"><span data-stu-id="25237-221">Set other custom properties on the `DependencyTelemetry`, such as the name and any other context you need.</span></span>
- <span data-ttu-id="25237-222">Ujistěte se, volání a počkejte na její závislosti.</span><span class="sxs-lookup"><span data-stu-id="25237-222">Make a dependency call and wait for it.</span></span>
- <span data-ttu-id="25237-223">Zastavte operaci s `StopOperation` po jeho dokončení.</span><span class="sxs-lookup"><span data-stu-id="25237-223">Stop the operation with `StopOperation` when it's finished.</span></span>
- <span data-ttu-id="25237-224">Zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="25237-224">Handle exceptions.</span></span>

<span data-ttu-id="25237-225">`StopOperation`zastaví pouze operace, která byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="25237-225">`StopOperation` only stops the operation that was started.</span></span> <span data-ttu-id="25237-226">Pokud aktuální běžící operace neodpovídá ten, který chcete zastavit, `StopOperation` se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="25237-226">If the current running operation doesn't match the one you want to stop, `StopOperation` does nothing.</span></span> <span data-ttu-id="25237-227">Tato situace může dojít, pokud spustíte více operací paralelně ve stejném kontextu spuštění:</span><span class="sxs-lookup"><span data-stu-id="25237-227">This situation might happen if you start multiple operations in parallel in the same execution context:</span></span>

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

<span data-ttu-id="25237-228">Ověřte, že je vždy volat `StartOperation` a spustit úlohu v jeho vlastní kontextu:</span><span class="sxs-lookup"><span data-stu-id="25237-228">Make sure you always call `StartOperation` and run your task in its own context:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="25237-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25237-229">Next steps</span></span>

- <span data-ttu-id="25237-230">Seznámíte se základy [telemetrie korelace](application-insights-correlation.md) ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="25237-230">Learn the basics of [telemetry correlation](application-insights-correlation.md) in Application Insights.</span></span>
- <span data-ttu-id="25237-231">Najdete v článku [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="25237-231">See the [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="25237-232">Sestavy vlastní [události a metriky](app-insights-api-custom-events-metrics.md) Application insights.</span><span class="sxs-lookup"><span data-stu-id="25237-232">Report custom [events and metrics](app-insights-api-custom-events-metrics.md) to Application Insights.</span></span>
- <span data-ttu-id="25237-233">Podívejte se na standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) pro kolekci vlastností kontextu.</span><span class="sxs-lookup"><span data-stu-id="25237-233">Check out standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) for context properties collection.</span></span>
- <span data-ttu-id="25237-234">Zkontrolujte [System.Diagnostics.Activity uživatelská příručka](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) zobrazíte, jak jsme korelovat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="25237-234">Check the [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) to see how we correlate telemetry.</span></span>
