---
title: "aaaAzure schématu aktivity protokolu události | Microsoft Docs"
description: "Pochopení hello událostí schéma pro data do hello protokol aktivit"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a>Azure schématu aktivity protokolu události
Hello **protokol činnosti Azure** protokolu, který poskytuje přehled o všech událostí na úrovni předplatného, k nimž došlo v Azure. Tento článek popisuje hello schématu události podle kategorie dat.

## <a name="administrative"></a>Pro správu
Tato kategorie obsahuje hello záznam všech vytvořit, operace aktualizace, odstranění a akce provést pomocí Správce prostředků. Příklady hello, které typy událostí, které zobrazí se v této kategorii "vytvořit virtuální počítač" a "odstranit skupinu zabezpečení sítě" každé akce uživatele nebo aplikace pomocí Správce prostředků je modelovaná jako operace na konkrétní typ prostředku. Pokud je typ operace hello zapsat, Delete nebo akce, záznamy hello hello start a úspěch nebo selhání této operace se zaznamenávají do administrativní kategorie hello. Administrativní kategorie Hello také zahrnuje všechny řízení přístupu na základě toorole změny v předplatném.

### <a name="sample-event"></a>Událost vzorku
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a>Popisy vlastností
| Název elementu | Popis |
| --- | --- |
| Autorizace |Objekt BLOB RBAC vlastností události hello. Obvykle zahrnuje vlastnosti "action", "role" a "obor" hello. |
| volající |E-mailovou adresu hello uživatele, který se má provést operaci hello, deklarace nebo SPN deklarace identity na základě dostupnosti. |
| Kanály |Jeden z hello následující hodnoty: "Admin", "Operace" |
| Deklarace identity |Hello JWT token používá služby Active Directory tooauthenticate hello uživatele nebo aplikace tooperform tuto operaci v správce prostředků. |
| correlationId |Obvykle GUID ve formátu řetězce hello. Události, které sdílejí correlationId patří toohello stejné uber akce. |
| description |Statický text popisu události. |
| eventDataId |Jedinečný identifikátor události. |
| požadavku HTTP |Objekt BLOB popisující hello požadavek Http. Obvykle zahrnuje hello "clientRequestId", "clientIpAddress" a "metodu" (metoda HTTP. For example, PUT). |
| úroveň |Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné" |
| Název skupiny prostředků |Název skupiny prostředků hello hello dopad prostředků. |
| resourceProviderName |Název zprostředkovatele prostředků hello hello dopad prostředků |
| resourceId |Id prostředku hello dopad prostředků. |
| operationId |Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace. |
| operationName |Název operace hello. |
| properties |Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello. |
| status |Řetězec s popisem hello stav operace hello. Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno. |
| Podřízený stav |Obvykle hello stavový kód HTTP hello odpovídající volání REST, ale můžou taky patřit jiných řetězců popisující podřízeného stavu, jako jsou tyto hodnoty běžné: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (HTTP Stavový kód: 204), chybný požadavek (kód stavu HTTP: 400), nebyl nalezen (kód stavu HTTP: 404), konflikt (kód stavu HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504). |
| eventTimestamp |Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost. |
| submissionTimestamp |Časové razítko, když hello události jsou k dispozici pro zadávání dotazů. |
| subscriptionId |ID předplatného Azure. |

## <a name="service-health"></a>Stav služby
Tato kategorie obsahuje záznam hello všechny služby stavu incidentů, kterým došlo v Azure. Příkladem hello typu události, které se zobrazí se v této kategorii je "SQL Azure v oblasti Východ USA dochází k výpadku." Události stavu služby existují ve pět variantách: je vyžadována akce, jíž obnovení, Incident, údržby, informace nebo zabezpečení a objeví jenom tehdy, pokud máte prostředku v hello předplatné, které by ovlivňovat hello událostí.

### <a name="sample-event"></a>Událost vzorku
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Popisy vlastností
Název elementu | Popis
-------- | -----------
Kanály | Patří mezi hello následující hodnoty: "Admin", "Operace"
correlationId | Obvykle je identifikátor GUID ve formátu řetězce hello. Události, patří toohello obvykle sdílet stejnou akci uber hello stejné correlationId.
description | Popis události hello.
eventDataId | Hello jedinečný identifikátor události.
EventName | Název Hello hello události.
úroveň | Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"
resourceProviderName | Název zprostředkovatele prostředků hello hello dopad prostředků. Pokud není známý, to bude mít hodnotu null.
Typ prostředku| Hello typ prostředku hello dopad prostředků. Pokud není známý, to bude mít hodnotu null.
Podřízený stav | Obvykle se hodnota null pro události stavu služby.
eventTimestamp | Časové razítko, když hello protokolu událostí se vygeneroval a odeslání toohello protokol aktivit.
submissionTimestamp |   Časové razítko, když jsou k dispozici v hello aktivity protokolu události hello.
subscriptionId | předplatné Azure, ve kterém se tato událost byla zaznamenána Hello.
status | Řetězec s popisem hello stav operace hello. Některé běžné hodnoty jsou: aktivní a vyřešit.
operationName | Název operace hello. Obvykle Microsoft.ServiceHealth/incident/action.
category | "ServiceHealth"
resourceId | Id prostředku hello dopad prostředků, pokud je znám. V opačném případě je zadáno ID předplatného.
Properties.Title | Hello lokalizovaný název pro tuto komunikaci. Hello výchozím jazykem je angličtina.
Properties.Communication | Hello lokalizované podrobnosti hello komunikace s značka jazyka HTML. Výchozí hello je angličtina.
Properties.incidentType | Možné hodnoty: AssistedRecovery, ActionRequired, informace, incidentů, údržby, zabezpečení
Properties.trackingId | Identifikuje hello incident, které se tato událost je přidružen. Pomocí tohoto toocorrelate hello události související tooan incidentu.
Properties.impactedServices | Uvozený blob JSON, který popisuje hello služeb a oblasti, které jsou ovlivněny incidentem hello. Seznam služeb, z nichž každá má ServiceName a seznam ImpactedRegions, z nichž každá má RegionName.
Properties.defaultLanguageTitle | Hello komunikace v angličtině
Properties.defaultLanguageContent | Hello komunikace v angličtině jako značka jazyka html nebo prostý text
Properties.Stage | Možné hodnoty pro AssistedRecovery, ActionRequired, informace, incidentů, zabezpečení: jsou aktivní, vyřešeno. Za účelem údržby jsou: aktivní, plánovaná, InProgress, zrušení, Rescheduled, vyřešeno, dokončeno
Properties.communicationId | komunikace Hello Tato událost souvisí.

## <a name="alert"></a>Výstrahy
Tato kategorie obsahuje záznam hello všechny aktivací Azure výstrah. Příkladem hello typu události, který zobrazí se v této kategorii je "% využití procesoru na Můjvp přes 80 pro hello za posledních 5 minut." Výstrahy koncept mají různé systémy Azure – můžete definovat pravidla nějaká a přijímat oznámení v případě podmínky odpovídají daného pravidla. Pokaždé, když podporovaných Azure typu výstrahy, aktivuje,' nebo hello podmínky jsou splněny toogenerate oznámení, záznam hello aktivace je také poslat toothis kategorii hello protokol aktivit.

### <a name="sample-event"></a>Událost vzorku

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Popisy vlastností
| Název elementu | Popis |
| --- | --- |
| volající | Vždy Microsoft.Insights/alertRules |
| Kanály | Vždy "Admin, operace" |
| Deklarace identity | Objekt blob JSON typu hello hlavního názvu služby (hlavní název služby), nebo prostředku, hello výstrahy stroje. |
| correlationId | Identifikátor GUID ve formátu řetězce hello. |
| description |Statický text popis výstrah událostí hello. |
| eventDataId |Jedinečný identifikátor hello výstrah události. |
| úroveň |Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné" |
| Název skupiny prostředků |Název skupiny prostředků hello hello dopad prostředků, pokud se jedná o metriky výstrahu. Pro ostatní typy výstrah jde hello název skupiny prostředků hello, který obsahuje hello výstrahy samotné. |
| resourceProviderName |Název zprostředkovatele prostředků hello hello ovlivněná prostředků je metriky výstrahy. Pro ostatní typy výstrah jde hello název zprostředkovatele prostředků hello hello výstrahy sám sebe. |
| resourceId | Název hello ID prostředku pro hello dopad prostředků, pokud se jedná o metriky výstrahu. Pro ostatní typy výstrah Toto je ID prostředku hello hello výstrahy prostředku sám sebe. |
| operationId |Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace. |
| operationName |Název operace hello. |
| properties |Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello. |
| status |Řetězec s popisem hello stav operace hello. Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno. |
| Podřízený stav | Obvykle se hodnota null pro výstrahy. |
| eventTimestamp |Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost. |
| submissionTimestamp |Časové razítko, když hello události jsou k dispozici pro zadávání dotazů. |
| subscriptionId |ID předplatného Azure. |

### <a name="properties-field-per-alert-type"></a>Vlastnosti pole na každý typ výstrahy
Hello vlastnosti pole bude obsahovat různé hodnoty v závislosti na hello zdroj hello výstrah událostí. Dva poskytovatelé běžné výstrah událostí jsou výstrahy protokol aktivit a metriky výstrahy.

#### <a name="properties-for-activity-log-alerts"></a>Vlastnosti výstrah protokol aktivit
| Název elementu | Popis |
| --- | --- |
| properties.subscriptionId | ID předplatného Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| properties.eventDataId | ID události dat Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| properties.resourceGroup | Skupina prostředků Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| properties.resourceId | ID prostředku Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| properties.eventTimestamp | časové razítko události Hello hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| properties.operationName | Název operace Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován. |
| Properties.status | Stav Hello z hello aktivity protokolu události, který chybu způsobil této aktivity protokolu pravidlo výstrahy toobe aktivován.|

#### <a name="properties-for-metric-alerts"></a>Vlastnosti metriky výstrah
| Název elementu | Popis |
| --- | --- |
| Vlastnosti. RuleUri | ID prostředku hello metriky pravidlo výstrahy sám sebe. |
| Vlastnosti. RuleName | Hello název metriky pravidlo výstrahy hello. |
| Vlastnosti. RuleDescription | Popis Hello hello metriky pravidlo výstrahy (jak je definována v pravidlo výstrahy hello). |
| Vlastnosti. Prahová hodnota | Prahová hodnota Hello hello vyhodnocení hello metriky pravidlo výstrahy. |
| Vlastnosti. WindowSizeInMinutes | velikost okna Hello hello vyhodnocení hello metriky pravidlo výstrahy. |
| Vlastnosti. Agregace | Typ agregace Hello definované v hello metriky pravidlo výstrahy. |
| Vlastnosti. Operátor | Podmíněný operátor Hello hello vyhodnocení hello metriky pravidlo výstrahy. |
| Vlastnosti. MetricName | Název metriky Hello hello metriky hello vyhodnocení hello metriky pravidlo výstrahy. |
| Vlastnosti. MetricUnit | Hello metriky jednotky pro metriku hello hello vyhodnocení hello metriky pravidlo výstrahy. |

## <a name="autoscale"></a>Automatické škálování
Tato kategorie obsahuje záznam hello jakékoli operace související toohello události modulu hello škálování podle nastavení automatického škálování, které jste definovali ve vašem předplatném. Příkladem hello typu události, které se zobrazí se v této kategorii je "Škálování rozšiřování škálování využívajících akce se nezdařila." Použití automatického škálování, můžete automaticky škálovat nebo škálování hello podle počtu instancí v podporovaných prostředků typu čas, den nebo zatížení (metriky) dat, na které se používá nastavení automatického škálování. Při splnění podmínek hello tooscale nahoru nebo dolů, hello spuštění a úspěšné nebo neúspěšné události budou popsané v této kategorii.

### <a name="sample-event"></a>Událost vzorku
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Popisy vlastností
| Název elementu | Popis |
| --- | --- |
| volající | Vždy Microsoft.Insights/autoscaleSettings |
| Kanály | Vždy "Admin, operace" |
| Deklarace identity | Objekt blob JSON s hello hlavního názvu služby (hlavní název služby), nebo prostředek typu hello škálování stroje. |
| correlationId | Identifikátor GUID ve formátu řetězce hello. |
| description |Statický text popis události škálování hello. |
| eventDataId |Jedinečný identifikátor události škálování hello. |
| úroveň |Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné" |
| Název skupiny prostředků |Název skupiny prostředků hello nastavení automatického škálování hello. |
| resourceProviderName |Název zprostředkovatele prostředků hello pro nastavení automatického škálování hello. |
| resourceId |Id prostředku nastavení automatického škálování hello. |
| operationId |Identifikátor GUID sdílen hello události, které odpovídají tooa jedné operace. |
| operationName |Název operace hello. |
| properties |Sada `<Key, Value>` páry (tedy slovník) popisující hello podrobnosti události hello. |
| Vlastnosti. Popis | Podrobný popis co modul škálování hello dělal. |
| Vlastnosti. ResourceName | ID prostředku hello dopad prostředků (hello prostředků, na které hello byla provedena akce škálování) |
| Vlastnosti. OldInstancesCount | Hello počet instancí před akci škálování hello trvalo vliv. |
| Vlastnosti. NewInstancesCount | Hello počet instancí po akci škálování hello trvalo vliv. |
| Vlastnosti. LastScaleActionTime | razítko Hello při akci škálování hello došlo k chybě. |
| status |Řetězec s popisem hello stav operace hello. Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno. |
| Podřízený stav | Obvykle se hodnota null pro škálování. |
| eventTimestamp |Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost. |
| submissionTimestamp |Časové razítko, když hello události jsou k dispozici pro zadávání dotazů. |
| subscriptionId |ID předplatného Azure. |

## <a name="next-steps"></a>Další kroky
* [Další informace o hello protokol aktivit (dříve protokoly auditu)](monitoring-overview-activity-logs.md)
* [Stream hello protokol činnosti Azure tooEvent rozbočovače](monitoring-stream-activity-logs-event-hubs.md)
