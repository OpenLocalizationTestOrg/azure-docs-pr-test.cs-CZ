---
title: "aaaConfigure webhooky Azure metriky výstrah | Microsoft Docs"
description: "Přesměrovat Azure výstrahy tooother mimo Azure systémy."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: bc4153ccdcff41c5b9d3c081e59a1bf260d8a283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurace webhook, jehož na výstrahu Azure metriky
Webhooky umožňují tooroute Azure výstrahy systémy tooother oznámení pro následné zpracování nebo vlastní akce. Webhook, jehož můžete použít na výstrahy tooroute ho tooservices, které odesílají SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí. Tento článek popisuje, jak tooset webhooku na Azure metriky výstrah a jaké datové části hello webhooku tooa hello HTTP POST bude vypadat takto. Informace o nastavení hello a schéma pro výstrahu protokol činnosti Azure (výstraha na událostech) [místo toho najdete na této stránce](insights-auditlog-to-webhook-email.md).

Azure výstrahy HTTP POST hello výstrahy obsah ve formátu JSON, schéma definovaná níže tooa webhooku identifikátor URI, který je zadat při vytváření hello výstrahy. Tento identifikátor URI musí být platný koncový bod HTTP nebo HTTPS. Azure účtuje jeden záznam za požadavek při aktivaci výstrahy.

## <a name="configuring-webhooks-via-hello-portal"></a>Konfigurace webhooky přes portál hello
Můžete přidat nebo aktualizovat webhook hello URI v úvodní obrazovka vytvořit nebo aktualizovat výstrahy v hello [portál](https://portal.azure.com/).

![Přidání pravidla výstrahy](./media/insights-webhooks-alerts/Alertwebhook.png)

Můžete také nakonfigurovat webhook tooa výstrahy toopost URI pomocí hello [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-hello-webhook"></a>Ověřování webhooku hello
Hello webhooku můžete ověřit pomocí ověřování na základě tokenu. Hello webhook identifikátor URI je uložit s tokenu ID, např. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Datová část schématu
Hello operaci POST obsahuje hello datové části JSON a schéma pro všechny metrika výstrahy.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Pole | Povinné | Opravené sadu hodnot | Poznámky |
|:--- |:--- |:--- |:--- |
| status |Ano |"Aktivovaná", "Přeložit" |Stav výstrahy hello na základě ujednání hello, že jste nastavili. |
| Kontext |Ano | |Hello kontext výstrahy. |
| časové razítko |Ano | |Hello čas, na které hello byla výstraha. |
| id |Ano | |Každé pravidlo výstrahy má jedinečné id. |
| jméno |Ano | |Název výstrahy Hello. |
| description |Ano | |Popis výstrahy hello. |
| conditionType |Ano |"Metriky", "Událost" |Podporuje dva typy výstrah. Jeden na základě metriky podmínky a hello jiných založené na události v protokolu aktivit hello. Toocheck tuto hodnotu použijte, pokud hello výstraha je založen na metrika nebo událostí. |
| Podmínka |Ano | |Hello toocheck konkrétních polí pro podle hello conditionType. |
| metricName |Metriky výstrahy | |Název Hello hello metrikou, která definuje jaké hello pravidlo monitoruje. |
| metricUnit |Metriky výstrahy |"Bajtů", "BytesPerSecond", "Count", "CountPerSecond", "Procenta", "Seconds" |jednotka Hello v hello metrika povoleny. [Povolené hodnoty jsou zde uvedeny](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Metriky výstrahy | |Hello skutečnou hodnotou hello metriku, která způsobila výstrahu hello. |
| Prahová hodnota |Metriky výstrahy | |Hello prahovou hodnotu, při které hello výstraha je aktivována. |
| velikost_okna |Metriky výstrahy | |Hello období čas, který je použité toomonitor výstrahy aktivity podle hello prahovou hodnotu. Musí být mezi 5 minutami a 1 dnem. Doba trvání formátu ISO 8601. |
| Agregace času |Metriky výstrahy |"Průměru", "Posledního", "Maximální", "Minimální", "Žádný", "celkové" |Jak by měl být kombinovány hello data, která se shromažďují v čase. Hello výchozí hodnota je průměr. [Povolené hodnoty jsou zde uvedeny](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operátor |Metriky výstrahy | |operátor Hello použít toocompare hello aktuální data metriky toohello nastavenou prahovou hodnotu. |
| subscriptionId |Ano | |ID předplatného Azure. |
| Název skupiny prostředků |Ano | |Název skupiny prostředků hello hello dopad prostředků. |
| resourceName |Ano | |Název prostředků hello dopad prostředků. |
| Typ prostředku |Ano | |Typ prostředku hello dopad prostředků. |
| resourceId |Ano | |ID prostředku hello dopad prostředků. |
| resourceRegion |Ano | |Oblast nebo umístění hello dopad prostředků. |
| portalLink |Ano | |Přímý odkaz toohello portálu prostředků souhrnná stránka. |
| properties |N |Nepovinné |Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události hello. Hello vlastnosti pole je nepovinné. Do vlastní uživatelské rozhraní nebo logiku aplikace na základě pracovní postup mohou uživatelé zadat klíč/hodnota, které lze předat prostřednictvím hello datové části. Hello jiný způsob, jak toopass vlastní vlastnosti back toohello webhooku je prostřednictvím hello identifikátor uri webhooku samotné (jako parametry dotazu) |

> [!NOTE]
> pole s vlastnostmi Hello lze nastavit pouze pomocí hello [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Další kroky
* Další informace o Azure výstrahy a pomocí webhooků v hello video [integrovat Azure výstrahy s PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Spustit skripty Azure Automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Použití aplikace logiky toosend serveru služby SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Použití aplikace logiky toosend Slack zprávu z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Použití aplikace logiky toosend zpráva tooan Azure Queue z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
