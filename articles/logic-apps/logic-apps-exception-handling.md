---
title: "Chyba & výjimek - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 9af2f71b3d288cc6f4e271d0915545d43a1249bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a><span data-ttu-id="ebd03-103">Zpracování chyb a výjimek v Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="ebd03-103">Handle errors and exceptions in Azure Logic Apps</span></span>

<span data-ttu-id="ebd03-104">Azure Logic Apps nabízí bohaté nástroje a vzory pro vám pomůže zajistit, že vaše integrace robustní a je odolný proti selhání.</span><span class="sxs-lookup"><span data-stu-id="ebd03-104">Azure Logic Apps provides rich tools and patterns to help you make sure your integrations are robust and resilient against failures.</span></span> <span data-ttu-id="ebd03-105">Všechny Architektura integrace představuje výzvu služby a zkontrolujte, zda správně zpracovat problémy ze závislých systémů nebo výpadek.</span><span class="sxs-lookup"><span data-stu-id="ebd03-105">Any integration architecture poses the challenge of making sure to appropriately handle downtime or issues from dependent systems.</span></span> <span data-ttu-id="ebd03-106">Díky Logic Apps je zpracování chyb v první třídy rozhraní, která poskytuje nástroje, které potřebujete tak, aby fungoval na výjimek a chyb v vaše pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="ebd03-106">Logic Apps makes handling errors a first-class experience, giving you the tools you need to act on exceptions and errors in your workflows.</span></span>

## <a name="retry-policies"></a><span data-ttu-id="ebd03-107">Opakujte zásady</span><span class="sxs-lookup"><span data-stu-id="ebd03-107">Retry policies</span></span>

<span data-ttu-id="ebd03-108">Zásady opakování je nejzákladnější typ výjimky a zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="ebd03-108">A retry policy is the most basic type of exception and error handling.</span></span> <span data-ttu-id="ebd03-109">Pokud počáteční požadavek vypršel časový limit nebo se nezdařilo (každá žádost, jejímž výsledkem 429 nebo 5xx odpověď), tato zásada určuje, zda akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="ebd03-109">If an initial request timed out or failed (any request that results in a 429 or 5xx response), this policy defines whether the action should retry.</span></span> <span data-ttu-id="ebd03-110">Ve výchozím nastavení opakujte všechny akce 4 další časy intervalech 20 sekund.</span><span class="sxs-lookup"><span data-stu-id="ebd03-110">By default, all actions retry 4 additional times over 20-second intervals.</span></span> <span data-ttu-id="ebd03-111">Ano, pokud přijme první požadavek `500 Internal Server Error` odpovědi, modul pracovních postupů pozastaví 20 sekund a znovu se pokusí žádosti.</span><span class="sxs-lookup"><span data-stu-id="ebd03-111">So if the first request receives a `500 Internal Server Error` response, the workflow engine pauses for 20 seconds, and attempts the request again.</span></span> <span data-ttu-id="ebd03-112">Pokud po všech opakovaných pokusů, odpověď stále výjimku nebo selhání, pracovní postup bude pokračovat a označí stav akce jako `Failed`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-112">If after all retries, the response is still an exception or failure, the workflow continues and marks the action status as `Failed`.</span></span>

<span data-ttu-id="ebd03-113">Můžete nakonfigurovat zásady opakování v **vstupy** pro určitou akci.</span><span class="sxs-lookup"><span data-stu-id="ebd03-113">You can configure retry policies in the **inputs** for a particular action.</span></span> <span data-ttu-id="ebd03-114">Můžete například nakonfigurovat zásady opakování pokusit až 4 x 1 hodinu intervalech.</span><span class="sxs-lookup"><span data-stu-id="ebd03-114">For example, you can configure a retry policy to try as many as 4 times over 1-hour intervals.</span></span> <span data-ttu-id="ebd03-115">Úplné podrobnosti o vlastnosti vstupu najdete v tématu [akce pracovního postupu a aktivační události][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="ebd03-115">For full details about input properties, see [Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

<span data-ttu-id="ebd03-116">Pokud byste chtěli akci HTTP a opakujte 4 časy vyčkejte 10 minut mezi jednotlivými pokusy o, měli byste použít následující definice:</span><span class="sxs-lookup"><span data-stu-id="ebd03-116">If you wanted your HTTP action to retry 4 times and wait 10 minutes between each attempt, you would use the following definition:</span></span>

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

<span data-ttu-id="ebd03-117">Další informace o podporovaných syntaxi najdete v tématu [části zásady opakování akce pracovního postupu a aktivační události][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="ebd03-117">For more information on supported syntax, see the [retry-policy section in Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

## <a name="catch-failures-with-the-runafter-property"></a><span data-ttu-id="ebd03-118">Catch – selhání s vlastností RunAfter</span><span class="sxs-lookup"><span data-stu-id="ebd03-118">Catch failures with the RunAfter property</span></span>

<span data-ttu-id="ebd03-119">Každá akce logic app deklaruje akce, které musíte dokončit před spuštěním akce, jako je řazení podle kroků v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="ebd03-119">Each logic app action declares which actions must finish before the action starts, like ordering the steps in your workflow.</span></span> <span data-ttu-id="ebd03-120">V definici akce toto řazení se označuje jako `runAfter` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ebd03-120">In the action definition, this ordering is known as the `runAfter` property.</span></span> <span data-ttu-id="ebd03-121">Tato vlastnost je objekt, který popisuje, které akce a akce stavy provést akci.</span><span class="sxs-lookup"><span data-stu-id="ebd03-121">This property is an object that describes which actions and action statuses execute the action.</span></span> <span data-ttu-id="ebd03-122">Ve výchozím nastavení, jsou všechny akce, které jsou přidány prostřednictvím návrháře aplikace logiky hodnotu `runAfter` v předchozím kroku Pokud v předchozím kroku `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-122">By default, all actions added through the Logic App Designer are set to `runAfter` the previous step if the previous step `Succeeded`.</span></span> <span data-ttu-id="ebd03-123">Ale můžete přizpůsobit, tato hodnota má provést akce, pokud mají předchozí akce `Failed`, `Skipped`, nebo sadu možné z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ebd03-123">However, you can customize this value to fire actions when previous actions have `Failed`, `Skipped`, or a possible set of these values.</span></span> <span data-ttu-id="ebd03-124">Pokud chcete přidat položku do tématu Service Bus určené po určité akci `Insert_Row` selže, můžete použít následující `runAfter` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ebd03-124">If you wanted to add an item to a designated Service Bus topic after a specific action `Insert_Row` fails, you could use the following `runAfter` configuration:</span></span>

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

<span data-ttu-id="ebd03-125">Upozornění `runAfter` má provést, pokud je hodnota nastavena `Insert_Row` akce je `Failed`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-125">Notice the `runAfter` property is set to fire if the `Insert_Row` action is `Failed`.</span></span> <span data-ttu-id="ebd03-126">Akci spustit, pokud je stav akce `Succeeded`, `Failed`, nebo `Skipped`, použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="ebd03-126">To run the action if the action status is `Succeeded`, `Failed`, or `Skipped`, use this syntax:</span></span>

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> <span data-ttu-id="ebd03-127">Akce, které jsou spuštěny a dokončeny úspěšně po předchozí akce se nezdařila, jsou označeny jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-127">Actions that run and complete successfully after a preceding action has failed, are marked as `Succeeded`.</span></span> <span data-ttu-id="ebd03-128">Toto chování znamená, že pokud jste úspěšně catch všechny chyby v pracovním postupu spustit samotné je označen jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-128">This behavior means that if you successfully catch all failures in a workflow, the run itself is marked as `Succeeded`.</span></span>

## <a name="scopes-and-results-to-evaluate-actions"></a><span data-ttu-id="ebd03-129">Obory a výsledky, které slouží k vyhodnocení, akce</span><span class="sxs-lookup"><span data-stu-id="ebd03-129">Scopes and results to evaluate actions</span></span>

<span data-ttu-id="ebd03-130">Podobná jak můžete spustit po jednotlivé akce, můžete taky seskupit akce uvnitř [oboru](../logic-apps/logic-apps-loops-and-scopes.md), které fungují jako logické seskupení akce.</span><span class="sxs-lookup"><span data-stu-id="ebd03-130">Similar to how you can run after individual actions, you can also group actions together inside a [scope](../logic-apps/logic-apps-loops-and-scopes.md), which act as a logical grouping of actions.</span></span> <span data-ttu-id="ebd03-131">Obory jsou užitečné pro uspořádání vaše akce aplikace logiky i pro provádění agregační hodnocení na stav oboru.</span><span class="sxs-lookup"><span data-stu-id="ebd03-131">Scopes are useful both for organizing your logic app actions, and for performing aggregate evaluations on the status of a scope.</span></span> <span data-ttu-id="ebd03-132">Obor samotné obdrží stav po dokončení všech akcí v oboru.</span><span class="sxs-lookup"><span data-stu-id="ebd03-132">The scope itself receives a status after all actions in a scope have finished.</span></span> <span data-ttu-id="ebd03-133">Stav oboru je určen s stejná kritéria jako spustit.</span><span class="sxs-lookup"><span data-stu-id="ebd03-133">The scope status is determined with the same criteria as a run.</span></span> <span data-ttu-id="ebd03-134">Pokud je poslední akce v větev provádění `Failed` nebo `Aborted`, je stav `Failed`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-134">If the final action in an execution branch is `Failed` or `Aborted`, the status is `Failed`.</span></span>

<span data-ttu-id="ebd03-135">Chcete-li provést určité akce pro všechny chyby, které bylo provedeno v rámci oboru, můžete použít `runAfter` s oborem, který je označen `Failed`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-135">To fire specific actions for any failures that happened within the scope, you can use `runAfter` with a scope that is marked `Failed`.</span></span> <span data-ttu-id="ebd03-136">Pokud *žádné* selhání akce v oboru, spuštění po obor selže umožňuje vytvořit jednu akci k zachycení selhání.</span><span class="sxs-lookup"><span data-stu-id="ebd03-136">If *any* actions in the scope fail, running after a scope fails lets you create a single action to catch failures.</span></span>

### <a name="getting-the-context-of-failures-with-results"></a><span data-ttu-id="ebd03-137">Získávání kontextu selhání s výsledky.</span><span class="sxs-lookup"><span data-stu-id="ebd03-137">Getting the context of failures with results</span></span>

<span data-ttu-id="ebd03-138">I když zachytávání chyb z oboru je užitečné, můžete také kontextu, které vám pomohou pochopit přesně akce, které se nezdařila a všechny chyby nebo stavové kódy, které byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="ebd03-138">Although catching failures from a scope is useful, you might also want context to help you understand exactly which actions failed, and any errors or status codes that were returned.</span></span> <span data-ttu-id="ebd03-139">`@result()` Funkce workflowu poskytuje kontext o výsledek všechny akce v oboru.</span><span class="sxs-lookup"><span data-stu-id="ebd03-139">The `@result()` workflow function provides context about the result of all actions in a scope.</span></span>

<span data-ttu-id="ebd03-140">`@result()`přijímá jeden parametr, název oboru a vrátí pole všech akce výsledků v rámci tohoto oboru.</span><span class="sxs-lookup"><span data-stu-id="ebd03-140">`@result()` takes a single parameter, scope name, and returns an array of all the action results from within that scope.</span></span> <span data-ttu-id="ebd03-141">Tyto objekty akce zahrnují stejné atributy, jako `@actions()` výstupy objektu, včetně čas spuštění akce, akce koncový čas, stav akce, akce vstupy, akce korelace ID a akce.</span><span class="sxs-lookup"><span data-stu-id="ebd03-141">These action objects include the same attributes as the `@actions()` object, including action start time, action end time, action status, action inputs, action correlation IDs, and action outputs.</span></span> <span data-ttu-id="ebd03-142">Kontext všechny akce, které se nezdařilo odeslat v rámci oboru, můžete snadno spárujte `@result()` fungovat s `runAfter`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-142">To send context of any actions that failed within a scope, you can easily pair an `@result()` function with a `runAfter`.</span></span>

<span data-ttu-id="ebd03-143">K provedení akce *pro každou* akce v oboru, `Failed`, filtrovat pole výsledky na akce, které se nezdařilo, může párovat `@result()` s  **[pole filtru](../connectors/connectors-native-query.md)**  akce a  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  smyčky.</span><span class="sxs-lookup"><span data-stu-id="ebd03-143">To execute an action *for each* action in a scope that `Failed`, filter the array of results to actions that failed, you can pair `@result()` with a **[Filter Array](../connectors/connectors-native-query.md)** action and a **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)** loop.</span></span> <span data-ttu-id="ebd03-144">Můžete provést pole filtrované výsledek a provedení akce pro každé selhání pomocí **ForEach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="ebd03-144">You can take the filtered result array and perform an action for each failure using the **ForEach** loop.</span></span> <span data-ttu-id="ebd03-145">Tady je příklad, za nímž následuje podrobné vysvětlení, který odesílá požadavek HTTP POST s text odpovědi o všechny akce, které se nepodařilo v rámci oboru `My_Scope`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-145">Here's an example, followed by a detailed explanation, that sends an HTTP POST request with the response body of any actions that failed within the scope `My_Scope`.</span></span>

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

<span data-ttu-id="ebd03-146">Zde je podrobný postup popisující, co se stane:</span><span class="sxs-lookup"><span data-stu-id="ebd03-146">Here's a detailed walkthrough to describe what happens:</span></span>

1. <span data-ttu-id="ebd03-147">Chcete-li získat výsledek všechny akce v rámci `My_Scope`, **pole filtru** filtrů Akce `@result('My_Scope')`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-147">To get the result of all actions within `My_Scope`, the **Filter Array** action filters `@result('My_Scope')`.</span></span>

2. <span data-ttu-id="ebd03-148">Podmínky pro **pole filtru** libovolnou `@result()` položku, která je rovna stavu `Failed`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-148">The condition for **Filter Array** is any `@result()` item that has status equal to `Failed`.</span></span> <span data-ttu-id="ebd03-149">Tato podmínka filtry pole s všechny výsledky akce z `My_Scope` do pole s pouze se nezdařilo výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="ebd03-149">This condition filters the array with all action results from `My_Scope` to an array with only failed action results.</span></span>

3. <span data-ttu-id="ebd03-150">Provedení **pro každou** akce **filtrovat pole** výstupy.</span><span class="sxs-lookup"><span data-stu-id="ebd03-150">Perform a **For Each** action on the **Filtered Array** outputs.</span></span> <span data-ttu-id="ebd03-151">Tento krok provede akci *pro každou* výsledek akce, který byl dříve nefiltruje se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ebd03-151">This step performs an action *for each* failed action result that was previously filtered.</span></span>

    <span data-ttu-id="ebd03-152">Pokud v oboru jednu akci se nezdařilo, akce v `foreach` spustit jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="ebd03-152">If a single action in the scope failed, the actions in the `foreach` run only once.</span></span> 
    <span data-ttu-id="ebd03-153">Mnoho selhání akce způsobí, že jednu akci za selhání.</span><span class="sxs-lookup"><span data-stu-id="ebd03-153">Many failed actions cause one action per failure.</span></span>

4. <span data-ttu-id="ebd03-154">Odeslání požadavku HTTP POST na `foreach` položky text odpovědi, nebo `@item()['outputs']['body']`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-154">Send an HTTP POST on the `foreach` item response body, or `@item()['outputs']['body']`.</span></span> <span data-ttu-id="ebd03-155">`@result()` Tvar položka je stejný jako `@actions()` utvářejí a lze analyzovat stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ebd03-155">The `@result()` item shape is the same as the `@actions()` shape, and can be parsed the same way.</span></span>

5. <span data-ttu-id="ebd03-156">Patří dva vlastní hlavičky s názvem selhání akce `@item()['name']` a neúspěšný spusťte klienta, ID sledování `@item()['clientTrackingId']`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-156">Include two custom headers with the failed action name `@item()['name']` and the failed run client tracking ID `@item()['clientTrackingId']`.</span></span>

<span data-ttu-id="ebd03-157">Pro referenci tady je příklad jednoho `@result()` položky, zobrazuje `name`, `body`, a `clientTrackingId` vlastnosti, které jsou analyzovány v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ebd03-157">For reference, here's an example of a single `@result()` item, showing the `name`, `body`, and `clientTrackingId` properties that are parsed in the previous example.</span></span> <span data-ttu-id="ebd03-158">Mimo `foreach`, `@result()` vrátí pole z těchto objektů.</span><span class="sxs-lookup"><span data-stu-id="ebd03-158">Outside of a `foreach`, `@result()` returns an array of these objects.</span></span>

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

<span data-ttu-id="ebd03-159">K provedení různých zpracování vzory výjimek, můžete použít výrazy uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="ebd03-159">To perform different exception handling patterns, you can use the expressions shown previously.</span></span> <span data-ttu-id="ebd03-160">Můžete zvolit provedení jednoho výjimka zpracování akce mimo rozsah, který přijímá pole celý filtrované chyb a odebrat `foreach`.</span><span class="sxs-lookup"><span data-stu-id="ebd03-160">You might choose to execute a single exception handling action outside the scope that accepts the entire filtered array of failures, and remove the `foreach`.</span></span> <span data-ttu-id="ebd03-161">Můžete použít také další užitečné vlastnosti z `@result()` odpovědi uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="ebd03-161">You can also include other useful properties from the `@result()` response shown previously.</span></span>

## <a name="azure-diagnostics-and-telemetry"></a><span data-ttu-id="ebd03-162">Azure diagnostiky a telemetrii</span><span class="sxs-lookup"><span data-stu-id="ebd03-162">Azure Diagnostics and telemetry</span></span>

<span data-ttu-id="ebd03-163">Předchozích jsou skvělý způsob, jak zpracování chyb a výjimek v rámci spuštění, ale můžete také určit a reagují na chyby, které jsou nezávislé na spuštění sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ebd03-163">The previous patterns are great way to handle errors and exceptions within a run, but you can also identify and respond to errors independent of the run itself.</span></span> 
<span data-ttu-id="ebd03-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) poskytuje jednoduchý způsob, jak odeslat všechny události pracovního postupu (včetně všech stavů spustit a akce) účtu služby Azure Storage nebo centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="ebd03-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) provides a simple way to send all workflow events (including all run and action statuses) to an Azure Storage account or an Azure Event Hub.</span></span> <span data-ttu-id="ebd03-165">Abyste mohli vyhodnotit spuštění stavy, můžete sledování metrik a protokolování nebo publikovat je do libovolného monitorování nástroje, kterému dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="ebd03-165">To evaluate run statuses, you can monitor the logs and metrics, or publish them into any monitoring tool you prefer.</span></span> <span data-ttu-id="ebd03-166">Jednou z možných možností je k vysílání datového proudu všechny události prostřednictvím centra událostí Azure do [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="ebd03-166">One potential option is to stream all the events through Azure Event Hub into [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> <span data-ttu-id="ebd03-167">Do služby Stream Analytics lze zapsat za provozu dotazy vypnout všechny anomálií, průměry nebo selhání z diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="ebd03-167">In Stream Analytics, you can write live queries off any anomalies, averages, or failures from the diagnostic logs.</span></span> <span data-ttu-id="ebd03-168">Stream Analytics můžete snadno výstup do jiných zdrojů dat, jako jsou fronty, témata, SQL, databáze Cosmos Azure a Power BI.</span><span class="sxs-lookup"><span data-stu-id="ebd03-168">Stream Analytics can easily output to other data sources like queues, topics, SQL, Azure Cosmos DB, and Power BI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebd03-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebd03-169">Next Steps</span></span>

* [<span data-ttu-id="ebd03-170">V tématu Jak zákazník sestavení službou Azure Logic Apps zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="ebd03-170">See how a customer builds error handling with Azure Logic Apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [<span data-ttu-id="ebd03-171">Najít další aplikace logiky příkladů a scénářů</span><span class="sxs-lookup"><span data-stu-id="ebd03-171">Find more Logic Apps examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="ebd03-172">Naučte se vytvořit automatické nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="ebd03-172">Learn how to create automated deployments for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="ebd03-173">Vytvoření a nasazení aplikací logiky s využitím sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebd03-173">Build and deploy logic apps with Visual Studio</span></span>](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
