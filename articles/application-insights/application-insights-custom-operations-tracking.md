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
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Sledování vlastní operace s Application Insights .NET SDK

Azure sadách Application Insights SDK automaticky sledovat HTTP příchozí požadavky a volá toodependent služby, jako je například požadavků HTTP a dotazy SQL. Sledování a korelace mezi požadavky a závislosti a získáte přehled o rychlost reakce a spolehlivost hello celou aplikaci přes všechny mikroslužeb, které spojují této aplikace. 

Existuje třída schémat aplikace, které nepodporují se obecně. Správné sledování tyto vzory vyžaduje ruční kód instrumentace. Tento článek se zabývá několik vzorů, které můžou vyžadovat ruční instrumentace, jako je například vlastní fronty zpracování a spuštění úlohy dlouho běžící na pozadí.

Tento dokument obsahuje pokyny k jak tootrack vlastní operace s hello Application Insights SDK. Tato dokumentace je důležité pro:

- Application Insights pro rozhraní .NET (také označované jako základní sady SDK) verze 2.4 +.
- Application Insights pro webové aplikace (používající technologii ASP.NET) verze 2.4 +.
- Application Insights pro ASP.NET Core verze 2.1 +.

## <a name="overview"></a>Přehled
Operace je logické část práce spustit aplikace. Má název, čas spuštění, doba trvání, výsledek a kontext spuštění jako uživatelské jméno, vlastnosti a výsledek. Pokud byla zahájena operace A operace B, pak operaci B je nastaven jako nadřazený pro A. Operace může mít jen jednu nadřazenou položku, ale může mít mnoho podřízené operací. Další informace o operacích a telemetrie korelace najdete v tématu [Azure Application Insights telemetrie korelace](application-insights-correlation.md).

V hello Application Insights .NET SDK, je popsán hello operaci hello abstraktní třída [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) a jeho následníky [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) a [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Příchozí operace sledování 
Hello webové služby Application Insights SDK automaticky shromažďuje požadavky protokolu HTTP pro aplikace ASP.NET, které běží v kanálu služby IIS a všechny aplikace ASP.NET Core. Nejsou podporované komunity řešení pro jiné platformy a rozhraní. Ale pokud aplikace hello nepodporuje hello standard nebo podporovaná komunitou řešení, můžete instrumentovat ji ručně.

Další příklad, který vyžaduje vlastní sledování je hello pracovního procesu, která přijímá položky z fronty hello. Pro některé fronty hello volání tooadd zprávu, kterou toothis fronty je sledovat jako závislost. Hello základní operace, která popisuje zpracování zprávy se však nejsou shromažďovány automaticky.

Podívejme se, jak jsme můžete sledovat tyto operace.

Na vysoké úrovni, úloha hello je toocreate `RequestTelemetry` a nastavte známých vlastností. Po dokončení operace hello je sledovat hello telemetrie. Hello následující příklad ukazuje tuto úlohu.

### <a name="http-request-in-owin-self-hosted-app"></a>Požadavek HTTP ve vlastním hostováním aplikace Owin
V tomto příkladu jsme podle hello [protokolu HTTP pro korelačního](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Byste měli očekávat tooreceive hlavičky, které jsou popsány existuje.

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

Hello protokolu HTTP pro korelačního také deklaruje hello `Correlation-Context` záhlaví. Nicméně je vynechaný sem pro jednoduchost.

## <a name="queue-instrumentation"></a>Fronty instrumentace
Pro komunikaci pomocí protokolu HTTP vytvořili jsme protokol toopass korelace podrobnosti. S protokoly některé fronty můžete předat další metadata, společně s uvítací zprávu a s ostatními, které není možné.

### <a name="service-bus-queue"></a>Fronty Service Bus
S hello Azure [fronty Service Bus](../service-bus-messaging/index.md), můžete předat kontejneru objektů a dat společně s uvítací zprávu. Použití ID toopass hello korelace.

fronty Service Bus Hello používá protokoly založených na protokolu TCP. Application Insights nesleduje automaticky operace fronty, takže jsme sledovat ručně. dequeue – Hello operace je rozhraní API nabízené style a máme nelze tootrack ho.

#### <a name="enqueue"></a>Zařazování

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

#### <a name="process"></a>Proces
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

### <a name="azure-storage-queue"></a>Fronty Azure Storage
Následující příklad ukazuje, jak Hello tootrack hello [fronty Azure Storage](../storage/queues/storage-dotnet-how-to-use-queues.md) operace a correlate telemetrie mezi hello producent, hello příjemce a Azure Storage. 

fronty úložiště Hello používá rozhraní API HTTP. Všechny fronty toohello volání jsou sledovány objektem hello Application Insights závislostí kolekce pro požadavky HTTP.
Zajistěte, aby byla `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` v `applicationInsights.config`. Pokud ji nemáte, přidejte ho programově, jak je popsáno v [filtrování a předzpracování v hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).

Pokud nakonfigurujete Application Insights ručně, ujistěte se, vytvoření a inicializace `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` podobně jako:
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

Můžete také chtít toocorrelate hello Application Insights ID operace s ID hello úložiště požadavku. Informace o tom, jak tooset a získání úložiště požadavků klienta a ID žádosti serveru najdete v tématu [monitorování, Diagnostika a řešení potíží s Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Zařazování
Fronty úložiště nepodporují hello rozhraní API HTTP, všechny operace s frontou hello sledovány automaticky pomocí Application Insights. V mnoha případech tato instrumentace by vám měly dostatečně. Ale toocorrelate trasování na straně příjemce hello s producent trasování, je nutné předat některé korelace kontextu podobně toohow jsme to provést hello protokolu HTTP pro korelačního. 

V tomto příkladu jsme sledovat hello volitelné `Enqueue` operaci. Můžete:

 - **Korelovat opakovaných pokusů (pokud existuje)**: všechny mají jeden společným nadřazeným prvkem, který je hello `Enqueue` operaci. Jinak se sledují jako podřízené objekty hello příchozího požadavku. Pokud existují více logických požadavky toohello fronty, může být obtížné toofind výsledkem které volání opakování.
 - **Korelovat protokoly úložiště (Pokud je potřeba)**: jste korelační s telemetrie Application Insights.

Hello `Enqueue` operaci je podřízeným hello nadřazené operace (například příchozí žádosti HTTP). volání závislostí Hello HTTP je podřízeným hello hello `Enqueue` operace a hello pod podřízená hello příchozích požadavků:

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

sestavy aplikace tooreduce hello množství telemetrie nebo pokud nechcete, aby tootrack hello `Enqueue` operace z jiných důvodů, použijte hello `Activity` přímo rozhraní API:

- Vytvořit (a spuštění) novou `Activity` místo spuštění operace hello Application Insights. Provedete *není* potřebovat tooassign žádné vlastnosti v něm kromě název operace hello.
- Serializovat `yourActivity.Id` do datovou část zprávy hello místo `operation.Telemetry.Id`. Můžete také použít `Activity.Current.Id`.


#### <a name="dequeue"></a>Dequeue –
Podobně příliš`Enqueue`, frontu úložiště služby skutečné HTTP žádost toohello automaticky sledován pomocí Application Insights. Ale hello `Enqueue` operaci pravděpodobně dojde v kontextu nadřazené hello, jako je například kontextu příchozí žádosti. Sadách Application Insights SDK automaticky korelovat takovou operaci (a jeho součástí HTTP) s nadřazenou žádostí hello a další telemetrií uvedený v hello stejný obor.

Hello `Dequeue` operace je složité. Hello Application Insights SDK automaticky sleduje požadavky HTTP. Ale neví hello korelace kontextu, dokud je analyzována uvítací zprávu. Není možné toocorrelate hello HTTP žádost tooget uvítací zprávu se zbytkem hello hello telemetrie.

V mnoha případech může být užitečné toocorrelate toohello fronty požadavků HTTP hello se také další trasování. Hello následující příklad ukazuje, jak toodo ho:

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

#### <a name="process"></a>Proces

V následujícím příkladu hello, jsme příchozí zprávu trasování způsobem podobně toohow jsme trasování HTTP příchozí žádosti:

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

Podobně můžete instrumentovány jiné operace fronty. Funkce Náhled operace by měla instrumentována podobným způsobem jako operace dequeue. Instrumentace operace fronty správy není nezbytné. Application Insights sleduje operací, jako je například HTTP a ve většině případů je dost.

Pokud jste instrumentace odstranění zprávy, ujistěte se, že nastavíte hello operaci identifikátory (korelace). Alternativně můžete použít hello `Activity` rozhraní API. Protože Application Insights provede za vás pak není třeba identifikátory operace tooset na hello telemetrie položky:

- Vytvořte novou `Activity` po vám položky z fronty hello.
- Použití `Activity.SetParentId(message.ParentId)` toocorrelate příjemce a producent protokoly.
- Spustit hello `Activity`.
- Sledování dequeue – zpracovat a operace odstranění pomocí `Start/StopOperation` pomocné rutiny. Provést z hello stejné asynchronní řízení toku (kontext spuštění). Tímto způsobem že jste korelační správně.
- Zastavení hello `Activity`.
- Použití `Start/StopOperation`, nebo volejte `Track` telemetrie ručně.

### <a name="batch-processing"></a>Dávkové zpracování
Některé fronty můžete dequeue – více zpráv s jeden požadavek. Zpracování takové zprávy je pravděpodobně z důvodu nezávislé a patří toohello různých logických operací. V takovém případě není možné toocorrelate hello `Dequeue` zpracování zprávy tooparticular operaci.

Každá zpráva, měla by být zpracována v jeho vlastní asynchronní řízení toku. Další informace najdete v tématu hello [sledování závislosti odchozí](#outgoing-dependencies-tracking) části.

## <a name="long-running-background-tasks"></a>Dlouho běžící úlohy na pozadí
Některé aplikace spusťte dlouhotrvající operace, které mohou být způsobeny požadavků uživatele. Z hlediska hello trasování nebo instrumentace se neliší od instrumentace požadavku nebo závislost: 

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

V tomto příkladu používáme `telemetryClient.StartOperation` toocreate `RequestTelemetry` a výplně hello korelace kontextu. Řekněme, že máte nadřazené operace, který byl vytvořen příchozích požadavků, které naplánovanou operaci hello. Tak dlouho, dokud `BackgroundTask` spustí v hello stejného asynchronní řízení toku jako příchozí žádosti, je vztažen v této operaci nadřazené. `BackgroundTask`a všechny vnořené telemetrie položky se automaticky korelační s hello požadavek, který způsobil, i po končí hello požadavku.

Při spuštění úlohy hello z vlákna na pozadí hello, který nemá všechny operace (`Activity`) přidružený `BackgroundTask` nemá některé z nadřazených. Ale ho můžete vnořené operace. Všechny položky telemetrie nahlásila hello úloh jsou korelační toohello `RequestTelemetry` vytvořené v `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Odchozí závislosti sledování
Můžete sledovat vlastní typ závislosti nebo operace, který není podporován nástrojem Application Insights.

Hello `Enqueue` metoda fronty Service Bus hello nebo fronty úložiště hello může sloužit jako příklady takových vlastní sledování.

Hello obecný přístup k vlastní závislost sledování je:

- Volání hello `TelemetryClient.StartOperation` metoda (rozšíření), který vyplní celé hello `DependencyTelemetry` vlastnosti, které jsou potřeba ke korelaci a některé další vlastnosti (čas spuštění razítka, doba trvání).
- Nastavit další vlastnosti, vlastní na hello `DependencyTelemetry`, jako jsou například název hello a další kontext, budete potřebovat.
- Ujistěte se, volání a počkejte na její závislosti.
- Zastavte operaci hello s `StopOperation` po jeho dokončení.
- Zpracování výjimek.

`StopOperation`pouze zastaví hello operace, která byla spuštěna. Pokud aktuální běžící operace hello neodpovídá hello jeden chcete toostop, `StopOperation` se nic nestane. Tato situace může dojít, pokud spustíte více operací paralelně v hello stejný kontext spuštění:

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

Ověřte, že je vždy volat `StartOperation` a spustit úlohu v jeho vlastní kontextu:
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

## <a name="next-steps"></a>Další kroky

- Další hello Základy [telemetrie korelace](application-insights-correlation.md) ve službě Application Insights.
- V tématu hello [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Sestavy vlastní [události a metriky](app-insights-api-custom-events-metrics.md) tooApplication statistiky.
- Podívejte se na standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) pro kolekci vlastností kontextu.
- Zkontrolujte hello [System.Diagnostics.Activity uživatelská příručka](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee jak jsme korelovat telemetrii.
