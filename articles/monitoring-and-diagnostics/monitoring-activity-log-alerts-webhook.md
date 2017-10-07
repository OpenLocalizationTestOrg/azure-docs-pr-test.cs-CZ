---
title: "aaaUnderstand hello webhooku schématu použít ve výstrahách aktivity protokolu | Microsoft Docs"
description: "Další informace o schématu hello hello formátu JSON, který je odeslána adresa URL webhooku tooa, pokud aktivuje výstrahu protokolu aktivit."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75562e0589222d3e392ea73eacfd7414a422d115
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhooky Azure aktivity protokolu výstrahy
Jako součást definice hello akce skupiny můžete nakonfigurovat webhooku koncové body tooreceive aktivity protokolu oznámení o výstrahách. Pomocí webhooků je možné směrovat tyto systémy tooother oznámení pro následné zpracování nebo vlastní akce. Tento článek ukazuje, jaké datové části hello hello HTTP POST tooa webhooku vypadá jako.

Další informace o protokolu upozornění, najdete v části Jak příliš[vytvořit Azure aktivitu protokolu výstrahy](monitoring-activity-log-alerts.md).

Informace o skupinách akce, najdete v části Jak příliš[vytvořit skupiny akce](monitoring-action-groups.md).

## <a name="authenticate-hello-webhook"></a>Ověření webhooku hello
Hello webhooku můžete volitelně použít ověření na základě tokenu pro ověřování. Hello webhooku identifikátor URI je uložit s tokenu ID, například `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Datová část schématu
datová část JSON Hello obsažené v hello operaci POST liší v závislosti na hello datové data.context.activityLog.eventSource pole.

###<a name="common"></a>Běžné
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Pro správu
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

Podrobnosti konkrétní schématu služby stavu oznámení aktivity protokolu výstrah najdete v tématu [oznámení o stavu služby](monitoring-service-notifications.md).

Podrobnosti konkrétní schématu na všechny ostatní výstrahy protokolu aktivit najdete v tématu [přehled protokol činnosti Azure hello](monitoring-overview-activity-logs.md).

| Název elementu | Popis |
| --- | --- |
| status |Používá pro výstrahy, metriky. Vždy nastaven příliš "aktivovaný" pro aktivitu protokolu výstrahy. |
| Kontext |Kontext události hello. |
| resourceProviderName |Poskytovatel prostředků Hello hello dopad prostředků. |
| conditionType |Vždy "událost". |
| jméno |Název pravidla výstrahy hello. |
| id |ID prostředku hello výstrahy. |
| description |Popis výstrahy nastavené, když se vytvoří výstraha hello. |
| subscriptionId |ID předplatného Azure. |
| časové razítko |Čas, na které hello vygenerovalo událost hello služby Azure, který zpracovává požadavek hello. |
| resourceId |ID prostředku hello dopad prostředků. |
| Název skupiny prostředků |Název skupiny prostředků hello hello dopad prostředků. |
| properties |Sada `<Key, Value>` páry (tedy `Dictionary<String, String>`) obsahující podrobnosti o události hello. |
| Události |Element, který obsahuje metadata o hello událostí. |
| Autorizace |Řízení přístupu na základě Role vlastnosti Hello hello události. Tyto vlastnosti obvykle zahrnují hello akce, hello role a obor hello. |
| category |Kategorie události hello. Podporované hodnoty jsou správy, výstrahy, zabezpečení, ServiceHealth a doporučení. |
| volající |E-mailovou adresu hello uživatele, který provedl hello operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti. Může mít hodnotu null pro určité systémová volání. |
| correlationId |Obvykle GUID ve formátu řetězce. Události se correlationId patří toohello stejné větší akce a obvykle sdílet correlationId. |
| eventDescription |Statický text popisu události hello. |
| eventDataId |Jedinečný identifikátor pro událost hello. |
| EventSource |Název hello služby Azure nebo infrastrukturu, že generovaný hello událost. |
| požadavku HTTP |Hello žádosti obvykle zahrnuje hello clientRequestId, clientIpAddress a metoda HTTP (například přidat). |
| úroveň |Jeden z následujících hodnot hello: kritická, chyba, upozornění, informační a Verbose. |
| operationId |Obvykle GUID sdílen hello události odpovídající toosingle operaci. |
| operationName |Název operace hello. |
| properties |Vlastnosti události hello. |
| status |Řetězec. Stav operace hello. Běžné hodnoty zahrnují Začínáme, probíhá, bylo úspěšné, neúspěšné, aktivní a vyřešeno. |
| Podřízený stav |Obvykle zahrnuje stavový kód HTTP hello hello odpovídající volání REST. Může také obsahovat další řetězce, které popisují podřízeného stavu. Běžné substatus hodnoty zahrnují OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409 ), Vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503) a vypršel časový limit brány (kód stavu HTTP: 504). |

## <a name="next-steps"></a>Další kroky
* [Další informace o protokolu aktivit hello](monitoring-overview-activity-logs.md).
* [Spustit skripty služby Azure automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Použít toosend aplikace logiky serveru služby SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
* [Použití logiku aplikace toosend Slack zprávu z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
* [Použít toosend aplikace logiky fronty zpráv tooan Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). V tomto příkladu je metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
