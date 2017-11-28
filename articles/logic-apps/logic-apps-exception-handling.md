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
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a><span data-ttu-id="5a2ae-103">Zpracování chyb a výjimek v Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="5a2ae-103">Handle errors and exceptions in Azure Logic Apps</span></span>

<span data-ttu-id="5a2ae-104">Azure Logic Apps nabízí bohaté nástroje, a vzory toohelp ujistěte, že jsou vaše integrace robustní a odolný proti selhání.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-104">Azure Logic Apps provides rich tools and patterns toohelp you make sure your integrations are robust and resilient against failures.</span></span> <span data-ttu-id="5a2ae-105">Všechny Architektura integrace představuje výzvu hello převedení zda tooappropriately popisovač výpadku nebo problémů na závislé systémy.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-105">Any integration architecture poses hello challenge of making sure tooappropriately handle downtime or issues from dependent systems.</span></span> <span data-ttu-id="5a2ae-106">Zpracování chyb Logic Apps usnadňují hello prvotřídní prostředí, která poskytuje nástroje, které potřebujete tooact na výjimky a chyby ve svých pracovních postupech.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-106">Logic Apps makes handling errors a first-class experience, giving you hello tools you need tooact on exceptions and errors in your workflows.</span></span>

## <a name="retry-policies"></a><span data-ttu-id="5a2ae-107">Opakujte zásady</span><span class="sxs-lookup"><span data-stu-id="5a2ae-107">Retry policies</span></span>

<span data-ttu-id="5a2ae-108">Zásady opakování je hello nejzákladnější typ výjimky a zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-108">A retry policy is hello most basic type of exception and error handling.</span></span> <span data-ttu-id="5a2ae-109">Pokud počáteční požadavek vypršel časový limit nebo se nezdařilo (každá žádost, jejímž výsledkem 429 nebo 5xx odpověď), tato zásada určuje, zda hello akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-109">If an initial request timed out or failed (any request that results in a 429 or 5xx response), this policy defines whether hello action should retry.</span></span> <span data-ttu-id="5a2ae-110">Ve výchozím nastavení opakujte všechny akce 4 další časy intervalech 20 sekund.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-110">By default, all actions retry 4 additional times over 20-second intervals.</span></span> <span data-ttu-id="5a2ae-111">Ano, pokud obdrží hello první požadavek `500 Internal Server Error` odpovědi, modul pracovních postupů hello pozastavuje 20 sekund a pokusy o hello požadavek znovu.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-111">So if hello first request receives a `500 Internal Server Error` response, hello workflow engine pauses for 20 seconds, and attempts hello request again.</span></span> <span data-ttu-id="5a2ae-112">Pokud po všech opakovaných pokusů, hello odpovědi je stále výjimku nebo selhání, pokračuje hello pracovního postupu a značky hello stav akce jako `Failed`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-112">If after all retries, hello response is still an exception or failure, hello workflow continues and marks hello action status as `Failed`.</span></span>

<span data-ttu-id="5a2ae-113">Můžete nakonfigurovat zásady opakování v hello **vstupy** pro určitou akci.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-113">You can configure retry policies in hello **inputs** for a particular action.</span></span> <span data-ttu-id="5a2ae-114">Například můžete nastavit tootry zásady opakování až 4 časy intervalech 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-114">For example, you can configure a retry policy tootry as many as 4 times over 1-hour intervals.</span></span> <span data-ttu-id="5a2ae-115">Úplné podrobnosti o vlastnosti vstupu najdete v tématu [akce pracovního postupu a aktivační události][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="5a2ae-115">For full details about input properties, see [Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

<span data-ttu-id="5a2ae-116">Pokud jste chtěli vaší tooretry akce HTTP 4 časy a vyčkejte 10 minut mezi jednotlivými pokusy o, měli byste použít hello následující definice:</span><span class="sxs-lookup"><span data-stu-id="5a2ae-116">If you wanted your HTTP action tooretry 4 times and wait 10 minutes between each attempt, you would use hello following definition:</span></span>

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

<span data-ttu-id="5a2ae-117">Další informace o podporovaných syntaxi najdete v tématu hello [části zásady opakování akce pracovního postupu a aktivační události][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="5a2ae-117">For more information on supported syntax, see hello [retry-policy section in Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

## <a name="catch-failures-with-hello-runafter-property"></a><span data-ttu-id="5a2ae-118">Catch – selhání s hello RunAfter vlastnost</span><span class="sxs-lookup"><span data-stu-id="5a2ae-118">Catch failures with hello RunAfter property</span></span>

<span data-ttu-id="5a2ae-119">Každá akce logic app deklaruje akce, které musíte dokončit před spuštěním hello akce, jako je řazení hello kroky v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-119">Each logic app action declares which actions must finish before hello action starts, like ordering hello steps in your workflow.</span></span> <span data-ttu-id="5a2ae-120">V definici akce hello toto řazení se označuje jako hello `runAfter` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-120">In hello action definition, this ordering is known as hello `runAfter` property.</span></span> <span data-ttu-id="5a2ae-121">Tato vlastnost je objekt, který popisuje, které akce a akce stavy provést akci hello.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-121">This property is an object that describes which actions and action statuses execute hello action.</span></span> <span data-ttu-id="5a2ae-122">Standardně jsou všechny akce, které jsou přidány prostřednictvím hello návrhář aplikace na základě logiky nastavena příliš`runAfter` předchozí krok text hello, pokud hello předchozího kroku `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-122">By default, all actions added through hello Logic App Designer are set too`runAfter` hello previous step if hello previous step `Succeeded`.</span></span> <span data-ttu-id="5a2ae-123">Však můžete přizpůsobit akce toofire tuto hodnotu, pokud předchozí akce `Failed`, `Skipped`, nebo sadu možné z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-123">However, you can customize this value toofire actions when previous actions have `Failed`, `Skipped`, or a possible set of these values.</span></span> <span data-ttu-id="5a2ae-124">Pokud byste chtěli tooadd tooa položky určené tématu Service Bus po určité akci `Insert_Row` selže, můžete použít následující hello `runAfter` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5a2ae-124">If you wanted tooadd an item tooa designated Service Bus topic after a specific action `Insert_Row` fails, you could use hello following `runAfter` configuration:</span></span>

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

<span data-ttu-id="5a2ae-125">Všimněte si hello `runAfter` vlastnost nastavena toofire, pokud hello `Insert_Row` akce je `Failed`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-125">Notice hello `runAfter` property is set toofire if hello `Insert_Row` action is `Failed`.</span></span> <span data-ttu-id="5a2ae-126">Akce toorun hello, pokud je stav akce hello `Succeeded`, `Failed`, nebo `Skipped`, použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="5a2ae-126">toorun hello action if hello action status is `Succeeded`, `Failed`, or `Skipped`, use this syntax:</span></span>

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> <span data-ttu-id="5a2ae-127">Akce, které jsou spuštěny a dokončeny úspěšně po předchozí akce se nezdařila, jsou označeny jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-127">Actions that run and complete successfully after a preceding action has failed, are marked as `Succeeded`.</span></span> <span data-ttu-id="5a2ae-128">Toto chování znamená, že pokud jste úspěšně catch všechny chyby v pracovním postupu, hello spustit je označena jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-128">This behavior means that if you successfully catch all failures in a workflow, hello run itself is marked as `Succeeded`.</span></span>

## <a name="scopes-and-results-tooevaluate-actions"></a><span data-ttu-id="5a2ae-129">Akce tooevaluate obory a výsledky</span><span class="sxs-lookup"><span data-stu-id="5a2ae-129">Scopes and results tooevaluate actions</span></span>

<span data-ttu-id="5a2ae-130">Podobné toohow můžete spustit po jednotlivé akce, můžete taky seskupit akce uvnitř [oboru](../logic-apps/logic-apps-loops-and-scopes.md), které fungují jako logické seskupení akce.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-130">Similar toohow you can run after individual actions, you can also group actions together inside a [scope](../logic-apps/logic-apps-loops-and-scopes.md), which act as a logical grouping of actions.</span></span> <span data-ttu-id="5a2ae-131">Obory jsou užitečné pro uspořádání vaše akce aplikace logiky i pro provádění agregační hodnocení na dobrý stav oboru.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-131">Scopes are useful both for organizing your logic app actions, and for performing aggregate evaluations on hello status of a scope.</span></span> <span data-ttu-id="5a2ae-132">Hello oboru samotné obdrží stav po dokončení všech akcí v oboru.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-132">hello scope itself receives a status after all actions in a scope have finished.</span></span> <span data-ttu-id="5a2ae-133">stav oboru Hello je určen s hello stejná kritéria jako spustit.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-133">hello scope status is determined with hello same criteria as a run.</span></span> <span data-ttu-id="5a2ae-134">Pokud hello konečné akcí v větev provádění `Failed` nebo `Aborted`, je stav hello `Failed`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-134">If hello final action in an execution branch is `Failed` or `Aborted`, hello status is `Failed`.</span></span>

<span data-ttu-id="5a2ae-135">toofire konkrétní akce pro všechny chyby, které bylo provedeno v rámci oboru hello, můžete použít `runAfter` s oborem, který je označen `Failed`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-135">toofire specific actions for any failures that happened within hello scope, you can use `runAfter` with a scope that is marked `Failed`.</span></span> <span data-ttu-id="5a2ae-136">Pokud *žádné* akce v oboru hello nezdaří, spuštění po obor selže umožňuje vytvořit jednu akci toocatch selhání.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-136">If *any* actions in hello scope fail, running after a scope fails lets you create a single action toocatch failures.</span></span>

### <a name="getting-hello-context-of-failures-with-results"></a><span data-ttu-id="5a2ae-137">Získávání kontextu hello chyb s výsledky.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-137">Getting hello context of failures with results</span></span>

<span data-ttu-id="5a2ae-138">I když zachytávání chyb z oboru je užitečné, můžete také kontextu toohelp pochopit přesně akce, které se nezdařila, a všechny chyby nebo stavové kódy, které byly vráceny.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-138">Although catching failures from a scope is useful, you might also want context toohelp you understand exactly which actions failed, and any errors or status codes that were returned.</span></span> <span data-ttu-id="5a2ae-139">Hello `@result()` funkce workflowu poskytuje kontext o hello výsledek všechny akce v oboru.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-139">hello `@result()` workflow function provides context about hello result of all actions in a scope.</span></span>

<span data-ttu-id="5a2ae-140">`@result()`přijímá jeden parametr, název oboru a vrátí pole všechny výsledky hello akce z v rámci tohoto oboru.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-140">`@result()` takes a single parameter, scope name, and returns an array of all hello action results from within that scope.</span></span> <span data-ttu-id="5a2ae-141">Tyto objekty akce zahrnují hello stejné atributy jako hello `@actions()` výstupy objektu, včetně čas spuštění akce, akce koncový čas, stav akce, akce vstupy, akce korelace ID a akce.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-141">These action objects include hello same attributes as hello `@actions()` object, including action start time, action end time, action status, action inputs, action correlation IDs, and action outputs.</span></span> <span data-ttu-id="5a2ae-142">toosend kontextu všechny akce, které se nepodařilo v rámci oboru, můžete snadno spárujete `@result()` fungovat s `runAfter`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-142">toosend context of any actions that failed within a scope, you can easily pair an `@result()` function with a `runAfter`.</span></span>

<span data-ttu-id="5a2ae-143">tooexecute akce *pro každou* akce v oboru, `Failed`pole filtru hello tooactions výsledky, které selhaly, může párovat `@result()` s  **[pole filtru](../connectors/connectors-native-query.md)**  akce a  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  smyčky.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-143">tooexecute an action *for each* action in a scope that `Failed`, filter hello array of results tooactions that failed, you can pair `@result()` with a **[Filter Array](../connectors/connectors-native-query.md)** action and a **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)** loop.</span></span> <span data-ttu-id="5a2ae-144">Můžete provést hello filtrované výsledek pole a provedení akce pro každé selhání pomocí hello **ForEach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-144">You can take hello filtered result array and perform an action for each failure using hello **ForEach** loop.</span></span> <span data-ttu-id="5a2ae-145">Tady je příklad, za nímž následuje podrobné vysvětlení, který odesílá požadavek HTTP POST s text odpovědi hello všechny akce, které se nepodařilo v rámci oboru hello `My_Scope`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-145">Here's an example, followed by a detailed explanation, that sends an HTTP POST request with hello response body of any actions that failed within hello scope `My_Scope`.</span></span>

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

<span data-ttu-id="5a2ae-146">Zde je podrobný návod toodescribe, co se stane:</span><span class="sxs-lookup"><span data-stu-id="5a2ae-146">Here's a detailed walkthrough toodescribe what happens:</span></span>

1. <span data-ttu-id="5a2ae-147">výsledek hello tooget všechny akce v rámci `My_Scope`, hello **pole filtru** filtrů Akce `@result('My_Scope')`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-147">tooget hello result of all actions within `My_Scope`, hello **Filter Array** action filters `@result('My_Scope')`.</span></span>

2. <span data-ttu-id="5a2ae-148">Hello podmínku pro **pole filtru** libovolnou `@result()` položku, která obsahuje stav rovna příliš`Failed`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-148">hello condition for **Filter Array** is any `@result()` item that has status equal too`Failed`.</span></span> <span data-ttu-id="5a2ae-149">Tato podmínka filtry hello pole s všechny výsledky akce z `My_Scope` tooan pole s jediným se nezdařilo výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-149">This condition filters hello array with all action results from `My_Scope` tooan array with only failed action results.</span></span>

3. <span data-ttu-id="5a2ae-150">Provedení **pro každou** akce hello **filtrovat pole** výstupy.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-150">Perform a **For Each** action on hello **Filtered Array** outputs.</span></span> <span data-ttu-id="5a2ae-151">Tento krok provede akci *pro každou* výsledek akce, který byl dříve nefiltruje se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-151">This step performs an action *for each* failed action result that was previously filtered.</span></span>

    <span data-ttu-id="5a2ae-152">Pokud se jedna akce v oboru hello nezdařila, hello akce v hello `foreach` spustit jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-152">If a single action in hello scope failed, hello actions in hello `foreach` run only once.</span></span> 
    <span data-ttu-id="5a2ae-153">Mnoho selhání akce způsobí, že jednu akci za selhání.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-153">Many failed actions cause one action per failure.</span></span>

4. <span data-ttu-id="5a2ae-154">Odeslání požadavku HTTP POST na hello `foreach` položky text odpovědi, nebo `@item()['outputs']['body']`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-154">Send an HTTP POST on hello `foreach` item response body, or `@item()['outputs']['body']`.</span></span> <span data-ttu-id="5a2ae-155">Hello `@result()` tvar položka je hello stejné jako hello `@actions()` utvářejí a lze analyzovat hello stejný způsobem.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-155">hello `@result()` item shape is hello same as hello `@actions()` shape, and can be parsed hello same way.</span></span>

5. <span data-ttu-id="5a2ae-156">Patří dva vlastní hlavičky s názvem selhání akce hello `@item()['name']` a hello se nezdařilo spuštění klienta, ID sledování `@item()['clientTrackingId']`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-156">Include two custom headers with hello failed action name `@item()['name']` and hello failed run client tracking ID `@item()['clientTrackingId']`.</span></span>

<span data-ttu-id="5a2ae-157">Pro referenci tady je příklad jednoho `@result()` položku zobrazující hello `name`, `body`, a `clientTrackingId` vlastnosti, které jsou analyzovány v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-157">For reference, here's an example of a single `@result()` item, showing hello `name`, `body`, and `clientTrackingId` properties that are parsed in hello previous example.</span></span> <span data-ttu-id="5a2ae-158">Mimo `foreach`, `@result()` vrátí pole z těchto objektů.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-158">Outside of a `foreach`, `@result()` returns an array of these objects.</span></span>

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

<span data-ttu-id="5a2ae-159">vzory tooperform různých výjimek, můžete použít výrazy hello uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-159">tooperform different exception handling patterns, you can use hello expressions shown previously.</span></span> <span data-ttu-id="5a2ae-160">Můžete zvolit jednu výjimka zpracování akce hello oboru, který přijímá hello celý filtrované pole selhání tooexecute a odebrat hello `foreach`.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-160">You might choose tooexecute a single exception handling action outside hello scope that accepts hello entire filtered array of failures, and remove hello `foreach`.</span></span> <span data-ttu-id="5a2ae-161">Můžete použít také další užitečné vlastnosti z hello `@result()` odpovědi uvedený výše.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-161">You can also include other useful properties from hello `@result()` response shown previously.</span></span>

## <a name="azure-diagnostics-and-telemetry"></a><span data-ttu-id="5a2ae-162">Azure diagnostiky a telemetrii</span><span class="sxs-lookup"><span data-stu-id="5a2ae-162">Azure Diagnostics and telemetry</span></span>

<span data-ttu-id="5a2ae-163">jsou skvělý způsob toohandle chyby a výjimky v rámci spustit zprostředkovatele Hello předchozích, ale také můžete identifikovat a řešit tooerrors nezávislé hello spustit.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-163">hello previous patterns are great way toohandle errors and exceptions within a run, but you can also identify and respond tooerrors independent of hello run itself.</span></span> 
<span data-ttu-id="5a2ae-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) poskytuje jednoduchý způsob toosend všechny pracovní postup události (včetně všech stavů spustit a akce) tooan účet služby Azure Storage nebo centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) provides a simple way toosend all workflow events (including all run and action statuses) tooan Azure Storage account or an Azure Event Hub.</span></span> <span data-ttu-id="5a2ae-165">tooevaluate spusťte stavy, můžete monitorovat hello protokoly a metriky nebo publikovat je do libovolného monitorování nástroje, kterému dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-165">tooevaluate run statuses, you can monitor hello logs and metrics, or publish them into any monitoring tool you prefer.</span></span> <span data-ttu-id="5a2ae-166">Jednou z možných možností je toostream všechny události hello prostřednictvím centra událostí Azure do [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="5a2ae-166">One potential option is toostream all hello events through Azure Event Hub into [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> <span data-ttu-id="5a2ae-167">V Stream Analytics můžete napsat dotazy za provozu mimo jakékoli anomálie, průměry nebo selhání z hello diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-167">In Stream Analytics, you can write live queries off any anomalies, averages, or failures from hello diagnostic logs.</span></span> <span data-ttu-id="5a2ae-168">Stream Analytics můžete snadno výstupní tooother zdroje dat jako fronty, témata, SQL, databáze Cosmos Azure a Power BI.</span><span class="sxs-lookup"><span data-stu-id="5a2ae-168">Stream Analytics can easily output tooother data sources like queues, topics, SQL, Azure Cosmos DB, and Power BI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a2ae-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a2ae-169">Next Steps</span></span>

* [<span data-ttu-id="5a2ae-170">V tématu Jak zákazník sestavení službou Azure Logic Apps zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="5a2ae-170">See how a customer builds error handling with Azure Logic Apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [<span data-ttu-id="5a2ae-171">Najít další aplikace logiky příkladů a scénářů</span><span class="sxs-lookup"><span data-stu-id="5a2ae-171">Find more Logic Apps examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="5a2ae-172">Zjistěte, jak toocreate automatizované nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="5a2ae-172">Learn how toocreate automated deployments for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="5a2ae-173">Vytvoření a nasazení aplikací logiky s využitím sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a2ae-173">Build and deploy logic apps with Visual Studio</span></span>](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
