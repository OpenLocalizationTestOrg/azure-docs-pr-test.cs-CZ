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
# <a name="logic-apps-loops-scopes-and-debatching"></a><span data-ttu-id="696f3-103">Smyčky, obory a rozdělení dávek v Logic Apps</span><span class="sxs-lookup"><span data-stu-id="696f3-103">Logic Apps Loops, Scopes, and Debatching</span></span>
  
<span data-ttu-id="696f3-104">Služba Logic Apps poskytuje několik způsobů toowork s pole, kolekce, listy a smyčky v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="696f3-104">Logic Apps provides a number of ways toowork with arrays, collections, batches, and loops within a workflow.</span></span>
  
## <a name="foreach-loop-and-arrays"></a><span data-ttu-id="696f3-105">Pole a smyčka typu ForEach</span><span class="sxs-lookup"><span data-stu-id="696f3-105">ForEach loop and arrays</span></span>
  
<span data-ttu-id="696f3-106">Služba Logic Apps můžete tooloop v rámci sady dat a provedení akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="696f3-106">Logic Apps allows you tooloop over a set of data and perform an action for each item.</span></span>  <span data-ttu-id="696f3-107">To je možné prostřednictvím hello `foreach` akce.</span><span class="sxs-lookup"><span data-stu-id="696f3-107">This is possible via hello `foreach` action.</span></span>  <span data-ttu-id="696f3-108">V Návrháři hello, můžete zadat tooadd pro každou smyčku.</span><span class="sxs-lookup"><span data-stu-id="696f3-108">In hello designer, you can specify tooadd a for each loop.</span></span>  <span data-ttu-id="696f3-109">Po výběru hello pole, které chcete tooiterate přes, můžete začít přidáním akce.</span><span class="sxs-lookup"><span data-stu-id="696f3-109">After selecting hello array you wish tooiterate over, you can begin adding actions.</span></span>  <span data-ttu-id="696f3-110">Aktuálně jsou omezené tooonly jednu akci za smyčka typu foreach, ale toto omezení se zruší, pokud hello přicházející týdny.</span><span class="sxs-lookup"><span data-stu-id="696f3-110">Currently you are limited tooonly one action per foreach loop, but this restriction will be lifted in hello coming weeks.</span></span>  <span data-ttu-id="696f3-111">Jednou v rámci smyčky hello můžete začít toospecify co má probíhat na každou hodnotu pole hello.</span><span class="sxs-lookup"><span data-stu-id="696f3-111">Once within hello loop you can begin toospecify what should occur at each value of hello array.</span></span>

<span data-ttu-id="696f3-112">Pokud používáte zobrazení kódu, můžete zadat, pro každou smyčku jako níže.</span><span class="sxs-lookup"><span data-stu-id="696f3-112">If using code-view, you can specify a for each loop like below.</span></span>  <span data-ttu-id="696f3-113">Toto je příklad pro každou smyčku, která odešle e-mail pro každý e-mailovou adresu, která obsahuje 'microsoft.com.:</span><span class="sxs-lookup"><span data-stu-id="696f3-113">This is an example of a for each loop that sends an email for each email address that contains 'microsoft.com':</span></span>

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
  
  <span data-ttu-id="696f3-114">A `foreach` akce můžete iterace v polích až too5 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="696f3-114">A `foreach` action can iterate over arrays up too5,000 rows.</span></span>  <span data-ttu-id="696f3-115">Každé iteraci spustí paralelně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="696f3-115">Each iteration will execute in parallel by default.</span></span>  

### <a name="sequential-foreach-loops"></a><span data-ttu-id="696f3-116">Sekvenční smyčky ForEach</span><span class="sxs-lookup"><span data-stu-id="696f3-116">Sequential ForEach loops</span></span>

<span data-ttu-id="696f3-117">tooenable tooexecute smyčka typu foreach postupně, hello `Sequential` by měla být přidána možnost operaci.</span><span class="sxs-lookup"><span data-stu-id="696f3-117">tooenable a foreach loop tooexecute sequentially, hello `Sequential` operation option should be added.</span></span>

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a><span data-ttu-id="696f3-118">Dokud smyčky</span><span class="sxs-lookup"><span data-stu-id="696f3-118">Until loop</span></span>
  
  <span data-ttu-id="696f3-119">Dokud je splněna podmínka, můžete provést akci nebo posloupnost akcí.</span><span class="sxs-lookup"><span data-stu-id="696f3-119">You can perform an action or series of actions until a condition is met.</span></span>  <span data-ttu-id="696f3-120">Hello nejběžnější scénáře volá koncový bod dokud nezískáte hello odpovědi, které hledáte.</span><span class="sxs-lookup"><span data-stu-id="696f3-120">hello most common scenario for this is calling an endpoint until you get hello response you are looking for.</span></span>  <span data-ttu-id="696f3-121">V Návrháři hello, můžete zadat tooadd dokud smyčky.</span><span class="sxs-lookup"><span data-stu-id="696f3-121">In hello designer, you can specify tooadd an until loop.</span></span>  <span data-ttu-id="696f3-122">Po přidání akce uvnitř hello smyčky, vám může nastavit hello ukončovací podmínky, stejně jako hello omezení smyčky.</span><span class="sxs-lookup"><span data-stu-id="696f3-122">After adding actions inside hello loop, you can set hello exit condition, as well as hello loop limits.</span></span>  <span data-ttu-id="696f3-123">Mezi cykly smyčky dochází ke zpoždění 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="696f3-123">There is a 1 minute delay between loop cycles.</span></span>
  
  <span data-ttu-id="696f3-124">Pokud používáte zobrazení kódu, můžete zadat dokud smyčky jako níže.</span><span class="sxs-lookup"><span data-stu-id="696f3-124">If using code-view, you can specify an until loop like below.</span></span>  <span data-ttu-id="696f3-125">Toto je příklad volání koncový bod protokolu HTTP, dokud hello odpovědi má hodnotu hello "Dokončeno".</span><span class="sxs-lookup"><span data-stu-id="696f3-125">This is an example of calling an HTTP endpoint until hello response body has hello value 'Completed'.</span></span>  <span data-ttu-id="696f3-126">Když se dokončí buď</span><span class="sxs-lookup"><span data-stu-id="696f3-126">It will complete when either</span></span> 
  
  * <span data-ttu-id="696f3-127">Odpověď HTTP má stav "dokončeno.</span><span class="sxs-lookup"><span data-stu-id="696f3-127">HTTP Response has status of 'Completed'</span></span>
  * <span data-ttu-id="696f3-128">To nezkusí 1 hodinu</span><span class="sxs-lookup"><span data-stu-id="696f3-128">It has tried for 1 hour</span></span>
  * <span data-ttu-id="696f3-129">Smyčce 100krát</span><span class="sxs-lookup"><span data-stu-id="696f3-129">It has looped 100 times</span></span>
  
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
  
## <a name="spliton-and-debatching"></a><span data-ttu-id="696f3-130">SplitOn a debatching</span><span class="sxs-lookup"><span data-stu-id="696f3-130">SplitOn and debatching</span></span>

<span data-ttu-id="696f3-131">Někdy se může zobrazit pole položek chcete toodebatch a spustit pracovní postup, na položku aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="696f3-131">Sometimes a trigger may receive an array of items that you want toodebatch and start a workflow per item.</span></span>  <span data-ttu-id="696f3-132">Můžete to provést prostřednictvím hello `spliton` příkaz.</span><span class="sxs-lookup"><span data-stu-id="696f3-132">This can be accomplished via hello `spliton` command.</span></span>  <span data-ttu-id="696f3-133">Ve výchozím nastavení, pokud vaše swagger aktivační událost určuje datové části, která je pole `spliton` přidá a spustit na položku start.</span><span class="sxs-lookup"><span data-stu-id="696f3-133">By default, if your trigger swagger specifies a payload that is an array, a `spliton` will be added and start a run per item.</span></span>  <span data-ttu-id="696f3-134">SplitOn lze přidat pouze tooa aktivační události.</span><span class="sxs-lookup"><span data-stu-id="696f3-134">SplitOn can only be added tooa trigger.</span></span>  <span data-ttu-id="696f3-135">To lze ručně nakonfigurované nebo přepsání v definici zobrazení kódu.</span><span class="sxs-lookup"><span data-stu-id="696f3-135">This can be manually configured or overridden in definition code-view.</span></span>  <span data-ttu-id="696f3-136">Nyní můžete SplitOn debatch pole nahoru too5 000 položek.</span><span class="sxs-lookup"><span data-stu-id="696f3-136">Currently SplitOn can debatch arrays up too5,000 items.</span></span>  <span data-ttu-id="696f3-137">Nemůže mít `spliton` a také implementovat vzor hello synchronní odpovědi.</span><span class="sxs-lookup"><span data-stu-id="696f3-137">You cannot have a `spliton` and also implement hello synchronous response pattern.</span></span>  <span data-ttu-id="696f3-138">Jakýkoli pracovní postup, který volá má `response` akce kromě příliš`spliton` spustí asynchronně a odeslat okamžitého `202 Accepted` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="696f3-138">Any workflow called that has a `response` action in addition too`spliton` will run asynchronously and send an immediate `202 Accepted` response.</span></span>  

<span data-ttu-id="696f3-139">SplitOn lze zadat v zobrazení kódu jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="696f3-139">SplitOn can be specified in code-view as hello following example.</span></span>  <span data-ttu-id="696f3-140">To přijímá pole položek a debatches na každém řádku.</span><span class="sxs-lookup"><span data-stu-id="696f3-140">This receives an array of items and debatches on each row.</span></span>

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

## <a name="scopes"></a><span data-ttu-id="696f3-141">Obory</span><span class="sxs-lookup"><span data-stu-id="696f3-141">Scopes</span></span>

<span data-ttu-id="696f3-142">Je možné toogroup sérii akcí společně s použitím oboru.</span><span class="sxs-lookup"><span data-stu-id="696f3-142">It is possible toogroup a series of actions together using a scope.</span></span>  <span data-ttu-id="696f3-143">To je obzvláště užitečné pro implementace zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="696f3-143">This is particularly useful for implementing exception handling.</span></span>  <span data-ttu-id="696f3-144">V Návrháři hello můžete přidat nový obor a začnete přidávat všechny akce v rámci ho.</span><span class="sxs-lookup"><span data-stu-id="696f3-144">In hello designer you can add a new scope, and begin adding any actions inside of it.</span></span>  <span data-ttu-id="696f3-145">Můžete definovat obory v zobrazení kódu jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="696f3-145">You can define scopes in code-view like hello following:</span></span>


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