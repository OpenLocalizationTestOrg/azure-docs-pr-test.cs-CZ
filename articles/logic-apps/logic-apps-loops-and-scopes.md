---
title: "Vytvoření smyčky a obory nebo debatch data v pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření smyčky k iteraci v rámci dat, akce skupiny do oborů, nebo debatch data ke spuštění více pracovních postupů v Azure Logic Apps."
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
ms.openlocfilehash: 413a2ba9107ca259ed577825bf0a17ff5622f1ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a><span data-ttu-id="a330c-103">Smyčky, obory a rozdělení dávek v Logic Apps</span><span class="sxs-lookup"><span data-stu-id="a330c-103">Logic Apps Loops, Scopes, and Debatching</span></span>
  
<span data-ttu-id="a330c-104">Služba Logic Apps poskytuje několik způsobů, jak pracovat s poli, kolekcí, dávky a smyčky v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="a330c-104">Logic Apps provides a number of ways to work with arrays, collections, batches, and loops within a workflow.</span></span>
  
## <a name="foreach-loop-and-arrays"></a><span data-ttu-id="a330c-105">Pole a smyčka typu ForEach</span><span class="sxs-lookup"><span data-stu-id="a330c-105">ForEach loop and arrays</span></span>
  
<span data-ttu-id="a330c-106">Služba Logic Apps umožňuje smyčky v rámci sady dat a provedení akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="a330c-106">Logic Apps allows you to loop over a set of data and perform an action for each item.</span></span>  <span data-ttu-id="a330c-107">To je možné prostřednictvím `foreach` akce.</span><span class="sxs-lookup"><span data-stu-id="a330c-107">This is possible via the `foreach` action.</span></span>  <span data-ttu-id="a330c-108">V návrháři, můžete přidat pro každou smyčku.</span><span class="sxs-lookup"><span data-stu-id="a330c-108">In the designer, you can specify to add a for each loop.</span></span>  <span data-ttu-id="a330c-109">Až vyberete pole, která si přejete iterace, můžete začít přidáním akce.</span><span class="sxs-lookup"><span data-stu-id="a330c-109">After selecting the array you wish to iterate over, you can begin adding actions.</span></span>  <span data-ttu-id="a330c-110">Aktuálně jste omezeni na jenom jednu akci za smyčka typu foreach, ale toto omezení se zruší, v následujících týdnech.</span><span class="sxs-lookup"><span data-stu-id="a330c-110">Currently you are limited to only one action per foreach loop, but this restriction will be lifted in the coming weeks.</span></span>  <span data-ttu-id="a330c-111">V rámci smyčky začnete jednou zadejte, co má probíhat na každou hodnotu pole.</span><span class="sxs-lookup"><span data-stu-id="a330c-111">Once within the loop you can begin to specify what should occur at each value of the array.</span></span>

<span data-ttu-id="a330c-112">Pokud používáte zobrazení kódu, můžete zadat, pro každou smyčku jako níže.</span><span class="sxs-lookup"><span data-stu-id="a330c-112">If using code-view, you can specify a for each loop like below.</span></span>  <span data-ttu-id="a330c-113">Toto je příklad pro každou smyčku, která odešle e-mail pro každý e-mailovou adresu, která obsahuje 'microsoft.com.:</span><span class="sxs-lookup"><span data-stu-id="a330c-113">This is an example of a for each loop that sends an email for each email address that contains 'microsoft.com':</span></span>

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
  
  <span data-ttu-id="a330c-114">A `foreach` akce iterovat přes maticových až 5000 řádků.</span><span class="sxs-lookup"><span data-stu-id="a330c-114">A `foreach` action can iterate over arrays up to 5,000 rows.</span></span>  <span data-ttu-id="a330c-115">Každé iteraci spustí paralelně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a330c-115">Each iteration will execute in parallel by default.</span></span>  

### <a name="sequential-foreach-loops"></a><span data-ttu-id="a330c-116">Sekvenční smyčky ForEach</span><span class="sxs-lookup"><span data-stu-id="a330c-116">Sequential ForEach loops</span></span>

<span data-ttu-id="a330c-117">Chcete-li povolit smyčka typu foreach provést postupně, `Sequential` by měla být přidána možnost operaci.</span><span class="sxs-lookup"><span data-stu-id="a330c-117">To enable a foreach loop to execute sequentially, the `Sequential` operation option should be added.</span></span>

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a><span data-ttu-id="a330c-118">Dokud smyčky</span><span class="sxs-lookup"><span data-stu-id="a330c-118">Until loop</span></span>
  
  <span data-ttu-id="a330c-119">Dokud je splněna podmínka, můžete provést akci nebo posloupnost akcí.</span><span class="sxs-lookup"><span data-stu-id="a330c-119">You can perform an action or series of actions until a condition is met.</span></span>  <span data-ttu-id="a330c-120">Nejběžnější scénáře je volání koncový bod, dokud nezískáte odpověď, kterou hledáte.</span><span class="sxs-lookup"><span data-stu-id="a330c-120">The most common scenario for this is calling an endpoint until you get the response you are looking for.</span></span>  <span data-ttu-id="a330c-121">V návrháři, můžete přidat dokud smyčky.</span><span class="sxs-lookup"><span data-stu-id="a330c-121">In the designer, you can specify to add an until loop.</span></span>  <span data-ttu-id="a330c-122">Po přidání akce uvnitř smyčky, můžete nastavit ukončovací podmínky, jakož i smyčky omezení.</span><span class="sxs-lookup"><span data-stu-id="a330c-122">After adding actions inside the loop, you can set the exit condition, as well as the loop limits.</span></span>  <span data-ttu-id="a330c-123">Mezi cykly smyčky dochází ke zpoždění 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="a330c-123">There is a 1 minute delay between loop cycles.</span></span>
  
  <span data-ttu-id="a330c-124">Pokud používáte zobrazení kódu, můžete zadat dokud smyčky jako níže.</span><span class="sxs-lookup"><span data-stu-id="a330c-124">If using code-view, you can specify an until loop like below.</span></span>  <span data-ttu-id="a330c-125">Toto je příklad volání koncový bod protokolu HTTP, dokud text odpovědi má hodnotu "Dokončeno".</span><span class="sxs-lookup"><span data-stu-id="a330c-125">This is an example of calling an HTTP endpoint until the response body has the value 'Completed'.</span></span>  <span data-ttu-id="a330c-126">Když se dokončí buď</span><span class="sxs-lookup"><span data-stu-id="a330c-126">It will complete when either</span></span> 
  
  * <span data-ttu-id="a330c-127">Odpověď HTTP má stav "dokončeno.</span><span class="sxs-lookup"><span data-stu-id="a330c-127">HTTP Response has status of 'Completed'</span></span>
  * <span data-ttu-id="a330c-128">To nezkusí 1 hodinu</span><span class="sxs-lookup"><span data-stu-id="a330c-128">It has tried for 1 hour</span></span>
  * <span data-ttu-id="a330c-129">Smyčce 100krát</span><span class="sxs-lookup"><span data-stu-id="a330c-129">It has looped 100 times</span></span>
  
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
  
## <a name="spliton-and-debatching"></a><span data-ttu-id="a330c-130">SplitOn a debatching</span><span class="sxs-lookup"><span data-stu-id="a330c-130">SplitOn and debatching</span></span>

<span data-ttu-id="a330c-131">Někdy se může zobrazit pole položek, které chcete debatch a spustit pracovní postup, na položku aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="a330c-131">Sometimes a trigger may receive an array of items that you want to debatch and start a workflow per item.</span></span>  <span data-ttu-id="a330c-132">Můžete to provést prostřednictvím `spliton` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a330c-132">This can be accomplished via the `spliton` command.</span></span>  <span data-ttu-id="a330c-133">Ve výchozím nastavení, pokud vaše swagger aktivační událost určuje datové části, která je pole `spliton` přidá a spustit na položku start.</span><span class="sxs-lookup"><span data-stu-id="a330c-133">By default, if your trigger swagger specifies a payload that is an array, a `spliton` will be added and start a run per item.</span></span>  <span data-ttu-id="a330c-134">SplitOn lze přidat pouze pro aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="a330c-134">SplitOn can only be added to a trigger.</span></span>  <span data-ttu-id="a330c-135">To lze ručně nakonfigurované nebo přepsání v definici zobrazení kódu.</span><span class="sxs-lookup"><span data-stu-id="a330c-135">This can be manually configured or overridden in definition code-view.</span></span>  <span data-ttu-id="a330c-136">Nyní můžete debatch SplitOn maticových až 5 000 položek.</span><span class="sxs-lookup"><span data-stu-id="a330c-136">Currently SplitOn can debatch arrays up to 5,000 items.</span></span>  <span data-ttu-id="a330c-137">Nemůže mít `spliton` a také implementovat vzor synchronní odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a330c-137">You cannot have a `spliton` and also implement the synchronous response pattern.</span></span>  <span data-ttu-id="a330c-138">Jakýkoli pracovní postup, který volá má `response` akce kromě `spliton` spustí asynchronně a odeslat okamžitého `202 Accepted` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a330c-138">Any workflow called that has a `response` action in addition to `spliton` will run asynchronously and send an immediate `202 Accepted` response.</span></span>  

<span data-ttu-id="a330c-139">SplitOn lze zadat v zobrazení kódu jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="a330c-139">SplitOn can be specified in code-view as the following example.</span></span>  <span data-ttu-id="a330c-140">To přijímá pole položek a debatches na každém řádku.</span><span class="sxs-lookup"><span data-stu-id="a330c-140">This receives an array of items and debatches on each row.</span></span>

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

## <a name="scopes"></a><span data-ttu-id="a330c-141">Obory</span><span class="sxs-lookup"><span data-stu-id="a330c-141">Scopes</span></span>

<span data-ttu-id="a330c-142">Je možné seskupit sérii akcí společně s použitím oboru.</span><span class="sxs-lookup"><span data-stu-id="a330c-142">It is possible to group a series of actions together using a scope.</span></span>  <span data-ttu-id="a330c-143">To je obzvláště užitečné pro implementace zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="a330c-143">This is particularly useful for implementing exception handling.</span></span>  <span data-ttu-id="a330c-144">V Návrháři můžete přidat nový obor a začnete přidávat všechny akce v rámci ho.</span><span class="sxs-lookup"><span data-stu-id="a330c-144">In the designer you can add a new scope, and begin adding any actions inside of it.</span></span>  <span data-ttu-id="a330c-145">Můžete definovat obory v zobrazení kódu takto:</span><span class="sxs-lookup"><span data-stu-id="a330c-145">You can define scopes in code-view like the following:</span></span>


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