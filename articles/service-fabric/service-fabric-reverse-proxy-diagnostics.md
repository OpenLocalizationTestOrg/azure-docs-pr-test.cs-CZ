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
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a>Monitorování a diagnostikovat zpracování požadavku na reverzní proxy server hello

Od verze hello 5.7 Service Fabric, reverzní proxy server události jsou k dispozici pro kolekci. nejsou k dispozici ve dvou kanálů Hello události s pouze chybové události související s toorequest selhání zpracování na reverzní proxy server hello a druhý kanál obsahující podrobné události se záznamy pro úspěšných i neúspěšných požadavků.

Odkazovat příliš[shromažďování událostí reverzní proxy server](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable shromažďování událostí z těchto kanálů v místní a clustery Azure Service Fabric.

## <a name="troubleshoot-using-diagnostics-logs"></a>Řešení problémů pomocí protokolů diagnostiky
Zde jsou některé příklady na tom, jak můžete setkat než toointerpret hello běžné selhání protokoly:

1. Reverzní proxy server vrátí stavový kód odpovědi 504 (časový limit).

    Jedním z důvodů může být z důvodu selhání služby toohello tooreply během časového limitu požadavku hello.
Hello první událost zaznamená hello podrobnosti požadavku hello přijme hello reverzní proxy server. Hello druhá událost označuje, že hello požadavek se nezdařil při předávání tooservice náležitý příliš "vnitřní chyba = ERROR_WINHTTP_TIMEOUT" 

    datová část Hello zahrnuje:

    *  **traceId**: Tento identifikátor GUID lze použít toocorrelate všechny události hello odpovídající tooa jedné žádosti. V hello níže dvě události, hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, zdání patří toohello stejném požadavku.
    *  **requestUrl**: byl odeslán požadavek hello toowhich hello adresy URL (URL reverzní proxy server).
    *  **příkaz**: příkaz HTTP.
    *  **remoteAddress**: adresa odesílání hello požadavku klienta.
    *  **resolvedServiceUrl**: koncový bod adresy URL toowhich hello příchozí žádosti o službu byl vyřešen. 
    *  **Detaily chyby**: Další informace o selhání hello.

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

2. Reverzní proxy server vrátí stavový kód odpovědi 404 (není nalezena). 
    
    Tady je příklad událost kde reverzní proxy server vrátí 404, protože se jí nepovedlo toofind hello odpovídající koncový bod služby.
    Hello datové položky popište jsou:
    *  **processRequestPhase**: označuje hello fázi během zpracování požadavku, pokud došlo k chybě hello, ***TryGetEndpoint*** jednofaktorovému Při pokusu o toofetch hello služby koncový bod tooforward k. 
    *  **Detaily chyby**: uvádí kritéria vyhledávání hello koncový bod. Zde můžete zobrazit tento hello listenerName zadaný = **FrontEndListener** zatímco seznam koncových bodů repliky hello obsahuje pouze naslouchací proces s názvem hello **OldListener**.
    
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
    Další příklad, kdy vrátit 404 reverzní proxy server nebyl nalezen je: ApplicationGateway\Http konfigurační parametr **SecureOnlyMode** nastavena tootrue hello reverzní proxy server naslouchá na **HTTPS**, Koncové body hello repliky jsou ale nezabezpečeného (naslouchá na protokolu HTTP).
    Vrátí proxy 404 vrátit vzhledem k tomu, že koncový bod naslouchá na požadavek HTTPS tooforward hello nejde najít. Analýza hello parametry v datové části události hello pomáhá toonarrow dolů hello problému:
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. Žádost toohello reverzní proxy server nepodaří a dojde k vypršení časového limitu. 
    protokoly událostí Hello obsahovat událost s podrobnostmi hello přijal žádost (není tady zobrazené).
    následující události Hello ukazuje, že služba hello odpověděla s 404 stavový kód a zahájí reverzní proxy server znovu vyřešit. 

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
    Při shromažďování všech událostí hello, uvidíte train událostí zobrazující každých vyřešte a předat dál pokus.
    Hello poslední událost v řadě hello zobrazuje hello zpracování žádosti se nezdařilo s vypršení časového limitu, společně s hello počet pokusů o úspěšném vyřešit.
    
    > [!NOTE]
    > Se doporučuje tookeep hello podrobné kanál shromažďování událostí ve výchozím nastavení zakázaná a ji povolit pro řešení potíží s na základě potřeba.

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
    
    Pokud je kolekce pouze kritické chyby nebo události, zobrazí jedna událost s podrobnostmi o vypršení časového limitu hello a hello počet pokusů o vyřešení. 
    
    Pokud služba hello zaměřen toosend uživatele zpět toohello kód 404 stav, by měl být doplněny hlavičku "X-ServiceFabric". Po opravě to, uvidíte tohoto reverzní proxy server předává hello stav kód back toohello klienta.  

4. Případech, kdy hello klient se odpojil hello požadavku.

    Hello níže událost se zaznamená při reverzní proxy server je předávání hello odpovědi tooclient ale odpojení klienta hello:

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
> Zpracování žádosti související toowebsocket události se neprotokolují aktuálně. To bude přidána v příštím vydání hello.

## <a name="next-steps"></a>Další kroky
* [Seskupení událostí a kolekce s použitím Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) pro povolení shromažďování protokolů v Azure clustery.
* události tooview Service Fabric v sadě Visual Studio, najdete v části [monitorování a diagnostikovat místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Odkazovat příliš[konfigurovat reverzní proxy server tooconnect toosecure služby](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s možností ověřování certifikátu hello jinou službu.
* Čtení [Service Fabric reverse proxy](service-fabric-reverseproxy.md) toolearn Další.
