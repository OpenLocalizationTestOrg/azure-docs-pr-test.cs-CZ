---
title: "aaaAzure Application Insights datový Model | Microsoft Docs"
description: "Popisuje vlastnosti exportován průběžné exportu ve formátu JSON a použít jako filtry."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: bwren
ms.openlocfilehash: 5ff3ce7953b91cc69b5d96c0ea9b6d58a6016e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-export-data-model"></a>Application Insights Export datového modelu
Tato tabulka uvádí vlastnosti hello telemetrická data odesílaná z hello [Application Insights](app-insights-overview.md) portál toohello sady SDK.
Zobrazí se tyto vlastnosti v datovým výstupem z [průběžné exportovat](app-insights-export-telemetry.md).
Zobrazí se také v filtry vlastností v [Explorer metrika](app-insights-metrics-explorer.md) a [diagnostické vyhledávání](app-insights-diagnostic-search.md).

Toonote body:

* `[0]`v těchto tabulkách označuje bod v cestě hello, ve které máte tooinsert index; ale není vždy 0.
* Dobách trvání jsou v desetin mikrosekund, takže 10000000 == 1 sekunda.
* Data a časy jsou UTC a jsou uvedeny ve formátu ISO hello`yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Příklad
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent tooclient
        "success": true, // Default == responseCode<400
        // Request id becomes hello operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently hello following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized too0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent tooportal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Kontext
Všechny typy telemetrických dat se předěl doprovází oddíl kontextu. Ne všechny z těchto polí, se přenáší se každý datový bod.

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| Context.Custom.Dimensions [0] |objekt] |Páry klíč hodnota řetězce nastavit parametrem vlastní vlastnosti. Maximální délka klíče 100, hodnoty maximální délky 1024. Více než 100 jedinečné hodnoty vlastnosti hello lze vyhledat, ale nelze použít v případě segmentace. 200 maximální počet klíčů na ikey. |
| Context.Custom.Metrics [0] |objekt] |Nastavte parametr vlastní měření a TrackMetrics páry klíč hodnota. Maximální délka klíče 100, mohou být číselné hodnoty. |
| context.data.eventTime |Řetězec |ČAS UTC |
| context.data.isSynthetic |Logická hodnota |Žádost se zobrazí toocome robota nebo webový test. |
| context.data.samplingRate |Číslo |Procento generované hello SDK, která je odeslána tooportal telemetrie. V rozsahu 0,0 100.0. |
| Context.Device |Objekt |Klientské zařízení |
| Context.Device.Browser |Řetězec |IE Chrome... |
| context.device.browserVersion |Řetězec |Chrome 48,0... |
| context.device.deviceModel |Řetězec | |
| context.device.deviceName |Řetězec | |
| Context.Device.ID |Řetězec | |
| Context.Device.Locale |Řetězec |de-DE, en-GB... |
| Context.Device.Network |Řetězec | |
| context.device.oemName |Řetězec | |
| context.device.osVersion |Řetězec |Hostitelský operační systém |
| context.device.roleInstance |Řetězec |ID hostitelského serveru |
| context.device.roleName |Řetězec | |
| Context.Device.Type |Řetězec |Počítač, prohlížeč... |
| Context.Location |Objekt |Odvozená od když. |
| Context.location.City |Řetězec |Odvozené když, pokud je znám |
| Context.location.ClientIP |Řetězec |Poslední Osmiúhelník je anonymizovaná too0. |
| Context.location.Continent |Řetězec | |
| Context.location.Country |Řetězec | |
| Context.location.Province |Řetězec |Kraj |
| Context.Operation.ID |Řetězec |Položky, které mají stejné id operace se zobrazují jako související položky v portálu hello hello. Obvykle id žádosti hello. |
| Context.Operation.Name |Řetězec |Adresa URL nebo žádosti o název |
| context.operation.parentId |Řetězec |Umožňuje vnořené související položky. |
| Context.Session.ID |Řetězec |ID skupiny operací z hello stejný zdroj. Po dobu 30 minut bez operace signály hello ukončení relace. |
| context.session.isFirst |Logická hodnota | |
| context.user.accountAcquisitionDate |Řetězec | |
| context.user.anonAcquisitionDate |Řetězec | |
| context.user.anonId |Řetězec | |
| context.user.authAcquisitionDate |Řetězec |[Ověřený uživatel](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |Logická hodnota | |
| internal.data.documentVersion |Řetězec | |
| internal.data.ID |Řetězec | |

## <a name="events"></a>Události
Vlastní události vygenerované [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| počet událostí [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad 4 =&gt; 25 %. |
| Název události [0] |Řetězec |Název události.  Maximální délka 250. |
| Adresa url pro události [0] |Řetězec | |
| události [0] urlData.base |Řetězec | |
| události [0] urlData.host |Řetězec | |

## <a name="exceptions"></a>Výjimky
Sestavy [výjimky](app-insights-asp-net-exceptions.md) hello serveru a v prohlížeči hello.

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| sestavení [0] basicException |Řetězec | |
| počet basicException [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad 4 =&gt; 25 %. |
| exceptionGroup basicException [0] |Řetězec | |
| exceptionType basicException [0] |Řetězec | |
| failedUserCodeMethod basicException [0] |Řetězec | |
| failedUserCodeAssembly basicException [0] |Řetězec | |
| handledAt basicException [0] |Řetězec | |
| hasFullStack basicException [0] |Logická hodnota | |
| id basicException [0] |Řetězec | |
| Metoda basicException [0] |Řetězec | |
| zpráva basicException [0] |Řetězec |Zpráva o výjimce. Maximální délka 10 tis. |
| outerExceptionMessage basicException [0] |Řetězec | |
| outerExceptionThrownAtAssembly basicException [0] |Řetězec | |
| outerExceptionThrownAtMethod basicException [0] |Řetězec | |
| outerExceptionType basicException [0] |Řetězec | |
| outerId basicException [0] |Řetězec | |
| sestavení [0] parsedStack basicException [0] |Řetězec | |
| Název souboru parsedStack [0] basicException [0] |Řetězec | |
| úroveň parsedStack [0] basicException [0] |celé číslo | |
| basicException [0] [0] parsedStack řádku |celé číslo | |
| Metoda parsedStack [0] basicException [0] |Řetězec | |
| Zásobník basicException [0] |Řetězec |Maximální délka 10 TIS |
| typeName basicException [0] |Řetězec | |

## <a name="trace-messages"></a>Trasovací zprávy
Poslal [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)a podle hello [protokolování adaptéry](app-insights-asp-net-trace-logs.md).

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| zprávy [0] Název_protokolovače |Řetězec | |
| zprávy [0] Parametry |Řetězec | |
| zprávy [0] nezpracovaná |Řetězec |Hello zprávy protokolu, maximální délka 10 tis. |
| úroveň závažnosti zpráva [0] |Řetězec | |

## <a name="remote-dependency"></a>Vzdálené závislostí
Odesílá TrackDependency. Použít tooreport výkonu a využití [volá toodependencies](app-insights-asp-net-dependencies.md) hello server a volání AJAX do prohlížeče hello.

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| asynchronní remoteDependency [0] |Logická hodnota | |
| baseName remoteDependency [0] |Řetězec | |
| commandName remoteDependency [0] |Řetězec |Například "domovskou nebo index" |
| počet remoteDependency [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad 4 =&gt; 25 %. |
| dependencyTypeName remoteDependency [0] |Řetězec |PROTOKOLU HTTP, SQL... |
| durationMetric.value remoteDependency [0] |Číslo |Čas od volání toocompletion odpovědi závislostí |
| id remoteDependency [0] |Řetězec | |
| Název remoteDependency [0] |Řetězec |Adresa URL. Maximální délka 250. |
| resultCode remoteDependency [0] |Řetězec |z HTTP závislostí |
| Úspěch remoteDependency [0] |Logická hodnota | |
| Typ remoteDependency [0] |Řetězec |Protokolu HTTP, Sql... |
| Adresa url remoteDependency [0] |Řetězec |Maximální délka 2000 |
| urlData.base remoteDependency [0] |Řetězec |Maximální délka 2000 |
| urlData.hashTag remoteDependency [0] |Řetězec | |
| urlData.host remoteDependency [0] |Řetězec |Maximální délka 200 |

## <a name="requests"></a>Požadavky
Poslal [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest). Standardní moduly Hello použít tento tooreports doba odezvy serveru, měří ve hello server.

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| počet požadavku [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad: 4 =&gt; 25 %. |
| durationMetric.value požadavku [0] |Číslo |Čas, ze které tooresponse požadavku. 1e7 == hodnotami 1 |
| id požadavku [0] |Řetězec |Id operace |
| Název žádosti [0] |Řetězec |Základní adresa url + GET nebo POST.  Maximální délka 250 |
| responseCode požadavku [0] |celé číslo |Tooclient odeslané odpovědi HTTP |
| úspěšné žádosti [0] |Logická hodnota |Výchozí == (responseCode &lt; 400) |
| Adresa url požadavku [0] |Řetězec |Není včetně hostitele |
| urlData.base požadavku [0] |Řetězec | |
| urlData.hashTag požadavku [0] |Řetězec | |
| urlData.host požadavku [0] |Řetězec | |

## <a name="page-view-performance"></a>Stránka zobrazení výkonu
Posílá prohlížeč hello. Míry hello tooprocess čas na stránce z uživatele inicializace hello požadavek toodisplay dokončení (s výjimkou asynchronní volání AJAX).

Kontext hodnoty zobrazit klientského operačního systému a verze prohlížeče.

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| clientProcess.value clientPerformance [0] |celé číslo |Čas od konce přijetí stránku hello toodisplaying hello HTML. |
| Název clientPerformance [0] |Řetězec | |
| networkConnection.value clientPerformance [0] |celé číslo |Doba trvání tooestablish připojení k síti. |
| receiveRequest.value clientPerformance [0] |celé číslo |Čas od konce odesílání hello požadavek tooreceiving hello HTML v odpovědi. |
| sendRequest.value clientPerformance [0] |celé číslo |Čas od přijatá toosend hello HTTP žádosti. |
| total.value clientPerformance [0] |celé číslo |Čas spuštění toosend hello požadavek toodisplaying hello stránky. |
| Adresa url clientPerformance [0] |Řetězec |Adresa URL této žádosti |
| urlData.base clientPerformance [0] |Řetězec | |
| urlData.hashTag clientPerformance [0] |Řetězec | |
| urlData.host clientPerformance [0] |Řetězec | |
| urlData.protocol clientPerformance [0] |Řetězec | |

## <a name="page-views"></a>Zobrazení stránky
Poslal trackPageView() nebo [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| Počet zobrazení [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad 4 =&gt; 25 %. |
| zobrazení [0] durationMetric.value |celé číslo |Volitelně můžete nastavit v trackPageView() nebo startTrackPage() - hodnota stopTrackPage(). Není hello stejné jako clientPerformance hodnoty. |
| Název zobrazení [0] |Řetězec |Název stránky.  Maximální délka 250 |
| Adresa url zobrazení [0] |Řetězec | |
| zobrazení [0] urlData.base |Řetězec | |
| zobrazení [0] urlData.hashTag |Řetězec | |
| zobrazení [0] urlData.host |Řetězec | |

## <a name="availability"></a>Dostupnost
Sestavy [testy dostupnosti webu](app-insights-monitor-web-app-availability.md).

| Cesta | Typ | Poznámky |
| --- | --- | --- |
| availabilityMetric.name dostupnosti [0] |Řetězec |dostupnosti |
| availabilityMetric.value dostupnosti [0] |Číslo |1.0 nebo 0,0 |
| počet dostupnosti [0] |celé číslo |100 / ([vzorkování](app-insights-sampling.md) rychlost). Příklad 4 =&gt; 25 %. |
| dataSizeMetric.name dostupnosti [0] |Řetězec | |
| dataSizeMetric.value dostupnosti [0] |celé číslo | |
| durationMetric.name dostupnosti [0] |Řetězec | |
| durationMetric.value dostupnosti [0] |Číslo |Doba trvání testu. 1e7 == hodnotami 1 |
| zpráva dostupnosti [0] |Řetězec |Selhání diagnostiky |
| výsledek dostupnosti [0] |Řetězec |Přijetí nebo vyloučení |
| runLocation dostupnosti [0] |Řetězec |Geograficky zdroj žádosti http |
| Název_testu dostupnosti [0] |Řetězec | |
| testRunId dostupnosti [0] |Řetězec | |
| testTimestamp dostupnosti [0] |Řetězec | |

## <a name="metrics"></a>Metriky
Generované TrackMetric().

je Hello metriky hodnota nalezena v context.custom.metrics[0]

Například:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>O metriky hodnoty
Metriky, v metriky sestavy i jinde, jsou uvedeny se strukturou standardní objektu. Například:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Aktuálně – Přestože to může změnit v hello budoucí – všechny hodnoty nahlásila hello standardní moduly SDK, `count==1` a pouze hello `name` a `value` pole jsou užitečné. Hello pouze případ, kdy by být odlišné by pokud napíšete voláními TrackMetric v který nastavíte hello další parametry.

Hello účel hello další pole je toobe metriky tooallow agregován v hello SDK, portálu toohello tooreduce provoz. Například může průměrná několik následných odečty před odesláním všechny metriky sestavy. Potom by vypočítat hello min, max, směrodatná odchylka a celkovou hodnotu (suma nebo průměr) a nastavit počet toohello počet odečty reprezentována hello sestavy.

V tabulkách hello výše jsme zapomněli hello málo používané pole count, min, max, stdDev a sampledValue.

Namísto předem prostředku metriky, můžete použít [vzorkování](app-insights-sampling.md) Pokud potřebujete tooreduce hello svazku telemetrie.

### <a name="durations"></a>Doby trvání
Pokud není uvedeno jinak, jinak jsou reprezentované doby trvání v desetin mikrosekund, tak, aby 10000000.0 znamená 1 sekunda.

## <a name="see-also"></a>Viz také
* [Application Insights](app-insights-overview.md)
* [Průběžné exportu](app-insights-export-telemetry.md)
* [Ukázky kódu](app-insights-export-telemetry.md#code-samples)
