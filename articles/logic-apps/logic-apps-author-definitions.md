---
title: "pracovní postupy aaaDefine s JSON - Azure Logic Apps | Microsoft Docs"
description: "Jak toowrite definice pracovního postupu ve formátu JSON pro logic apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 0d69d334ecee9c3e7f8684cfde68ef0e85280358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Vytvoření definice pracovního postupu pro logic apps pomocí JSON

Vytvořením definice pracovního postupu pro [Azure Logic Apps](logic-apps-what-are-logic-apps.md) jednoduchý a deklarativní jazyce JSON. Pokud jste to ještě neudělali, přečtěte si nejprve [jak toocreate svou první aplikaci logiky pomocí návrháře aplikace logiky](logic-apps-create-a-logic-app.md). Viz také hello [úplné referenční informace pro jazyk definic workflowů hello](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Opakujte kroky pro seznam

tooiterate prostřednictvím pole, které má až too10 000 položek a provedení akce pro každou položku, použijte hello [typu foreach](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Zpracování chyb, pokud dojde k chybě

Obvykle chcete tooinclude *nápravy krok* – některé logiky, která provede *jenom v případě* jeden nebo více voláními nezdaří. Tento příklad načte data z různých míst, ale pokud hello volání selže, chceme tooPOST zprávu někde tak jsme můžete sledovat tohoto selhání později:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

toospecify, `postToErrorMessageQueue` spouští pouze `readData` má `Failed`, použijte hello `runAfter` vlastnosti, například toospecify seznamu možných hodnot tak, aby `runAfter` může být `["Succeeded", "Failed"]`.

Nakonec, protože v tomto příkladu teď zpracovává hello chyba, jsme už označit hello spustit jako `Failed`. Vzhledem k tomu, že jsme přidali hello krok pro zpracování této chyby v tomto příkladu, má hello spustit `Succeeded` i když jeden krok `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Paralelní spuštění dvou nebo více kroků

toorun více akcí paralelně, hello `runAfter` vlastnost musí být shodná za běhu. 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

V tomto příkladu obě `branch1` a `branch2` jsou nastaveny toorun po `readData`. V důsledku toho obou poboček spustit souběžně. Hello časové razítko pro obě pobočky se shoduje.

![Paralelní](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Připojení dvě paralelních větvích

Toho se můžete zapojit dvě akce, které jsou nastaveny toorun paralelně přidáním položky toohello `runAfter` vlastnost jako v předchozím příkladu hello.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Paralelní](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-tooa-different-configuration"></a>Mapování jinou konfiguraci tooa seznamu položek

Dále Řekněme, že má být tooget jiný obsah na základě hello hodnoty vlastnosti. Můžeme vytvořit mapu toodestinations hodnoty jako parametr:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

V takovém případě nám nejdřív získat seznam článků. Na základě hello kategorie, která byla definována jako parametr, druhý krok text hello používá mapy toolook až hello adresu URL pro získání obsahu hello.

Některé časy toonote tady: 

*   Hello [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funkce kontroluje, zda hello kategorie odpovídá jeden hello známé definovaných kategorií.

*   Po hello kategorie se nám získat, jsme pull hello položku z hello mapy pomocí hranatými závorkami:`parameters[...]`

## <a name="process-strings"></a>Proces řetězce

Můžete použít různé funkce toomanipulate řetězce. Předpokládejme například, že máme řetězec chceme toopass tooa systému, že jsme nejsou jisti, o správné zpracování pro kódování znaků. Jednou z možností je toobase64 kódování tento řetězec. Ale tooavoid uvozovací znaky v adrese URL, přidáme tooreplace pár znaků. 

Chceme také dílčí řetězec názvu hello pořadí, protože se nepoužívají hello prvních 5 znaků.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Funkční z uvnitř toooutside:

1. Získat hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) pro název hello orderer, takže se nám získat zpět hello celkový počet znaků.

2. Odečtena 5, protože chceme kratší řetězec.

3. Ve skutečnosti, trvat hello [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Začneme v indexu `5` a přejděte hello zbytek hello řetězec.

4. Převést tento dílčí řetězec tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) řetězec.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny hello `+` znaků a obsahující `-` znaků.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny hello `/` znaků a obsahující `_` znaků.

## <a name="work-with-date-times"></a>Práce s data a času

Hodnoty data a času může být užitečná, zejména v případě, že se pokoušíte toopull data ze zdroje dat, která nepodporuje přirozeně *aktivační události*. Můžete taky data a času pro hledání, jak dlouho jednotlivých kroků trvá.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

V tomto příkladu jsme extrahujte hello `startTime` hello v předchozím kroku. Potom jsme hello získat aktuální čas a odečítání sekundu:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Můžete použít jiné jednotky doby, jako je třeba `minutes` nebo `hours`. Nakonec jsme můžete porovnat tyto dvě hodnoty. Pokud hello první hodnota je menší než druhá hodnota, která hello pak více než jedna sekunda byla úspěšná, protože byl nejprve umístit hello pořadí.

tooformat kalendářních dat, můžeme použít formátování řetězce. Například tooget hello RFC1123, používáme [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). toolearn o formátování data, najdete v části [jazyk definic workflowů](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Parametry nasazení pro různá prostředí

Běžně mají životní cykly nasazení prostředí pro vývoj, pracovní prostředí a provozním prostředí. Například můžete použít stejné definice v těchto prostředích hello ale použít různé databáze. Podobně můžete chtít toouse hello stejné definice v různých oblastech pro vysokou dostupnost, ale má každé logiku aplikace instance tootalk toothat oblasti databáze.
Tento scénář se liší od trvá parametry v *runtime* kde místo toho používejte hello `trigger()` fungovat stejně jako v předchozím příkladu hello.

Můžete začít s základní definice následujícím způsobem:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

V hello skutečné `PUT` požadavku pro hello logic apps, můžete zadat parametr hello `uri`. Protože výchozí hodnota už existuje, datové části aplikace logiky hello vyžaduje tento parametr:

```
{
    "properties": {},
        "definition": {
          // Use hello definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

V každé prostředí je zadat jinou hodnotu pro hello `connection` parametr. 

Všechny hello možnosti, které máte k vytváření a správě aplikací logiky, najdete v části hello [dokumentace k REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
