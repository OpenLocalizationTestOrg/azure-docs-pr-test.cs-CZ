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
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="f1dc6-103">Vytvoření definice pracovního postupu pro logic apps pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="f1dc6-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="f1dc6-104">Vytvořením definice pracovního postupu pro [Azure Logic Apps](logic-apps-what-are-logic-apps.md) jednoduchý a deklarativní jazyce JSON.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="f1dc6-105">Pokud jste to ještě neudělali, přečtěte si nejprve [jak toocreate svou první aplikaci logiky pomocí návrháře aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-105">If you haven't already, first review [how toocreate your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="f1dc6-106">Viz také hello [úplné referenční informace pro jazyk definic workflowů hello](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-106">Also, see hello [full reference for hello Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="f1dc6-107">Opakujte kroky pro seznam</span><span class="sxs-lookup"><span data-stu-id="f1dc6-107">Repeat steps over a list</span></span>

<span data-ttu-id="f1dc6-108">tooiterate prostřednictvím pole, které má až too10 000 položek a provedení akce pro každou položku, použijte hello [typu foreach](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-108">tooiterate through an array that has up too10,000 items and perform an action for each item, use hello [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="f1dc6-109">Zpracování chyb, pokud dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="f1dc6-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="f1dc6-110">Obvykle chcete tooinclude *nápravy krok* – některé logiky, která provede *jenom v případě* jeden nebo více voláními nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-110">Usually, you want tooinclude a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="f1dc6-111">Tento příklad načte data z různých míst, ale pokud hello volání selže, chceme tooPOST zprávu někde tak jsme můžete sledovat tohoto selhání později:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-111">This example gets data from various places, but if hello call fails, we want tooPOST a message somewhere so we can track down that failure later:</span></span>  

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

<span data-ttu-id="f1dc6-112">toospecify, `postToErrorMessageQueue` spouští pouze `readData` má `Failed`, použijte hello `runAfter` vlastnosti, například toospecify seznamu možných hodnot tak, aby `runAfter` může být `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-112">toospecify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use hello `runAfter` property, for example, toospecify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="f1dc6-113">Nakonec, protože v tomto příkladu teď zpracovává hello chyba, jsme už označit hello spustit jako `Failed`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-113">Finally, because this example now handles hello error, we no longer mark hello run as `Failed`.</span></span> <span data-ttu-id="f1dc6-114">Vzhledem k tomu, že jsme přidali hello krok pro zpracování této chyby v tomto příkladu, má hello spustit `Succeeded` i když jeden krok `Failed`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-114">Because we added hello step for handling this failure in this example, hello run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="f1dc6-115">Paralelní spuštění dvou nebo více kroků</span><span class="sxs-lookup"><span data-stu-id="f1dc6-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="f1dc6-116">toorun více akcí paralelně, hello `runAfter` vlastnost musí být shodná za běhu.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-116">toorun multiple actions in parallel, hello `runAfter` property must be equivalent at runtime.</span></span> 

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

<span data-ttu-id="f1dc6-117">V tomto příkladu obě `branch1` a `branch2` jsou nastaveny toorun po `readData`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-117">In this example, both `branch1` and `branch2` are set toorun after `readData`.</span></span> <span data-ttu-id="f1dc6-118">V důsledku toho obou poboček spustit souběžně.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="f1dc6-119">Hello časové razítko pro obě pobočky se shoduje.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-119">hello timestamp for both branches is identical.</span></span>

![Paralelní](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="f1dc6-121">Připojení dvě paralelních větvích</span><span class="sxs-lookup"><span data-stu-id="f1dc6-121">Join two parallel branches</span></span>

<span data-ttu-id="f1dc6-122">Toho se můžete zapojit dvě akce, které jsou nastaveny toorun paralelně přidáním položky toohello `runAfter` vlastnost jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-122">You can join two actions that are set toorun in parallel by adding items toohello `runAfter` property as in hello previous example.</span></span>

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

## <a name="map-list-items-tooa-different-configuration"></a><span data-ttu-id="f1dc6-124">Mapování jinou konfiguraci tooa seznamu položek</span><span class="sxs-lookup"><span data-stu-id="f1dc6-124">Map list items tooa different configuration</span></span>

<span data-ttu-id="f1dc6-125">Dále Řekněme, že má být tooget jiný obsah na základě hello hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-125">Next, let's say that we want tooget different content based on hello value of a property.</span></span> <span data-ttu-id="f1dc6-126">Můžeme vytvořit mapu toodestinations hodnoty jako parametr:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-126">We can create a map of values toodestinations as a parameter:</span></span>  

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

<span data-ttu-id="f1dc6-127">V takovém případě nám nejdřív získat seznam článků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="f1dc6-128">Na základě hello kategorie, která byla definována jako parametr, druhý krok text hello používá mapy toolook až hello adresu URL pro získání obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-128">Based on hello category that was defined as a parameter, hello second step uses a map toolook up hello URL for getting hello content.</span></span>

<span data-ttu-id="f1dc6-129">Některé časy toonote tady:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-129">Some times toonote here:</span></span> 

*   <span data-ttu-id="f1dc6-130">Hello [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funkce kontroluje, zda hello kategorie odpovídá jeden hello známé definovaných kategorií.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-130">hello [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether hello category matches one of hello known defined categories.</span></span>

*   <span data-ttu-id="f1dc6-131">Po hello kategorie se nám získat, jsme pull hello položku z hello mapy pomocí hranatými závorkami:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="f1dc6-131">After we get hello category, we can pull hello item from hello map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="f1dc6-132">Proces řetězce</span><span class="sxs-lookup"><span data-stu-id="f1dc6-132">Process strings</span></span>

<span data-ttu-id="f1dc6-133">Můžete použít různé funkce toomanipulate řetězce.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-133">You can use various functions toomanipulate strings.</span></span> <span data-ttu-id="f1dc6-134">Předpokládejme například, že máme řetězec chceme toopass tooa systému, že jsme nejsou jisti, o správné zpracování pro kódování znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-134">For example, suppose we have a string that we want toopass tooa system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="f1dc6-135">Jednou z možností je toobase64 kódování tento řetězec.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-135">One option is toobase64 encode this string.</span></span> <span data-ttu-id="f1dc6-136">Ale tooavoid uvozovací znaky v adrese URL, přidáme tooreplace pár znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-136">However, tooavoid escaping in a URL, we are going tooreplace a few characters.</span></span> 

<span data-ttu-id="f1dc6-137">Chceme také dílčí řetězec názvu hello pořadí, protože se nepoužívají hello prvních 5 znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-137">We also want a substring of hello order's name because hello first five characters are not used.</span></span>

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

<span data-ttu-id="f1dc6-138">Funkční z uvnitř toooutside:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-138">Working from inside toooutside:</span></span>

1. <span data-ttu-id="f1dc6-139">Získat hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) pro název hello orderer, takže se nám získat zpět hello celkový počet znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-139">Get hello [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for hello orderer's name, so we get back hello total number of characters.</span></span>

2. <span data-ttu-id="f1dc6-140">Odečtena 5, protože chceme kratší řetězec.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="f1dc6-141">Ve skutečnosti, trvat hello [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-141">Actually, take hello [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="f1dc6-142">Začneme v indexu `5` a přejděte hello zbytek hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-142">We start at index `5` and go hello remainder of hello string.</span></span>

4. <span data-ttu-id="f1dc6-143">Převést tento dílčí řetězec tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) řetězec.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-143">Convert this substring tooa [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="f1dc6-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny hello `+` znaků a obsahující `-` znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="f1dc6-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny hello `/` znaků a obsahující `_` znaků.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="f1dc6-146">Práce s data a času</span><span class="sxs-lookup"><span data-stu-id="f1dc6-146">Work with Date Times</span></span>

<span data-ttu-id="f1dc6-147">Hodnoty data a času může být užitečná, zejména v případě, že se pokoušíte toopull data ze zdroje dat, která nepodporuje přirozeně *aktivační události*.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-147">Date Times can be useful, particularly when you are trying toopull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="f1dc6-148">Můžete taky data a času pro hledání, jak dlouho jednotlivých kroků trvá.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-148">You can also use Date Times for finding how long various steps are taking.</span></span>

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

<span data-ttu-id="f1dc6-149">V tomto příkladu jsme extrahujte hello `startTime` hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-149">In this example, we extract hello `startTime` from hello previous step.</span></span> <span data-ttu-id="f1dc6-150">Potom jsme hello získat aktuální čas a odečítání sekundu:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-150">Then we get hello current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="f1dc6-151">Můžete použít jiné jednotky doby, jako je třeba `minutes` nebo `hours`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="f1dc6-152">Nakonec jsme můžete porovnat tyto dvě hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="f1dc6-153">Pokud hello první hodnota je menší než druhá hodnota, která hello pak více než jedna sekunda byla úspěšná, protože byl nejprve umístit hello pořadí.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-153">If hello first value is less than hello second value, then more than one second has passed since hello order was first placed.</span></span>

<span data-ttu-id="f1dc6-154">tooformat kalendářních dat, můžeme použít formátování řetězce.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-154">tooformat dates, we can use string formatters.</span></span> <span data-ttu-id="f1dc6-155">Například tooget hello RFC1123, používáme [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-155">For example, tooget hello RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="f1dc6-156">toolearn o formátování data, najdete v části [jazyk definic workflowů](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-156">toolearn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="f1dc6-157">Parametry nasazení pro různá prostředí</span><span class="sxs-lookup"><span data-stu-id="f1dc6-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="f1dc6-158">Běžně mají životní cykly nasazení prostředí pro vývoj, pracovní prostředí a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="f1dc6-159">Například můžete použít stejné definice v těchto prostředích hello ale použít různé databáze.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-159">For example, you might use hello same definition in all these environments but use different databases.</span></span> <span data-ttu-id="f1dc6-160">Podobně můžete chtít toouse hello stejné definice v různých oblastech pro vysokou dostupnost, ale má každé logiku aplikace instance tootalk toothat oblasti databáze.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-160">Likewise, you might want toouse hello same definition across different regions for high availability but want each logic app instance tootalk toothat region's database.</span></span>
<span data-ttu-id="f1dc6-161">Tento scénář se liší od trvá parametry v *runtime* kde místo toho používejte hello `trigger()` fungovat stejně jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-161">This scenario differs from taking parameters at *runtime* where instead, you should use hello `trigger()` function as in hello previous example.</span></span>

<span data-ttu-id="f1dc6-162">Můžete začít s základní definice následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-162">You can start with a basic definition like this example:</span></span>

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

<span data-ttu-id="f1dc6-163">V hello skutečné `PUT` požadavku pro hello logic apps, můžete zadat parametr hello `uri`.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-163">In hello actual `PUT` request for hello logic apps, you can provide hello parameter `uri`.</span></span> <span data-ttu-id="f1dc6-164">Protože výchozí hodnota už existuje, datové části aplikace logiky hello vyžaduje tento parametr:</span><span class="sxs-lookup"><span data-stu-id="f1dc6-164">Because a default value no longer exists, hello logic app payload requires this parameter:</span></span>

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

<span data-ttu-id="f1dc6-165">V každé prostředí je zadat jinou hodnotu pro hello `connection` parametr.</span><span class="sxs-lookup"><span data-stu-id="f1dc6-165">In each environment, you can provide a different value for hello `connection` parameter.</span></span> 

<span data-ttu-id="f1dc6-166">Všechny hello možnosti, které máte k vytváření a správě aplikací logiky, najdete v části hello [dokumentace k REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1dc6-166">For all hello options that you have for creating and managing logic apps, see hello [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
