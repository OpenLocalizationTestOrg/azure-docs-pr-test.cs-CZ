---
title: "Definovat pracovní postupy s JSON - Azure Logic Apps | Microsoft Docs"
description: "Jak napsat definice pracovního postupu ve formátu JSON pro logic apps"
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
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="e0906-103">Vytvoření definice pracovního postupu pro logic apps pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="e0906-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="e0906-104">Vytvořením definice pracovního postupu pro [Azure Logic Apps](logic-apps-what-are-logic-apps.md) jednoduchý a deklarativní jazyce JSON.</span><span class="sxs-lookup"><span data-stu-id="e0906-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="e0906-105">Pokud jste to ještě neudělali, přečtěte si nejprve [postup vytvoření první aplikace logiky pomocí návrháře aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e0906-105">If you haven't already, first review [how to create your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="e0906-106">Další informace naleznete [úplné referenční dokumentace pro jazyk definic workflowů](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="e0906-106">Also, see the [full reference for the Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="e0906-107">Opakujte kroky pro seznam</span><span class="sxs-lookup"><span data-stu-id="e0906-107">Repeat steps over a list</span></span>

<span data-ttu-id="e0906-108">K iteraci v rámci pole, které má až 10 000 položek a provedení akce pro každou položku, použijte [typu foreach](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="e0906-108">To iterate through an array that has up to 10,000 items and perform an action for each item, use the [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="e0906-109">Zpracování chyb, pokud dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="e0906-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="e0906-110">Obvykle, které chcete zahrnout *nápravy krok* – některé logiky, která provede *jenom v případě* jeden nebo více voláními nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e0906-110">Usually, you want to include a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="e0906-111">Tento příklad načte data z různých míst, ale pokud volání selže, chceme, takže jsme můžete sledovat tohoto selhání později někde odeslat zprávu:</span><span class="sxs-lookup"><span data-stu-id="e0906-111">This example gets data from various places, but if the call fails, we want to POST a message somewhere so we can track down that failure later:</span></span>  

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

<span data-ttu-id="e0906-112">Chcete-li určit, že `postToErrorMessageQueue` spouští pouze `readData` má `Failed`, použít `runAfter` vlastnosti, například pro zadání seznamu možných hodnot, tak, aby `runAfter` může být `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="e0906-112">To specify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use the `runAfter` property, for example, to specify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="e0906-113">Nakonec, protože v tomto příkladu teď zpracovává chyby, jsme už označit spustit jako `Failed`.</span><span class="sxs-lookup"><span data-stu-id="e0906-113">Finally, because this example now handles the error, we no longer mark the run as `Failed`.</span></span> <span data-ttu-id="e0906-114">Vzhledem k tomu, že jsme přidali v kroku pro zpracování této chyby v tomto příkladu, má spustit `Succeeded` i když jeden krok `Failed`.</span><span class="sxs-lookup"><span data-stu-id="e0906-114">Because we added the step for handling this failure in this example, the run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="e0906-115">Paralelní spuštění dvou nebo více kroků</span><span class="sxs-lookup"><span data-stu-id="e0906-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="e0906-116">Ke spouštění více akcí paralelně, `runAfter` vlastnost musí být shodná za běhu.</span><span class="sxs-lookup"><span data-stu-id="e0906-116">To run multiple actions in parallel, the `runAfter` property must be equivalent at runtime.</span></span> 

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

<span data-ttu-id="e0906-117">V tomto příkladu obě `branch1` a `branch2` jsou nastaveny na spouštění `readData`.</span><span class="sxs-lookup"><span data-stu-id="e0906-117">In this example, both `branch1` and `branch2` are set to run after `readData`.</span></span> <span data-ttu-id="e0906-118">V důsledku toho obou poboček spustit souběžně.</span><span class="sxs-lookup"><span data-stu-id="e0906-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="e0906-119">Časové razítko pro obě pobočky se shoduje.</span><span class="sxs-lookup"><span data-stu-id="e0906-119">The timestamp for both branches is identical.</span></span>

![Paralelní](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="e0906-121">Připojení dvě paralelních větvích</span><span class="sxs-lookup"><span data-stu-id="e0906-121">Join two parallel branches</span></span>

<span data-ttu-id="e0906-122">Toho se můžete zapojit dvě akce, které jsou nastaveny na spouštění paralelní přidáním položky `runAfter` vlastnost jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e0906-122">You can join two actions that are set to run in parallel by adding items to the `runAfter` property as in the previous example.</span></span>

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

## <a name="map-list-items-to-a-different-configuration"></a><span data-ttu-id="e0906-124">Položky seznamu mapy do jiné konfigurace</span><span class="sxs-lookup"><span data-stu-id="e0906-124">Map list items to a different configuration</span></span>

<span data-ttu-id="e0906-125">Dále Řekněme, že má získat jiný obsah na základě hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0906-125">Next, let's say that we want to get different content based on the value of a property.</span></span> <span data-ttu-id="e0906-126">Jako parametr jsme vytvořit mapování hodnot na cíle:</span><span class="sxs-lookup"><span data-stu-id="e0906-126">We can create a map of values to destinations as a parameter:</span></span>  

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

<span data-ttu-id="e0906-127">V takovém případě nám nejdřív získat seznam článků.</span><span class="sxs-lookup"><span data-stu-id="e0906-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="e0906-128">Podle kategorie, která byla definována jako parametr, druhý krok používá mapu vyhledat adresu URL pro získání obsahu.</span><span class="sxs-lookup"><span data-stu-id="e0906-128">Based on the category that was defined as a parameter, the second step uses a map to look up the URL for getting the content.</span></span>

<span data-ttu-id="e0906-129">Některé časy si zde:</span><span class="sxs-lookup"><span data-stu-id="e0906-129">Some times to note here:</span></span> 

*   <span data-ttu-id="e0906-130">[ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) Funkce kontroluje, zda kategorii odpovídá jedné ze známé definovaných kategorií.</span><span class="sxs-lookup"><span data-stu-id="e0906-130">The [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether the category matches one of the known defined categories.</span></span>

*   <span data-ttu-id="e0906-131">Po kategorie se nám získat, jsme načítat položky od mapy pomocí hranatými závorkami:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="e0906-131">After we get the category, we can pull the item from the map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="e0906-132">Proces řetězce</span><span class="sxs-lookup"><span data-stu-id="e0906-132">Process strings</span></span>

<span data-ttu-id="e0906-133">Různé funkce můžete použít k manipulaci s řetězci.</span><span class="sxs-lookup"><span data-stu-id="e0906-133">You can use various functions to manipulate strings.</span></span> <span data-ttu-id="e0906-134">Předpokládejme například, máme řetězec, který chcete předat do systému, ale nemůžeme nejsou jisti, o správné zpracování pro kódování znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-134">For example, suppose we have a string that we want to pass to a system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="e0906-135">Jednou z možností je ve formátu Base64 kódování tento řetězec.</span><span class="sxs-lookup"><span data-stu-id="e0906-135">One option is to base64 encode this string.</span></span> <span data-ttu-id="e0906-136">Ale abyste se vyhnuli uvozovací znaky v adrese URL, přidáme nahrazení pár znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-136">However, to avoid escaping in a URL, we are going to replace a few characters.</span></span> 

<span data-ttu-id="e0906-137">Chceme také dílčí řetězec názvu pořadí, protože se nepoužívají prvních 5 znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-137">We also want a substring of the order's name because the first five characters are not used.</span></span>

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

<span data-ttu-id="e0906-138">Pracovní z uvnitř k mimo:</span><span class="sxs-lookup"><span data-stu-id="e0906-138">Working from inside to outside:</span></span>

1. <span data-ttu-id="e0906-139">Získat [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) pro název orderer, takže se nám získat zpět celkový počet znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-139">Get the [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for the orderer's name, so we get back the total number of characters.</span></span>

2. <span data-ttu-id="e0906-140">Odečtena 5, protože chceme kratší řetězec.</span><span class="sxs-lookup"><span data-stu-id="e0906-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="e0906-141">Ve skutečnosti, proveďte [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="e0906-141">Actually, take the [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="e0906-142">Začneme v indexu `5` a přejděte zbytek řetězce.</span><span class="sxs-lookup"><span data-stu-id="e0906-142">We start at index `5` and go the remainder of the string.</span></span>

4. <span data-ttu-id="e0906-143">Převést tento dílčí [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) řetězec.</span><span class="sxs-lookup"><span data-stu-id="e0906-143">Convert this substring to a [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="e0906-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `+` znaků a obsahující `-` znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="e0906-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `/` znaků a obsahující `_` znaků.</span><span class="sxs-lookup"><span data-stu-id="e0906-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="e0906-146">Práce s data a času</span><span class="sxs-lookup"><span data-stu-id="e0906-146">Work with Date Times</span></span>

<span data-ttu-id="e0906-147">Hodnoty data a času může být užitečná, zejména v případě, že se pokoušíte vyžádá data ze zdroje dat, která nepodporuje přirozeně *aktivační události*.</span><span class="sxs-lookup"><span data-stu-id="e0906-147">Date Times can be useful, particularly when you are trying to pull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="e0906-148">Můžete taky data a času pro hledání, jak dlouho jednotlivých kroků trvá.</span><span class="sxs-lookup"><span data-stu-id="e0906-148">You can also use Date Times for finding how long various steps are taking.</span></span>

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

<span data-ttu-id="e0906-149">V tomto příkladu jsme extrahujte `startTime` z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="e0906-149">In this example, we extract the `startTime` from the previous step.</span></span> <span data-ttu-id="e0906-150">Potom jsme získat aktuální čas a odečítání sekundu:</span><span class="sxs-lookup"><span data-stu-id="e0906-150">Then we get the current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="e0906-151">Můžete použít jiné jednotky doby, jako je třeba `minutes` nebo `hours`.</span><span class="sxs-lookup"><span data-stu-id="e0906-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="e0906-152">Nakonec jsme můžete porovnat tyto dvě hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e0906-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="e0906-153">Pokud první hodnota je menší než druhá hodnota, která pak více než jedna sekunda byla úspěšná, protože byl nejprve umístit pořadí.</span><span class="sxs-lookup"><span data-stu-id="e0906-153">If the first value is less than the second value, then more than one second has passed since the order was first placed.</span></span>

<span data-ttu-id="e0906-154">K formátování kalendářních dat, můžeme použít formátování řetězce.</span><span class="sxs-lookup"><span data-stu-id="e0906-154">To format dates, we can use string formatters.</span></span> <span data-ttu-id="e0906-155">Například RFC1123 získáte používáme [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="e0906-155">For example, to get the RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="e0906-156">Další informace o formátování data, najdete v části [jazyk definic workflowů](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="e0906-156">To learn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="e0906-157">Parametry nasazení pro různá prostředí</span><span class="sxs-lookup"><span data-stu-id="e0906-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="e0906-158">Běžně mají životní cykly nasazení prostředí pro vývoj, pracovní prostředí a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0906-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="e0906-159">Například můžete použít stejné definice v těchto prostředích ale použití různých databází.</span><span class="sxs-lookup"><span data-stu-id="e0906-159">For example, you might use the same definition in all these environments but use different databases.</span></span> <span data-ttu-id="e0906-160">Podobně můžete chtít použít stejné definice v různých oblastech pro vysokou dostupnost, ale má každá instance aplikace logiky ke komunikaci s danou oblast databáze.</span><span class="sxs-lookup"><span data-stu-id="e0906-160">Likewise, you might want to use the same definition across different regions for high availability but want each logic app instance to talk to that region's database.</span></span>
<span data-ttu-id="e0906-161">Tento scénář se liší od trvá parametry v *runtime* kde místo toho by měla použít `trigger()` fungovat jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e0906-161">This scenario differs from taking parameters at *runtime* where instead, you should use the `trigger()` function as in the previous example.</span></span>

<span data-ttu-id="e0906-162">Můžete začít s základní definice následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e0906-162">You can start with a basic definition like this example:</span></span>

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

<span data-ttu-id="e0906-163">V skutečnou `PUT` požadavku pro logic apps, můžete zadat parametr `uri`.</span><span class="sxs-lookup"><span data-stu-id="e0906-163">In the actual `PUT` request for the logic apps, you can provide the parameter `uri`.</span></span> <span data-ttu-id="e0906-164">Protože výchozí hodnota už existuje, datové části aplikace logiky vyžaduje tento parametr:</span><span class="sxs-lookup"><span data-stu-id="e0906-164">Because a default value no longer exists, the logic app payload requires this parameter:</span></span>

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
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

<span data-ttu-id="e0906-165">V každém prostředí, můžete zadat jinou hodnotu pro `connection` parametr.</span><span class="sxs-lookup"><span data-stu-id="e0906-165">In each environment, you can provide a different value for the `connection` parameter.</span></span> 

<span data-ttu-id="e0906-166">Všechny možnosti, které máte k vytváření a správě aplikací logiky najdete v tématu [dokumentace k REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0906-166">For all the options that you have for creating and managing logic apps, see the [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
