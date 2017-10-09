---
title: aaaAzure Service Fabric reverse proxy diagnostiky | Microsoft Docs
description: "Zjistěte, jak toomonitor a diagnostikovat zpracování požadavku na reverzní proxy server hello."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: 9687b9688dc26ba619cbdfab1b1f49a3035345c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a><span data-ttu-id="83a30-103">Monitorování a diagnostikovat zpracování požadavku na reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="83a30-103">Monitor and diagnose request processing at hello reverse proxy</span></span>

<span data-ttu-id="83a30-104">Od verze hello 5.7 Service Fabric, reverzní proxy server události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="83a30-104">Starting with hello 5.7 release of Service Fabric, reverse proxy events are available for collection.</span></span> <span data-ttu-id="83a30-105">nejsou k dispozici ve dvou kanálů Hello události s pouze chybové události související s toorequest selhání zpracování na reverzní proxy server hello a druhý kanál obsahující podrobné události se záznamy pro úspěšných i neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="83a30-105">hello events are available in two channels, one with only error events related toorequest processing failure at hello reverse proxy and second channel containing verbose events with entries for both successful and failed requests.</span></span>

<span data-ttu-id="83a30-106">Odkazovat příliš[shromažďování událostí reverzní proxy server](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable shromažďování událostí z těchto kanálů v místní a clustery Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="83a30-106">Refer too[Collect reverse proxy events](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable collecting events from these channels in local and Azure Service Fabric clusters.</span></span>

## <a name="troubleshoot-using-diagnostics-logs"></a><span data-ttu-id="83a30-107">Řešení problémů pomocí protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="83a30-107">Troubleshoot using diagnostics logs</span></span>
<span data-ttu-id="83a30-108">Zde jsou některé příklady na tom, jak můžete setkat než toointerpret hello běžné selhání protokoly:</span><span class="sxs-lookup"><span data-stu-id="83a30-108">Here are some examples on how toointerpret hello common failure logs that one can encounter:</span></span>

1. <span data-ttu-id="83a30-109">Reverzní proxy server vrátí stavový kód odpovědi 504 (časový limit).</span><span class="sxs-lookup"><span data-stu-id="83a30-109">Reverse proxy returns response status code 504 (Timeout).</span></span>

    <span data-ttu-id="83a30-110">Jedním z důvodů může být z důvodu selhání služby toohello tooreply během časového limitu požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="83a30-110">One reason could be due toohello service failing tooreply within hello request timeout period.</span></span>
<span data-ttu-id="83a30-111">Hello první událost zaznamená hello podrobnosti požadavku hello přijme hello reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="83a30-111">hello first event below logs hello details of hello request received at hello reverse proxy.</span></span> <span data-ttu-id="83a30-112">Hello druhá událost označuje, že hello požadavek se nezdařil při předávání tooservice náležitý příliš "vnitřní chyba = ERROR_WINHTTP_TIMEOUT"</span><span class="sxs-lookup"><span data-stu-id="83a30-112">hello second event indicates that hello request failed while forwarding tooservice, due too"internal error = ERROR_WINHTTP_TIMEOUT"</span></span> 

    <span data-ttu-id="83a30-113">datová část Hello zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="83a30-113">hello payload includes:</span></span>

    *  <span data-ttu-id="83a30-114">**traceId**: Tento identifikátor GUID lze použít toocorrelate všechny události hello odpovídající tooa jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="83a30-114">**traceId**: This GUID can be used toocorrelate all hello events corresponding tooa single request.</span></span> <span data-ttu-id="83a30-115">V hello níže dvě události, hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, zdání patří toohello stejném požadavku.</span><span class="sxs-lookup"><span data-stu-id="83a30-115">In hello below two events, hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, implying they belong toohello same request.</span></span>
    *  <span data-ttu-id="83a30-116">**requestUrl**: byl odeslán požadavek hello toowhich hello adresy URL (URL reverzní proxy server).</span><span class="sxs-lookup"><span data-stu-id="83a30-116">**requestUrl**: hello URL (Reverse proxy URL) toowhich hello request was sent.</span></span>
    *  <span data-ttu-id="83a30-117">**příkaz**: příkaz HTTP.</span><span class="sxs-lookup"><span data-stu-id="83a30-117">**verb**: HTTP verb.</span></span>
    *  <span data-ttu-id="83a30-118">**remoteAddress**: adresa odesílání hello požadavku klienta.</span><span class="sxs-lookup"><span data-stu-id="83a30-118">**remoteAddress**: Address of client sending hello request.</span></span>
    *  <span data-ttu-id="83a30-119">**resolvedServiceUrl**: koncový bod adresy URL toowhich hello příchozí žádosti o službu byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="83a30-119">**resolvedServiceUrl**: Service endpoint URL toowhich hello incoming request was resolved.</span></span> 
    *  <span data-ttu-id="83a30-120">**Detaily chyby**: Další informace o selhání hello.</span><span class="sxs-lookup"><span data-stu-id="83a30-120">**errorDetails**: Additional information about hello failure.</span></span>

    ```
    {
      "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
      "ProviderName": "Microsoft-ServiceFabric",
      "Id": 51477,
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
      "ProcessId": 57696,
      "Level": "Informational",
      "Keywords": "0x1000000000000021",
      "EventName": "ReverseProxy",
      "ActivityID": null,
      "RelatedActivityID": null,
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
        "verb": "GET",
        "remoteAddress": "::1",
        "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
        "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
      }
    }

    {
      "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
      ...
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request tooservice: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
      ...
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "statusCode": 504,
        "description": "Reverse Proxy Timeout",
        "sendRequestPhase": "FinishSendRequest",
        "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
      }
    }
    ```

2. <span data-ttu-id="83a30-121">Reverzní proxy server vrátí stavový kód odpovědi 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="83a30-121">Reverse proxy returns response status code 404 (Not Found).</span></span> 
    
    <span data-ttu-id="83a30-122">Tady je příklad událost kde reverzní proxy server vrátí 404, protože se jí nepovedlo toofind hello odpovídající koncový bod služby.</span><span class="sxs-lookup"><span data-stu-id="83a30-122">Here is an example event where reverse proxy returns 404 since it failed toofind hello matching service endpoint.</span></span>
    <span data-ttu-id="83a30-123">Hello datové položky popište jsou:</span><span class="sxs-lookup"><span data-stu-id="83a30-123">hello payload  entries of interest here are:</span></span>
    *  <span data-ttu-id="83a30-124">**processRequestPhase**: označuje hello fázi během zpracování požadavku, pokud došlo k chybě hello, ***TryGetEndpoint*** jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="83a30-124">**processRequestPhase**: Indicates hello phase during request processing when hello failure occurred, ***TryGetEndpoint*** i.e</span></span> <span data-ttu-id="83a30-125">Při pokusu o toofetch hello služby koncový bod tooforward k.</span><span class="sxs-lookup"><span data-stu-id="83a30-125">while trying toofetch hello service endpoint tooforward to.</span></span> 
    *  <span data-ttu-id="83a30-126">**Detaily chyby**: uvádí kritéria vyhledávání hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="83a30-126">**errorDetails**: Lists hello endpoint search criteria.</span></span> <span data-ttu-id="83a30-127">Zde můžete zobrazit tento hello listenerName zadaný = **FrontEndListener** zatímco seznam koncových bodů repliky hello obsahuje pouze naslouchací proces s názvem hello **OldListener**.</span><span class="sxs-lookup"><span data-stu-id="83a30-127">Here you can see that hello listenerName specified = **FrontEndListener** whereas hello replica endpoint list only contains a listener with hello name **OldListener**.</span></span>
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward tooservice: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
      "ProcessId": 57696,
      "Level": "Warning",
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
        ...
        "processRequestPhase": "TryGetEndoint",
        "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
      }
    }
    ```
    <span data-ttu-id="83a30-128">Další příklad, kdy vrátit 404 reverzní proxy server nebyl nalezen je: ApplicationGateway\Http konfigurační parametr **SecureOnlyMode** nastavena tootrue hello reverzní proxy server naslouchá na **HTTPS**, Koncové body hello repliky jsou ale nezabezpečeného (naslouchá na protokolu HTTP).</span><span class="sxs-lookup"><span data-stu-id="83a30-128">Another example where reverse proxy can return 404 Not Found is: ApplicationGateway\Http configuration parameter **SecureOnlyMode** is set tootrue with hello reverse proxy listening on **HTTPS**, however all of hello replica endpoints are unsecure (listening on HTTP).</span></span>
    <span data-ttu-id="83a30-129">Vrátí proxy 404 vrátit vzhledem k tomu, že koncový bod naslouchá na požadavek HTTPS tooforward hello nejde najít.</span><span class="sxs-lookup"><span data-stu-id="83a30-129">Reverse proxy returns 404 since it cannot find an endpoint listening on HTTPS tooforward hello request.</span></span> <span data-ttu-id="83a30-130">Analýza hello parametry v datové části události hello pomáhá toonarrow dolů hello problému:</span><span class="sxs-lookup"><span data-stu-id="83a30-130">Analyzing hello parameters in hello event payload helps toonarrow down hello issue:</span></span>
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. <span data-ttu-id="83a30-131">Žádost toohello reverzní proxy server nepodaří a dojde k vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="83a30-131">Request toohello reverse proxy fails with a timeout error.</span></span> 
    <span data-ttu-id="83a30-132">protokoly událostí Hello obsahovat událost s podrobnostmi hello přijal žádost (není tady zobrazené).</span><span class="sxs-lookup"><span data-stu-id="83a30-132">hello event logs contain an event with hello received request details (not shown here).</span></span>
    <span data-ttu-id="83a30-133">následující události Hello ukazuje, že služba hello odpověděla s 404 stavový kód a zahájí reverzní proxy server znovu vyřešit.</span><span class="sxs-lookup"><span data-stu-id="83a30-133">hello next event shows that hello service responded with a 404 status code and reverse proxy initiates a re-resolve.</span></span> 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request tooservice returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    <span data-ttu-id="83a30-134">Při shromažďování všech událostí hello, uvidíte train událostí zobrazující každých vyřešte a předat dál pokus.</span><span class="sxs-lookup"><span data-stu-id="83a30-134">When collecting all hello events, you see a train of events showing every resolve and forward attempt.</span></span>
    <span data-ttu-id="83a30-135">Hello poslední událost v řadě hello zobrazuje hello zpracování žádosti se nezdařilo s vypršení časového limitu, společně s hello počet pokusů o úspěšném vyřešit.</span><span class="sxs-lookup"><span data-stu-id="83a30-135">hello last event in hello series shows hello request processing has failed with a timeout, along with hello number of successful resolve attempts.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="83a30-136">Se doporučuje tookeep hello podrobné kanál shromažďování událostí ve výchozím nastavení zakázaná a ji povolit pro řešení potíží s na základě potřeba.</span><span class="sxs-lookup"><span data-stu-id="83a30-136">It is recommended tookeep hello  verbose channel event collection disabled by default and enable it for troubleshooting on a need basis.</span></span>

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    <span data-ttu-id="83a30-137">Pokud je kolekce pouze kritické chyby nebo události, zobrazí jedna událost s podrobnostmi o vypršení časového limitu hello a hello počet pokusů o vyřešení.</span><span class="sxs-lookup"><span data-stu-id="83a30-137">If collection is enabled for critical/error events only, you see one event with details about hello timeout and hello number of resolve attempts.</span></span> 
    
    <span data-ttu-id="83a30-138">Pokud služba hello zaměřen toosend uživatele zpět toohello kód 404 stav, by měl být doplněny hlavičku "X-ServiceFabric".</span><span class="sxs-lookup"><span data-stu-id="83a30-138">If hello service intends toosend a 404 status code back toohello user, it should be accompanied by an "X-ServiceFabric" header.</span></span> <span data-ttu-id="83a30-139">Po opravě to, uvidíte tohoto reverzní proxy server předává hello stav kód back toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="83a30-139">After fixing this, you will see that reverse proxy forwards hello status code back toohello client.</span></span>  

4. <span data-ttu-id="83a30-140">Případech, kdy hello klient se odpojil hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="83a30-140">Cases when hello client has disconnected hello request.</span></span>

    <span data-ttu-id="83a30-141">Hello níže událost se zaznamená při reverzní proxy server je předávání hello odpovědi tooclient ale odpojení klienta hello:</span><span class="sxs-lookup"><span data-stu-id="83a30-141">hello below event is recorded when reverse proxy is forwarding hello response tooclient but hello client disconnects:</span></span>

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable toosend response tooclient: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```

> [!NOTE]
> <span data-ttu-id="83a30-142">Zpracování žádosti související toowebsocket události se neprotokolují aktuálně.</span><span class="sxs-lookup"><span data-stu-id="83a30-142">Events related toowebsocket request processing are not currently logged.</span></span> <span data-ttu-id="83a30-143">To bude přidána v příštím vydání hello.</span><span class="sxs-lookup"><span data-stu-id="83a30-143">This will be added in hello next release.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83a30-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83a30-144">Next steps</span></span>
* <span data-ttu-id="83a30-145">[Seskupení událostí a kolekce s použitím Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) pro povolení shromažďování protokolů v Azure clustery.</span><span class="sxs-lookup"><span data-stu-id="83a30-145">[Event aggregation and collection using Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) for enabling log collection in Azure clusters.</span></span>
* <span data-ttu-id="83a30-146">události tooview Service Fabric v sadě Visual Studio, najdete v části [monitorování a diagnostikovat místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="83a30-146">tooview Service Fabric events in Visual Studio, see [Monitor and diagnose locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>
* <span data-ttu-id="83a30-147">Odkazovat příliš[konfigurovat reverzní proxy server tooconnect toosecure služby](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s možností ověřování certifikátu hello jinou službu.</span><span class="sxs-lookup"><span data-stu-id="83a30-147">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="83a30-148">Čtení [Service Fabric reverse proxy](service-fabric-reverseproxy.md) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="83a30-148">Read [Service Fabric reverse proxy](service-fabric-reverseproxy.md) toolearn more.</span></span>
