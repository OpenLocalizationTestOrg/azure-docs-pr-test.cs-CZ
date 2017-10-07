---
title: "aaaError & výjimek - Azure Logic Apps | Microsoft Docs"
description: "Vzory pro chybové události a zpracování výjimek v Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 326a252310c8dfb154e583f91c9421675e448d1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Zpracování chyb a výjimek v Azure Logic Apps

Azure Logic Apps nabízí bohaté nástroje, a vzory toohelp ujistěte, že jsou vaše integrace robustní a odolný proti selhání. Všechny Architektura integrace představuje výzvu hello převedení zda tooappropriately popisovač výpadku nebo problémů na závislé systémy. Zpracování chyb Logic Apps usnadňují hello prvotřídní prostředí, která poskytuje nástroje, které potřebujete tooact na výjimky a chyby ve svých pracovních postupech.

## <a name="retry-policies"></a>Opakujte zásady

Zásady opakování je hello nejzákladnější typ výjimky a zpracování chyb. Pokud počáteční požadavek vypršel časový limit nebo se nezdařilo (každá žádost, jejímž výsledkem 429 nebo 5xx odpověď), tato zásada určuje, zda hello akci opakujte. Ve výchozím nastavení opakujte všechny akce 4 další časy intervalech 20 sekund. Ano, pokud obdrží hello první požadavek `500 Internal Server Error` odpovědi, modul pracovních postupů hello pozastavuje 20 sekund a pokusy o hello požadavek znovu. Pokud po všech opakovaných pokusů, hello odpovědi je stále výjimku nebo selhání, pokračuje hello pracovního postupu a značky hello stav akce jako `Failed`.

Můžete nakonfigurovat zásady opakování v hello **vstupy** pro určitou akci. Například můžete nastavit tootry zásady opakování až 4 časy intervalech 1 hodinu. Úplné podrobnosti o vlastnosti vstupu najdete v tématu [akce pracovního postupu a aktivační události][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Pokud jste chtěli vaší tooretry akce HTTP 4 časy a vyčkejte 10 minut mezi jednotlivými pokusy o, měli byste použít hello následující definice:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Další informace o podporovaných syntaxi najdete v tématu hello [části zásady opakování akce pracovního postupu a aktivační události][retryPolicyMSDN].

## <a name="catch-failures-with-hello-runafter-property"></a>Catch – selhání s hello RunAfter vlastnost

Každá akce logic app deklaruje akce, které musíte dokončit před spuštěním hello akce, jako je řazení hello kroky v pracovním postupu. V definici akce hello toto řazení se označuje jako hello `runAfter` vlastnost. Tato vlastnost je objekt, který popisuje, které akce a akce stavy provést akci hello. Standardně jsou všechny akce, které jsou přidány prostřednictvím hello návrhář aplikace na základě logiky nastavena příliš`runAfter` předchozí krok text hello, pokud hello předchozího kroku `Succeeded`. Však můžete přizpůsobit akce toofire tuto hodnotu, pokud předchozí akce `Failed`, `Skipped`, nebo sadu možné z těchto hodnot. Pokud byste chtěli tooadd tooa položky určené tématu Service Bus po určité akci `Insert_Row` selže, můžete použít následující hello `runAfter` konfigurace:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Všimněte si hello `runAfter` vlastnost nastavena toofire, pokud hello `Insert_Row` akce je `Failed`. Akce toorun hello, pokud je stav akce hello `Succeeded`, `Failed`, nebo `Skipped`, použijte následující syntaxi:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Akce, které jsou spuštěny a dokončeny úspěšně po předchozí akce se nezdařila, jsou označeny jako `Succeeded`. Toto chování znamená, že pokud jste úspěšně catch všechny chyby v pracovním postupu, hello spustit je označena jako `Succeeded`.

## <a name="scopes-and-results-tooevaluate-actions"></a>Akce tooevaluate obory a výsledky

Podobné toohow můžete spustit po jednotlivé akce, můžete taky seskupit akce uvnitř [oboru](../logic-apps/logic-apps-loops-and-scopes.md), které fungují jako logické seskupení akce. Obory jsou užitečné pro uspořádání vaše akce aplikace logiky i pro provádění agregační hodnocení na dobrý stav oboru. Hello oboru samotné obdrží stav po dokončení všech akcí v oboru. stav oboru Hello je určen s hello stejná kritéria jako spustit. Pokud hello konečné akcí v větev provádění `Failed` nebo `Aborted`, je stav hello `Failed`.

toofire konkrétní akce pro všechny chyby, které bylo provedeno v rámci oboru hello, můžete použít `runAfter` s oborem, který je označen `Failed`. Pokud *žádné* akce v oboru hello nezdaří, spuštění po obor selže umožňuje vytvořit jednu akci toocatch selhání.

### <a name="getting-hello-context-of-failures-with-results"></a>Získávání kontextu hello chyb s výsledky.

I když zachytávání chyb z oboru je užitečné, můžete také kontextu toohelp pochopit přesně akce, které se nezdařila, a všechny chyby nebo stavové kódy, které byly vráceny. Hello `@result()` funkce workflowu poskytuje kontext o hello výsledek všechny akce v oboru.

`@result()`přijímá jeden parametr, název oboru a vrátí pole všechny výsledky hello akce z v rámci tohoto oboru. Tyto objekty akce zahrnují hello stejné atributy jako hello `@actions()` výstupy objektu, včetně čas spuštění akce, akce koncový čas, stav akce, akce vstupy, akce korelace ID a akce. toosend kontextu všechny akce, které se nepodařilo v rámci oboru, můžete snadno spárujete `@result()` fungovat s `runAfter`.

tooexecute akce *pro každou* akce v oboru, `Failed`pole filtru hello tooactions výsledky, které selhaly, může párovat `@result()` s  **[pole filtru](../connectors/connectors-native-query.md)**  akce a  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  smyčky. Můžete provést hello filtrované výsledek pole a provedení akce pro každé selhání pomocí hello **ForEach** smyčky. Tady je příklad, za nímž následuje podrobné vysvětlení, který odesílá požadavek HTTP POST s text odpovědi hello všechny akce, které se nepodařilo v rámci oboru hello `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Zde je podrobný návod toodescribe, co se stane:

1. výsledek hello tooget všechny akce v rámci `My_Scope`, hello **pole filtru** filtrů Akce `@result('My_Scope')`.

2. Hello podmínku pro **pole filtru** libovolnou `@result()` položku, která obsahuje stav rovna příliš`Failed`. Tato podmínka filtry hello pole s všechny výsledky akce z `My_Scope` tooan pole s jediným se nezdařilo výsledky akce.

3. Provedení **pro každou** akce hello **filtrovat pole** výstupy. Tento krok provede akci *pro každou* výsledek akce, který byl dříve nefiltruje se nezdařilo.

    Pokud se jedna akce v oboru hello nezdařila, hello akce v hello `foreach` spustit jenom jednou. 
    Mnoho selhání akce způsobí, že jednu akci za selhání.

4. Odeslání požadavku HTTP POST na hello `foreach` položky text odpovědi, nebo `@item()['outputs']['body']`. Hello `@result()` tvar položka je hello stejné jako hello `@actions()` utvářejí a lze analyzovat hello stejný způsobem.

5. Patří dva vlastní hlavičky s názvem selhání akce hello `@item()['name']` a hello se nezdařilo spuštění klienta, ID sledování `@item()['clientTrackingId']`.

Pro referenci tady je příklad jednoho `@result()` položku zobrazující hello `name`, `body`, a `clientTrackingId` vlastnosti, které jsou analyzovány v předchozím příkladu hello. Mimo `foreach`, `@result()` vrátí pole z těchto objektů.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

vzory tooperform různých výjimek, můžete použít výrazy hello uvedený výše. Můžete zvolit jednu výjimka zpracování akce hello oboru, který přijímá hello celý filtrované pole selhání tooexecute a odebrat hello `foreach`. Můžete použít také další užitečné vlastnosti z hello `@result()` odpovědi uvedený výše.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiky a telemetrii

jsou skvělý způsob toohandle chyby a výjimky v rámci spustit zprostředkovatele Hello předchozích, ale také můžete identifikovat a řešit tooerrors nezávislé hello spustit. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) poskytuje jednoduchý způsob toosend všechny pracovní postup události (včetně všech stavů spustit a akce) tooan účet služby Azure Storage nebo centra událostí Azure. tooevaluate spusťte stavy, můžete monitorovat hello protokoly a metriky nebo publikovat je do libovolného monitorování nástroje, kterému dáváte přednost. Jednou z možných možností je toostream všechny události hello prostřednictvím centra událostí Azure do [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). V Stream Analytics můžete napsat dotazy za provozu mimo jakékoli anomálie, průměry nebo selhání z hello diagnostické protokoly. Stream Analytics můžete snadno výstupní tooother zdroje dat jako fronty, témata, SQL, databáze Cosmos Azure a Power BI.

## <a name="next-steps"></a>Další kroky

* [V tématu Jak zákazník sestavení službou Azure Logic Apps zpracování chyb](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Najít další aplikace logiky příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Zjistěte, jak toocreate automatizované nasazení pro logic apps](../logic-apps/logic-apps-create-deploy-template.md)
* [Vytvoření a nasazení aplikací logiky s využitím sady Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
