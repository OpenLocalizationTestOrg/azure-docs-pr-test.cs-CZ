---
title: "aaaCreate v cyklu a rozsahy nebo debatch data v pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření smyčky tooiterate prostřednictvím data, akce skupiny do oborů, nebo debatch toostart dat více pracovních postupů v Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: e612ec2e83541f028916a07bf12c44e7b1f57ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Smyčky, obory a rozdělení dávek v Logic Apps
  
Služba Logic Apps poskytuje několik způsobů toowork s pole, kolekce, listy a smyčky v pracovním postupu.
  
## <a name="foreach-loop-and-arrays"></a>Pole a smyčka typu ForEach
  
Služba Logic Apps můžete tooloop v rámci sady dat a provedení akce pro každou položku.  To je možné prostřednictvím hello `foreach` akce.  V Návrháři hello, můžete zadat tooadd pro každou smyčku.  Po výběru hello pole, které chcete tooiterate přes, můžete začít přidáním akce.  Aktuálně jsou omezené tooonly jednu akci za smyčka typu foreach, ale toto omezení se zruší, pokud hello přicházející týdny.  Jednou v rámci smyčky hello můžete začít toospecify co má probíhat na každou hodnotu pole hello.

Pokud používáte zobrazení kódu, můžete zadat, pro každou smyčku jako níže.  Toto je příklad pro každou smyčku, která odešle e-mail pro každý e-mailovou adresu, která obsahuje 'microsoft.com.:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akce můžete iterace v polích až too5 000 řádků.  Každé iteraci spustí paralelně ve výchozím nastavení.  

### <a name="sequential-foreach-loops"></a>Sekvenční smyčky ForEach

tooenable tooexecute smyčka typu foreach postupně, hello `Sequential` by měla být přidána možnost operaci.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Dokud smyčky
  
  Dokud je splněna podmínka, můžete provést akci nebo posloupnost akcí.  Hello nejběžnější scénáře volá koncový bod dokud nezískáte hello odpovědi, které hledáte.  V Návrháři hello, můžete zadat tooadd dokud smyčky.  Po přidání akce uvnitř hello smyčky, vám může nastavit hello ukončovací podmínky, stejně jako hello omezení smyčky.  Mezi cykly smyčky dochází ke zpoždění 1 minuta.
  
  Pokud používáte zobrazení kódu, můžete zadat dokud smyčky jako níže.  Toto je příklad volání koncový bod protokolu HTTP, dokud hello odpovědi má hodnotu hello "Dokončeno".  Když se dokončí buď 
  
  * Odpověď HTTP má stav "dokončeno.
  * To nezkusí 1 hodinu
  * Smyčce 100krát
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn a debatching

Někdy se může zobrazit pole položek chcete toodebatch a spustit pracovní postup, na položku aktivační událost.  Můžete to provést prostřednictvím hello `spliton` příkaz.  Ve výchozím nastavení, pokud vaše swagger aktivační událost určuje datové části, která je pole `spliton` přidá a spustit na položku start.  SplitOn lze přidat pouze tooa aktivační události.  To lze ručně nakonfigurované nebo přepsání v definici zobrazení kódu.  Nyní můžete SplitOn debatch pole nahoru too5 000 položek.  Nemůže mít `spliton` a také implementovat vzor hello synchronní odpovědi.  Jakýkoli pracovní postup, který volá má `response` akce kromě příliš`spliton` spustí asynchronně a odeslat okamžitého `202 Accepted` odpovědi.  

SplitOn lze zadat v zobrazení kódu jako hello následující ukázka.  To přijímá pole položek a debatches na každém řádku.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Obory

Je možné toogroup sérii akcí společně s použitím oboru.  To je obzvláště užitečné pro implementace zpracování výjimek.  V Návrháři hello můžete přidat nový obor a začnete přidávat všechny akce v rámci ho.  Můžete definovat obory v zobrazení kódu jako hello následující:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```