---
title: "aaaCall webhooku protokol činnosti Azure výstrah | Microsoft Docs"
description: "Směrujte aktivity protokolu události tooother služby pro vlastní akce. Například odeslání serveru SMS, protokolu chyby nebo upozornění tým prostřednictvím chatu nebo zasílání zpráv služby service."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Volat webhook, jehož protokol činnosti Azure výstrah
Webhooky umožňují tooroute Azure výstrahy systémy tooother oznámení pro následné zpracování nebo vlastní akce. Webhook, jehož můžete použít na výstrahy tooroute ho tooservices, které odesílají SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí. Tento článek popisuje, jak tooset webhooku toobe volána, když se aktivuje upozornění protokol činnosti Azure. Také ukazuje, jaké datové části hello hello HTTP POST tooa webhooku vypadá jako. Informace o nastavení hello a schéma pro výstrahu Azure metriky [místo toho najdete na této stránce](insights-webhooks-alerts.md). Můžete také nastavit protokol aktivit výstrahy toosend e-mailem při aktivaci.

> [!NOTE]
> Tato funkce je aktuálně ve verzi preview a odeberou se v určitém okamžiku v budoucnu hello.
>
>

Můžete nastavit výstrahu protokolu aktivit pomocí hello [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx). V současné době nelze nastavit ho vytvořit pomocí hello portálu Azure.

## <a name="authenticating-hello-webhook"></a>Ověřování webhooku hello
Hello webhooku můžete ověřit pomocí některé z těchto metod:

1. **Na základě tokenu autorizace** -hello webhooku identifikátor URI je uložit s tokenu ID, například`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Základní ověřování** -hello webhooku identifikátor URI je uložit pomocí uživatelského jména a hesla, například`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Datová část schématu
Hello operaci POST obsahuje hello datové části JSON a schéma pro všechny aktivity protokolu výstrahy. Toto schéma je podobné toohello jeden používá metrika na základě výstrahy.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Název elementu | Popis |
| --- | --- |
| status |Používá pro výstrahy, metriky. Vždy nastaven příliš "aktivovaný" pro protokol aktivit výstrahy. |
| Kontext |Kontext události hello. |
| resourceProviderName |Poskytovatel prostředků Hello hello dopad prostředků. |
| conditionType |Vždy "událost". |
| jméno |Název pravidla výstrahy hello. |
| id |ID prostředku hello výstrahy. |
| description |Popis výstrahy jako sada během vytváření výstrahy hello. |
| subscriptionId |ID předplatného Azure. |
| časové razítko |Čas, na které hello vygenerovalo událost hello služby Azure, který zpracovává požadavek hello. |
| resourceId |ID prostředku hello dopad prostředků. |
| Název skupiny prostředků |Název skupiny prostředků hello hello dopad prostředků |
| properties |Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události hello. |
| Události |Element obsahující metadata o hello událostí. |
| Autorizace |Vlastnosti RBAC Hello hello události. Obvykle patří hello "action" a "role" hello "obor." |
| category |Kategorie události hello. Podporované hodnoty jsou: pro správu, výstrahy, zabezpečení, ServiceHealth, doporučení. |
| volající |E-mailovou adresu hello uživatele, který provedl hello operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti. Může mít hodnotu null pro určité systémová volání. |
| correlationId |Obvykle GUID ve formátu řetězce. Události se correlationId patří toohello stejné větší akce a obvykle sdílet correlationId. |
| eventDescription |Statický text popisu události hello. |
| eventDataId |Jedinečný identifikátor pro událost hello. |
| EventSource |Název hello služby Azure nebo infrastrukturu, že generovaný hello událost. |
| požadavku HTTP |Obvykle zahrnuje hello "clientRequestId", "clientIpAddress" a "metodu" (například PUT metoda HTTP). |
| úroveň |Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Verbose." |
| operationId |Obvykle GUID sdílen hello události odpovídající toosingle operaci. |
| operationName |Název operace hello. |
| properties |Vlastnosti události hello. |
| status |Řetězec. Stav operace hello. Běžné hodnoty jsou: "Spustit", "Probíhá", "Bylo úspěšně dokončeno", "Se nezdařilo", "Aktivní", "Přeložit". |
| Podřízený stav |Obvykle zahrnuje stavový kód HTTP hello hello odpovídající volání REST. Může také obsahovat jiných řetězců popisující podřízeného stavu. Běžné substatus hodnoty patří: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504) |

## <a name="next-steps"></a>Další kroky
* [Další informace o hello protokol aktivit](monitoring-overview-activity-logs.md)
* [Spustit skripty Azure Automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Použití aplikace logiky toosend serveru služby SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
* [Použití aplikace logiky toosend Slack zprávu z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
* [Použití aplikace logiky toosend zpráva tooan Azure Queue z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). V tomto příkladu je pro metriky výstrahy, ale může být upravený toowork s výstrahu protokolu aktivit.
